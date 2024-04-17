# Regular Expression

## 개요

 정규표현식 혹은 정규식은 프로그래밍에서 문자열을 다룰 때 패턴을 표현하는 일종의 형식언어이다.

정규식을 이용하면 복잡하게 메소드를 사용하여 찾아내지 않아도 찾길 원하는 패턴을 가진 문자열을 쉽게 찾아낼 수 있다.

형식에 맞지 않는 입력값들을 걸러낼 수도 있다. 우리가 어느 사이트에 회원가입을 할 때 사이트에서 규정한 형식과 맞지 않으면 회원가입이 되지 않는다. 이런 부분에서 정규식을 유용하게 사용될 수 있다.

## 사용법

### 생성법

1. 생성자
2. 리터럴

생성자 방식은 자바스크립트에서 객체를 생성하듯이 사용한다.

`new RegExp('표현', '옵션');`
`new RegExp('[a-z]', 'gi');`

리터럴 방식은 다음과 같이 사용한다.
일반적으로 자바스크립트의 한 줄 주석은 // 슬래시 두 개를 사용한다.

이 사이에 정규표현식이 들어간다.

예를 들면 다음과 같다.

`/표현/옵션`  
`/[a-z]/gi`  

## 사용법
```js
cibst str - 'The quick brown fox jumps over the lazy dog.';

const regexp = new RegExp('the', '');  
console.log(str.match(regexp));
```
이렇게 하면 가장 처음 등장하는 단어를 찾아서 반환한다.  
옵션으로 `g`를 사용하면 해당하는 단어를 모두 찾고 `i`를 추가하면 소문자뿐만 아니라 대문자도 찾게된다.

```js
cibst str - 'The quick brown fox jumps over the lazy dog.';

const regexp = /the/gi;
console.log(str.match(regexp));
```

개요는 이렇다.

실제 사용할 경우에는 주로 validation에 사용하는 경우가 많을 것이다.

이럴 때 사용할 수 있는 메소드는 `test()`이다.

예를 들어 `regexp.text(str)`과 같이 사용할 수 있다.

그리고 해당하는 문자를 찾아 다른 문자로 변환할 수도 있다.

`str.replace(regexp, 'cat')`과 같은 식이다.


## 옵션

사용할 수 있는 옵션은 g, i, m이 있다.

g는 global의 약자로 모든 문자 일치를 적용할 수 있다.  
i는 Ignore case라는 의미로 영어 대소문자를 구분하지 않고 검색한다.  
m은 multi line이라는 의미로 각자의 줄을 시작과 끝으로 인식한다.

## 예제

간단한 예로 설명하자면 아래와 같다.
```javascript
var result = "113ABC".match(/\d/)[0];//첫번째로 나오는 숫자 하나를 찾는다.
var result = "12345".match(/\d{5}/)[0];//5자 우편번호 찾기.
var result = "123-123".match(/\d{3}-\d{3}/)[0];//구 우편번호 찾기
var result = "010-1111-2222".match(/\d{3}-\d{4}-\d{4}/)[0];//전화번호 찾기.
```