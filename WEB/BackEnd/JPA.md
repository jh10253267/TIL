# JPA

## 개요
mysql을 먼저 접하고 JPA를 접한 케이스다보니 JPA의 개념이 낮설고 헷갈림의 연속이였다.

JPA를 공부해보고 뭘 위한 것인지 공부해보려한다.

## JPA가 뭘까?

결국 Mysql이다. JPA에서 아무리 깊게 고민해서 코드를 짠다고 해도 결국 RDB의 쿼리로 변환되어 서버에 전송된다.

위의 한 줄을 가지고 JPA를 깊이 이해해보려고한다.

### 패러다임의 불일치

DB 테이블 설계는 PK를 가진 테이블을 먼저 설계하고 이 PK를 FK로 갖는 테이블을 설계한다.

이렇게 하면 PK를 가진 테이블에서 시작해서 FK를 갖는 테이블로 갈 수 있고 FK를 가진 테이블에서 PK를 가진 테이블로 갈 수 있다.

왼쪽 테이블을 기준으로 join을 해도, 오른쪽 테이블을 기준으로 join을 해도 조회가 가능하고 결과도 같다.

이게 DB의 방식이다. 

그러나 자바의 방식은 조금 다르다.

게시글이 댓글을 참조할 수도 있고 댓글이 게시글을 참조할 수도 있고 게시글이 댓글을 참조하고 댓글이 게시글을 참조하는 것도 가능하다.

고전적인 JDBC를 이용해서 DB를 사용하려면 쿼리문의 결과를 자바의 객체로 값을 셋팅해준다.

dto은 다음과 같이 생성한다.
```java
@Data
public class Post {
  private String title;
  private String content;
	private List<Comment> comments;
}
```
게시글과 코멘트를 같은 id값으로 즉, post의 id가 1인 정보를 조회해서 게시글에 대한 정보를 조회하고 추가적으로 fk를 이용해 댓글에 대한 정보를 조회한다. 

쿼리를 날린다면 다음과 같다.
```sql
SELECT * FROM POST WHERE ID = 1;
SELECT * FROM COMMENT WHERE POST_ID = 1;
```

이렇게 받아온 정보들을 dto안에 넣어준다. post.set(...);

이러한 방식은 DB의 방식에 맞춘 방식이다.

그러나 자바만 따로 놓고 보면 Post 안에 Comment를 넣고 자신에게 달린 댓글을 관리할 수도 있고 comment에서 post를 필드로 갖게 하고 관리할 수도 있다.

```java
@Data
public class Post {
  private Long postId;
  private String title;
  private String content;
  private List<Comment> comments;
}
```
새로운 코멘트가 달렸다면 이렇게 할 수 있다.
```java
post.getComments().add(new Comment(...));
```

이를 댓글의 관점에서 본다면 이렇다.

```java
@Data
public class Comments {
  private Long commentId;
  private String content;
  private Post post;
}
```

새로운 댓글이 달렸다면 이렇게 관리할 수 있을 것이다.
```java
Comment comment = new Comment();
comment.setCommentId(1L);
comment.setContent("comment content");
comment.setPost(post);
```

아니면 두 방식을 조합해서 관리할 수도 있을것이다.

여기서 패러다임의 불일치가 생긴다.

이러한 불일치를 해결하기 위한 개념이 바로 ORM이다.

ORM(Object Relation Mapping)이란 객체 지향으로 구성한 시스템을 관계형 데이터 베이스에 매핑하는 패러다임이다.

쉽게 말해 이전에는 DB의 방식을 Java가 따랐다면 Java의 방식으로 DB를 사용하는 것이다. 주도권을 가져왔다고 표현해도 될 것 같다.

이러한 ORM의 패러다임을 실현한게 바로 JPA이다.

JPA를 이용한 개발의 핵심은 객체지향을 통해서 영속계층을 처리하는데 있다.

앞에서 말했듯이 주도권을 Java가 가진다. 직접 테이블과 SQL을 다루는 것이 아니라 자바의 데이터 객체를 `엔티티 객체`로 다루고 JPA로 이를 DB에 매핑시킨다.

여기서 엔티티 객체란 DB로 따지면 PK를 가지는 테이블이다.
Java로 따지면 PK를 가지는 객체이다.

Spring Data JPA는 엔티티 객체를 이용해서 JPA를 사용하는 편리한 방법을 지원해주는 스프링 관련 라이브러리다. Spring Data JPA는 자동으로 객체를 생성하고 이를 통해서 여러 동작들을 자동으로 처리해주는데 이를 위해서 제공되는 인터페이스가 JpaRepository이다.

테이블을 생성한다. -> 엔티티 객체를 생성한다.

JPA를 사용하면 쿼리는 어떻게 쓰는걸까?

크게 3가지 방법이 있다.

쉽게 사용할 수 있는 방법으로는 메소드 이름이 쿼리가 되는 쿼리메소드이다.

많이 사용하는 기본적인 쿼리들을 기본적으로 사용할 수 있다.

SELECT, INSERT, UPDATE, DELETE.

각각 findById, save, save, deleteById 메소드이다.

findById는 일반적인 select와 같고 update는 먼저 select 쿼리가 실행되고 update가 실행된다.

delete를 실행시켜보면 먼저 select 쿼리가 실행되고 delete 쿼리가 실행된다.

여기서 바로 수정하고 삭제하면 되는데 굳이 select를 먼저 실행시키는 이유를 생각해보면 JPA를 이용한다는 것은 영속 컨텍스트와 데이터베이스를 동기화시켜서 관리한다는 의미이다.

따라서 엔티티 객체가 추가되면 영속 컨텍스트에 추가하고 데이터베이스와 동기화가 이루어져야한다. 따라서 수정이나 삭제를 한다면 일단 영속컨텍스트에 해당 엔티티 객체가 존재해야하기에 먼저 select를 실행해 엔티티 객체를 영속 컨텍스트에 저장해서 이를 삭제한 후에 delete가 이루어지게된다.

## 개념

개발할 때 repository 인터페이스를 생성하고 JpaRepository를 상속받기만 하면 crud작업을 할 수 있다.

구현체가 없는데 이게 어떻게 가능할까?

일단 JpaRepository의 구현체는 SimpleJpaRepository이다.
이 구현체를 살펴보면 기본적인 메소드들이 이미 구현이 되어있다. 따라서 인터페이스를 선언하기만 해도 사용이 가능하다.

즉 JPA를 한 단계 더 추상화시킨 JpaRepository를 사용하여 기본적인 CURD를 사용할 수 있는 것이다.

JPA의 모든 데이터 변경은 트랜잭션 안에서 실행된다.

즉 트랜잭션 밖에서의 데이터 변경은 반영되지 않는다.

save메소드를 자세히 보면 새로운 엔티티라면 em.persist를 통해 저장하고 아니라면 덮어씌우는 로직을 볼 수 있다.

여기서 save는 사실 db에 저장하는 것이 아니라 어플리케이션과 데이터베이스 사이에 존재하는 영속성 컨텍스트에 저장이 된다. 쉽게 말하면 엔티티를 저장하고 관리하는 저장소로 가상 데이터베이스같은 역할이다.

지금까지 컨텍스트라는 단어를 많이 접해봤을 것이다. 예를 들면 ApplicationContext, ServletContext 등등. 이들의 공통점은 바로 무언가가 생성되고 라이프사이클을 수행하는 공간이다. 영속성 컨텍스트도 마찬가지이다. 만들어지고(등록되고) 관리된다.

이 때 엔티티는 영속 상태에 있다고 한다. 영속 상태는 영속성 컨텍스트에서 관리되고 있는 상태를 말한다.

그렇다면 왜 중간에 가상의 데이터베이스가 있어야하는 것일까?

영속성 컨텍스트의 내부에는 1차 캐시를 가지고 있다. persist를 하는 순간 pk값, 타입과 객체를 매핑하여 1차캐시에서 가지고있다.

한 트랜잭션 내에 1차 캐시에 이미 있는 값을 조회하는 경우 DB를 조회하지 않고 1차 캐시에 있는 내용을 그대로 가져온다. 이 때의 캐시는 한 트랜잭션 내에서 공유되는 개념이다.

1차 캐시에 넣고 쓰기 지연 SQL 저장소에 쿼리를 만들어 쌓는다.

여기서 저장을 했을 때 데이터베이스에 반영하기 위한 쿼리를 만들어서 쓰기 지연 저장소에 쿼리를 쌓아서 넣어두게 된다.

또 다른 엔티티를 저장하면 위의 과정을 똑같이 수행한다.

그리고 모든 비즈니스 로직이 끝나게 되면 쓰기지연 저장소에 쌓아두었던 쿼리를 한 번에 데이터베이스에 반영하게 된다.

예외도 있는데 엔티티 클래스를 생성할 때 pk값을 db에서 알아서 처리하도록 한다(auto_increment 방식). 그러나 1차 캐시에서 pk값을 알아야하기 때문에 먼저 DB에 저장을 하고 1차 캐시에 올리는 작업이 일어난다.

 JPA를 사용하다 보면 조금 이상한 부분을 발견할 수 있는데 바로 애플리케이션에서 자바 엔티티의 값을 변경했을 뿐인데 데이터베이스 업데이트 쿼리가 발생하고 실제 DB에도 반영이 된다는 점이다. repository를 거치지 않았으니 DB에 반영되지 않고 객체에만 변경이 생길 것이라 예상했는데 말이다.
  그 원리는 더티 체킹때문이다. 여기서 더티란 상태의 변화가 생긴 정도를 의미한다. 따라서 더티체킹이란 엔티티 상태 변경 검사라고 할 수 있다.

하나의 트랜잭션이 끝나는 시점에 변화가 있는 모든 엔티티 객체를 데이터 베이스에 자동으로 반영해준다.

JPA는 commit하는 순간 내부적으로 flush가 호출되고 이 때 엔티티와 스냅샷을 비교한다. 

1차 캐시에는 처음 들어온 상태인 엔티티 스냅샷을 넣어두고 commit하는 순간 변경된 값이 있는지 비교하여 변경된 값이 있으면 update 쿼리를 아까 살펴본 쓰기 지연 SQL에 넣어둔다.

더티체킹에는 조건이 있는데 1차 캐시와 쓰기 지연 SQL을 사용하기 때문에 영속성 컨텍스트에 등록되지 않는다면 우리가 처음에 예상했던대로 자바의 엔티티객체에만 값이 변경되고 DB에는 반영되지 않는다.

아까 설명했던 대로 JPA의 데이터 변경은 트랜잭션 내부에서 일어나기 때문에 @Transactional 애노테이션을 붙여주지 않으면 데이터 변경이 일어나지 않는다.

## 페이징 처리

페이징에서 가장 중요한 개념은 바로 SQL의 limit 문법이다. 아무리 복잡하게 페이징을 구현한다고 해도 결국 limit로 select로 가져올 데이터를 제한하는 것이다.

JPA에서는 이를 매우 간단하게 처리할 수 있다.

바로 Page<E>와 Pageable 타입이다.

페이징 처리는 이러한 Pagealbe 인터페이스 타입의 객체를 구성해서 파라미터로 전달하면 된다. 

일반적으로는 PageRequest.of()기능을 사용한다.

Pageable pageable = PageRequest.of(페이지 번호, 사이즈, Sort) 와 같은 식으로 사용한다.

Page<Board> result = boardRepository.findAll(pageable);

실행되는 쿼리를 보면 데이터를 조회하는 쿼리가 나가고 전체 데이터의 개수를 처리하는 쿼리가 같이 실행된다.

리턴 타입인 Page는 페이징 처리에 필요한 정보들을 담고있다.

예를 들어 다음 페이지가 존재하는지, 이전페이지가 존재하는지 등의 정보이다.

여러 건의 조회의 경우 성능을 위해 페이징 처리가 필수적이다. 데이터가 얼마 없다면 상관 없겠지만 천 건, 만 건이 넘어간다면 매번 필요도 없는 데이터를 불러와야 할 것이다.

예를 들어 10개의 데이터만 불러오고 싶을 때 다음과 같이 작성할 수 있다.

```sql
SELECT * FROM ARTICLES LIMIT 10;
```
그리고 그 다음 10개의 데이터를 불러 올 땐 이렇게 작성할 수 있다.
```sql
SELECT * FROM ARTICLES LIMIT 10, 10;
```

 Pageable은 인터페이스로 설계되어있고 PageRequest.of() 기능을 이용하면 쉽게 페이징 처리 조건을 구성할 수 있다. 주의할 점은 다른 자료구조와 마찬가지로 우리가 생각하는 1이 여기선 0이다.

```java
PageRequest.of(0, 10); // 첫 번째 페이지, 페이지 크기가 10인 페이지 요청
PageRequest.of(0, 10, Sort.by("id").ascending()); // 첫 번째 페이지, 페이지 크기가 10인 페이지, id를 기준으로 오름차순 정렬
```
```java
Sort multiSortDesc = Sort.by("field1").descending().and(Sort.by("field2").ascending());
// 위와 같이 여러 값들을 정렬 조건으로 넣을 수도 있다.
```

파라미터로 Pageable을 사용하면 리턴 타입은 Page<T>타입을 이용할 수 있다.
Page 타입은 단순한 리스트 조회 결과 뿐만 아니라 숫자 데이터도 제공한다.

예를 들어 조회결과의 갯수는 얼마인지, 다음 페이지가 있는지, 전체 페이지수는 몇 개인지 등등.

JpaRepository에는 fildAll()이라는 메소드를 제공한다. 인자로는 Pageable타입을 받는다.

이를 통해서 간단한 페이징 처리가 가능하다.

그리고 검색 조건을 추가할 수도 있다.

예를 들어 게시글을 조회하는데 검색 결과를 페이징 처리하고싶다면 쿼리 메소드로 다음과 같이 작성할 수 있다.

```java
Page<Article> findByTitleContaining(String title, Pageable pageable);
```

### 쿼리를 작성하는 방법

앞에서 살펴본 쿼리메소드가 쿼리를 가장 쉽게 작성하고 사용할 수 있는 방법이다. 

그러나 검색 조건들이 많아진다면 쿼리메소드는 굉장히 길어진다.

따라서 단순한 쿼리를 작성할 때 주로 사용된다.

여러 조건이 있는 쿼리를 작성할 땐 JPQL(JPA에서 사용하는 쿼리 언어)을 사용하거나 Querydsl을 사용한다.

JPQL을 사용하면 조인과 같이 복잡한 쿼리를 실행할 수 있고 원하는 속성들을 추출해서 객체로 처리하거나 nativeQuery를 사용해서 데이터베이스에서 동작하는 SQL을 사용하는 기능을 이용할 수 있다.


### 정적인 쿼리의 한계

위의 방법들을 사용하면 무척 편하지만 쿼리가 정적으로 고정된 형태라는 한계가 있다.

예를 들어 검색기능을 생각해보자.

조건이 없을 수도 있고 하나일 수도 있고 두 개 세 개일 수도 있다.

만약 그렇다면 경우의 수를 고려하여 별도의 메소드를 작성해야한다.

이러한 문제를 해결하는 방법중 하나가 바로 Querydsl이다.

Querydsl은 JPA의 구현체인 Hibernate 프레임워크가 사용하는 HQL을 동적으로 생성할 수 있는 프레임워크로 JPA를 지원하고 있다.

Querydsl은 쉽게 말해 자바코드로 쿼리를 생성하는 방식이다.

자바 코드를 이용하기 때문에 런타임 시점이 아니라 컴파일 시점에 오류를 찾아낼 수 있기 때문에 타입의 안정성을 확보할 수 있다는 장점이있다.

Querydsl에서는 Q도메인 이라는 것이 필요한데 이는 Querydsl의 설정을 통해서 기존의 엔티티 클래스를 Querydsl에서 사용하기 위해 별도의 코드로 생성하는 클래스이다.

Querydsl을 사용하는 방법은 여러가지인데 그중에서 QuerydslRepositorySupport라는 클래스의 기능을 이용할 수 있다.

```java
@Override
public Page<Board> searchAll(Pageable pageable) {
  QArticle article = QArticle.article;
  JPQLQuery<Article> query = from(article);

  query.where(article.title.contains("test"));

  List<Article> list = query.fetch();

  Long count = query.fetchCount();

  return new PageImpl<>(list, pageable, count);
}
```















