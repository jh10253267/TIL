# Interceptor

## 인터셉터란?

컨트롤러 실행 전과 실행된 후에 공통 처리를 할 수 있도록 도와주는 클래스를 말한다.<br>

인터셉터는 디스패쳐 서블릿에서 컨트롤러로 요청을 보낼 때 컨트롤러에서 디스패쳐 서블릿에 응답을 보낼 때 사용된다.<br>
비슷한 개념으로는 서블릿 필터가 있다.<br><br>
![intercepter](https://cphinf.pstatic.net/mooc/20180222_261/1519262329628q8DQN_JPEG/1.jpg?type=w760)

스프링이 동작될 때 클라이언트로부터 요청이 들어올 때 필터가 존재한다면 필터를 수행하고 DispatcherServlet이 수행된다.

즉 필터는 요청을 받아내기 전, 응답이 나가기 전에 수행된다.

그림을 보면 디스패쳐서블릿이 선처리 작업을 해주고 핸들러매핑을 통해서 어떤 핸들러가 동작을 해야하는지 알아낸 뒤 핸들러가 실행된다. 이 중간에 핸들러 인터셉터라는 것이 있다.

요청이 끝났다면 view의 정보를 디스패쳐서블릿에 넘겨주고 이제 뷰 리졸버를 통해서 뷰의 정보를 얻어오고 해당 뷰를 찾아서 응답하는 일까지 수행을 한다.

인터셉터는 디스패쳐서블릿에서 핸들러로 요청을 보낼 때, 핸들러에서 디스패쳐서블릿으로 응답을 보낼 때 동작을 하게 된다.

이를 이용해서 컨트롤러 실행 전, 실행 후 콘솔에 로그를 남길 수 있다.

## 사용 방법

인터셉터는 HandlerInterceptor 인터페이스를 구현하거나 아니면 HandlerInterceptorAdapter클래스를 상속받으면 사용할 수 있다. 

인터셉터 패키지를 만들어준다.

그런 다음 클래스를 상속받아서 preHandle, postHandle을 오버라이딩하여 내용을 작성한다.

아래의 코드는 request객체를 이용하여 서버에 요청을 보낸 클라이언트의 IP와 요청 경로를 콘솔에 출력하고 있다.
(물론 웹이기 때문에 println을 사용해선 안된다. 임시로 작성해본 것이다.)

```java
public class LoggingInterceptor extends HandlerInterceptorAdapter {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("요청 클라이언트 IP: " + request.getRemoteAddr());
        System.out.println("요청 URI: " + request.getRequestURI());
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    }
}
```

이렇게 작성해준 뒤 WebMvcConfig클래스에서 addInterceptors 메소드를 상속받는다.

```java
public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoggingInterceptor());
    }
```

이런식으로 작성한 인터셉터 객체를 넣어주면 인터셉터를 사용할 수 있다.




