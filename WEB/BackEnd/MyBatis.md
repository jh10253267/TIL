# MyBatis

## 개요
스프링 프레임워크의 장점 중 하나는 다른 프레임워크들을 쉽게 결합해서 사용할 수 있다는 점이다. 이는 스프링 프레임워크가 웹이나 데이터베이스와 같이 특정한 영역을 구애받지 않고 시스템의 객체지향 구조를 만드는 데 이용된다는 성격 때문이다.

스프링에서 데이터베이스를 이용할 때 Spring JDBC를 사용할 수도 있고 MyBatis를 이용하는 방식도 가능하다.

MyBatis는 SQL Mapping Freamework라고 표현하는데 이는 sql의 실행 결과를 객체지향으로 매핑해준다는 의미이다.

## 메리츠

기존에 하나하나 처리해야했던 ResultSet의 getXXX()를 마이 바티스가 알아서 처리해준다.

자동으로 사용한 객체를 close해준다.

SQL문을 분리해서 사용할 수 있다.
스프링 프레임워크는 마이 바티스와 쉽게 연동할 수 있는 라이브러리와 API들을 제공한다.

## 사전 준비
* spring-jdbc
* spring=tx
* mybatis-spring

마이 바티스를 사용하기 위해서 스프링에 설정해둔 히카리cp를 이용해서 SqlSessionFactory라는 빈을 설정한다.

마이바티스는 SQL파일을 별도로 처리할 수 있지만 인터페이스와 어노테이션만으로도 처리가 가능하다.
```java
@Select("select now()")
String getTime();
```

이렇게 작성한 인터페이스를 매퍼 인터페이스라고 하는데 이 부분을 xml파일에 등록해주어야한다.


이러한 개발방식은 개발자가 실제 동작하는 클래스와 객체를 생성하지 않고, 스프링에서 자동으로 생성하는 방식을 이용하게 된다.
스프링에서 자동으로 생성된 객체를 이용하기 때문에 개발자가 직접 코드를 수정할 수 없지만 간단하게 사용 가능하다는 장점이 있다.

