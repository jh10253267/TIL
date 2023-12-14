# Spring

## 개요
 오늘날의 자바를 이용한 웹 개발은 서블릿과 JSP를 이용하지 않고 스프링 프레임워크를 이용하는 것이 기본이다.<br>

 앞서 살펴본 기술들이 어떻게 변화해왔는지 어떤 점에서 편의성을 주는지 살펴보려한다.<br>

 우리가 무언가를 만들 때 재료부터 하나하나 손질해가며 만들지 않는다. 이미 어느정도 완성되어있는 제품을 반제품이라고 한다.<br>

 이를 구매하고 내가 원하는 상품을 만들면 된다.<br>

 프로그래밍을 할 때도 마찬가지이다. 모든 걸 다 처음부터 만들지 않고 이미 잘 만들어진 반제품을 사용할 수 있다면 개발자의 부담이 줄어들 수 있다.<br>

 프레임워크가 바로 이런 역할이다. 그럼 우리는 프레임워크를 이용해서 우리가 만들고 싶은 제품을 만들어내면 된다.<br>

## 의존성 주입과 제어의 역전

### DI(Dependency Injection)
의존성 주입이란 잘 알려진 객체지향적 설계패턴 중 하나이다. 스프링이 최초로 의존성 주입의 아이디어를 낸 것이 아니다.<br>

스프링 이외에도 여가지 의존성 주입 프레임 워크가 있는데 스프링 프레임워크의 등장 당시 여러 종류의 비슷한 컨셉의 프레임워크가 등장했지만<br> 다른 프레임워크와는 다르게 개발과 설계 전반에 대한 문제들을 같이 다루었기 때문에 결론적으로 스프링이 가장 성공한 프레임워크로 남게 되었다. (PicoContainer, Guice등의 다양한 DI프레임워크가 존재한다.)<br><br>

 의존성 주입은 클래스 사이의 의존 관계를 빈(Bean) 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말한다.<br>

 여기서 빈 이라고 하는 것은  쉽게 말하면 예전에는 Visual 한 컴포넌트를 Bean이라고 불렀지만, 근래 들어서는 일반적인 Java클래스를 Bean클래스라고 객체를 가르키는 것이라고 이해할 수 있다.<br>

 의존성 주입은 어떻게 하면 객체와 객체 간의 관계를 더 유연하게 유지할 것인가 에대한 고민으로 객체의 생성과 관계를 효과적으로 분리시킬 수 있는 방법에 대한 고민이다.
  
 예를 들어 A라는 객체는 B를 이용해야한다. 이를 A객체가 B객체에 의존적이라고 표현한다.<br>

 즉 의존성이란 하나의 객체가 자신이 해야하는 일을 하기 위해서 다른 객체의 도움이 필요한 관계를 말한다.<br>

 의존한다는 의미는 B가 변하면 A에 영향을 미친다는 것이고 의존대상 B의 변화에 취약해지곤한다. 그래서 A가 직접적으로 B에 의존하지 않도록 중간에 인터페이스를 두어 A와 인터페이스 구현체와의 결합도가 느슨해지도록 설계할 수 있다.<br>

런타임시 의존관계를 맺는 대상을 외부에서 결정하고 주입해 주는 방법으로 관리할 수 있다.<br>

DI의 장점으로는 의존성 주입을 인터페이스 기반으로 설계하면 코드가 유연해지고 결합도가 낮은 객체끼리는 부품을 쉽게 갈아끼울 수 있다.<br>

 과거에는 의존성을 해결하기 위해 컨트롤러에서 직접 서비스 객체를 생성하거나 하나의 객체만을 생성해서 활용하는 등의 다양한 설계 패턴을 설계해서 적용해 왔는데<br>

 스프링 프레임워크에선 이런 점을 프레임 워크자체에서 지원하고 있다. 스프링 프레임워크는 다양한 방식으로 필요한 객체를 찾아서 이용할 수 있도록 XML이나 자바 설정을 이용할 수 있다.<br>

<b>DI가 적용되지 않은 예</b><br>

개발자가 직접 인스턴스를 생성하고 있다.

```java
 class 엔진 {

}

class 자동차 {
     엔진 v5 = new 엔진();
}
```

![이미지1](https://cphinf.pstatic.net/mooc/20181218_284/1545136782491NSgAa_JPEG/3.7.2-1.jpg?type=w760)


<b>DI가 적용된 예</b><br><br>
컨테이너가 변수에 인스턴스를 할당해주고 있다.<br>

```java
@Component
class 엔진 {

}

@Component
class 자동차 {
     @Autowired
     엔진 v5;
}

```


![이미지2](https://cphinf.pstatic.net/mooc/20181218_190/1545137156742y8WiS_JPEG/3.7.2-2.jpg?type=w760)



### IoC(Inversion of Control) 
컨테이너가 코드 대신 오브젝트의 제어권을 갖고 있어 IoC(제어의 역전)이라 한다.

예를 들어, 서블릿 클래스는 개발자가 만들지만, 그 서블릿의 메소드를 알맞게 호출하는 것은 WAS이다.

이렇게 개발자가 만든 어떤 클래스나 메소드를 다른 프로그램이 대신 실행해주는 것을 제어의 역전이라고 한다.

### ApplicationContext와 Bean
스프링에서는 빈이라고 부르는 객체들을 관리하기 위해서 ApplicationContext를 사용한다. 빈이 등록되면 해당 공간 안에서 객체를 생성하고 관리하기 시작한다.

자체적으로 객체를 생성하고 관리하면서 필요한 곳으로 객체를 주입해주는데 이 때 관련된 정보는 XML파일을 생성해서
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="userBean" class="kr.or.connect.diexam01.UserBean"></bean>

</beans>
```
 다음과 같은 형식으로 등록할 수 있다. 이번 기회에 한 번 스프링이 어떤 과정을 거쳐서 의존성을 주입해 줄 수 있는지 생각해보려한다.<br><br>
 우선 어떤 객체에 대해 스프링이 의존성 주입을 할 것인지 그 정보를 알아야한다. 이 부분이 빈을 등록하는 부분일 것이다.<br><br>
 그리고 객체 인스턴스를 만들려면 생성자가 필요하다. 그럼 스프링은 생성자를 인식해서 가져올 수 있어야할 것이다. 그리고 생성자에 어떤 파라미터들이 있어야하는 지 알아야한다.<br><br>
 그런다음 객체 인스턴스를 생성할 수 있을것이다. <br><br>
 그래서 빈이 반드시 지켜야하는 부분이 있다고 하면 다음과 같을것이다.<br>
 * 내부 필드는 private로 선언되어야한다.(캡슐화, 보안)
 * getter, setter메소드를 가진다(캡슐화를 유지하면서 내부에 접근할 수 있게하기 위함
 * 기본 생성자를 가진다.(내부적으로 Reflection을 사용하기 때문)
 <br>

 [di 관련 실습 레포지토리](https://github.com/jh10253267/di-practice)

 <br><br>

```java
public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext( 
				"classpath:applicationContext.xml"); 
```
위와 같이 작성한 뒤 ```ac```인스턴스의 ```getBean()```메소드를 통해 원하는 인스턴스를 가져올 수 있다.<br>
<br>

만약 다음과 같은  클래스들이 있다고 하자

자동차 클래스는 엔진 객체 외부로 부터 받아 엔진을 설정해 달린다.

```java
public class Engine {
	public Engine() {
		System.out.println("Engine 생성자");
	}
	
	public void exec() {
		System.out.println("엔진이 동작합니다.");
	}
}
```

```java
public class Car {
	Engine v8;
	
	public Car() {
		System.out.println("Car 생성자");
	}
	
	public void setEngine(Engine e) {
		this.v8 = e;
	}
	
	public void run() {
		System.out.println("엔진을 이용하여 달립니다.");
		v8.exec();
	}
}
```
위의 클래스가 동작하려면 다음과 같이 코드를 작성해야한다.
```java
Engine e = new Engine();
Car c = new Car();
c.setEngine( e );
c.run();
```
이를 xml파일로 설정한다면 다음과 같다
```java
<bean id="e" class="kr.or.connect.diexam01.Engine"></bean>
<bean id="car" class="kr.or.connect.diexam01.Car">
	<property name="engine" ref="e"></property>
</bean>
```
위의 설정은 다음과 같은 의미를 가진다
```java
Engine e = new Engine();
Car c = new Car();
c.setEngine(e);
```
이렇게 설정을 하고 코드를 실행시켜보면
```java
public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext( 
				"classpath:applicationContext.xml"); 

		Car car = (Car)ac.getBean("car");
		car.run();
		
	}
```
다음과 같은 결과가 나온다.
```java
Engin 생성자
Car 생성자
엔진을 이용하며 달립니다.
엔진이 동작합니다.
```
### Java Config를 이용한 설정
위에서 알아본 설정 방법은 스프링의 설정 방법중 XML을 이용한 설정이였고 이제 Java Config를 이용한 설정을 알아보려한다.

우선 알기 쉽게 위에서 살펴본 코드를 Java Config방식으로 바꾸면 다음과 같다.

```java
@Configuration
public class ApplicationConfig {
	@Bean
	public Car car(Engine e) {
		Car c = new Car();
		c.setEngine(e);
		return c;
	}
	
	@Bean
	public Engine engine() {
		return new Engine();
	}
}
```
```@Configuration```은 스프링 설정 클래스라는 의미를 가진다.
이를 스프링이 인식한다. 그리고 이전과 다르게 ```ApplicationContext```중에 
```AnnotationConfigApplicationContext```라는 
```ApplicationContext```중에서 ```AnnotationConfigApplicationContext```는 JavaConfig클래스를 읽어들여 IoC와 DI를 적용하게 된다. 다음과 같이 사용할 수 있다.
```java
ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

Car car = (Car)ac.getBean("car");
		car.run();
```

이때 설정파일 중에 @Bean이 붙어 있는 메소드들을 AnnotationConfigApplicationContext는 자동으로 실행하여 그 결과로 리턴하는 객체들을 기본적으로 싱글턴으로 관리를 하게 된다.<br>

이번에는 조금 다른 방식으로 사용해보자면<br>

```java
@Configuration
@ComponentScan("kr.or.connect.diexam01")
public class ApplicationConfig2 {
}
```
기존 JavaConfig에서 빈을 생성하는 메소드를 모두 제거한 뒤

```@Configuration```아래에 ```@ComponentScan```이라는 어노테이션을 추가했다.

```@ComponentScan```어노테이션은 파라미터로 들어온 패키지 이하에서 ```@Controller, @Service, @Repository, @Component``` 어노테이션이 붙어 있는 클래스를 찾아 메모리에 몽땅 올려주게 된다.<br>

기존의 클래스들을 수정해보자면 다음과 같다.
```java
@Component
public class Engine {
	public Engine() {
		System.out.println("Engine 생성자");
	}
	
	public void exec() {
		System.out.println("엔진이 동작합니다.");
	}
}
```
```java
@Component
public class Car {
	@Autowired
	private Engine v8;
	
	public Car() {
		System.out.println("Car 생성자");
	}
	
	public void run() {
		System.out.println("엔진을 이용하여 달립니다.");
		v8.exec();
	}
}
```
이렇게 코드를 수정한 뒤 실행을 시켜보면<br>

```java
public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig2.class);
		   
		Car car = ac.getBean(Car.class);
		car.run();
		
	}
```

```java
Engin 생성자
Car 생성자
엔진을 이용하며 달립니다.
엔진이 동작합니다.
```
이전과 같은 결과가 나오는 것을 확인할 수 있다.<br>

잠깐 생각을 해보자면 스프링 설정 방법은 XML을 이용한 설정과 Java Config를 이용한 설정이 있었다. 각각의 장단점을 알아보자

<b>XML을 이용한 설정</b>
* 장점
  * <b>자바 코드와 설정을 분리할 수 있다</b>
  * <b>유지 보수에 이점이 있다</b> : 만약 클래스를 변경할 필요가 있을 때
  애노테이션을 이용한다면 클래스들을 직접 열어봐서 애노테이션이 붙어있는지 확인해야 할 수 있다.
  * <b>레거시 시스템과의 호환성</b> : 오래된 시스템에서는 XML을 주로 사용하기 때문에, 기존 레거시 시스템과의 통합에 용이하다.

* 단점

  * <b>오타와 에러 처리</b> : XML은 오타나 잘못된 구문 등의 오류를 찾기 어려울 수 있다. 실제 빈이나 프로퍼티의 이름을 변경할 때, 이를 XML 설정에서 변경하는 작업은 번거로울 수 있다.
  * <b>타입 안정성 부족</b> : XML은 컴파일 타임에 타입 체크를 할 수 없으므로, 설정에 관한 오류는 런타임에 발견될 수 있다.
  * <b>번거로운 작성</b> : 위에서 본 예제들의 경우 xml파일을 간단히 작성할 수 있었지만 대규모 애플리케이션의 경우 XML 파일이 길어지고 복잡해질 수 있으며, 이로 인해 유지보수가 어려울 수 있다.

<b>Java Config를 이용한 설정</b>

 * 장점
    * <b>타입 안정성</b> : 자바 코드를 이용하여 설정하기 때문에 컴파일 시점에 타입 체크가 가능하다.
    * <b>IDE 지원</b> : 자바 코드로 작성되기 때문에 IDE에서 코드 자동완성, 리팩토링 등의 지원을 받을 수 있다.
    * <b>프로그래밍적 유연성</b> : 자바 코드를 사용하기 때문에 프로그래밍적인 작업을 통해 유연하게 설정할 수 있다.

 * 단점
    * <b>가독성</b> : 코드가 길어지고 설정이 복잡해질 경우에는 가독성이 떨어질 수 있다.
    * <b>재배포의 어려움</b> : 설정 변경 시 코드를 다시 컴파일해야 하므로 재배포가 번거로울 수 있다.
    * <b>초기 학습 곡선</b> : XML에 비해 자바 설정 방식을 습득하는 데 시간이 필요할 수 있다.
