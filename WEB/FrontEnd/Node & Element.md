# Node & Element

## 개요

자바스크립트는 html을 동적으로 만들기 위한 요소이다.

일반적인 방시은 html 태그를 선택해 어떤 이벤트가 발생했을 때 함수를 실행시키는 방식이다.

## Node

`Node`란 HTML 요소, 텍스트, 주석 등 모든 것을 의미한다.


```javascript
const element = document.querySelector("h1");
console.log(element.textContent);
```

위의 코드는 h1태그를 선택하고 h1태그의 텍스트를 출력한다.

이렇게 자바스크립트를 통해서 html을 조작한다.

이러한 명령들을 `DOM Api`라고 부른다.

```javascript
const parent = document.querySelector(".parent);

console.log(parent.childNodes);
console.log(parent.children);

console.dir(parent); // 객체 데이터로 출력하기 위함.
```

Element라는 것은 Node라는 객체에서 상속되어 만들어진 개념이다.











