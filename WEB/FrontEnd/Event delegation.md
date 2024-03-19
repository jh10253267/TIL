# Event delegation

## 개요

당신이 프론트 페이지를 만들면서 탭 메뉴를 구성하려 한다고 생각해보자. 탭을 클릭하면 정해둔 로직이 실행된다.

이벤트 리스너 -> 함수의 간단한 흐름이다. 그러나 이내 고민에 빠질 것이다. 

이벤트 리스너는 어떤 요소를 선택하여 어떠한 이벤트가 일어났을 경우 어떠한 함수를 실행시키는 것인데 메뉴마다 이벤트 리스너를 걸자니 로직이 중복되고 감각적으로 이게 좋은 방법이 아니라는 생각이 들 것이다.

## 이벤트 버블링

ul는 리스트를 나열하는 대표적인 태그이다. ul 내부엔 li태그들이 있을 것이다.

이 때 ul태그에 이벤트 리스너 클릭 이벤트를 걸고 li태그에도 동일하게 이벤트를 걸어본다.

그리고 나서 ul태그를 클릭하면 ul에 정의해둔 함수 뿐만 아니라 li에 정의해둔 함수도 실행이 되는 것을 볼 수 있다. 이는 li태그가 ul 내부에 속해있기 때문인데

가장 하위의 이벤트부터 가장 상위의 이벤트 순서로 실행된다.
이러한 현상을 이벤트 버블링이라고 부른다.(거품이 생기는 이미지)

클릭한 지점이 하위 엘리먼트라고 해도 그 것을 감싸고 있는 상위 엘리먼트까지 올라가면서 이벤트 리스너를 찾는 것이다.

여기서는 다루지 않겠지만 propagation이라는 개념도 있다. 버블링을 막아주는 속성이다.

## target

탭 버튼을 구현할 때 클릭 이벤트 리스너 함수의 인자로 event를 정의해주고 이를 사용할 수 있다. 많이 사용하는 속성은 event.target.tagName이다. 이는 유저가 클릭한 타겟의 태그 이름을 반환한다.

예를 들어 ul태그 안에 여러개의 li태그와 하위 엘리먼트를 갖도록 구성했을 때 반복문을 돌면서 각각의 요소에 이벤트를 거는 것이 아니라 ul태그에 이벤트를 걸어주고 event 객체를 사용하는 식으로 구현할 수 있다.

```js
let ul = document.querySelector("ul");
ul.addEventListener("click",function(evt) {
    if(evt.target.tagName === "IMG") {
      log.innerHTML = "clicked" + evt.target.src;
    }
});
```

만약 img태그들 사이에 공백이 있다면 어떻게 해야할까?

공백이 있다면 if문에서 체크해주면 된다. 다음과 같다.

```js
let ul = document.querySelector("ul");
ul.addEventListener("click",function(evt) {
    if(evt.target.tagName === "IMG") {
      log.innerHTML = "clicked" + evt.target.src;
    } else if (evt.target.tagName === "LI") {
      log.innerHTML = "clicked" + evt.target.firstChild.src;
    }
});
```




