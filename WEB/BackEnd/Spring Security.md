# Spring Security

## 개요
사용자의 로그인과 세션 트래킹은 웹 애플리케이션에서 필수적인 기능이다.<br>
과거에는 HttpSession과 Cookie를 이용해서 직접 구현했지만 스프링을 적용할 때는 Spring Security와 약간의 설정을 적용하는 것 만으로도 구현이 가능하다.<br>

스프링 시큐리티는 원래는 별도의 프레임워크로 시작되었다고 한다. 그 후 스프링으로 프로젝트가 통합되었고 이를 사용하면 개발자는 약간의 코드와 설정만으로 로그인 처리와 자동 로그인, 로그인 후에 페이지 이동등을 처리할 수 있기 때문에 개발의 생산성을 높일 수 있다.<br>

만약 이러한 프레임워크가 없었다면 세션을 체크하는 로직을 구현해야할 것이고 리다이렉션을 시키는 로직을 개발자가 직접 구현해야할 것이다.


## 개념

스프링 시큐리티는 필터 기반으로 동작한다. 즉 스프링의 외부에서 동작하는 것이다.

클라이언트가 요청을 보내고 그 요청을 서블릿이나 JSP가 처리한다. 처음 응답을 받는 서블릿은 디스패쳐 서블릿이다. 필터는 요청이 디스패쳐 서블릿에 들어오기 전에 여러겹의 필터를 거치는데 이를 필터 체인이라고 한다. 마치 체인처럼 연결되어 작동하는 느낌이다.

우리가 스프링 시큐리티의 존재를 모르고 로그인 로그아웃을 구현한다면 아마 이렇게 할 것이다.

아이디와 비밀번호를 DB에 저장한다.

사용자의 로그인 요청이 들어오면 작동할 쿼리를 작성한다.

```sql
select * from user where id = 입력받은 아이디 and pw = 입력받은 비밀번호;
```

이렇게 로그인이 성공하면 정보를 세션에 담아 쿠키를 발행한다.

로그인이 필요한 경로에서 쿠키를 받아 세션을 불러오고 세션이 존재하는지 체크한다. 만약 없다면 로그인 페이지로 리다이렉션 시킨다. 검증이 필요한 경로에서 매번 로직을 작성하니 중복된 코드를 개발자가 기계적으로 작성해야한다.

이를 해결하기 위해 필터를 활용한다.

특정한 경로에 접근할 때 필터가 동작하도록 해서 동일한 로직을 필터로 분리시킨다.

로그인 서블릿 클래스
```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) {
  String id = req.getParameter("id");
  String pw = req.getParameter("pw");

  HttpSession session = req.getSession();

  session.setAttribute("loginInfo", id);

  resp.sendRedirect("/mainpage");
}
```
로그인 체크 서블릿 클래스
```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
  //사용자의 세션 불러오기
  HttpSession session = req.getSession();

  if(session.isNew()) {
    resp.sendRedirect("/login");
    return;
  }
  
  if(session.getAttribute("loginInfo")) {
    resp.sendRedirect("/login");
    return;
  }

  resp.sendRedirect("/mainpage");
}
```

필터 적용
```java
@WebFilter(urlPatterns = {"/board/*"})
public class loginCheckFilter() implements Filter{
  @Override
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
    HttpServletRequest req = (HttpServletRequest)request;
    HttpServletResponse resp = (HttpServletResponse)response;

    HttpSession session = req.getSession();

    if(session.getAttribute("loginInfo") == null) {
      resp.sendRedirect("/login");
      return;
    }

    //다음 필터나 목적지로 갈 수 있도록 함.
    chain.doFilter(request, response);
  }
}
```

이러한 기능들을 스프링 시큐리티를 사용한다면 간단히 설정만해도 사용할 수 있다.
(프레임워크 == 반제품)

## 스프링 시큐리티 필터 체인
![Alt text](https://cphinf.pstatic.net/mooc/20200301_110/1583061393744OaYB4_PNG/mceclip1.png)

* SecurityContextPersistenceFilter : SecurityContextRepository에서 SecurityContext를 가져오거나 저장한다.

* LogoutFilter : 로그아웃 요청을 감시하고 해당 유저를 로그아웃처리한다.

* UsernamePasswordAuthenticationFilter : 설정한 경로로 오는 요청을 감시하고 유저 인증처리를 한다. 인증 과정은 다음과 같다. 1. AuthenticationManager를 이용한 인증 실행 2. 인증 성공시 Authentication 객체를 SecurityContext에 저장후 AuthenticationSuccessHandler 실행 3. 인증 실패시 AuthenticationFailureHandler 실행 

* DefaultLoginPageGeneratingFilte : 인증을 위한 로그인폼 경로를 감시한다.

* BasicAuthenticationFilter : HTTP 기본 인증 헤더를 감시하여 처리

* RequestCacheAwareFilter : 로그인 성공후 원래 요청 정보를 재구성하기위해 사용

* SecurityContextHolderAwareRequestFilter : 필터 체인상의 다음 필터들에게 부가 정보를 제공한다.

* AnonymousAuthenticationFilter : 이 필터가 호출되는 시점까지 사용자 정보가 인증되지 않으면 인증 토큰에 사용자가 익명 사용자로 나타난다.

* SessionManagementFilter : 인증된 사용자와 관련된 모든 세션을 추적한다.

* ExceptionTranslationFilter : 보호된 요청을 처리하는 중에 발생할 수 있는 예외를 위임하거나 전달하는 역할을 한다.

* FilterSecurityInterceptor : AccessDecisionManager로 권한부여 처리를 위임해 접근 제어결정을 돕는다.

![Alt text](https://cphinf.pstatic.net/mooc/20200301_136/1583062306462164xS_PNG/mceclip2.png)

실제 로그인을 처리하는 필터는 `UsernamePasswordAuthenticationFilter`이다. 이는 `AbstractAuthenticationProcessionFilter`를 구현한 것이다.

과거에는 `AuthenticationFilter`를 사용했다고 한다.

필터의 동작 순서는 이러하다.

유저가 로그인을 시도한다.

* AuthenticationManager
* AuthenticationProvider
* UserDetailsService

이들을 통해 유저의 정보를 읽어오고 인증을 처리한다.

이중 가장 중요한 요소는 실제로 인증을 처리하는 `UserDetailsService이다`.

`UserDetailsService`는 인터페이스로 `loadUesrByUsername`이라는 단 하나의 메소드만 가진다. 인터페이스니 개발자가 구현해서 사용하면 되는데 유저의 정보는 db에 저장되어있고 이는 프로젝트 스펙에 따라 달라지기때문이다.

여기서 username은 스프링 시큐리티에서 사용하는 명칭으로 우리가 흔히 사용하는 아이디를 말한다.

반환 타입은 `UserDetails`라는 이름의 인터페이스 타입이다.

사용자 인증과 관련된 정보들을 저장하는 역할을한다.

주로 User클래스를 사용하곤 하는데 이는 `UserDetails`의 구현체로 스프링 시큐리티에서 제공하고있다. 빌더타입을 지원하고 username, password, authorities를 넣어 생성하면 된다.

스프링 시큐리티는 인메모리 세션 저장소인 `SecurityContextHolder`에 `UserDetails`정보를 저장한다.

클라이언트에게 세션ID와 함께 응답을 한다.

이후 요청이 들어왔을 때 쿠키의 세션ID정보를 통해 로그인된 정보가 존재하는지 확인한다.

유효한 정보라면 인증처리를 해준다.


## 사용

스프링 시큐리티는 단순히 `applicatioin.properties`를 이용하는 설정보다 코드를 이용해서 설정을 조정하는 경우가 더 많다.

Config파일을 하나 작성해서 사용하면 된다.
`CustomSecurityConfig`클래스를 추가해서 작성한다.

처음 적용을 하면 내 프로젝트의 모든 경로에 대해서 로그인을 필요로 한다. 로그인이 필요한 경로와 그렇지 않은 경우가 있기에 따로 셋팅을 해주는 게 좋다.
해당 Config파일에 `SecurityFilterChain` 객체를 반환하는 메소드를 작성한다.

일단 별도의 인증절차를 거치지 않도록 구성한다.

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) {
 return http.build(); 
}
```
이렇게 설정하면 별도의 인증절차없이 사용이 가능하다.

스프링 시큐리티의 동작은 웹에서 사용하는 필터를 통해서 동작하고 많은 수의 필터들이 단계별로 동작한다. 따라서 문제가 발생하면 어떤 필터에서 문제가 생겼는지 알 수 있도록 로그 설정을 최대한 낮게 설정해서 관련된 에러를 볼 수 있게 설정하는 것이 좋다.

정적자원의 처리:

어떤 파일에 접근하는 것은 필터를 적용하지 않아도 된다. 예를 들면
정적 파일 css나 js와 같은 파일에도 필터가 적용되곤 한다. 
이를 Config파일에서 설정해 줄 수 있다.
다음과 같은 메소드를 추가해준다.
```java
@Bean
public WebSecurityCustomizer webSecurityCustomizer() {
  return (web) -> web.ignoring().requestMatchers(PathRequest.toStaticResources(.atCommonLocations());
}
```

## 인증과 인가
인증 : 스스로를 증명하다라는 뜻이며 로그인의 개념이다.
인가 : 허가나 권한이라는 개념으로 특정한 자원에 접근할 수 있는 권한이 있는지를 확인하는 개념이다.
 스프링 시큐리티에서 로그인에 해당하는 인증 단계는 다음과 같은 과정을 거친다.
 1. 사용자 아이디만으로 사용자의 정보를 로딩한다.
 2. 로딩된 사용자의 정보를 이용해서 패스워드를 검증한다.

 인증처리는 `인증 제공자(Authentication Provider)` 라는 존재를 이용해서 처리되는데 인증 제공자와 그 이하의 흐름은 일반적으로 커스터마이징해서 사용하는 경우는 거의 없다.

 인증 처리를 위한 `UserDetailService`

 위에서 살펴본 것 처럼 가장 중요한 객체는 실제로 인증을 처리하는 `UserDetailService`라는 인터페이스의 구현체이다.

 개발자가 구현해야하는 부분은 해당 인터페이스를 구현해서 username이라는 사용자의 아이디 인증을 코드로 구현하는 것이다.

 `loadUserByUsername`의 반환 타입은 `UserDetails`라는 인터페이스 타입으로 지정되어있다. 이는 사용자인증과 관련된 정보들을 저장하는 역할을 한다.

 스프링 시큐리티는 내부적으로 `UserDetails`타입의 객체를 이용해서 패스워드를 검사하고 사용자 권한을 확인하는 방식으로 동작한다.

 스프링 시큐리티의 api에는 `userDetails`라는 인터페이스를 구현한 User라는 클래스를 제공하고 빌더방식을 지원하므로 앞에서 사용한 loadUserByUsername()에 약간의 코드를 추가해줄 수 있다.

 * `PasswordEncoder`

    스프링 시큐리티는 기본적으로 PasswordEncoder를 필요로 한다. 이는 인터페이스로 제공되는데 이를 구현하거나 스프링 시큐리티 api에서 제공하는 클래스를 지정할 수 있다. 해당 타입의 클래스중 BCryptPasswordEncoder는 해시 알고리즘으로 암호화 처리되는데 같은 문자열이라도 매번 해시 처리된 결과가 다르다는 특징이 있다.
 
 로그인 기능이 제대로 동작하려면 Config파일에 PasswordEncoder를 Bean으로 등록하고 이를 주입해줘야한다. 

권한 체크는 특정한 경로에 지정하는 경우가 일반적인데 예를 들어 게시글 목록은 모두가 볼 수 있어야하지만 게시글 작성은 로그인이 되어야한다. 이런 권한 설정은 코드로 작성할 수도 있고 어노테이션을 이용해서 지정할 수도 있다. 어노테이션을 통해 지정하는 방식은 설정 관련 클래스에
`@EnableGlobalMethodSecurity` 를 추가해줘야 한다.
그리고 `@PostAuthrize`어노테이션을 통해 사전, 사후 권한을 체크할 수 있다. 여기서 자주 사용하는 표현식은 다음과 같다.
* authenticated()
* permitAll()
* anonymous
* hasRole
* hasAnyRole<br>


## JWT를 이용한 인증/인가

### 개요

로그인 방법은 세션만 있는 것이 아니다. JWT(JSON Web Token)는 json문자열로 이루어진 문자열로 인증/인가를 요청한다.

최근의 프론트엔드는 React나 Vue를 이용하여 서버와 클라이언트의 개발을 분리시켜 진행한다. 프론트엔드는 Ajax를 이용해서 서버를 호출하고 데이터를 받아 처리하게된다.

머나먼 과거의 서블릿을 사용하여 백엔드에서 html파일을 보관하여 서비스하던 때와 비교하면 많이 달라졌다.

이러한 이유로 HttpSession이나 쿠키를 사용한 인증방식에 제한이 생긴다. 이를 해결하기 위해 인증받은 사용자들이 토큰을 이용하여 인증/인가를 인증절차를 거치는 방법을 사용한다.

API서버와 MVC서버의 가장 큰 차이점은 바로 화면의 유무이다. 

### 필요성

API서버가 미리 약속해둔 경로로 요청이 들어오면 데이터를 조회하여 응답한다. 그러나 이러한 경로를 아무나 호출할 수 있게 된다면 누구나 글을 작성할 수 있고 데이터를 조회할 수 있다.

ㄴ> API서버에서도 인증/인가가 필요한 상황

방법은 여러가지가 있다.

* API서버를 호출하는 프로그램의 IP를 서버에 저장해두고 요청이 들어왔을 때 호출한 IP와 서버에 저장된 IP를 비교하여 허용된 IP에 대해서만 서버에서 결과를 만들어주는 방식. IP와 더불어 정해진 키 값을 이용하는 것이 일반적이라고 한다.

* 토큰을 이용하는 방식

인증에 성공하면 토큰을 발급해준다. API를 호출할 때 토큰을 같이 전달해 API서버에서 토큰을 확인하고 결과를 만들어주는 방식이다.

이러한 토큰을 Access Token이라고 부른다.

### 스프링 시큐리티 + JWT를 사용하는 방법

구현 방법은 크게 두 가지가 있다.

 * 컨트롤러단에서 로그인을 하고 서비스단에서 토큰을 발급해주는 방법

 * 필터를 사용해 로그인을 하는 방법.(스프링 시큐리티를 활용한 방법)


위에서 살펴봤듯이 스프링 시큐리티에선 이미 로그인을 처리하는 필터와 방식이 존재한다.

`UsernamePasswordAuthenticationFilter`에서 로그인을 처리한다.

개인적으론 필터를 통한 로그인 방식을 선호한다. 스프링 시큐리티를 사용하며 프레임워크의 기능을 이용하는게 좋다는 생각이 들어서이다.

스프링 시큐리티에서도 말하자면 커스텀해서 사용할 수 있는 로그인 필터를 제공한다.

바로 `AbstractAuthenticationProcessingFilter`이다.

이를 상속받은 클래스가 바로 `UsernamePasswordAuthenticationFilter`이다.

`AbstractAuthenticationProcessingFilter`는 `AuthenticationManager`를 필수적으로 설정해줘야한다. 위에서 살펴본 이유이다.

이는 `SecurityConfig` 설정 클래스에서 할 수 있다.

인증을 하는데 `UserDetailsService`도 필요하기 때문에 같이 설정해준다.

```java
AuthenticationManagerBuilder authenticationManagerBuilder =
                http.getSharedObject(AuthenticationManagerBuilder.class);

        authenticationManagerBuilder
                .userDetailsService(apiUserDetailsService)
                .passwordEncoder(passwordEncoder());

        AuthenticationManager authenticationManager =
                authenticationManagerBuilder.build();

        http.authenticationManager(authenticationManager);
```


그리고 LoginFilter를 만들어준다.

위에서 말한대로 `AbstractAuthenticationProcessingFilter`를 상속받아 구현한다.

필요한 로직은
1. json을 파싱하여 id와 pw를 알아내는 로직
2. 인증정보를 만드는 부분.

1번의 로직은 어렵지 않고 인증 정보를 만드는 부분은 UsernamePasswordAuthenticationToken을 만들어서 설정해둔 AuthenticationManager를 사용해 Authentication타입의 객체를 리턴해주면 된다.

테스트해보면 로그인 처리가 완료되어 루트 경로로 리다이렉션되는 것을 볼 수 있다.

로그인이 성공한 뒤 스프링 시큐리티는 디폴트로 / 경로로 리다이렉션을 시켜준다. 이는 AuthenticationSuccessHandler의 역할이다. 이 부분을 커스텀해주면 된다.

여담 : 내가 누군가에게 오버라이드를 쉽게 설명해줄 때 나는 오버라이드는 커스텀이라고 한다. 일반화되어있는 클래스를 상속받아 기본적인 골격과 기능을 하나하나 직접 구현할 필요없이 사용하고 내 필요에 맞게 수정해서 사용하는 것이다.

`AuthenticationSuccessHandler`를 상속받아 커스텀 `LoginSuccessHandler`클래스를 작성해준다. jwt를 아용한 인증/인가 방식을 사용하고 있으니 로그인이 성공한 뒤 수행해줘야하는 작업은 Access Token을 발급하는 것이다.

jwt토큰을 생성하고 검증하는 등의 로직은 직접 구현할 필요없이 jjwt라이브러리를 사용하면 된다.

개발자가 작성해야할 부분은 어떤 정보들을 claim으로 넣어줄 것인지, 어떤 알고리즘을 사용할지, 어떤 암호화, 복호화 키를 사용할 것인지 이다.


지금까지의 과정을 정리해보면 아래와 같다.

1. 클라이언트가 `/로그인 필터에서 지정해둔 경로` 요청을 보낸다.

2. 필터를 거쳐 커스텀한 로그인 필터에 도착한다.

3. 로그인을 시도하고 성공한다면 `커스텀 AuthenticationSuccessHandler`가 낚아채서 토큰을 생성한다.


### jwt 토큰 검증

필터를 사용해 인증이 필요한 경로에 대한 요청에 대해 검증해주면 된다.

이 때 사용하는 필터는 `OncePerRequestFilter`이다.

이름 그대로 하나의 요청에 대해서 한번씩 동작하는 필터로 서블릿의 필터와 유사하다.

이 클래스를 상속받아 토큰을 검증해주면 된다.

이러한 방식으로 인증을 수행할 때 스프링 시큐리티에서 사용하는 `SecurityContextHolder`와 관련된 기능들을 사용할 수 없다는 단점이 있다.

그래서 토큰 체크 필터에서 토큰을 체크하고 유효한 토큰이라면 인증 정보를 `SecurityContextHolder`에 저장하여 사용하도록 설정해줄 수 있다.

```java
SecurityContextHolder.getContext().setAuthentication(authentication);
```

이렇게 하면 컨트롤러단에서 유저의 인증정보를 받아서 사용할 수 있다.

예를 들면 게시글의 작성자만 수정할 수 있게하기 위해 컨트롤러에서 요청한 유저의 정보를 받아서 비즈니스 로직에서 요청자와 작성자를 비교한다던지 ADMIN 권한을 가진 사용자만 접근할 수 있게 한다던지 할 수 있다.

## 쿠키? 로컬 스토리지?
너무 생각을 고집했던 것 같다.

나는 특정한 방법으로 토큰을 발급해주기 보단 값만 던져주고 프론트에서 알아서 처리하도록 구현했었다. API서버는 특정 기술을 사용해 응답하기보단 범용적으로 사용되어야한다는 생각이 있었기 때문이다. 서버와 클라이언트의 분리.

근데 쿠키를 사용해도 괜찮을 것 같다. 이미 그러한 사이트들을 봐왔고 전에 객체지향에 대한 글을 본적이 있었다. 현실세계의 것들을 1:1로 옮겼을 때 유지보수와 개발의 어려움이 생긴다면 좋은 방법이라고 할 수 있을까 하는 내용이였는데 너무 융퉁성이 없었나 싶다.

이상적으로 생각하지 않고 현실적으로 생길 수 있는 문제를 생각하고 대처해야겠다.



