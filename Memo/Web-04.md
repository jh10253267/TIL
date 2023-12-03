# Spring Web MVC

## 개요
### MVC란?

MVC는 Model-View-Controller의 약자로원래는 제록스 연구소에서 일하던 트뤼그베 린즈커그가 처음으로 소개한 개념으로, 데스트톱 어플리케이션용으로 고안되었다.<br>
* Model : 모델은 뷰가 렌더링하는데 필요한 데이터이다. 예를 들어 사용자가 요청한 상품 목록이나, 주문 내역이 이에 해당한다.
* View : 웹 애플리케이션에서 뷰(View)는 실제로 보이는 부분이며, 모델을 사용해 렌더링을 한다. 뷰는 JSP, JSF, PDF, XML등으로 결과를 표현한다.
* Controller : 컨트롤러는 사용자의 액션에 응답하는 컴포넌트이다. 컨트롤러는 모델을 업데이트하고, 다른 액션을 수행한다.

## MVC의 종류
MVC 모델은 지속적으로 개선되고 발전해왔는데 다음과 같다. <br>
### MVC Model 1 아키텍처
![MVC1](https://cphinf.pstatic.net/mooc/20180219_180/1519003368125BcfqV_PNG/1.png?type=w760)

### MVC Model 2 아키텍처
![MVC2](https://cphinf.pstatic.net/mooc/20180219_65/1519003382079lUcI5_PNG/2.png?type=w760)
### MVC Model2 발전형태
![MVC2 advanced](https://cphinf.pstatic.net/mooc/20180219_149/15190034013354diDI_PNG/3.png?type=w760)

 클라이언트가 보내는 모든 요청을 프론트 컨트롤러라는 서블릿 클래스가 받는다.<br> 

 프론트 컨트롤러는 요청만 받고 실제로 일을 수행하지 않고 컨트롤러 클래스(혹은 핸들러 클래스)에게 위임한다.<br>

 서블릿은 관련된 요청을 처리하기에 불편한 점이 많다.<br>
 
  그래서 이러한 문제를 해결하기 위해 프론트 컨트롤러는 요청만 받고 그 요청에 맞는 컨트롤러를 찾아 일을 위임하는 것이다.<br>
  
   그리고 이런 컨트롤러 클래스는 자바 빈즈를 이용해서 결과를 만들어내고 그 결과를 모델에 담고 프론트 컨트롤러에게 보내면 알맞은 뷰에게 모델은 전달하고 그 결과를 출력하게된다.<br> 
   
   스프링 MVC는 Model2 MVC패턴을 채택하고 있다.

   <b>여기서 잠깐!</b> 

   프론트 컨트롤러 개념을 이용했을 때의 장점이 뭐가있을까??

   * 모든 요청을 한 곳에서 받으니 일관된 처리를 할 수 있을것이다.
   * 전체적인 흐름을 통제할 수 있을것이다.
  
  정리하자면 <br>

  * 요청에 대한 각각의 뷰와 컨트롤러가 독립적으로 호출되고 실행된다면 서버입장에서 관리하기 어려워진다.

  * 이럴 때 대표 컨트롤러(Front Controller)를 두고 뷰에서 들어오는 모든 요청을 담당하여 웹 애플리케이션을 실행하는 모든 요청을 일괄적으로 처리할 수 있다.<br>

  * 이러한 패턴을 프론트 컨트롤러 패턴 혹은 퍼사드 패턴(모든 흐름이 하나의 객체를 통해서 진행되는 패턴)이라고 한다.

## Spring MVC의 기본 동작 흐름
![Spring MVC](https://cphinf.pstatic.net/mooc/20180219_116/1519003779294ejdEx_PNG/1.png?type=w760)

모든 요청을 디스패쳐 서블릿이라고 하는 프론트 컨트롤러가 받는다.
그림을 잘 보면 파란색 블록과 보라색 블록이 보인다. 파란색 블럭은 스프링에서 제공해주는 것이라서 따로 작성할 필요가 없고 보라색이 우리가 작성해줘야하는 부분이다.

스프링 MVC의 작동 방식은 다음과 같다.

1. 모든 요청을 디스패쳐 서블릿이 받는다.
2. 요청을 처리해줄 컨트롤러와 메소드가 무엇인지 핸들러 매핑에게 물어본다. 어떤 요청을 어떤 컨트롤러가 동작할 지 xml이나 javaconfig로 설정해준다. 
3. 이런 부분들을 핸들러매핑이 인식하고 관리하기 시작한다. 디스 핸들러매핑으로부터 이러한 정보를 알아낸다. 
5. 핸들러 어댑터에게 실행을 요청한다. 
6. 여기서 결정된 컨트롤러와 해당 메소드가 실행된다. 
7.  결과를 모델에 받아서 디스패쳐 서블릿에게 전달을 하게 된다. 8. 컨트롤러가 리턴한 뷰 네임을 알아내게되고 적절한 뷰 리졸버를 통해서 뷰를 출력하게된다.


## @RestController

Spring 4 에서 Rest API 또는 Web API를 개발하기 위해 등장한 애노테이션이다.
이전 버전의 @Controller와 @ResponseBody를 포함하고있다.
스프링 3버전까지는 컨트롤러와 리스폰스바디 애노테이션을 결합시켜 쓰는 방식으로 사용했던게 하나로 합쳐졌다.

### MessageConvertor
외부에서 전달받는 Json객체를 자바의 객체로 변환하거나 자바 객체를 Json형식으로 리턴해준다거나 

자바 객체와 HTTP 요청 / 응답 바디를 변환하는 역할
@ResponseBody, @RequestBody
@EnableWebMvc 로 기본 설정
WebMvcConfigurationSupport 를 사용하여 Spring MVC 구현
Default MessageConvertor 를 제공 

기본 메시지 컨버터는 잭슨 라이브러리를 사용하고 있다. 이런 이유로 스프링(스프링 부트 아님)에서 RestController를 사용하는데 Jackson라이브러리를 의존성에 추가하지 않았다면 500번 서버 내부 에러를 발생시킨다.

컨트롤러의 메소드에서는 JSON으로 변환될 객체를 반환한다.

jackson라이브러리를 추가할 경우 객체를 JSON으로 변환하는 메시지 컨버터가 사용되도록 @EnableWebMvc에서 기본으로 설정되어 있다.
jackson라이브러리를 추가하지 않으면 JSON메시지로 변환할 수 없어 500오류가 발생합니다.
사용자가 임의의 메시지 컨버터(MessageConverter)를 사용하도록 하려면 WebMvcConfigurerAdapter의 configureMessageConverters메소드를 오버라이딩 하도록 한다.

## DispatcherServlet

디스패쳐 서블릿은 쉽게 말하자면 우리가 어떤 기업에 전화를 걸 때 해당 부서에 직접걸지 않고 고객센터 대표번호로 전화를 건다. 그리고 고객센터에선 내 요청에 맞는 부서로 전화를 연결시켜주곤한다. 디스패쳐 서블릿은 여기서 고객센터에 해당한다.

![디스패쳐 서블릿](https://cphinf.pstatic.net/mooc/20180219_281/1519003870301bOehw_PNG/2.png?type=w760)

1. 요청이 들어오면 요청에 대한 선처리 작업을 한다.
2. HandlerExecutionChain을 결정한다.
3. HandlerExecutionChain을 실행한다.
4. 예외가 발생하지 않았다면 뷰를 렌더링한다.
### 선처리 작업
![선처리 작업](https://cphinf.pstatic.net/mooc/20180219_91/1519003885824QT31y_PNG/3.png?type=w760)
스프링 MVC는 지역화를 지원한다. 우리가 어떤 웹사이트에 들어갔을 때 원래는 영어로 작성된 사이트인데도 한글로 보이는 경우가 있다. 그것은 해당 클라이언트로부터 오는 요청의 헤더에 따라서 각 언어에 맞는 페이지를 보여주는 것을 지역화라고 한다.

컨트롤러가 가진 메소드 안에서 리퀘스트 객체를 사용하는 경우가 있는데 이럴 때 선언만 해주면 사용할 수 있다. 이게 RequestContextHolder 부분이다. 이런 방식은 스프링이 웹 기술에 종속되는 문제점을 갖는다. 그렇기에 권장하는 방법은 아니다.

플래시 맵 스프링3에서 추가된 기능 리다이렉트로 값을 전달할 때 사용됨. ?파리미터 같은 걸 이용. 그러나 복잡해지고 길이제한도 있다. 그래서 딱 한번 리다이렉트 될 때 값을 유지시킬 수 있게 해준다. 현재 실행이 리다이렉션이 되었을 때만.(선처리)

사용자가 파일 업로드를 했을 때 파일에 대한 특수한 리퀘스트객체가 필요. 멀티파트라는 요청이 들어왔을 때 멀티파트 리졸버가 이 부분을 결정하고 실행해줌.

```
요청 선처리 작업시 사용되는 컴포넌트

org.springframework.web.servlet.LocaleResolver

지역 정보를 결정해주는 전략 오브젝트이다.
디폴트인 AcceptHeaderLocalResolver는 HTTP 헤더의 정보를 보고 지역정보를 설정해준다.
org.springframework.web.servlet.FlashMapManager

FlashMap객체를 조회(retrieve) & 저장을 위한 인터페이스
RedirectAttributes의 addFlashAttribute메소드를 이용해서 저장한다.
리다이렉트 후 조회를 하면 바로 정보는 삭제된다.
org.springframework.web.context.request.RequestContextHolder

일반 빈에서 HttpServletRequest, HttpServletResponse, HttpSession 등을 사용할 수 있도록 한다.
해당 객체를 일반 빈에서 사용하게 되면, Web에 종속적이 될 수 있다.
org.springframework.web.multipart.MultipartResolver

멀티파트 파일 업로드를 처리하는 전략
```
 ![](https://cphinf.pstatic.net/mooc/20180219_20/1519003954110F9wyd_PNG/4.png?type=w760)

요청 전달

핸들러 매핑으로 결정

발견 못함 -> 404

핸들러 어댑터 결정

없다면 서버의 잘못

```
org.springframework.web.servlet.HandlerMapping

HandlerMapping구현체는 어떤 핸들러가 요청을 처리할지에 대한 정보를 알고 있다.
디폴트로 설정되는 있는 핸들러매핑은 BeanNameHandlerMapping과 DefaultAnnotationHandlerMapping 2가지가 설정되어 있다.

org.springframework.web.servlet.HandlerExecutionChain

HandlerExecutionChain구현체는 실제로 호출된 핸들러에 대한 참조를 가지고 있다.
즉, 무엇이 실행되어야 될지 알고 있는 객체라고 말할 수 있으며, 핸들러 실행전과 실행후에 수행될 HandlerInterceptor도 참조하고 있다.

org.springframework.web.servlet.HandlerAdapter

실제 핸들러를 실행하는 역할을 담당한다.
핸들러 어댑터는 선택된 핸들러를 실행하는 방법과 응답을 ModelAndView로 변화하는 방법에 대해 알고 있다.
디폴트로 설정되어 있는 핸들러어댑터는 HttpRequestHandlerAdapter, SimpleControllerHandlerAdapter, AnnotationMethodHanlderAdapter 3가지이다.

@RequestMapping과 @Controller 애노테이션을 통해 정의되는 컨트롤러의 경우 DefaultAnnotationHandlerMapping에 의해 핸들러가 결정되고, 그에 대응되는 AnnotationMethodHandlerAdapter에 의해 호출이 일어난다.
```
 ![](https://cphinf.pstatic.net/mooc/20180219_167/1519004040926yL8eC_PNG/5.png?type=w760)
 결정 되었다면 사용 가능한 인터셉터가 있는지 확인한다. 
 프리한들을 호출해 요청 처리 -> 핸들러 실행
 리턴 객체가 모델 뷰가 뷰를 갖지 않는다면 RequestToViewNameTranslator 실행.

 그 외엔 인터셉터
 ```
 org.springframework.web.servlet.ModelAndView

ModelAndView는 Controller의 처리 결과를 보여줄 view와 view에서 사용할 값을 전달하는 클래스이다.
org.springframework.web.servlet.RequestToViewNameTranslator

컨트롤러에서 뷰 이름이나 뷰 오브젝트를 제공해주지 않았을 경우 URL과 같은 요청정보를 참고해서 자동으로 뷰 이름을 생성해주는 전략 오브젝트이다. 디폴트는 DefaultRequestToViewNameTranslator이다.
 ```

 ![](https://cphinf.pstatic.net/mooc/20180219_26/1519004078279fGdRP_PNG/6.png?type=w760)
 ```
 org.springframework.web.servlet.handlerexceptionresolver

기본적으로 DispatcherServlet이 DefaultHandlerExceptionResolver를 등록한다.
HandlerExceptionResolver는 예외가 던져졌을 때 어떤 핸들러를 실행할 것인지에 대한 정보를 제공한다.
 ```
 ![](https://cphinf.pstatic.net/mooc/20180219_66/1519004113425TanBR_PNG/7.png?type=w760)
 
 ```
 org.springframework.web.servlet.ViewResolver

컨트롤러가 리턴한 뷰 이름을 참고해서 적절한 뷰 오브젝트를 찾아주는 로직을 가진 전략 오프젝트이다.
뷰의 종류에 따라 적절한 뷰 리졸버를 추가로 설정해줄 수 있다.
 ```
 ![](https://cphinf.pstatic.net/mooc/20180219_296/1519004150778ofOPV_PNG/8.png?type=w760)

 마지막으로 핸들러 익스큐션 체인이 존재한다면 인터셉터의 애프터 컴필리션 메소드를 실행한다. 그리고 리퀘스트 핸들 이벤트가 발생.

 요청이 처리됨.
 ## Spring MVC
스프링 MVC는 서블릿 API를 좀 더 추상화된 형태로 작성된 라이브러리이고 기존의 서블릿과 JSP를 이용할 때 필요한 기능들을 기본적으로 제공하고있다.

상식적으로 생각해봤을 때 프론트 컨트롤러가 모든 요청을 받고 동작의 흐름을 결정한다. 그리고 모든 동작의 공통적인 처리를 할 수 있다. 그렇다면 우리가 작성해야할 부분은 매번 다른 처리를 하는 부분이 될것이다. 이러한 부분을 @Controller를 통해서 처리한다.

스프링의 MVC 구조는 기존의 서블릿과 JSP를 이용할 때보다 편리하고 다양한 기능을 사용할 수 있지만 조금 귀찮고 배우기 힘든 부분이 존재한다. 바로 기본 설정 부분인데 이부분이 스프링 부트의 탄생에 한몫 거들지 않았나 생각한다.

기본적인 설정은 web.xml을 사용하는 방법과 WebApplicationInitializer를 구현해서 설정하는 방법이 있다. 대부분의 책에서 web.xml을 이용해서 설정하고 있으니 web.xml을 사용해서 등록하는 방법만 정리해보려 한다.
### web.xml작성

스프링 MVC를 사용하려면 프론트 컨트롤러 역할을 하는 DispatcherServlet을 설정해주어야한다. 이를 위해 webapp -> WEB-INF 폴더에 web.xml파일을 작성해야한다. 서블릿 3.0미만에서 사용하던 서블릿클래스를 web.xml파일에 등록하는 방식에서 조금 변형하면 된다.

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app>

	<display-name>Spring JavaConfig Sample</display-name>
	<context-param>
		<param-name>contextClass</param-name>
		<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext
		</param-value>
	</context-param>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>kr.or.connect.reservation.config.ApplicationConfig
		</param-value>
	</context-param>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener
		</listener-class>
	</listener>

	<servlet>
		<servlet-name>mvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet
		</servlet-class>
		<init-param>
			<param-name>contextClass</param-name>
			<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext
			</param-value>
		</init-param>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>kr.or.connect.reservation.config.WebMvcContextConfiguration
			</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>mvc</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter
		</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
</web-app>
```
전에 스프링 MVC를 사용해서 프로젝트를 할 때 설정했던 web.xml파일이다.

우선 중요한 부분은
```
<servlet-name>mvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet
		</servlet-class>
```
디스패쳐 서블릿의 이름과 그 실제 클래스를 패키지이름을 포함하여 명시해둔 부분이다.

서블릿 네임은 서블릿 매핑이 가지고있는 서블릿 네임과 일치하기만 하면 된다.


```
	<init-param>
			<param-name>contextClass</param-name>
			<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext
			</param-value>
		</init-param>
```
이 부분에선 디스패쳐 서블릿까지는 스프링이 제공하기 때문에 실제로 내가 어떤 일을 수행하고 싶은지 알 수 없다. 이 부분을 알려주는 게 init-param부분이다.
```
<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>kr.or.connect.reservation.config.WebMvcContextConfiguration
			</param-value>
		</init-param>
```
이 부분은 web.xml이 아니라 JavaConfig파일을 기반으로 디스패쳐 서블릿의 설정을 읽어오는 부분이다.

```
<servlet-mapping>
		<servlet-name>mvc</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
```
그리고 이 부분은 아까 설정했던 서블릿과 어떤 경로로 오는 요청을 처리할 것인지를 명시해주는 부분으로 디스패쳐 서블릿은 모든 요청을 처리함으로 경로가 / 로 설정되어있는 부분을 확인할 수 있다.

그리고 이제 자바 클래스를 작성해주면 되는데 좀전에 initparam에서 명시해둔 클래스 명으로 작성하면 된다. 이렇게 생성한 클래스는 디스패쳐 서블릿의 기본 설정 외에 추가적인 설정을 할 때 사용하면 되는데 기본적으로 ```WebMvcConfigurer```인터페이스를 구현해주면 된다.

기본적으로 설정클래스이니 ```@Configuration```을 명시해주고 스프링 MVC를 사용하기 위한 @EnableWebMvc를 명시해준다.
해당 어노테이션은 메세지 컨버터 등 웹에 필요한 빈들의 대부분을 자동으로 설정해준다.

그리고 어떤 패키지에있는 클래스들을 사용해야할지 알려줄 @ComponentScan애노테이션이 필요하다.

설정해주면 좋은 것들은

 따로 설정해주지 않으면 정적인 경로에 대한 요청을 리퀘스트 매핑에서 해당 url을 찾기 때문에 오류가 난다 따라서 특정 경로의 요청에 대해서 이러이러한 프로젝트 디렉토리에서 찾아라 하는 것을 알려줘야한다.
```java
@Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/assets/**").addResourceLocations("classpath:/META-INF/resources/webjars/").setCachePeriod(31556926);
        registry.addResourceHandler("/css/**").addResourceLocations("/css/").setCachePeriod(31556926);
        registry.addResourceHandler("/img/**").addResourceLocations("/img/").setCachePeriod(31556926);
        registry.addResourceHandler("/js/**").addResourceLocations("/js/").setCachePeriod(31556926);
    }
```
그리고 파라미터로 전달받은 해당 객체에서 enable()이라는 메소드를 호출해서 디폴트 서블릿 핸들러 컨피규어를 사용하도록 해주는 부분이다. 파라미터로 전달받은 DefaultServletHandlerConfigurer의 enable 메소드 호출은 default servlet handler를 사용한다: 매핑정보가 없는 url 요청은 스프링의 was의 default servlet에게 해당 일을 넘기게 된다.
```java
@Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
```

그리고 특정 경로에 대한 응답 정보를 @RequestMapping을 통해서 설정하지 않고 다음과 같이 설정할 수 있다. 루트경로에 대한 요청은 main이라는 이름의 뷰를 리턴하게된다.
```java
@Override
    public void addViewControllers(final ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("main");
    }
```
그리고 뷰의 이름만 가지고는 응답을 해줄 수 없다. 어떤 경로에 뷰가 들어있는지, 어떤 종류의 파일인지를 알려줘야하는다. 다음과 같이 설정해 줄 수 있다.

뷰는 /WEB-INF/views아래에서 찾고 파일 확장자 명은 .jsp 이다.

```java
 @Bean
    public InternalResourceViewResolver getInternalResourceViewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
```


이렇게 설정이 좀 복잡하지만 다행히도 한 번 설정하면 더 만질일이 없어서  우리가 구현하고 싶은 부분을 작성하면 된다.


### 컨트롤러 작성

서블릿을 이용한 MVC모델의 경우 Servlet을 상속받아서 doGet/doPost와 같은 제한적인 메소드를 오버라이드해서 사용했지만 스프링 MVC의 경우 하나의 컨트롤러를 이용해서 여러 경로의 호출을 모두 처리할 수 있다.(서블릿을 이용한 개발의 단점 보완)

원래는 ```@RequestMapping(value="/todo", method=RequestMethod.GET)```의 형식으로 경로와 메소드를 설정해줬지만 스프링 4버전 이후부턴 @GetMapping()과 같이 각각의 요청 메소드를 직접 적을 필요없이 Get으로 받을 것이다 하면@GetMapping post로 받을것이다 하면 @PostMapping으로 사용할 수 있다.(편리해진 점)

### 파라미터 자동수집

파라미터 자동수짐은 자바의 객체를 메소드의 파라미터로 설정하면 자동으로 전달되는 HttpServletRequest의 파라미터들을 수집해주는 기능으로 문자열, 숫자, 배열, 리스트, 첨부파일등도 가능하다. 

파라미터 수집은 다음과 같은 기준으로 동작하는데 기본 자료형의 경우는 자동으로 형변환 처리가 되고 그렇지 않은 DTO 객체같은 경우는 setXXX를 통해 처리된다.
객체가 우선 생성되고 -> 생성자 필요
setXXX메소드를 통해 처리된다.


따라서 객체 자료형의 경우 생성자가 없거나 파라미터가 없는 생성자가 필요하다.

### Model
스프링 MVC는 기본적으로 웹 MVC와 동일한 방식으로 모델이라고 부르는 데이터를 JSP까지 전달해야한다. 이는 서블릿을 이용한 MVC모델에서 request.setAttribute()로 jsp로 출력할 데이터를 전달해주는 것과 같다. 스프링에서는 이를 Model이라는 객체를 이용해서 처리하는 것이다.

모델에는 addAttribute라는 메소드를 이용해서 뷰에 전달할 이름과 값을 지정할 수 있다.

### 리턴 타입

스프링 MVC의 리턴타입으론 여러가지가 올 수 있다. 뷰이름을 스트링값으로 리턴할 수도 있고  ModelAndView에 Model과 view이름을 넣어서 리턴할 수 있고 ResponseEntity를 사용할 수도 있다.(Rest방식을 구한할 때 주로 사용됨) void는 컨트롤러의 @RequestMapping값과 @GetMapping 등의 메소드에 선언된 값을 그대로 뷰의 이름으로 사용하게 된다.

### 메소드의 파라미터

* @RequestParam - Request에 있는 특정한 이름의 데이터를 파라미터로 받아서 처리하는 경우에 사용
* @PathVariable - URL경로의 일부를 변수로 지정해 처리하기 위해 사용
기타 @CookieValue, @RequestBody, @Valid 등이 있다.



