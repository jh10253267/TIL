# Event

## 개요

자바스크립트는 웹을 동적으로 만들어주는 언어라고 배운 적이 있을 것이다.

 어떤 부분을 클릭하면 글씨가 커지고 애니메이션이 동작하는등.. 이러한 동작들이 일어나는 조건을 이벤트라고 한다. 클릭 이벤트, 마우스 오버 이벤트 등등...

그리고 이러한 이벤트를 등록하고 실행될 함수를 정의하여 동적인 만드는 방식으로 사용한다.

## 이벤트를 거는 방법

요소를 선택하고 이벤트를 추가해준다.

요소를 선택하고 -> querySelector();

이벤트를 등록한다. -> addEventListener("이벤트", 콜백함수);

대표적으로 click이벤트를 사용할 때가 많다. 탭을 클릭하면(이벤트 발생) Ajax요청을 보내고 응답받은 데이터로 대체시킨다.(콜백 함수 실행)

```js
querySelector(".btn").addEventListener("click" , 콜백 함수);
```

이벤트 리스너를 "이벤트를 듣고있는 자"라고 번역하면 딱 맞겠다. 그래서 이를 이벤트 청취라고 부른다.

## 기본 동작

브라우저에서 제공하는 기본 동작들이 있다. 예를 들어 스크롤이라던지 클릭이라던지 a태그를 클릭하면 설정해둔 링크로 이동한다던지 이러한 것들이 기본동작에 해당된다.

웹사이트의 회원가입 창을 떠올려봤을 때 양식에 맞지 않는 값을 입력하고 제출을 누르면 내가 작성한 form데이터가 제출되지 않고 양식이 잘못되었다는 사인을 준다.

 이러한 작업을 유효성 검사라고 하는데 직접 구현을 해보려고 파일을 작성하고 input태그의 타입을 submit이라고 설정한 뒤 눌러보면 기본 동작으로 인해 유효성 검사를 할 틈도 없이 바로 제출이 된다. 이럴 때 필요한 개념이 바로 이벤트 방지이다.

addEventListener의 콜백 함수에 event객체를 전달하고 그 내부에서 event.preventDefault()를 적어주면 된다.

## 버블링과 캡쳐링

### 버블링

ul는 리스트를 나열하는 대표적인 태그이다. ul 내부엔 li태그들이 있을 것이다.

이 때 ul태그에 이벤트 리스너 클릭 이벤트를 걸고 li태그에도 동일하게 이벤트를 걸어본다.

그리고 나서 ul태그를 클릭하면 ul에 정의해둔 함수 뿐만 아니라 li에 정의해둔 함수도 실행이 되는 것을 볼 수 있다. 이는 li태그가 ul 내부에 속해있기 때문인데

가장 하위의 이벤트부터 가장 상위의 이벤트 순서로 실행된다.
이러한 현상을 이벤트 버블링이라고 부른다.(거품이 뽀글뽀글 생기는 이미지와 같다.)

클릭한 지점이 하위 엘리먼트라고 해도 그 것을 감싸고 있는 상위 엘리먼트까지 올라가면서 이벤트 리스너를 찾는 것이다.

만약 이러한 버블링을 막아야하는 경우가 있다고 하면 `event.stopPropagation()` 속성을 사용할 수 있다.

이렇게 하면 하위 요소의 이벤트가 발생하고 부모요소로 전파되는 것을 막을 수 있다.

### 캡쳐링

캡쳐링은 다음과 같은 형식으로 사용한다.

```js
document.body.addEventListener('click', function() {
  console.log("body");
}), { capture: true};
```

이런식으로 속성을 추가해주면 이벤트 버블링이 일어날 때 일반적으로 하위요소에서부터 상위요소로 전파되는 방식에서 캡쳐링 속성이 존재하는 이벤트가 먼저 동작되게된다.

만약 상위 요소 여러개에 캡쳐링 속성을 걸어주면 어떻게 동작할까?

바로 상위 요소의 순서부터 동작한다. 예를 들어 window, body, ul, li, a에 이벤트를 걸어주고 window, body, ul에 캡쳐링 속성을 선언해주면 window, body, ul 순서로 이벤트가 실행된다.

캡쳐링을 사용할 때 주의할 점이 하나 있다.

```js
const parentEl = document.querySelector('.parent')

const handler = () => {
  console.log('Parent!')
}

parentEl.addEventListener('click', handler, {
  capture: true
})
parentEl.removeEventListener('click', handler)
```

이렇게 작성하고 실행을 시켜보면 의도한대로 이벤트가 제거되지 않는 걸 발견할 수 있다.

그래서 이벤트 캡쳐옵션을 사용해서 이벤트를 등록했다면 삭제할 때도 같은 속성을 추가해줘야한다.

## 사용할 수 있는 옵션

addEventListener를 사용할 때 사용할 수 있는 옵션들이 있다.

하나씩 알아보자.


### once

```js
const parentEl = document.querySelector('.parent')

parentEl.addEventListener('click', {
  console.log('Parent!')
}, {
  once: true
})
```

위의 코드를 실행시켜보면 여러번 클릭을 해도 이벤트가 한 번만 실행된다.

만약 어떤 이벤트가 딱 한 번만 실행되길 의도했다면 위의 속성을 사용해볼 수 있을 것이다. 예를 들어 form을 제출할 때 태그 이벤트를 막고 네트워크 요청 시간동안 여러 번의 제출을 막기위해서 사용할 수 있겠다.

### passive

요소의 기본 동작과 핸들러 실행 분리

```js
const parentEl = document.querySelector('.parent')

parentEl.addEventListener('wheel', {
  // console.log('Parent')

  for(let i = 0; i <10000; i += 1) {
    console.log(i)
  }
})
```

위의 코드는 기본 이벤트인 wheel과 wheel이벤트가 일어날 때 실행될 함수를 정의하고있다.

스크롤이 일어날 때마다 0부터 10,000까지 숫자를 증가시키는 함수가 들어가있다. 이를 실행해보면 기본 스크롤이 버벅거리는 문제가 생긴다. 이벤트에 따라 처리해야하는 정보가 많아졌기 때문인데 이럴 때 사용할 수 있는 속성이 passive이다.

이렇게 기본동작과 실행 로직을 분리시켜서 동작하게 하는 것이다.

```js
const parentEl = document.querySelector('.parent')

parentEl.addEventListener('wheel', {
  for(let i = 0; i <10000; i += 1) {
    console.log(i)
  }
}, {
  passive: true
})
```