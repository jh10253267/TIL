# REST API
 클라이언트의 종류가 웹 브라우저, 안드로이드 앱, ios등 다양해지면서 클라이언트들에게 정보를 제공하는 방식을 일원화시키고 싶어졌다. 그 대표적인 방법이 HTTP프로토콜로 API를 제공하는 것이다. 이를 REST API라고 한다.

## API란?
 응용 프로그램에서 사용할 수 있도록 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.<br>
 이를 통해 우리는 구현 코드를 알지 못해도 인터페이스만 알면 사용할 수 있는 것이다.

## REST API란?

### REST
 REST는 REpresentational State Transfer라는 용어의 약자로 2000년도에 로이 필딩 (Roy Fielding)의 박사학위 논문인 "Archithectural Styles and the Design of Network-based Software Architectures" 에서 최초로 소개되었다.
 분산 하이퍼 미디어 시스템을 위한 아키텍쳐 스타일인 것이다.

### REST + API
 REST API란 말 그대로 REST형식의 API를 말한다.
 REST API란 핵심 컨텐츠 및 기능을 외부 사이트에서 활용할 수 있도록 제공되는 인터페이스다.
 예를 들어, 네이버에서 블로그에 글을 저장하거나, 글 목록을 읽어갈 수 있도록 외부에 기능을 제공하거나 택베 송장번호를 조회하거나 미세먼지 농도를 제공하는 것들을 REST API라 부른다.

 다양한 클라이언트들이 등장하면서 그러한 클라이언트들에게 대응하기 위해 REST API가 널리 쓰이게 됐다.<br>
 서비스 업체들이 다양한 REST API를 제공하면서 클라이언트는 이러한 REST API들을 조합하여 새로운 웹을 만들 수도 있는데 이런 방식을 매시업이라고 한다.

### 대부분의 REST API는 Restful하지 않다
 REST API라는 명칭이 널리 쓰이고 있지만 REST를 논문으로 최초 소개한 로이필딩은 대부분의 REST API라고 하는 것들이 REST API가 아니라고 말한다. 그 이유는
 REST API는 반드시 다음과 같은 스타일을 지켜야한다고 말한다.
 1. client-server
 2. stateless
 3. cache
 4. uniform interface
 5. layered system
 6. code-on-demand (optional)

 여기서 스타일은 제약조건의 집합을 의미한다. 
 즉 위의 내용을 지켜야만 REST라고 할 수 있다는 것이다.  
 웹이라면 각각의 항목을 쉽게 지킬 수 있다.

 그렇다면 API는?<br>
 클라이언트 서버 모델을 따르고있고 statelsee하다 그리고 캐시를 사용하며 계층화된 시스템을 사용하고 있다.
  
 그러나 4번의 uniform interface가 문제가 된다. 

 uniform interface의 스타일을 살펴보면 다음과 같다.

 * 리소스가 URI로 식별되야 한다.
 * 리소스를 생성,수정,추가하고자 할 때 HTTP메시지에 표현을 해서 전송해야 한다.
 * 메시지는 스스로 설명할 수 있어야 한다.
 (Self-descriptive message)
 * 애플리케이션의 상태는 Hyperlink를 이용해 전이되야 한다.(HATEOAS)
  Hypermedia As The Engine Of Application State

 첫번째와 두번째는 지키기 쉽지만 세번째와 네번째를 지키기가 API로는 쉽지가 않다.
 그 이유는 응답 결과에 보통 JSON메시지를 사용하게 되는데 이 메세지가 어디로 전달이 되는지 그리고 이 메시지를 구성하는 것이 어떤 의미를 표현해야만 메시지 스스로 설명할 수 있다고 할 수 있는데 그게 쉽지 않은 부분이다. 연관된 웹페이지들 끼리는 하이퍼링크를 통해 연결되어 있다. 이를 HATEOAS라고 한다. 이런 부분을 JSON 응답메시지를 가지는 API가 제공하는 것은 쉽지 않다.<br><br>
 REST의 uniform interface를 지원하는 것은 쉽지 않기 때문에, 결국 많은 서비스가 REST에서 바라는 것을 모두 지원하지 않고 API를 만들게 된다. 그 방식은 두 가지가 있는데

 * REST의 모든 것을 제공하지 않고 REST API라고 말하는 경우가 있다.
 * REST의 모든 것을 제공하지 않고 Web API 혹은 HTTP API라고 부르는 경우가 있다.

 보통은 첫번째 방식으로 사용하고 있다.

## REST API 이대로 괜찮은가?

 IT 업계에선 비슷하지만 다른 다양한 용어가 있다. 그 말은 즉 세세하게 여러 부분으로 쪼갰다는 의미이다.<br>용어가 명확히 구분되어 쓰이지 않는다면 소통에 문제가 생기고 이 문제는 점차 큰 단위로 퍼져나갈 수 있다.<br><br> REST를 주장한 로이 필딩은 강연등을 다니며 REST의 형식을 지키지 않았다면 REST API라는 명칭을 쓰지 말아달라며 호소하고 있다고 한다. REST는 우리가 알고있는 것보다 훨씬 까다롭고 복잡했던것이다.

 REST에 대해 더 깊이 이해해보자 REST가 무엇을 위한 것인지부터 생각해봐야한다. 어떤 맥락에서 나온 개념이고 어떤 의도였는지 살펴보려한다.<br><br>
 우선, 웹의 역사를 한 번 따라가보자.
 팀 버너스리의 WEB(1991)은 어떻게 인터넷에서 정보를 공유할 것인가에 대한 해답이였다. 팀 버너스리는 정보들을 하이퍼텍스트로 연결해서 정보를 공유하는 방식을 제안했고 표현 형식으로 HTML, 정보들의 식별자로 URL, 그리고 전송 방법으로 HTTP을 고안했다.

 로이 필딩은 1994년~1996년 HTTP/1.0의 개발에 참여하고 있었고 웹은 급속도로 성장하고 있던 시점이었다. 그러던 중 고민이 생겼다. 로이 필딩이 HTTP에 기능을 더하고 수정한다면 이미 전 세계적으로 쓰이고 있는 HTTP와 기존의 웹 페이지들과의 호환성 문제를 피하기 어렵다고 느꼈다. 그래서 로이 필딩이 한 고민은 이렇다.
 > How do I improve HTTP without breaking the Web?"

 그가 내놓은 해답은 HTTP Object Model. 이게 4년 후 REST라는 이름으로 세상에 알려지게 된다.

 왜 이런 스타일들이 필요할까?<br>
  ㄴ> 독립적 진화를 하기 위해서<br>
  <br>
 클라이언트와 서버가 독립적으로 진화한다.<br>
 ㄴ> 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다.
 <br>ㄴ> REST를 만들게 된 계기.

 ### 우리 곁에 있는 REST
 우리가 사용하는 웹은 웹 페이지가 변경되었다고 해도 웹 브라우저(크롬, 사파리 등등)을 업데이트 할 필요는 없다.
 웹 브라우저의 버전이 달라졌어도 웹 페이지를 접속하지 못하는 경우는 없다.
 이게 가능한 이유는? 각 분야 전문가들의 집착에 가까운 노력.
 * HTTP명세가 변경되어도 문제없다 HTTP2.0이 나왔는데도   웹을 사용하는데 문제가 없다.
 * HTML5.1이 나왔어도 웹은 잘 동작한다.<br>
 * HTTP referer 오타 정확한 철자는 Referrer이다.
  그러나 하위 호환성 때문에 고치지 못한다. 25년전 오타. 고치는 순간 웹이 깨진다.
 * charset 잘못지은 이름. 원래 인코딩으로 지어야 적절하다.
 * HTTP 상태코드 418번 개발 포기. 만우절 때 만든 이벤트성 스펙의 HTCPC(Hyper Text Coffee Pot Control) 상태코드에 418 I'm a teapot을 설정했고 코드의 의미는 "(커피포트가 아니라) 찻주전자라서 커피 끓이기 불가" 이 걸 다른 곳에서 http상태코드로 구현을 해버리는 바람에. 영구 결번으로 남아있다.
 * HTTP/0.9 아직도 지원함. 크롬에서 이제 해당 버전의 지원을 하지 않아도 괜찮지 않을까 해서 검토를 해본적이 있었다고 한다. 그러나 지원을 안했더니 일부 웹 기능지원이 안된다는 것을 발견했고 웹을 깨트릴 순 없기에 아직까지 HTTP/0.9를 지원히고 있다. 전문가들의 숨은 노력이 없다면 웹이 깨진다.
 <br><br>
 이제 우리는 REST가 나온 맥락과 그 의미를 알았다.<br>
 REST는 웹의 독립적 진화를 위한 개념이다.<br>
 그렇다면 REST가 웹의 독립적 진화에 실질적인 도움을 주었을까?<br><br>
 * http에 지속적으로 영향을 줌
 * host헤더 추가 
 * 길이 제한 
 * url에서 리소스의 정의가 추상적으로 변경됨.
 * 기타 여러가지 영향<br>

 ***즉, REST는 성공했다.***

그럼 다시 돌아와서 우리의 관심사인 REST API는?
우리의 API도 반드시 이러한 까다로운 제약조건들을 다 지켜야하는걸까?<br>

>An API that provides network-based access to resources via a uniform interface of self-descriptive messages containing hypertext to indicate potention state transitions might be part of an overall system tha is a RESTful Applicaiton - Roy T.Fielding

그렇다 REST API는 반드시 Restful해야한다.<br><br>
그렇다면 Web API는 반드시 Restful해야하는가?<br>

> REST emphasizes evolvability to sustain an uncontrollable system,. if you think you have control over the system or aren't interested in evolvability, don't wast your time arguing about REST. - Roy T.Fielding


그건 아니라고한다.

그럼 우린 선택을 해야한다. <br>
* REST를 따르고 REST API라 부른다.
* REST를 따르지 않고 Web API, HTTP API로 부른다.
* REST를 따르지 않고 REST API로 부른다.

여기서는 REST API를 직접 구현해보려한다.

응답 메시지의 content-Type을 보고 media type이 application/json임을 확인한다.<br>
명세에 midia type은 IANA에 등록되어있다고 하므로 IANA에서 applicaiton/json의 설명을 찾는다. 명세에는 json문서를 파싱하는 방법이 명시되어 있으므로 파싱이 가능하다. 그러나 여전히 id가 무엇을 의미하고 title이 무엇을 의미하는지 알 방법은 없다.<br>
ㄴ> self-descriptive하지 못하다.<br>
다음 상태로 전이할 링크가 없다. 
<br>ㄴ> HATEOAS를 만족하지 못한다.

이것들이 독립적인 진화에 어떻게 도움이 될까
* self-descriptive : 확장 가능한 커뮤니케이션이    가능하게한다.
서버나 클라이언트가 변하더라도
언제나 메시지만 가지고 해석이 가능하다.
* HATEOAS : 서버가 링크를 바꾼다고해도 기능에는 문제가 없다. 어떤 링크를 따라 그 페이지에 가고 다음 전이될 수 있는 상태가 결정된다. 서버가 링크를 동적으로 바꿀 수 있다.
* 해결책
  * 미디어 타입을 하나 정의한다. 문서를 작성해 각 항목들이 뭘 의미하는 지 명시한다. 그리고 IANA에 미디어타입을 등록한다. 이제 이 메시지를 보는 사람은 명세를 찾아갈 수 있으므로 메시지의 의미를 온전히 해석할 수 있게 된다.
  * 프로필을 이용한다.
   링크에 링크를 걸 수 있다. 명세를 작성한다. 메시지를 보면 명세를 찾아갈 수 있으므로 문서의 의미를 온전히 해석할 수 있다.
  * HATEOAS 데이터에 다양한 방법으로 하이퍼링크를 표현한다.
        헤더에 링크를 표현한다.
Web API에선 위와 같은 방법으로 REST를 달성할 수 있다.

정리를 하자면
* REST는 웹과 클라이언트가 독립적으로 진화하기 위함이다.
* REST API는 반드시 REST를 제공해야한다.
* 모든 Web API들이 Restful할 필요는 없다.
* REST를 따르지 않겠다면 REST를 만족하지 않는 API를 뭐라고 부를지 결정해야 할 것이다.
* Web API? HTTP API? 아니면 그냥 여전히 REST API?
