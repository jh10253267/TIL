# onreadystatechange

## 개요

(이 메소드는 왜 카멜케이스가 적용이 안되어있을까 잘못 적은 것 같은 느낌에 불편하다.)

이 메소드는 XMLHttpRequest 객체의 현재 상태를 나타낸다.

이전에 작성한 기본 Ajax함수 구조는 다음과 같다.

```js
function sendAjax(url) {
  let oReq = new XMLHttpRequest();
  oReq.addEventListener("load", function() {
    // 로직 작성
  });
  oReq.open('GET', url);
  oReq.send() 
}
```

## onreadystatechange 프로퍼티

onreadystatechange 프로퍼티는 XMLHttpRequest 객체의 readyState 프로퍼티 값이 변할 때마다 자동으로 호출되는 함수를 설정할 수 있다.


readyState의 값은 다음과 같다.
0 : XMLHttpRequest 객체가 생성됨.
1 : 서버와 연결됨.
2 : 요청 수신
3 : 요청한 데이터를 처리중.
4 : 요청이 보내졌고 응답할 준비가 완료됨.

 이를 이용하면 서버로부터 응답이 도착하는 순간을 특정할 수 있다.