# Lombok

## 개요
스프링 개발에서 필수적으로 사용되는 롬복 라이브러리에 대해 알아보자

## 주의점
스프링이니시얼라이저에서 추가하면 테스트 환경에서 롬복을 사용할 수 없다. 테스트 환경에서 롬복을 사용하고 싶다면  직접 project lombok사이트에서 테스트 의존성을 복사해와서 사용해야한다.

## Getter/Setter
인텔리제이나 이클립스 어떤 것을 써도 IDE에서 자체적으로 쉽게 게터와 세터를 오버라이드 할 수 있다.

그러나 단순한 기능들만 있는 메소드들이 클래스에 선언되게 되고 그로인해 가독성이 나빠진다.

롬복의 @Getter @Setter어노테이션을 붙여주기만 해도 Getter와 Setter를 사용할 수 있다.

## Data
주로 DTO클래스에 붙여서 사용하는 경우가 많다. 일반적인 DTO를 사용할 때 필수적인 기능들이 포함되어있는데 이런 이유로 종합 선물세트로 비유하는 경우가 많다.

포함되어있는 기능은 다음과 같다.

* Getter
* Setter
* ToString
* EqualsAndHashCode
* RequiredArgsConstructor

## RequiredArgsConstructor
필수적으로 필요한 파라미터들을 포함한 생성자를 만들어준다.

주로 의존성 주입을 할 때 사용한다. 예전에는 @Autowired어노테이션을 붙여주어 필드주입 방식으로 의존성을 주입해줬지만 요즘에는 생성자 주입 방식을 많이 사용한다.

의존성을 주입해줄 클래스를 선언하고 @RequiredArgsConstructor를 붙여주면 간단히 사용할 수 있다.

## AllArgsConstructor
모든 필드를 담은 생성자를 만들어준다.

## NoArgsConstructor
파라미터가 없는 기본 생성자를 만들어준다.

## Builder
어노테이션만 붙여서 빌더 패턴을 간단히 사용할 수 있다.