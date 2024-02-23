# 📻 News Feed Project
![header](https://capsule-render.vercel.app/api?type=waving&color=timeGradient&text=NewsFeedProject!!&animation=twinkling&fontSize=35&fontAlignY=40&fontAlign=50&height=250)

## 📇 개요
<b>뉴스피드(New Speed 아님!) 팀 프로젝트</b><br>
코딩하면서 어떤 노래를 들으시나요? 한눈에 볼 수 있는 개발자들의 플레이리스트<br>
서로의 플레이리스트를 공유하며 다양한 음악을 접해보자!

## 💾 디렉토리 구조
계층별로 나누지 않고 도메인으로 나누어 관리하는 전략을 선택했다.
```
── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── sparta/
│   │   │           └── newsfeed/
│   │   │               ├── NewsFeedApplication.java
│   │   │               ├── advice/
│   │   │               │   └── CustomRestAdvice.java
│   │   │               ├── auth/
│   │   │               │   └── MainController.java
│   │   │               ├── board/
│   │   │               │   ├── controller/
│   │   │               │   ├── domain/
│   │   │               │   ├── dto/
│   │   │               │   ├── repository/
│   │   │               │   └── service/
│   │   │               ├── comment/
│   │   │               │   ├── controller/
│   │   │               │   ├── domain/
│   │   │               │   ├── dto/
│   │   │               │   ├── repository/
│   │   │               │   └── service/
│   │   │               ├── config/
│   │   │               │   ├── CustomServletConfig.java
│   │   │               │   └── RootConfig.java
│   │   │               ├── domain/
│   │   │               │   └── BaseEntity.java
│   │   │               ├── file/
│   │   │               │   ├── controller/
│   │   │               │   ├── domain/
│   │   │               │   └── dto/
│   │   │               ├── member/
│   │   │               │   ├── controller/
│   │   │               │   ├── domain/
│   │   │               │   ├── dto/
│   │   │               │   ├── repository/
│   │   │               │   └── service/
│   │   │               └── security/
│   │   │                   ├── config/
│   │   │                   ├── jwt/
│   │   │                   └── service/
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── static/
│   │       └── templates/
│   │           ├── login.html
│   │           ├── mainpage.html
│   │           └── signup.html
│   └── test/
│       └── java/
│           └── com/
│               └── sparta/
│                   └── newsfeed/
│                       ├── DataSourceTests.java
│                       ├── NewsFeedApplicationTests.java
│                       ├── board/
│                       │   ├── domain/
│                       │   ├── repository/
│                       │   └── service/
│                       ├── comment/
│                       │   ├── repository/
│                       │   └── service/
│                       ├── member/
│                       │   └── repository/
│                       └── security/
│                           └── jwt/
└── upload/
    ├── b7dea34d-3338-47e9-bfa9-91204954fe6a_2620.jpg
    ├── s_9fae02c9-f0f5-4d97-a97e-3ff64f5b8585_2620.jpg
    └── s_b7dea34d-3338-47e9-bfa9-91204954fe6a_2620.jpg

```

## 📔 Stacks
 <img src="https://img.shields.io/badge/SpringBoot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white">
 <img src="https://img.shields.io/badge/java-%23ED8B00?style=for-the-badge&logo=openjdk&logoColor=white"><img src="https://img.shields.io/badge/17-515151?style=for-the-badge">
<img src="https://img.shields.io/badge/Gradle-02303A?style=for-the-badge&logo=Gradle&logoColor=white">

## ⚙ 기능
### 회원가입
  * 사용자는 해당 서비스에 아이디와 패스워드로 가입할 수 있다.
### 로그인/로그아웃
  * 사용자는 회원가입한 아이디로 로그인하여 해당 서비스를 이용할 수 있다.
### 토큰 기반 인증
  * 로그인에 성공하면 서버에서 JWT토큰을 발급해주고 토큰이 필요한 경로에 요청을 보내 서비스를 이용할 수 있다.
### 뉴스피드 작성하기
*  사용자는 로그인한 뒤 이미지등의 파일과 함께 자신의 플레이리스트를 동료 개발자들과 공유할 수 있다.
### 뉴스피드 수정하기
  * 사용자는 자신이 작성한 뉴스피드를 수정할 수 있다.
### 뉴스피드 삭제하기
  * 사용자는 자신이 작성한 뉴스피드를 삭제할 수 있다.
### 뉴스피드에 댓글 남기기
  * 댓글을 통해 자신의 의견을 나타낼 수 있다.
### 댓글 수정/삭제
  * 자신이 남긴 댓글을 수정 또는 삭제할 수 있다.

## 📚 API 명세서
### 📊 Swagger-Ui
* [API 테스트](http://localhost:8080/swagger-ui/index.html)  
### 게시글
* ```POST``` ```/api/boards``` 게시글 작성<br>
* ```PUT``` ```/api/boards/{boardId}``` 게시글 수정<br>
* ```DELETE``` ```/api/boards/{boardId}``` 게시글 조회<br>
* ```DELETE``` ```/api/boards/{boardId}``` 게시글 삭제<br>
* ```DELETE``` ```/api/boards/{boardId}``` 게시글 삭제<br>
### 댓글
* ```POST``` ```/api/boards/{boardId}/comments``` 댓글 작성<br>
* ```PUT``` ```/api/boards/{boardId}/comments/{commentID}``` 댓글 수정<br>
* ```DELETE``` ```/api/boards/{boardId}/comments/{commentID}``` 댓글 삭제<br>
### 회원 
* ```GET``` ```/api/user/{username}``` 유저 정보 조회
* ```PUT``` ```/api/user/{username}``` 유저 정보 수정
### 파일 업로드
* ```POST``` ```/api/files/``` 이미지 파일 업로드
* ```GET``` ```/api/files/{fileName}``` 이미지 파일 조회
* ```DELETE``` ```/api/files/{fileName}``` 이미지 파일 삭제
## 프로젝트를 진행하며...
### 구상
첫번째로는 주제를 정했다. 우리의 주제는 개발자들의 플레이리스트 공유 커뮤니티
그리고 필요한 도메인을 식별했다. 우선 게시글이 있어야하고 회원이 있어야하고 댓글이 있어야한다고 생각했다.
얼추 도매인을 식별하는게 끝나서 문서 작업을 시작했다. ERD다이어그램을 그려서 각 도메인간의 연관관계라던지 필드 등을 식별했고 와이어프레임을 그려 서비스의 흐름을 정했다.

필수 요구사항을 먼저 구현하기로했고 선택요구사항이지만 게시판의 필수요소인 댓글까지 구현하기로 결정되었다.
### 프로젝트 일정
현재 남아있는 시간을 분할해서 첫 날에 레포지토리와 서비스 둘째날에 컨트롤러와 예외처리를 하기로 했다. 계획대로라면 목요일날 끝나고 남은 기간동안 여유롭게 검토할 수 있을것이라 생각했다. 예상되는 일정보다 일정을 여유롭게 잡는 게 좋을 것 같다.

### 프로젝트중 문제
초반 설계를 좀 두루뭉실하게 했다. 왜냐면 프로젝트 구상단계에선 생각하지 못했던 변경사항이 개발중 나올 것이라고 생각해서였다. 그러나 이런 부분이 프로젝트 진행을 더디게 했던 것 같다. <br>
역할 분담을 도메인 단위로 했는데 이러면 한 다른 도메인으로 넘어와서 도와주거나 하기 힘들 것 같다. 자신의 도메인은 자신이 제일 잘 알지만 다른 도메인을 담당했던 분이 작업하시기엔 어려움이 있지 않을까 생각이 들었다.

### 프로젝트 진행 과정
1. 간단한 문서화작업을 했다.
2. 작업할 프로젝트를 원격에 업로드한다.
3. 각자 파트를 나누고 영속계층부터 올라가며 작업한다.
4. 프로젝트를 완료한 시점에서 생긴 문서의 수정사항을 처리한다.
5. 리드미를 작성한다.

### 프로젝트를 마치며 성장한점.
* JPA관련 에러들을 만나며 그 원인을 알아내기 위해서 JPA에 대해 공부했다.
* 일정을 중요성을 깨달았다.
* 파트 분배의 중요성을 깨달았다.
* 야근하면 안된다는 것을 깨달았다.
* 의사소통의 중요성을 깨달았다.

### 프로젝트중 고민한 것들
* 엔티티간의 연관관계가 다 연결되어있는 것은 아닐까? 지금은 엔티티의 수가 적지만 나중에 서비스를 확장하여 엔티티가 더 많아진다면 유지보수에 어려움이 있지 않을까
* 생각해보면 유저가 행동하는 주체이니 유저의 정보가 중요하겠지만 이렇게되면 유저가 사용자가 아니라 마치 서비스의 하나의 컨텐츠같은 느낌이 들었다. 게시물(유저)에 수많은 코멘트나 파일등이 붙어있는 형상
* 서로의 이해도가 다르다 어떻게하면 가장 최선의 결과를 낼 수 있을까




