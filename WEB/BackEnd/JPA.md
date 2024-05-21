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

왼쪽 테이블을 기준으로 join을 해도, 오른쪽 테이블을 기준으로 join을 해도 조회가 가능하고 동일한 결과가 나온다.

이게 DB의 방식이다. 방향성이 없다고도 말한다.

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

이러한 방식은 DB의 방식에 맞춘 방식이다. 자바 애플리케이션과 데이터베이스를 완전히 다른 서버로 보는 관점.

그러나 자바만 따로 놓고 보면 Post 안에 Comment를 넣고 자신에게 달린 댓글을 관리할 수도 있고 comment에서 post를 필드로 갖게 하고 관리할 수도 있다.

즉, 여러가지의 방법이 존재한다. (DB에서는 어느 곳에서 접근해도 상관없고 동일한 결과를 리턴한다.)

```sql
--왼쪽 테이블인 post를 기준으로 join
SELECT post.id, post.title, post.content, comment.id AS comment_id, comment.content AS comment_contnet FROM post JOIN comment ON post.id = comment.post_id; 

-- 오른쪽 테이블인 comment를 기준으로 join
SELECT post.id, post.title, post.content, comment.id AS comment_id, comment.content AS comment_contnet FROM comment JOIN POST ON comment.post_id = post.id;
```

자바에선 새로운 코멘트가 달렸다면 이렇게 할 수 있다.
```java
@Data
public class Post {
  private Long postId;
  private String title;
  private String content;
  private List<Comment> comments;
}
```

```java
post.getComments().add(new Comment(...));
```

이렇게 연관관계를 설정하는 것이 바로 `@OneToMany`이다.

자바의 객체로 본다면 post에서 comment 방향으로 접근하지만 comment에서 Post로는 접근할 수 없다.(comment는 post의 존재를 모른다.)

이번엔 게시글과 댓글의 관게를 댓글의 관점에서 본다면 이렇다.

```java
@Data
public class Comments {
  private Long commentId;
  private String content;
  private Post post;
}
```

1번 게시글에 새로운 댓글이 달렸다면 이렇게 관리할 수 있을 것이다.
```java
Post post = findPost(1L);
Comment comment = new Comment();
comment.setCommentId(1L);
comment.setContent("comment content");
comment.setPost(post);
```

이렇게 설정하는 것이 바로 `@ManyToOne`이다.

이렇게 한다면 post는 comment의 존재를 모른다. post에서 comment의 접근은 불가능하고 comment에서 Post에 접근하는 방식이다.

아니면 두 방식을 조합해서 관리할 수도 있을 것이다.

두 방식을 조합해서 관리하는 방식을 양방향 관계라고 한다.

여기서 DB와 객체지향 간의 패러다임의 불일치가 생긴다.

이러한 불일치를 해결하기 위한 개념이 바로 ORM이다.

**ORM(Object Relation Mapping)** 이란 객체 지향으로 구성한 시스템을 관계형 데이터 베이스에 매핑하는 패러다임이다.

쉽게 말해 이전에는 DB의 방식을 Java가 따랐다면 Java의 방식으로 DB를 사용하는 것이다. 주도권을 가져왔다고 표현해도 될 것 같다.

ORM을 통해 자바의 객체를 이용해서 프로그램을 작성하듯이 DB를 사용할 수 있는 것이다.
DB에 대해 전혀 이해하지 못하고 있더라도 DB를 이용한 웹 서비스를 개발할 수 있다.

반복되고 무의미한 작업들(반복되는 쿼리 작성, 단순한 쿼리 작업)을 ORM(우리에게는 JPA)에게 맡기는 것이다.
(Spring JDBC의 SimpleJdbcInsert를 보면 이러한 고민의 흔적이 보인다.)

만약 post테이블의 pk를 comment테이블에서 fk로 사용하고 있다면 개발자가 직접 join쿼리를 이용하는 것이 아니라 `post.getComments();`와 같이 작성하면 JPA에서 알아서 join쿼리 작성하고 수행한다.

일반적으론 FK를 기준으로, 변화가 많은 쪽을 선택하여 연관관계를 설정해주는데 게시글과 댓글로 본다면 댓글에서 @ManyToOne으로 단방향 연관관계를 설정해주는 식이다.(단방향 연관관계란 두 개의 클래스에서 오직 하나만 다른 하나의 클래스의 존재를 알고있다는 의미이다.)

그러나 한국어를 기준으로 생각해보면 의문이 생길 수도 있다. 게시판을 생각해보면 게시글을 클릭하면 게시글과 댓글이 같이 있으니 하나의 게시글이 여러개의 댓글을 가지는 것이 좀 더 자연스럽지 않을까?

여기에는 문제가 있다.

```java
@Entity
public class Post {

@Id
private Long id;
private String content;
private String title;
private String writer;

private List<Comment> comments = new ArrayList<>();

public void addComment(Comment comment) {
  this.comments.add(comment);
}
}
```

```java
@Entity
public class Comment {
  @Id
  private Long id;
  private String content;
  private String writer;
}
```
이렇게 작성하고 실행시켜보면 테이블은 정상적으로 생성된다.

그러나 이렇게 생성된 테이블과 엔티티간의 짝이 맞지 않게된다.

생성된 테이블에는 Comment에 FK인 post_id가 있지만 정작 엔티티 클래스에는 post_id가 없고 오히려 이 FK를 Post에서 관리한다.

이런 상황에서 Comment를 수정하게 된다면 정작 쿼리는 Post에서 나가게 된다.

이렇게 되면 엔티티와 DB간의 관계가 명확하지 않게된다.

그리고 객체지향 시스템을 RDB에 매핑시키는 것이 ORM의 목적이므로 객체지향의 관점에서 봐보자. Post와 Comment가 엔티티가 아닌 일반 자바 클래스라고 해보자.

1. Post 클래스가 Comment를 관리하는 것이 주된 책임이라면 댓글리스트를 가지는 것이 더 적절할 것이고 Comment 클래스가 댓글 하나의 정보를 관리하는 것이 주된 책임이라면 Post에 대한 참조를 갖는 것이 더 적절하다.
2. 다중성이 적은 방향을 선택하는 것이 바람직하다. 자신의 컬렉션 필드를 관리하는 노력이 필요하기 때문이다.

이런 이유로 @OneToMany를 사용하여 단방향 연관관계를 맺는 것 보다 @ManyToOne을 사용하여 단방향 연관관계를 맺는 것이 좋다.

그러나 이렇게 하면 댓글의 관점에서의 해석이기 때문에 post에서는 comment의 존재를 모른다.

그렇기 때문에 Post엔티티를 통해 comment의 정보를 읽어올 수 없다.

이 것이 바로 단방향 참조가 가지는 단점이다.

필요한 정보에 하나의 엔티티를 통해서 접근할 수 없다는 것이다.

그래서 만약 post정보와 comment정보를 둘 다 사용하고 싶다면 단순히 객체를 이용해서 접근하는 것이 아니라 join을 사용해야하는데 일반 Join을 사용하면 안된다.

그 이유는 한쪽에만 데이터가 존재하는 상황이 있기 때문이다.

예를 들어 특정 게시물은 댓글이 없는 경우가 있을 수 있다. 그렇기 때문에 left (outer) join을 사용해야한다.

```java
JPQLQuery<Post> query = from(post);
query.leftjoin(comment).on(comment.post.eq(post));
```

## JPA의 개념

JPA를 이용한 개발의 핵심은 객체지향을 통해서 영속계층을 처리하는데 있다.

앞에서 말했듯이 주도권을 Java가 가진다. 직접 테이블과 SQL을 다루는 것이 아니라 자바의 데이터 객체를 `엔티티 객체`로 다루고 JPA로 이를 DB에 매핑시킨다.

여기서 엔티티 객체란 DB로 따지면 PK를 가지는 테이블이다.
Java로 따지면 PK를(고유한 번호를) 가지는 객체이다.

엔티티 클래스를 생성한다. -> 테이블을 생성한다가 되는 것이다.

엔티티 인스턴스를 만든다 -> 새로운 DB 튜플을 만든다 가 되는 것이다.

JPA의 사용은 단순히 JpaRepository 인터페이스를 구현하는 것만으로도 가능하다.

Spring Data JPA는 엔티티 객체를 이용해서 JPA를 사용하는 편리한 방법을 지원해주는 스프링 관련 라이브러리다. Spring Data JPA는 자동으로 객체를 생성하고 이를 통해서 여러 동작들을 자동으로 처리해주는데 이를 위해서 제공되는 인터페이스가 JpaRepository이다.

```java
public interface PostRepository extends JpaRepository<Post, Long> {

}
```

이렇게 사용하는 것만으로도 간단한 crud작업을 처리할 수 있다.


```
그리고 양방향 관계를 맺지 않으면 안된다고 생각하는 경우를 많이 봤다.

바로 상위 엔티티가 삭제될 때 하위 엔티티들이 문제가 된다는 것인데 위에서 말한대로 JPA로 어떻게 하던지 결국 DB의 방식을 따른다.  
DB에선 참조 무결성이라는 개념이 존재한다. 바로 PK를 FK로 갖는 튜플이 있는 상태에서 PK를 가진 튜플이 삭제된다면 데이터의 무결성에 문제가 생긴다.

따라서 이를 막기위해 cascade와 restriction으로 참조 무결성을 지킨다.

cascade는 PK를 가진 튜플이 삭제될 때 PK를 FK로 갖는 튜플들을 자동으로 삭제해주는 기능.

restrict는 삭제 명령을 취소시키는 기능이다.

만약 상위 튜플을 삭제하려면 어떤 방식이건 하위 튜플들을 모두 삭제시킨 후 상위 튜플을 삭제하면 되는 것이다.
```
### 주의점

JPA의 모든 데이터 변경은 트랜잭션 안에서 실행된다.

즉 트랜잭션 밖에서의 데이터 변경은 반영되지 않는다.

### 영속성 컨텍스트

save메소드를 자세히 보면 새로운 엔티티라면 em.persist를 통해 저장하고 아니라면 덮어씌우는 로직을 볼 수 있다.

여기서 save는 사실 db에 저장하는 것이 아니라 어플리케이션과 데이터베이스 사이에 존재하는 영속성 컨텍스트에 저장이 된다. 쉽게 말하면 엔티티를 저장하고 관리하는 저장소로 가상 데이터베이스같은 역할이다.

지금까지 컨텍스트라는 단어를 많이 접해봤을 것이다. 예를 들면 ApplicationContext, ServletContext 등등. 이들의 공통점은 바로 무언가가 생성되고 라이프사이클을 수행하는 공간이다. 영속성 컨텍스트도 마찬가지이다. 만들어지고(등록되고) 관리된다.

이 때 엔티티는 영속 상태에 있다고 한다. 영속 상태는 영속성 컨텍스트에서 관리되고 있는 상태를 말한다.

그렇다면 왜 이처럼 중간에 가상의 데이터베이스가 있어야하는 것일까?

영속성 컨텍스트의 내부에는 1차 캐시를 가지고 있다. persist를 하는 순간 pk값, 타입과 객체를 매핑하여 1차캐시에서 가지고있다.

한 트랜잭션 내에 1차 캐시에 이미 있는 값을 조회하는 경우 DB를 조회하지 않고 1차 캐시에 있는 내용을 그대로 가져온다. 이 때의 캐시는 한 트랜잭션 내에서 공유되는 개념이다.

1차 캐시에 넣고 쓰기 지연 SQL 저장소에 쿼리를 만들어 쌓는다.

여기서 저장을 했을 때 데이터베이스에 반영하기 위한 쿼리를 만들어서 쓰기 지연 저장소에 쿼리를 쌓아서 넣어두게 된다.

또 다른 엔티티를 저장하면 위의 과정을 똑같이 수행한다.

그리고 모든 비즈니스 로직이 끝나게 되면 쓰기지연 저장소에 쌓아두었던 쿼리를 한 번에 데이터베이스에 반영하게 된다.

예외도 있는데 엔티티 클래스를 생성할 때 pk값을 db에서 알아서 처리하도록 한다(auto_increment 방식). 그러나 1차 캐시에서 pk값을 알아야하기 때문에 먼저 DB에 저장을 하고 1차 캐시에 올리는 작업이 일어난다.

JPA를 사용하다 보면 조금 이상한 부분을 발견할 수 있는데 바로 애플리케이션에서 자바 엔티티의 값을 변경했을 뿐인데 데이터베이스 업데이트 쿼리가 발생하고 실제 DB에도 반영이 된다는 점이다. 사실 튜플 수정을 위한 메소드인 repository.save()가 존재한다. 이 메소드를 사용하지 않았을 때 repository를 거치지 않았으니 DB에 반영되지 않고 객체에만 변경이 생길 것이라 예상했는데 말이다.

그 원리는 더티 체킹때문이다. 여기서 더티란 상태의 변화가 생긴 정도를 의미한다. 따라서 더티체킹이란 엔티티 상태 변경 검사라고 할 수 있다.

하나의 트랜잭션이 끝나는 시점에 변화가 있는 모든 엔티티 객체를 데이터 베이스에 자동으로 반영해준다.

JPA는 commit하는 순간 내부적으로 flush가 호출되고 이 때 엔티티와 스냅샷을 비교한다. 

1차 캐시에는 처음 들어온 상태인 엔티티 스냅샷을 넣어두고 commit하는 순간 변경된 값이 있는지 비교하여 변경된 값이 있으면 update 쿼리를 아까 살펴본 쓰기 지연 SQL에 넣어둔다.

더티체킹에는 조건이 있는데 1차 캐시와 쓰기 지연 SQL을 사용하기 때문에 영속성 컨텍스트에 등록되지 않는다면 우리가 처음에 예상했던대로 자바의 엔티티객체에만 값이 변경되고 DB에는 반영되지 않는다.

아까 설명했던 대로 JPA의 데이터 변경은 트랜잭션 내부에서 일어나기 때문에 @Transactional 애노테이션을 붙여주지 않으면 데이터 변경이 일어나지 않는다.

### 양방향 관계

양방향 관계란 Post에선 OneToMany를 사용하고 댓글에선 ManyToOne을 사용하여 관계를 설정하는 것을 말한다.

양방향 OneToMany는 기본적으로 각 엔티티에 해당하는 테이블을 독립적으로 생성하고 중간에 이들을 매핑해주는 테이블을 생성한다.

이를 사용하지 않는 방법으로는 단방향으로 OneToMany를 사용하는 방법과 mappedBy속성을 사용하는 방법이 있다.

흔히 mappedBy를 연관관계의 주인이라고 해석하기도 한다.

예를 들어 게시물 관점에서 보면 각 댓글들은 모두 별개의 존재로 하나의 댓글이 여러 개의 게시물에 사용될 수 있는 구조를 가정하고 생성하게 된다.  
반대로 댓글의 관점에서는 하나의 게시물을 참조하는 구조를 가정하기 때문에 mappedBy를 이용하여 댓글쪽의 해석을 적용할 것을 명시한다.

```java
public class Post {
...
@OneToMany(mappedBy = "post")
private Set<Comment> commentSet = new HashSet<>();
...
}
```

이렇게 하면 댓글이 연관관계의 주인이므로 매핑 테이블이 사라지고 @ManyToOne의 구조처럼 테이블이 생성된다.


### N+1 문제

JPA에서 굉장히 유명한 문제가 있는데 바로 N+1문제이다.  
OneToMany 방식은 자신의 하위에 여러 하위 엔티티를 가진다.

객체지향 시스템이라면 한 엔티티를 불러오면 연관되어있는 엔티티도 당연히 같이 불러와져서 사용할 수 있다.

그러나 JPA에서는 두가지 로딩 방식을 지원하고있다.  
하나는 지연 로딩, 다른 하나는 즉시 로딩

지연 로딩 방식은 게시글 엔티티에 댓글 엔티티가 있는 경우 게시글 엔티티가 호출될 때 일단 게시글 엔티티만을 조회한다.  
만약 댓글 엔티티 조회가 일어나면 그 때 데이터베이스에 요청을 보내 조회해온다.

일반적으로는 지연로딩을 디폴트로 사용한다. 왜냐하면 게시글만 사용하는 로직에서 댓글과 관련된 쿼리가 나가는 것은 비효율적이기 때문이다.

즉시 로딩은 사용하면 객체지향 시스템을 다룰 때와 동일하게 JPA를 사용할 수 있다.
먄약 지연 로딩방식을 사용할 경우 로직에서 상위 엔티티 뿐만 아니라 연관된 엔티티도 항상 같이 사용한다면 두 번의 쿼리가 나가게 되는 것이기 때문에 성능적으로 손해를 본다.

이럴 경우 즉시로딩이 적합할 수 있지만 데이터가 많아진다면 연관된 엔티티를 모두 조인하기 때문에 방대한 쿼리가 나갈 수 있다.

만약 이러한 상황에서 게시글의 목록을 읽어오면 문제가 생긴다.

목록을 가져오는 쿼리 한 번(1)과 하나의 게시글마다 댓글을 읽어오는 쿼리(N)가 실행된다.  
따라서 굉장히 많은 양의 쿼리가 발생하게된다.

이 문제에 대한 가장 간단한 해결책은 @BatchSize를 이용하는 것이다. 이는 N번의 쿼리를 모아서 한 번에 실행하는 size를 지정하는 애노테이션이다.

이렇게 작성한 뒤 실행시켜보면 이전의 결과와는 다른 것을 확인할 수 있다.

댓글을 조회할 때 한 번에 In조건으로 사용되어 조건의 범위를 지정하거나 지정된 값 중에서 하나 이상과 일치하면 조건에 맞는 것으로 판단하게 한다.

### @EntityGraph

로딩이란 하나의 엔티티를 조회할 때 연관되어있는 엔티티들을 어떻게 가져올 것이냐를 말한다.

JPA에선 객체와 필드를 보고 쿼리를 생성하는데 다른 객체가 필드로 명시되어있으면 그 객체들까지 조회한다. 

가장 간단한 방법은 즉시(eager)로딩을 적용하는 것이지만 성능적인 이슈가 생길 수 있다.

일반적으로는 지연(lazy)로딩을 사용하는데 이는 필요한 시점에 연관된 객체의 데이터를 불러오는 것이다.

@OneToMany의 경우 기본적으로 지연 로딩을 한다.

게시물을 조회하는 경우 Post객체와 Comment객체들을 생성해야하므로 2번의 SELECT가 수행된다.

테스트 메소드를 작성해서 확인해보면
```java
Optional<Post> result = postRepository.findById(1L);

Post post = result.orElseThrow();

log.info(post);
log.info(post.getCommentSet());
```

post를 출력하고 다시 select를 실행하려고 했을 때 데이터베이스의 연결이 이미 끝난 상태이다. 따라서 "no session"이라는 메시지가 뜬다.

이를 해결하려면 @Transactional을 명시해줄 수 있다.

그러나 지연로딩을 사용하면서도 한 번에 조인 처리를 해서 SELECT가 이루어지도록 하는 방법이 있는데 바로 @EntityGraph를 사용하는 것이다.
```java
@EntityGraph(attributePaths = {"commentSet"})
@Query("select p from Post p where p.id = :postId")
Optional<Post> findByIdWithComments(Long postId);
```

이렇게 작성하고 테스트 메소드를 실행해보면...
```java
Optional<Post> result = postRepository.findById(1L);

Post post = result.orElseThrow();

log.info(post);
for (Comment comment : post.getCommentSet()) {
  log.info(comment);
}
```

실행 결과를 보면 post테이블과 comment 테이블의 조인처리가 된 상태로 select가 실행되면서 post엔티티와 comment엔티티를 한 번에 처리할 수 있게 되었다.

이처럼 @OneToMany를 사용할 때의 장점중 하나가 바로 이러한 하위 엔티티의 처리라고 할 수 있다.

### 영속성의 전이

상위 엔티티와 하위 엔티티의 연관관계를 싱위의 엔티티에서 관리했을 때 중요한 것은 상위 엔티티의 상태가 변경되었을 때 하위 엔티티 객체들 역시 같이 영향을 받는다는 것이다.

이를 영속성의 전이라고 한다.

예를 들어 Comment객체가 JPA에 의해 관리되면 이를 참조하고 있는 Post객체 역시 같이 처리되어야 한다.

이러한 경우 연관관계에 cascade 속성을 부여해서 이를 제어하도록 한다.


### JPA를 사용하면 쿼리는 어떻게 쓰는걸까?

크게 3가지 방법이 있다.

쉽게 사용할 수 있는 방법으로는 메소드 이름이 쿼리가 되는 쿼리메소드이다.

많이 사용하는 기본적인 쿼리들을 기본적으로 사용할 수 있다.

SELECT, INSERT, UPDATE, DELETE.

각각 findById, save, save, deleteById 메소드이다.

findById는 일반적인 select와 같고 update는 먼저 select 쿼리가 실행되고 update가 실행된다.

delete를 실행시켜보면 먼저 select 쿼리가 실행되고 delete 쿼리가 실행된다.

여기서 바로 수정하고 삭제하면 되는데 굳이 select를 먼저 실행시키는 이유를 생각해보면 JPA를 이용한다는 것은 영속 컨텍스트와 데이터베이스를 동기화시켜서 관리한다는 의미이다. 즉 java 애플리케이션에서 DB를 다루겠다는 의미이다.

따라서 엔티티 객체가 추가되면 영속 컨텍스트에 추가하고 데이터베이스와 동기화가 이루어져야한다. 따라서 수정이나 삭제를 한다면 일단 영속컨텍스트에 해당 엔티티 객체가 존재해야하기에 먼저 select를 실행해 엔티티 객체를 영속 컨텍스트에 저장해서 이를 삭제한 후에 실제 delete가 이루어지게된다.

이러한 쿼리 외에도 다양한 쿼리메소드가 존재한다. 예를 들면 `findByNameContains`와 같은 식이다. 이 쿼리메소드는 해당 문자열을 포함하고 있는 결과를 조회해서 리턴한다.

만약 간단히 검색기능을 구현하고 싶다면 이렇게 작성할 수 있다.

```java
List<Post> findByPostTitleContains(String keyword);
```

## 페이징 처리

페이징에서 가장 중요한 개념은 바로 SQL의 limit 문법이다. 아무리 복잡하게 페이징을 구현한다고 해도 결국 select로 가져올 데이터를 제한하는 것이다.

JPA에서는 이를 매우 간단하게 처리할 수 있다.

바로 Page<E>와 Pageable 타입이다.

페이징 처리는 이러한 Pagealbe 인터페이스 타입의 객체를 구성해서 파라미터로 전달하면 된다. 

일반적으로는 PageRequest.of()기능을 사용한다.

Pageable pageable = PageRequest.of(페이지 번호, 사이즈, Sort) 와 같은 식으로 사용한다.

Page<Board> result = boardRepository.findAll(pageable);

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

 Pageable을 이용하면 쉽게 페이징 처리 조건을 구성할 수 있다. 주의할 점은 다른 자료구조와 마찬가지로 우리가 생각하는 1이 여기선 0이다.

 즉 첫 페이지를 가져오고 싶다면 0이다.

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

### 커스텀 쿼리를 작성하는 방법

앞에서 살펴본 쿼리메소드가 쿼리를 가장 쉽게 작성하고 사용할 수 있는 방법이다. 

그러나 검색 조건들이 많아진다면 쿼리메소드는 굉장히 길어진다.

따라서 쿼리 메소드는 단순한 쿼리를 작성할 때 주로 사용된다.

여러 조건이 있는 쿼리를 작성할 땐 JPQL(JPA에서 사용하는 쿼리 언어)을 사용하거나 Querydsl을 사용한다.

JPQL을 사용하면 조인과 같이 복잡한 쿼리를 실행할 수 있고 원하는 속성들을 추출해서 객체로 처리하거나 nativeQuery를 사용해서 데이터베이스에서 동작하는 SQL을 사용하는 기능을 이용할 수 있다.

### 정적인 쿼리의 한계

위의 방법들을 사용하면 편하지만 쿼리가 정적으로 고정된 형태라는 한계가 있다.

예를 들어 검색기능을 생각해보자.

조건이 없을 수도 있고 하나일 수도 있고 두 개 세 개일 수도 있다.

만약 그렇다면 경우의 수를 고려하여 별도의 메소드를 작성해야한다.

이러한 문제를 해결하는 방법중 하나가 바로 Querydsl이다.

Querydsl은 JPA의 구현체인 Hibernate 프레임워크가 사용하는 HQL을 동적으로 생성할 수 있는 프레임워크로 JPA를 지원하고 있다.

Querydsl은 쉽게 말해 자바코드로 쿼리를 생성하는 방식이다.

자바 코드를 이용하기 때문에 런타임 시점이 아니라 컴파일 시점에 오류를 찾아낼 수 있기 때문에 타입의 안정성을 확보할 수 있다는 장점이 있다. 이 이점은 생각보다 크다. 예를 들어 JPQL로 작성한 쿼리에 오류가 있는데 해당 경로가 호출되는 빈도가 굉장히 낮다고 하면 오류가 있다는 것을 알기까지 시간이 얼마나 걸릴지 모른다. 그러나 Querydsl을 사용하면 이런 오류를 빠르게 인지할 수 있다.

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