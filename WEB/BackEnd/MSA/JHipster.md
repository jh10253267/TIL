# JHipster

## 개요

어떻게 MSA의 아키텍쳐를 구성하고 연결할 수 있을까?

**JHipster**는 **J**ava **Hipster**로 웹 애플리케이션과 마이크로서비스 아키텍쳐를 빠르게 적용, 개발, 배포할 수 있도록 도와주는 오픈소스 개발 플랫폼이다.

생성시 옵션으로 모노리스, 마이크로서비스, 게이트웨이를 선택할 수 있고 도커, JPA등 의 환경을 구축하고 바로 실행 가능한 애플리케이션을 만들어준다.또한 인증 처리 및 REST API를 이용한 통신을 지원한다.

JHipster는 설치가 간편하고 디렉터리를 생성한 후 몇가지 옵션만 선택하면 바로 실행 가능한 웹 애플리케이션을 만들어준다. 

## 환경 구축

1. 자바 11 설치
2. Node.js 설치
3. JHipster 설치

## 시작하기

JHipster를 이용하여 내외부 아키텍처 빛 개발 환경을 구축할 수 있다. 여기서 생성되는 레지스트리는 스프링 유레카와 스프링 클라우드 컨피그를 사용하여 레지스트리 및 컨피그 서비스를 제공한다.

게이트웨이는 줄 기반이며 사용자 관리 및 로그인 기능을 제공하는 프론트엔드 서비스도 통합해서 제공한다.

![microservices_architecture_detail 003](https://github.com/jh10253267/TIL/assets/108499717/07d349f2-f57f-4c9d-aa97-404fd71b5010)


JHipster로 마이크로 서비스 애플리케이션을 개발하는 순서는 이렇다.

1. 게이트웨이 생성
2. 레지스트리 생성
3. 마이크로 서비스 생성
4. 생성한 마이크로 서비스에 엔티티 생성
5. 생성된 엔티티를 게이트웨이가 인식할 수 있도록 게이트웨이에 등록

우선 게이트웨이와 레지스트리를 생성해보자.  
[JHipster](https://www.jhipster.tech/)  
위의 링크에서 4번까지 따라하면 된다.

```bash
What is the base name of your application? gateway
? Which *type* of application would you like to create? Gateway application
? Do you want to generate a feign client? No
? Besides Junit, which testing frameworks would you like to use? 
? As you are running in a microservice architecture, on which port would like 
your server to run? It should be unique to avoid port conflicts. 8080
? What is your default Java package name? com.mycompany.myapp
? Which service discovery server do you want to use? JHipster Registry (legacy, 
uses Eureka, provides Spring Cloud Config support)
? Which *type* of authentication would you like to use? JWT authentication 
(stateless, with a token)
? Which *type* of database would you like to use? SQL (H2, PostgreSQL, MySQL, 
MariaDB, Oracle, MSSQL)
? Which *production* database would you like to use? MySQL
? Which *development* database would you like to use? H2 with in-memory 
persistence
? Which cache do you want to use? (Spring cache abstraction) Hazelcast 
(distributed cache, for multiple nodes, supports rate-limiting for gateway 
applications)
? Do you want to use Hibernate 2nd level cache? Yes
? Would you like to use Maven or Gradle for building the backend? Gradle
? Which other technologies would you like to use? 
WARNING! Non reactive gateway is not supported. Switching to reactive.
? Which *Framework* would you like to use for the client? React
? Do you want to enable *microfrontends*? No
? Besides Jest/Vitest, which testing frameworks would you like to use? 
? Do you want to generate the admin UI? Yes
? Would you like to use a Bootswatch theme (https://bootswatch.com/)? Default 
JHipster
? Would you like to enable internationalization support? Yes
? Please choose the native language of the application Korean
? Please choose additional languages to install 
```

위에서 원하는 옵션을 선택하고 엔터를 누르다보면 지정한 디렉터리에 프로젝트가 생성되는 것을 볼 수 있다.

<img width="658" alt="스크린샷 2024-05-11 시간: 22 14 14" src="https://github.com/jh10253267/TIL/assets/108499717/775ad786-b63d-4d2f-a8bd-c676c0646fc4">

여기까지 성공하면 게이트웨이, 프론트엔드, 레지스트리가 모두 포함된 소스코드가 생성된 것이다.

이렇게 생성된 소스코드를 실행해보자.

레지스트리는 도커 컨테이너로 제공되기 때문에 도커 컴포즈로 실행하면 된다.
```bash
docker-compose -f src/main/docker/jhipster-registry.yml up 
```
실행하면 다음과 같이 로컬 주소가 출력된다.

```bash
gateway-jhipster-registry-1  | ----------------------------------------------------------
gateway-jhipster-registry-1  | 	Application 'jhipster-registry' is running! Access URLs:
gateway-jhipster-registry-1  | 	Local: 		http://localhost:8761/
gateway-jhipster-registry-1  | 	External: 	http://172.20.0.2:8761/
gateway-jhipster-registry-1  | 	Profile(s): 	[composite, dev, api-docs]
gateway-jhipster-registry-1  | ---------------------------------------------------------- 
```

접속해보면...

<img width="1438" alt="스크린샷 2024-05-11 시간: 22 19 24" src="https://github.com/jh10253267/TIL/assets/108499717/d8473ca2-2c8a-4438-a3bc-bb58a09104bf">

이렇게 부트스트랩으로 만든 듯한 UI가 나온다.

기본 아이디와 비밀번호는 둘 다 `admin`이다.

로그인을 하면 다음과 같은 화면이 나온다.
<img width="1414" alt="스크린샷 2024-05-11 시간: 22 22 51" src="https://github.com/jh10253267/TIL/assets/108499717/27ac2b49-7ed9-4d6f-9ec0-19369eb1406e">


이제 게이트웨이를 실행해보자.

위에서 실행시킨 레지스트리를 종료하지 말고 새로운 터미널을 열어서 gradle이라면 `./gradlew` 명령어를 통해 실행시킨다. 혹여나 인텔리제이나 IDE에서 직접 실행시키면 안된다.

성공적으로 실행이 되었다면 이렇게 접속 가능한 주소를 출력한다.
```bash
----------------------------------------------------------
        Application 'gateway' is running! Access URLs:
        Local:          http://localhost:8080/
        External:       http://127.0.0.1:8080/
        Profile(s):     [dev, api-docs]
----------------------------------------------------------
```



<img width="1426" alt="스크린샷 2024-05-11 시간: 23 29 37" src="https://github.com/jh10253267/TIL/assets/108499717/2312175c-e3fc-4977-8fe5-8fcd286db1a9">


우측 상단의 인증을 눌러서 레지스트리와 동일하게 `admin`으로 로그인한다.

이제 마이크로 서비스를 만들어볼 차례이다.

디렉터리를 만들고 처음에 게이트웨이를 생성했던 것처럼 똑같이 진행해준다.

포트번호와 패키지 설정을 다르게 해야하는 것에 유의한다.

게이트 웨이의 포트번호가 8080이니 8081, 8082... 정도로 설정해준다.

각 서비스에 맞게 설정을 해서 생성해줬으면 이제 각 서버를 실행시켜준다.  
그리고 레지스트리 ip인 `http://localhost:8761/`에 접속해주면 이렇게 생성한 서비스들이 올라와있는 것을 볼 수 있다.

<img width="1409" alt="스크린샷 2024-05-12 시간: 21 29 10" src="https://github.com/jh10253267/TIL/assets/108499717/6cbce106-c92b-4369-a30d-01d55615e10a">

여기까지 했으니 다음 단계는 엔티티를 생성하는 것이다.

엔티티를 생성하는 방법에는 2가지가 있다.

1. 터미널에서 명령어를 통해서 생성하는 방법
2. JHipster Online을 통해 생성하는 방법.

마이크로 서비스가 생성된 디렉토리 cd명령어를 통해 들어간다.

그 다음 아래의 명령어를 통해 엔티티를 생성한다.
```bash
jhipster entity 엔티티이름
```
 
이번에는 JHipster Online을 통해서 생성하는 방법을 알아보자.  
[JHipster Online](https://start.jhipster.tech/)  
위의 링크에 들어가서 왼쪽 하단의 JDL Stdio를 클릭하여 마치 ERD를 짜듯이 작성해주면 된다.

그런 다음 오른쪽 상단의 다운받기 버튼을 클릭하면 `jhipster-jdl.jdl`파일이 다운받아진다. 이 것을 마이크로 서비스 디렉토리의 내부에 옮긴 뒤 
`jhipster import-jdl jhipster-jdl.jdl` 명령어를 입력해준다.

그런 다음 이렇게 추가된 엔티티를 게이트웨이에 등록해준다.
```
cd 게이트웨이
jhipster entity 등록할 엔티티 이름
```

(만일 생성한 엔티티가 4개라면 jhipster entity 명령어를 4번 실행시켜 주는 것이다.)

이렇게 하고 백엔드 서비스를 다시 실행해준다.

도커 컴포즈 -> 게이트웨이 실행 -> 마이크로서비스 실행

여기까지 하고 게이트웨이 아이피로 접속하여 로그인한 뒤 오른쪽 상단의 entity를 선택하여 확인해본다.

JHipster에서 더미데이터를 생성해주고 CRUD작업이 가능한 상태로 제공해준다.

<img width="396" alt="스크린샷 2024-05-12 시간: 22 14 51" src="https://github.com/jh10253267/TIL/assets/108499717/8b699321-d839-4422-adb8-fc24cf8a8626">


이렇게 해서 MSA 아키텍쳐를 구성할 수 있다.

