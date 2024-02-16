# CORS

## 개요

CORS는 Cross-Origin Request Sharing의 약자로 웹 브라우저는 보안상 SOP(same-origin-policy)라는 정책을 사용한다.

이는 같은 출처가 아니라면 요청과 응답을 차단하는 정책이다.

같은 출처라는 것은 url, 프로토콜, 포트를 포함하는 개념으로 이중 하나라도 다르면 요청과 응답을 받을 수 없다.

과거에는 프론트와 백이 같은 서버에서 구성되었다. 그러니 다른 출처로 요청을 보내는 것이 비정상적인 행동이었다. 그러나 최근 프론트 서버에서 백의 API서버를 호출하여 그 결과를 프론트에서 처리하는 방식이 많아지고 이를 위해 SOP에 예외를 만든 것이 바로 CORS이다.

## CORS 다루기

Ajax의 SOP를 해결하는 방법에는 여러가지 방법이 있는데 브라우저에서 서버를 직접 호출하지 않고 서버 내의 다른 프로그램을 이용해서 API를 호출하는 프록시 패턴을 이용하거나 JSON이 아닌 순수한 JS파일을 요청하는 방식등이 있다.

그러나 CORS가 SOP에서 예외를 설정하여 허용하는 정책이니 만큼 CORS 설정을 이용해 예외를 등록하는 것이 좋다.


