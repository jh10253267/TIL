# Regular Expression

 정규식은 프로그래밍에서 문자열을 다룰 때 패턴을 표현하는 일종의 형식언어이다.
정규식을 이용하면 복잡하게 메소드를 사용하여 찾아내지 않아도 찾길 원하는 패턴을 가진 문자열을 쉽게 찾아낼 수 있다.<br>
형식에 맞지 않는 입력값들을 걸러낼 수도 있다. 우리가 어느 사이트에 회원가입을 할 때 사이트에서 규정한 형식과 맞지 않으면 회원가입이 되지 않는다. 이런 부분에서 정규식을 유용하게 사용될 수 있다.

간단한 예로 설명하자면 아래와 같다.
```javascript
var result = "113ABC".match(/\d/)[0];//첫번째로 나오는 숫자 하나를 찾는다.
var result = "12345".match(/\d{5}/)[0];//5자 우편번호 찾기.
var result = "123-123".match(/\d{3}-\d{3}/)[0];//구 우편번호 찾기
var result = "010-1111-2222".match(/\d{3}-\d{4}-\d{4}/)[0];//전화번호 찾기.
```