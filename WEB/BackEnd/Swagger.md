# Swagger

## 개요

Web API를 개발하고 API의 사용자에게 공개하기 위해선 문서화가 필요하다.

API의 사용자는 같은 구성원중에는 API서버의 스펙을 보고 개발하는 프론트 개발자가 있을 수 있고 OpenAPI로 무언가 유용한 정보를 제공할 땐 일반 사용자가 될 수 있다.

스웨거는 대표적인 Web API 문서화를 위한 도구이다.

포스트맨을 사용해봤다면 알고있겠지만 조금은 힘든 작업이다. 직접 경로를 정의하고 셋팅하고 문서화한다.

만약 API 문서를 주로 사용하는 것이 프론트 개발자라면 프론트 개발자는 Web API가 만들어질 때까지 작업이 지연된다.

또한 서버의 스펙이 바뀌었을 때 이들 문서도 역시 변경이 되어야하는데 이를 유지하는 것이 쉬운일이 아닐 것이다.

스웨거는 이런 부분에 있어서 큰 이점을 가져다주는데 바로 스펙이 변경되었다고 해도 개발자가 직접 수정할 필요없이 문서가 자동으로 갱신되고 스펙이 변경되는 경우 말고도 개발자가 작성한 API가 즉시 등록된다는 점이다.

## 사용 방법

스프링 3.X 버전으로 넘어오면서 springfox가 아닌 springdoc을 사용하게 되었다.

```
implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.3.0
```

이렇게 의존성을 추가해주는 것만으로도 자동으로 명세화를 해준다.

그러나 디테일한 부분들을 커스텀하려면 별도의 설정 클래스를 정의해야한다.

설정 방법은 크게 두가지인데

1. 여러 API문서가 필요한 경우
2. 하나의 API문서만 필요한 경우

1번은 `GroupedOpenApi`를 사용하고  2번은 `OpenAPI`를 사용한다.

1번 설정 방법은 여러 API 버전이 필요한 경우 페이지를 나누어서 문서화 할 수 있다.


얘를 들어 MVC경로들을 명세화하고 별도로 REST API 경로들도 명세화하고싶다면 1번 `GroupedOpenApi`를 사용하면 된다.

그러나 사용해본 경험상 1번 방법이 좀 더 명확하다고 생각이 들어서 1번만 우선 정리하려한다.

```java
// 실행했을 때 타이틀과 설명을 추가하는 부분
@OpenAPIDefinition(
        info = @Info(title = " Board App",
                description = " Board App Api명세",
                version = "v1"))

// 시큐리티 헤더 설정
@SecurityScheme(
        name = "Bearer Authentication",
        type = SecuritySchemeType.HTTP,
        bearerFormat = "JWT",
        scheme = "bearer"
)

// 인증이 필요한 컨트롤러 혹은 메소드에 밑의 애노테이션을 붙여준다.
// @SecurityRequirement(name = "Bearer Authentication")
// 이 애노테이션이 붙은 경로는 자물쇠 표시가 생김.
// 이걸 붙여주지 않으면 토큰이 안들어간다.

@Configuration
public class SwaggerConfig {
    // GroupedOpenApi를 사용하면 경로를 그룹화해서 여러개의 Swagger 페이지를 만들 수 있다.
    // 예를 들어 rest api 명세와 mvc api 명세 스웨거를 둘 다 만들 수 있다.
    @Bean
    public GroupedOpenApi restOpenApi() {
        String[] paths = {"/api/**"};

        return GroupedOpenApi.builder()
                .group("Kanban Board API v1")
                .pathsToMatch(paths) // 어떤 경로들을 명세화 시킬지
                .build();
    }
    @Bean
    public GroupedOpenApi commonOpenApi() {

        return GroupedOpenApi.builder()
                .group("Common Board API v1")
                .pathsToMatch("/**/*") // 어떤 경로들을 명세화 시킬지
                .pathsToExclude("/api/**/*") //어떤 경로들을 제외시킬지
                .build();
    }
}
```
기본 셋팅은 이렇다.

자세한 내용은 공식 문서의 8번 목차를 보면 된다.

[Swagger 공식 문서](https://springdoc.org/#migrating-from-springfox)

## 어노테이션 정리

간단한 어노테이션을 설정해주면 스웨거 페이지를 꾸밀 수 있다.

그리고 각 컨트롤러에 주석을 남기는 것보다 어노테이션을 붙여서 메소드의 기능을 명시할 수 있으니 여러모로 좋다.

2점대 버전과 3점대 버전 어노테이션의 이름이 바뀐 부분이 있어서 나중에 보려고 기록해둔다.

큰 분류부터 내려가본다.

### @Tag

스웨거는 각 api경로들을 패키지별로 나눈다. 프로젝트를 도메인 별로 나누어 관리한다고하면 하나의 도메인은 스웨거에서 하나로 묶인다.

이럴 때 컨트롤러의 상단에 @Tag 어노테이션을 붙여서 해당 도메인에 대한 설명을 적을 수 있다.
@Tag의 속성은 name과 description이 있다.
```
@Tag(name = "게시글", description = "게시글 api")
```
이와 같이 사용할 수 있다.

### ApiResponse

응답 결과에 따른 response구조를 확인할 수 있는 애노테이션이다.

* `responseCode` : 상태코드
* `description` : response에 대한 설명
* `content` : 페이로드 구조
  * `schema` : 페이로드에서 사용하는 스키마
    * `hidden` : 스키마 숨김 여부
    * `implementation` : 스키마 대상 클래스

이렇게 작성하면 요청에 성공했을 경우 기대 결과의 구조와 실패했을 경우의 기대 결과를 정의할 수 있다.

### @Parameter

컨트롤러의 인자로 들어가는 파라미터의 설명을 적을 수 있다.

변수 명으로 그 변수의 의도를 알 수 있다면 좋겠지만 그렇지 않은 경우도 많을 것이다. 이럴 때 유용하게 사용할 수 있다.

### @Content

특정 미디어 타입이나 미디어 타입의 목록을 설명하는 애노테이션이다.

### @Schema

모델(컨트롤러에서 인자로 사용하는 DTO클래스)에 대한 정보를 작성하는 애노테이션이다.

### @Operation

컨트롤러의 각 메소드에 붙이는 어노테이션이다.
속성은 `summary`, `description`, `response`, `parameters`이 있는데 사용해본 것은 앞의 두 개 뿐이다.
나중에 스웨거에 힘 좀 주고싶을 때 사용하려 한다.

`summary`는 스웨거의 각 api항목을 펼치지 않았을 때 보이는 부분이다.

`description`은 스웨거의 api항목을 펼쳤을 때 보이는 부분이다.



