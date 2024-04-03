# Full Text Search적용

## 개요

Full Text Index를 생성하는 것은 DB에서만 해주면 된다. 설정해주고 검색을 해봤을 때 확실하게 성능이 개선된 것을 볼 수 있었다. 

이제 애플리케이션으로 와서 적용을 시켜볼 차례다.

Full Text Search를 사용하는 문법은 match-contains문법이다.

이는 모든 DB에서 지원하는 문법은 아니고 InnoDB, Mysql등의 특정 DB에서만 사용하는 문법이라 별도의 설정이 필요하다.

## 설정법

구글에 검색해보면 CustomDialect를 정의하여 사용하는 것으로 나오는데 하이버네이트 6.0 버젼부턴 이렇게 할 수 없는 것으로 보인다.

검색해본 결과 functioncontributor를 이용하여 Mysql의 match 문법을 함수로 등록하여 사용해야된다.

우선 아무 패키지에나 FunctionContributer 구현체를 생성한다. 

```java
public class CustomFunctionContributor implements FunctionContributor {
```

그리고 contributeFunctions를 오버라이드한다.

그리고 내부에서 사용할 함수를 정의해준다.
```java
functionContributions.getFunctionRegistry()
            .registerPattern("match_against", "match(?1) against (?2 in boolean mode)", functionContributions.getTypeConfiguration().getBasicTypeRegistry().resolve(StandardBasicTypes.DOUBLE));
```

이렇게 등록한뒤 해당 패키지를 등록하는 과정이 필요하다.

방법은 `/src/main/resources/META-INF/services/`경로에 `org.hibernate.boot.model.FunctionContributor`파일을 생성하고 그 안에 내가 좀전에 정의한 함수가 있는 패키지를 등록해준다.

그리고 Querydsl에서 메소드를 만들어 사용하면 된다.


