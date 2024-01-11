# Swagger 설정

## 개요

스프링 3.X 버전으로 넘어오면서 springfox가 아닌 springdoc을 사용하게 되었다.

의존성도 달라졌는데
 `implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.3.0'`

 위의 의존성을 사용한다.

## SwaggerConfig

https://springdoc.org/

스웨거의 설정 방법도 많이 달라졌는데 GroupedApi를 사용하여 경로들을 묶어서 명세화 시킬 수 있다. 이렇게 작성하고 swagger에 들어가보면 우측 상단에 내가 설정한대로 명세가 만들어지고 다른 페이지를 볼 수 있다.

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

여기까지가 기본 설정이다.

## 자주쓰는 어노테이션

`@Operation(summary = "")`

컨트롤러의 메소드에 선언하여 메소드에 대한 설명을 적어놓을 수 있다.



