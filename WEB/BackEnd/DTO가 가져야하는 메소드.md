# DTO가 가져야하는 메소드

## 개요

프로젝트를 진행하다 DTO에 필수적인 메소드를 선언해주지않아 오류가 생기는 경우를 주변에서 많이 봤다.

이미 알고있었던 정보지만 경험적으로 아는 것에 불과했다.

당분간 자료를 모으고 코드도 뜯어보면서 공부해보려한다.

## 검색결과

여러가지 키워드들이 나온다. 

`Reflextion` `Getter/Setter` `NoArgsConstructor`

## 거꾸로 접근하기

Clues

* DTO에 Getter/Setter와 생성자가 없으면 에러가난다.
* Getter와 Setter 둘 중 하나만 있어도 작동한다.


## 실험

기본 스프링부트 프로젝트를 생성한 뒤 실험해본다.

객체 데이터를 포함할 때랑 단순 자료형을 포함할 때랑 조건이 달라진다.



### 직렬화(Java Bean -> Json)


```java
com.fasterxml.jackson.databind.exc.InvalidDefinitionException: No serializer found for class com.example.demo.controller.TestModel and no properties discovered to create BeanSerializer (to avoid exception, disable SerializationFeature.FAIL_ON_EMPTY_BEANS) (through reference chain: java.util.HashMap["sample1"])
```

@Getter + @NoArgsConstructor

성공

@Setter + @NoArgsConstructor

성공

@AllArgsConstructor

성공
+ Getter 성공
+ Setter 실패

@Setter + @Getter
성공


NoArgs + setter+ getter 성공


직렬화 즉, ResponseBody를 생성할 때 필요한 것은 


### 역직렬화(Json->Java Object)

입력값은 아무 예외를 발생시키지 않았다.

예를 들어 private한 필드만 있어도 RequestBody로 받은 객체의 필드가 null일 뿐 에러는 나지 않았다.

기본생성자만 있는 경우 객체의 필드값은 null이다.

Getter를 선언해주었을 때 필드값이 제대로 들어간다.

Setter를 선언해 주었을 때 필드값이 제대로 들어간다.

역직열화에선 Getter와 Setter 하나만 있어도 Json -> Java 객체 매핑이 가능하다.






