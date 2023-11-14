# Spring Security

## 개요
사용자의 로그인과 세션 트래킹은 웹 애플리케이션에서 필수적인 기능이다.<br>
과거에는 HttpSession과 Cookie를 이용해서 처리했지만 스프링을 적용할 때는 Spring Security와 약간의 설정을 적용하는 것 만으로도 구현이 가능하다.<br>

스프링 시큐리티는 원래는 별도의 프레임워크로 시작되었다고 한다. 그 후 스프링으로 프로젝트가 통합되었고 이를 사용하면 개발자는 약간의 코드와 설정만으로 로그인 처리와 자동 로그인, 로그인 후에 페이지 이동등을 처리할 수 있기 때문에 개발의 생산성을 높일 수 있다.<br>

## 사용
스프링 시큐리티는 단순히 applicatioin.properties를 이용하는 설정보다 코드를 이용해서 설정을 조정하는 경우가 더 많다
Config파일을 하나 작성해서 사용하면 된다.
CustomSecurityConfig클래스를 추가해서 사용한다.
처음 적용을 하면 내 프로젝트의 모든 경로에 대해서 로그인을 필요로 한다. 로그인이 필요한 경로와 그렇지 않은 경우가 있기에 따로 셋팅을 해주는 게 좋다.
해당 Config파일에 SecurityFilterChain이라는 객체를 반환하는 메소드를 작성한다.

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) {
 return http.build(); 
}
```
해당 메소드가 동작하면 이전과 달리 접속이 가능하다.

스프링 시큐리티의 동작은 웹에서 사용하는 필터를 통해서 동작하고 많은 수의 필터들이 단계별로 동작한다. 따라서 문제가 발생하면 어떤 필터에서 문제가 생겼는지 알 수 있도록 로그 설정을 최대한 낮게 설정해서 관련된 에러를 볼 수 있게 설정하는 것이 좋다.

정적자원의 처리
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

 참고로 스프링 시큐리티에서 아이디는 username이라는 명칭을 사용한다.
 인증처리는 인증 제공자 라는 존재를 이용해서 처리되는데 인증 제공자와 그 이하의 흐름은 일반적으로 커스터마이징해서 사용하는 경우는 거의 없다.

 인증 처리를 위한 UserDetailService
 가장 중요한 객체는 실제로 인증을 처리하는 UserDetailService라는 인터페이스의 구현체이다.
 해당 인터페이스는 loadUserByUsername이라는 단 하나의 메소드를 가지는데 이것이 실제 인증을 처리할 때 호출되는 부분이다.
 개발자가 구현해야하는 부분은 해당 인터페이스를 구현해서 username이라는 사용자의 아이디 인증을 코드로 구현하는 것이다.

 loadUserByUsername()의 반환 타입은 UserDetails라는 인터페이스 타입으로 지정되어있다. 이는 사용자인증과 관련된 정보들을 저장하는 역할을 한다.
 스프링 시큐리티는 내부적으로 UserDetails타입의 객체를 이용해서 패스워드를 검사하고 사용자 권한을 확인하는 방식으로 동작한다.

 스프링 시큐리티의 api에는 userDetails라는 인터페이스를 구현한 User라는 클래스를 제공하고 빌더방식을 지원하므로 앞에서 사용한 loadUserByUsername()에 약간의 코드를 추가해줄 수 있다.

 * PasswordEncoder
 스프링 시큐리티는 기본적으로 PasswordEncoder를 필요로 한다. 이는 인터페이스로 제공되는데 이를 구현하거나 스프링 시큐리티 api에서 제공하는 클래스를 지정할 수 있다. 해당 타입의 클래스중 BCryptPasswordEncoder는 해시 알고리즘으로 암호화 처리되는데 같은 문자열이라도 매번 해시 처리된 결과가 다르다는 특징이 있다.
 
 로그인 기능이 제대로 동작하려면 Config파일에 PasswordEncoder를 Bean으로 등록하고 이를 주입해줘야한다. 

권한 체크는 특정한 경로에 지정하는 경우가 일반적인데 예를 들어 게시글 목록은 모두가 볼 수 있어야하지만 게시글 작성은 로그인이 되어야한다. 이런 권한 설정은 코드로 작성할 수도 있고 어노테이션을 이용해서 지정할 수도 있다. 어노테이션을 통해 지정하는 방식은 설정 관련 클래스에
@EnableGlobalMethodSecurity 를 추가해줘야 한다.
그리고 @PostAuthrize어노테이션을 통해 사전, 사후 권한을 체크할 수 있다. 여기서 자주 사용하는 표현식으로는
* authenticated()
* permitAll()
* anonymous
* hasRole
* hasAnyRole<br>
이 있다.
