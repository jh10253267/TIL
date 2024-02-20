# JPA Query

## 개요

JPA Query라는 단어는 없는걸로 알지만 데이터베이스에 있던 주도권을 자바로 가져오긴 했는데 Jpa에서는 어떻게 원하는 기능을 구현하는지 정리해보려한다.

## 쿼리메소드

기본적으로 레포지토리 클래스를 인터페이스로 선언하고 JpaRepository라는 인터페이스를 상속받아 기본적인 crud + 작업을 할 수 있다. 이미 다 구현이 되어있는 상이고 주로 많이 쓰는 메소드는 다음과같다.


```java
// 테이블의 pk값을 이용해 단일 튜플을 뽑아내는 메소드
// select
boardRepository.findById(1L);

// 데이터를 집어넣거나 수정하는 메소드
// insert/update 모두 save메소드를 사용한다.
boardRepository.save(board);

// 데이터를 삭제하는 메소드
boardRepository.delete(board);
```
기본 crud기능들 외에 다른 기능 구현을 위해 쿼리를 작성할 필요가있다면 어떻게 해야할까?

크게 세 가지 방법이 있다.

* 쿼리메소드
* @Query
* QueryDsl

쿼리 메소드는 보통 sql에서 사용하는 키워드와 칼럼들을 결합해서 구성하면 그 메소드 자체가 하나의 쿼리가 되는 방식이다.

```java
findByTitleContainingOrderByIdDesc();
```

위와 같은 방식으로 다양한 커스텀 쿼리를 작성할 수 있다.

그러나 보면 알겠지만 메소드이름이 너무 길어지는 단점이 있다. 그래서 간단한 기능들만 쿼리메소드로 구현하는 것이 좋다.

## @Query

실제 sql에서 쿼리를 작성하듯이 사용할 수 있는 방법이다.

조금 다른점이 있다면 실제 sql문법을 그대로 사용하지 않고 jpa내부에서 사용되는 쿼리언어인 JPQL을 사용한다는 점이다.

```java
@Query("select b from Board b where b.title like concat('%', :keyword, '%)")
Page<Board> findByKeyword(String keyword, Pageable pageable);
```

이런 식으로 작성할 수 있다.


## Querydsl

앞에서 정리한 방법들은 편리하긴하지만 고정된 형태라는 단점이 있다.

예를 들어 검색기능을 구현하고싶다고 했을 때 경우의 수가 다양할 수 있다. 제목/내용/작성자 단일 조건으로 검색하는 경우도 있지만 제목과 내용, 제목과 작성자 등 복합적인 검색조건을 설정하는 경우가 생길 수 있다.

그래서 동적 쿼리를 처리하기 위한 방법이 바로 querydsl이다.

사실 querydsl은 설정부터 까다롭다. 버전에 따라 다르고 인터넷에서 찾아봤던 설정이 잘동하지 않는 경우가 있다.

여러개 시도해보고 되는 설정을 복사해서 사용하고있다.

Querydsl을 이용하기위해선 Q도메인이 필요하다. Q도메인은 Querydsl의 설정을 통해서 기존의 엔티티 클래스를 querydsl에서 사용하기 위해서 별도의 코드로 생성하는 클래스이다.

```java
@Override
    public Page<Todo> searchAll(String[] types, String keyword, Pageable pageable) {
        QTodo todo = QTodo.todo;
        JPQLQuery<Todo> query = from(todo);

        if ((types != null && types.length > 0) && keyword != null) {

            BooleanBuilder booleanBuilder = new BooleanBuilder();

            for (String type : types) {

                switch (type) {
                    case "t":
                        booleanBuilder.or(todo.title.contains(keyword));
                        break;
                    case "c":
                        booleanBuilder.or(todo.content.contains(keyword));
                        break;
                    case "w":
                        booleanBuilder.or(todo.writer.contains(keyword));
                        break;
                }
            }
            query.where(booleanBuilder);
        }
        query.where(todo.tno.gt(0L));
        query.where(todo.deletedAt.isNull());

        this.getQuerydsl().applyPagination(pageable, query);

        List<Todo> list = query.fetch();
        long count = query.fetchCount();

        return new PageImpl<>(list, pageable, count);
    }
```

이런식으로 쿼리 dsl을 사용할 수 있다.

pageable은 일종의 페이지네이션 조건으로 jpa를 사용하면 페이징을 쉽게 구현할 수 있다. sql의 쿼리를 자바에서 사용하는 듯한 느낌을 준다.