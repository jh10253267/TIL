# Join

## 개요

데이터베이스는 영속계층이라고 하기도 하며 영구적으로 데이터를 저장하기 위함이다. 

흔히 사용하는 RDB는 엑셀 형태로 자료를 저장한다. 행이 있고 열이 있고 그 교차점에 데이터를 기록한다. 그렇다면 단순하게 모든 데이터를 한 곳에 보관하면 될까?

## 정규화

데이터를 다룰 때 원하지 않는 작업이 일어나는 것을 이상이라고 한다.

종류로는 삽입이상, 삭제이상 등이 있는데 이를 해결하기 위한 방법이 정규화이다. 데이터를 넣어나 삭제할 때 관련이 있는 정보만 삽입하고싶고 삭제하고 싶은데 같은 튜플로 묶인 정보도 같이 삭제되는 현상을 삭제 이상이라고 부른다. 예를 들어 학생 정보와 담당 선생님이 한 튜플에 존재하는 상황이라고 해보자. 이 상황에서 학생의 담당 선생님이 아직 정해지지 않은 경우애는 학생 정보를 
삽입할 수 없다. 이를 삽입 이상이라고 부른다.

 또한 선생님이 다른 학교로 전출가게 된다면 담당 선생님 데이터만 삭제하고 싶어도 학생 데이터까지 삭제가 되어버린다.

 이러한 이유 때문에 관련이 있는 데이터들끼리 묶어서 하나의 테이블을 구성하는 것인데 하나의 테이블에 모든 데이터를 담지 않기 때문에 RDB에서는 PK값(테이블 내에 유일하게 식별이 가능한 값)을 갖는 테이블을 참조하는 구조로 만든다. 이러한 구조를 FK(외래키)로 나타낸다.

이 때 두가지 테이블을 연관지어 데이터를 추출하고 싶을 때 사용하는 것이 바로 Join이다. 여러개의 테이블을 연관지어 추출하고 싶다면 Join을 연쇄적으로 사용하면 된다.

## 필요성

만약 인터넷 쇼핑몰 서비스를 설계한다고 해보자.

회원이 있을 것이고 회원이 구매한 상품이 있을 것이다.

물건을 배송하기 위해서는 회원 테이블의 회원 이름과 연락처 그리고 구매 테이블의 해당 회원이 구매한 상품정보가 필요할 것이다. 그리고 이 두개의 테이블을 합친 테이블을 주문 테이블로 칭할 수 있을 것이다.

이렇게 두 테이블을 합쳐 하나의 테이블로 만드는 경우가 잦은데 이를 위한 문법이 바로 조인이다. 

## 설명

Join은 집합으로 생각하면 쉽다.

책의 정보를 담은 데이터베이스가 있다고 하자.

이 안에는 책의 고유번호와 책의 이름이 있는 도서 테이블과 책의 고유번호와 가격이 있는 도서 가격 테이블이 있다. 이를 한 번의 쿼리로 추출하고 싶다.

-> 두 테이블의 교집합(기본키, 외래키)을 찾고 두 개의 테이블을 연결한다.

```sql
SELECT 테이블1.책번호, 테이블1.책이름, 테이블2.가격 FROM 테이블1 JOIN 테이블2 ON 테이블1.책번호 = 테이블2.책번호;
```

조인에도 여러종류가 있는데 위의 방식을 내부 조인이라고 한다.

내부 조인방식은 가장 많이 사용하는 방식이기 때문에 별도로 INNER 키워드를 입력하지 않아도 디폴트로 내부 조인으로 실행된다.

이렇게 실습을 하다보면 조금 귀찮은 부분이 있다. 바로 테이블1.책번호에서 테이블의 이름을 반복해서 적어줘야한다는 것인데 이는 별칭을 사용해서 간단하게 사용할 수 있다. 예를 들면 다음과 같다.

```sql
SELECT t1.책번호, t1.책이름, t2.가격 FROM 테이블1 t1 JOIN 테이블2 t2 ON t1.책번호 = t2.책번호;
```

내부 조인이 있으니 당연히 외부조인도 있는데 RIGHT와 LEFT 조인이 있다.

외부 조인은 필요한 내용이 한쪽 테이블에만 있어도 결과를 추출할 수 있는 방법이다.

예를 들어 고객과 상품 정보를 묶어서 주문 정보를 추출했다면 한 번이라도 주문한 적이 있는 고객의 정보만 추출할 수 있을 것이다. 만약 전체 고객의 구매 기록을 추출하고싶다면 외부 조인을 사용할 수 있다. 
```sql
SELECT M.mem_id, M.mem_name, B_prod_name, M.addr FROM member M LEFT OUTER JOIN buy B ON M.mem_id = B.mem_id ORDER BY M.mem_id;
```

여기서 RIGHT 외부 조인으로 바꾸고 싶다면 RIGHT OUTER JOIN으로 바꾸고 buy B 대신 member M을 넣어주고 FROM buy B로 바꾸어 주면 된다.

만약 주문한 내역이 없는 회원의 정보를 추출하고 싶을 땐 WHERE절로 prod_name이 NULL인 튜플들만 추출하면 된다.
