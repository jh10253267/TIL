# GraphQL


> A query Language for your API


##  GraphQL이란?

> A query Language for your API

데이터를 묘사하고

클라이언트에서 필요한 데이터를 받아오고

핀터레스트 페이스북

REST의 부족함을 보완하기 위해 나왔다.

프론트엔드 개발자는 백엔드 개발자가 REST API개발을 마칠때까지 기다리지 않아도 된다.
ㄴ> 병렬로 작업 가능(합리적)

오버패치와 언더패치를 막아준다.

한 번에 데이터를 가져올 수 있다.

유튜브 비디오를 읽어온다고 하면 여러 정보가 필요하다.

* 유튜브 기본 비디오 정보

* 해당 비디오 채널 정보

* 해당 비디오 댓글 정보


오버 패치는 필요한 데이터 이외의 데이터까지 받아와야하는 경우를 말하고 언더 패칭은 덜 가져와서 요청을 추가적으로 보내야하는 경우를 말한다.

예를 들어 스타워즈 api를 호출하는 경우 다음과 같이 원하는 정보만 가져올 수 있다.

{
  startship (starshipId : 9) {
    name
    model
    length
    crew
  }
}