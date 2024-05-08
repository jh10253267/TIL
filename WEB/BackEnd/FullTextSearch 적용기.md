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

```java
...
.where(contains(club.title, pageRequestDTO.getKeyword()));
...
private BooleanExpression contains(StringPath target, String searchWord) {
        if(searchWord == null) {
            return null;
        } else if (searchWord.isBlank()) {
            return null;
        }
        final String formattedSearchWord = "\"" + "+" + searchWord + "\"";
        return numberTemplate(Double.class, "function('match_against', {0}, {1})",
                target, formattedSearchWord)
                .gt(0);
    }
```

```java
public class CustomFunctionContributor implements FunctionContributor {
    private static final String FUNCTION_NAME = "match_against";
    private static final String FUNCTION_PATTERN = "match (?1) against (?2 in boolean mode)";

    @Override
    public void contributeFunctions(final FunctionContributions functionContributions) {
        functionContributions.getFunctionRegistry()
                .registerPattern(FUNCTION_NAME, FUNCTION_PATTERN,
                        functionContributions.getTypeConfiguration().getBasicTypeRegistry().resolve(StandardBasicTypes.DOUBLE));
    }
}
```

이렇게 작성해줬다.

## RDS 설정

만약 AWS의 RDS를 사용하고 있다면 RDS에서 파라미터를 수정해줘야한다.

기본적으로 최소 검색 단위는 4글자, 최소 토큰 크기는 3글자로 설정되어있는 것을 변경해주어야한다.

만약 도커를 이용해서 별도의 DB를 띄운다면 설정파일에 들어가서 설정을 해주어야하고 만약 DB가 RDS라면 파라미터 그룹을 수정해주어야한다.

RDS의 왼쪽 메뉴 하단을 보면 파라미터 그룹이라는 메뉴가 있다.

파라미터 그룹 생성을 눌러준다. 그리고 프로젝트의 RDB 종류를 선택해주고 생성해준다.

그리고 이렇게 생성된 파라미터 그룹이 DB에 제대로 적용되고 있는지 확인하려면 데이터베이스 탭에 들어가서 구성 탭을 누르고 하단을 살펴보면 `DB 인스턴스 파라미터 그룹`에 내가 등록한 그룹이 나오는지 확인하면 된다.

이렇게 생성된 파라미터 그룹에 들어가서 변경을 원하는 파라미터를 검색한다.

`ft_min_word_len`와 `innodb_ft_min_token_size`를 각각 원하는 값으로 설정해준다.

이렇게 변경을 해주고 RDS를 재부팅 시켜준다.

파라미터 그룹을 보면 파라미터 옆에 Dynamic, Static과 같은 문구가 있는데 이는 RDS를 재부팅을 하지 않아도 적용되는 재부팅해야 적용되는 파라미터인 것으로 보인다.

이렇게 변경을 해주고 콘솔에서 확인해준다.
```sql
show variables like '%ft_min%';
```

