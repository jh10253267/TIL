# Spring

## 자바의 겨울.

프로그램이나 라이브러리의 이름은 의미를 담은 경우가 많다. 히카리CP, 이클립스 등등. 히카리는 빛처럼 빠르고 가벼운 라이브러리라는 의미이고 이클립스는 태양을 삼킨다는 의미를 가지고 있다.
스프링도 마찬가지이다. 봄. 왜 이름을 이렇게 지었을까?
봄이 오려면 겨울 지나야한다. 스프링이 나오기 전 자바를 이용한 웹 개발 환경은 겨울이었다.
웹을 구현하려면 이 것 저 것 고민해봐야 할 것들이 많다.

* 동시에 여러 요청이 올 때 어떻게 처리해야할지
* 서버에 문제가 생긴다면 어떻게해야할지
* 어떤 방식으로 데이터전송을 최적화할지
* 분산환경이나 분산처리같은 문제들은?

서비스를 개발할 때마다 매번 이러한 고민을 해야한다면 개발비용과 시간이 많이 소요된다. 자바에선 이러한 처리를 자바 ee라는 기술 스펙으로 정리해두었고 서블릿과 jsp가 Java EE의 핵심 기술 스펙중 하나이다.<br><br>
방대한 기술중에 기본적인 기술은 서블릿과 jsp이다.
서블릿기술은 서버에서 동적으로 요청과 응답을 처리할 수 있는 api를 정의한 것.개발자들은 서블릿에서 제공하는 api를 이용해 코드를 작성하고 설정하는 식으로 서블릿 프로그램을 작성하게 된다.<br><br>
스프링이 등장하기 전까지 자바의 웹 개발은 Java EE 엔터프라이즈 에디션의 스펙을 구현한 EJB(Enterprise JavaBeans)를 사용했었다. <br>
여기서 EJB는 분산 객체를 대규모로 처리하기 위한 기술이다. 이는 기업 환경의 시스템을 구현하기위한 서버의 객체 처리 모델이다. 이러한 객체들을 관리하는 컨테이너 기술이 EJB컨테이너이다. 규모가 큰 프로젝트를 한다면 여러 객체들을 만들고 이러한 객체들을 컨테이너가 관리하고 객체를 주입해준다. 자바 진영에선 이 기술을 기업용 애플리케이션 개발에 사용하기를 권장했다. <br><br>
그러나EJB는 이론적으론 굉장히 뛰어난 기술이었지만 비쌌고 무거웠고 EJB의 기술들을 사용하기 위해 EJB에 종속적인 프로그래밍을 할 수밖에 없었다. 이로 인해 개발자들은 비즈니스 로직보다 EJB 설정을 위해 더 많은 시간을 할애할 수 밖에 없었다.

## 봄이 오다

 마틴 파울러는 EJB에 반발해 POJO라는 말을 남겼다. Plain Old Java Object의 약자다.
스프링은 로드 존슨이 EJB의 문제점을 지적하며 EJB가 없어도 고품질의 확장 가능한 애플리케이션을 개발할 수 있음을 보여주고
2002년 출간된 자신의 저서 "Expert One-on-One J2EE Design and Development"에 30,000 라인 이상의 코드를 보였고 이 코드에는 스프링의 핵심 개념인 BeanFactory, ApplicationContext, DI, IoC, POJO등이 포함되어 있었고 이게 스프링의 메인 컨셉이 되었다. <br><br>
스프링 프레임워크는 원래 웹이라는 제한적인 용도로만 쓰이는 것이 아니라 객체지향의 의존성주입 기법을 적용할 수 있는 객체지향 프레임워크였다.
스프링 프레임워크의 등장 당시 여러 종류의 비슷한 컨셉의 프레임워크가 등장했지만 다른 프레임워크와는 다르게 개발과 설계 전반에 대한 문제들을 같이 다루었기 때문에 결론적으로 스프링이 가장 성공한 프레임워크로 남게 되었다. (PicoContainer, Guice등의 다양한 DI프레임워크가 존재한다.)<br><br>

스프링 프레임워크는 가장 중요한 역할인 코어와 여러개의 추가적인 라이브러리를 결합하는 형태로 프로젝트를 구성하는데 Web MVC나 Spring-JDBC등이 많이 쓰인다.<br><Br>
![이미지](https://cphinf.pstatic.net/mooc/20180201_180/1517452205302mNfIy_PNG/2_10_1___.png?type=w760)

의존성 주입은 객체와 객체간의 관계를 어떻게 더 유연하게 유지할 수 있을까? 에 대한 해답이 되는 패턴이다.<br><br>
만약 A라는 클래스에서 B라는 클래스 객체를 이용해아 한다면 이를 A클래스는 B클래스에 의존적이라고 표현한다. 즉, 하나의 객체가 작동하기 위해서 다른 객체의 도움이 필요하다는 것이다.<br>

### IoC(Inversion of Control) 
컨테이너가 코드 대신 오브젝트의 제어권을 갖고 있어 IoC(제어의 역전)이라 한다.

예를 들어, 서블릿 클래스는 개발자가 만들지만, 그 서블릿의 메소드를 알맞게 호출하는 것은 WAS이다.

이렇게 개발자가 만든 어떤 클래스나 메소드를 다른 프로그램이 대신 실행해주는 것을 제어의 역전이라고 한다.


### DI(Dependency Injection)

DI는 의존성 주입이란 뜻을 가지고 있으며, 클래스 사이의 의존 관계를 빈(Bean) 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말한다.

DI가 적용되지 않은 예<br>
개발자가 직접 인스턴스를 생성하고 있다.


```java
 class 엔진 {

}

class 자동차 {
     엔진 v5 = new 엔진();
}
```
<br>

![이미지](https://cphinf.pstatic.net/mooc/20181218_284/1545136782491NSgAa_JPEG/3.7.2-1.jpg?type=w760)


DI가 적용된 예<br>
컨테이너가 변수에 인스턴스를 할당해주고 있다.<br>
```
@Component
class 엔진 {

}

@Component
class 자동차 {
     @Autowired
     엔진 v5;
}

```

<br>

![이미지](https://cphinf.pstatic.net/mooc/20181218_190/1545137156742y8WiS_JPEG/3.7.2-2.jpg?type=w760)
 

### ApplicationContext와 Bean
스프링에서는 빈이라고 부르는 객체들을 관리하기 위해서 ApplicationContext를 사용한다. 빈이 등록되면 해당 공간 안에서 객체를 생성하고 관리하기 시작한다.

빈이란 스프링 컨테이너가 관리하는 자바의 클래스 객체이다.

### Spring에서 제공하는 IoC/DI 컨테이너

* BeanFactory : IoC/DI에 대한 기본 기능을 가지고 있다.
* ApplicationContext : BeanFactory의 모든 기능을 포함하며, 일반적으로 BeanFactory보다 추천된다. 트랜잭션처리, AOP등에 대한 처리를 할 수 있다. 
* BeanPostProcessor, BeanFactoryPostProcessor등을 자동으로 등록하고, 국제화 처리, 어플리케이션 이벤트 등을 처리할 수 있다.
* BeanPostProcessor : 컨테이너의 기본로직을 오버라이딩하여 인스턴스화 와 의존성 처리 로직 등을 개발자가 원하는 대로 구현 할 수 있도록 한다.
* BeanFactoryPostProcessor : 설정된 메타 데이터를 커스터마이징 할 수 있다.
