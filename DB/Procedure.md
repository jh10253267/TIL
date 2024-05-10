# Procedure

## 개요

스토어드 프로시저는 MySQL에서 프로그래밍을 하기 위해 사용하는 데이터베이스 개체이다. 

쉽게 말하면 SQL을 한 문장씩 입력해서 사용하는 것만 아니라 프로그래밍 로직을 추가해서 사용하는 방법이다.


## 기본

Procedure란 쿼리 문의 집합이며 어떠한 동작을 일괄적으로 처리하기 위한 용도로 사용된다.

자주 사용하는 쿼리 묶음을 프로시저로 정의해놓고 필요할 때마다 간단히 호출하는 방식으로 사용한다.

```sql
DELIMITER $$
CREATE PROCEDURE 스토어드 프로시저 이름
BEGIN
로직 작성
END $$
DELIMITER ;
```

$$ 기호가 붙은 곳은 스토어드 프로시저를 묶어주는 기능을 한다. 1개만 사용해도 되지만 명확하기 표시하기 위해 2개를 사용한다.
이를 ##, %%, && 등으로 바꾸어도 된다.

여기서 DELIMITER는 구분자라는 의미로 일반적으로 MYSQL에서는 구분자로 ;(세미콜론) 기호를 사용하지만 프로시저의 내부에서도 많은 SQL문의 끝에 세미콜론을 사용하기 때문에 세미콜론이 나왔을 때 이 것이 SQL의 끝인지 스토어드 프로시저의 끝인지 헷갈릴 수 있기 때문에 프로시저의 구분자를 $$로 사용하여 구분한다.

마지막의 `DELIMITER ;`는 구분자를 다시 세미콜론으로 바꾸어주는 부분이다.

이렇게 프로시저를 정의한 뒤 `CALL 스토어드 프로시저 이름();` 형식으로 호출한다. 파라미터가 필요한 경우 일반적인 프로그래밍 언어 함수 사용과 마찬가지로 괄호안에 파라미터를 넣어주면 된다.

```sql
DELIMITER $$
CREATE PROCEDURE user_proc()
BEGIN
  SELECT * FROM member;
END $$
DELIMITER ;

CALL user_proc();
```

간단하게 프로시저를 정의하여 호출하고 있다. 프로시저가 호출되면 member테이블에 있는 모든 튜플을 조회한다.

프로시저의 삭제는 `DROP PROCEDURE`를 사용할 수 있다.

```sql
DROP PROCEDURE user_proc();
```

매개변수를 사용하는 방법을 알아보자.

`IN 입력 매개변수 이름 데이터 형식 CALL 프로시저 이름(전달 값);`과 같이 사용할 수 있다.

`OUT 출력 매개변수 이름 데이터 형식`

`CALL 프로시저 이름(@변수명); SELECT @변수명;`

```sql
DELIMITER $$
CREATE PROCEDURE user_proc(IN userName VARCHAR(10))
BEGIN
  SELECT * FROM member WHERE member_name = userName;
END $$
DELIMITER ;

CALL user_proc('테스트 유저1');
```

