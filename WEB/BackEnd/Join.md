# Join

## 개요

데이터베이스는 영속계층이라고 하기도 하며 영구적으로 데이터를 저장하기 위함이다. 

흔히 사용하는 RDB는 엑셀 형태로 자료를 저장한다. 행이 있고 열이 있고 그 교차점에 데이터를 기록한다. 그렇다면 단순하게 모든 데이터를 한 곳에 보관하면 될까?

## 정규화

데이터를 다룰 때 원하지 않는 작업이 일어나는 것을 이상이라고 한다.

종류로는 삽입이상, 삭제이상 등이 있는데 이를 해결하기 위한 방법이 정규화이다. 쉽게 말하면 관련이 있는 데이터들끼리 묶어서 하나의 테이블을 구성하는 것인데 하나의 테이블에 모든 데이터를 담지 않기 때문에 RDB에서는 PK값(테이블 내에 유일하게 식별이 가능한 값)을 갖는 테이블을 참조하는 구조로 만든다.

이 때 두가지 테이블을 연관지어 데이터를 추출하고 싶을 때 사용하는 것이 바로 Join이다. 여러개의 테이블을 연관지어 추출하고 싶다면 Join을 연쇄적으로 사용하면 된다.

## 

Join은 집합으로 생각하면 쉽다.

책의 정보를 담은 데이터베이스가 있다고 하자.

이 안에는 책의 고유번호와 책의 이름이 있는 도서 테이블과 책의 고유번호와 가격이 있는 도서 가격 테이블이 있다. 이를 한 번의 쿼리로 추출하고 싶다.

-> 두 테이블의 교집합(외래키)을 찾고 두 개의 테이블을 연결한다.

```sql
SELECT 테이블1.책번호, 테이블1.책이름, 테이블2.가격 FROM 테이블1 JOIN 테이블2 ON 테이블1.책번호 = 테이블2.책번호;
```
조인에도 여러종류가 있는데 위의 방식을 내부 조인이라고 한다.

내부 조인방식은 디폴트로 따로 INNER 키워드를 입력하지 않아도 내부 조인으로 실행된다.

외부조인도 있는데 RIGHT와 LEFT 조인이 있다.

RIGHT 외부조인은 오른쪽 테이블의 모든 데이터와 왼쪽 테이블의 동일 데이터를 추출하는 방식이다.
```sql
SELECT 도서.책번호, 도서.책이름, 도서가격.책번호, 도서가격.가격 FROM 도서
RIGHT JOIN 도서가격 ON 도서.책번호 = 도서가격.책번호;
```
LEFT 외부조인은 왼쪽 데이터의 모든 데이터와 오른쪽 테이블의 동일 데이터를 추출하는 기법이다.
```sql
SELECT 도서.책번호, 도서.책이름, 도서가격.책번호, 도서가격.가격 FROM 도서
LEFT JOIN 도서가격 ON 도서.책번호 = 도서가격.책번호;
```