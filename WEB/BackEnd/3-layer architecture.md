# 3-Layered Architecture

## 개요
웹개발을 하다보면 중복되는 부분이 있을 것이다.<br>
예를 들어 어떤 사이트에선 메인에서도 메일이 얼마나 쌓였는지 보여지고 다른 페이지로 이동해도 보여진다.<br>
이런 경우에 각 컨트롤러들은 중복된 로직을 가지게 될 것이다.<br><br>
중복되고 반복되는 코드는 개발자를 불안하게 한다.<br><br>

Q) 이런 중복되는 부분들을 처리하려면?<br><br>
A) 비즈니스 메소드를 별도의 Service객체에서 구현하고 컨트롤러는 Service객체를 사용한다.

![이미지](https://cphinf.pstatic.net/mooc/20180219_85/1519008848012uvMNx_PNG/1.png?type=w760)

서비스 객체라는 비즈니스 로직 : DAO와 컨트롤러와는 다른 비즈니스 로직들이 존재하는 계층을 서비스 계층이라고 부른다.
보통 하나의 비즈니스 로직은 하나의 트랜잭션으로 동작한다.<br>

중복되는 코드는 데이터 엑세스 메소드를 별도의 Repository(DAO)객체에서 구현하도록하고 Service는 Repository객체를 사용한다.

## 레이어드 아키텍처의 장점.
* 역할과 책임을 기준으로 분리되어 유지보수에 용이하고 중복되는 코드를 줄일 수 있다.
* 스프링에서는 설정파일을 프레젠테이션과 나머지를 분리할 수 있다.

지금은 웹이지만 앱이 될 수도 있다. 이럴 경우 프로그램을 새로 작성하는 게 아니라 프레젠테이션 계층과 그 나머지를 분리해서 서비스 할 수 있다.<br>

Spring 설정 파일을 프레젠테이션 레이어와 나머지를 분리시킬 수 있다.<br>
web.xml파일에서 프레젠테이션 레이어에 대한 스프링 설정은 DispatcherServlet이 읽도록하고 그 외의 설정은 ContextLoaderListener를 통해서 읽도록 한다.<br>
DispatcherServlet을 경우에 따라서 2개 이상 설정할 수 있는데 이 경우에는 각각의 DispathcerServlet의 ApplicationContext가 각각 독립적이기 때문에 각각의 설정 파일에서 생성한 빈을 서로 사용할 수 없다.<br>

위의 경우와 같이 동시에 필요한 빈은 ContextLoaderListener를 사용함으로써 공통으로 사용하게 할 수 있다<br>

ContextLoaderListener와 DispatcherServlet은 각각 ApplicationContext를 생성하는데, ContextLoaderListener가 생성하는 ApplicationContext가 root컨텍스트가 되고 DispatcherServlet이 생성한 인스턴스는 root컨텍스트를 부모로 하는 자식 컨텍스트가 된다.<br>
참고로, 자식 컨텍스트들은 root컨텍스트의 설정 빈을 사용할 수 있다.