# Call Back


## 개요
동기 방식의 코드

```javascript
const a = () => console.log(1);
const b = () => console.log(2);

a();
b();

// 1
// 2
```
비동기 방식의 코드
```javascript
const a = () => {
  setTimeout(()=> {
    console.log(1);
  }, 1000)
}

const b = () => console.log(2);

a();
b();

// 2
// 1
```
이런 식으로 함수를 호출하는 순서와 관계없이 출력이 바뀌는 비동기 코드로 바꿀 수 있다.

만약 1과 2의 순서를 정확하게 맞추어주려면 어떻게 할까?

바로 비동기 코드에서 콜백이라는 패턴으로 코드를 작성하면 된다.


## 콜백 지옥
비동기 코드에서 함수의 호출 순서와 출력 순서를 맞추어보자

즉, 1이 먼저 출력되고 2가 다음으로 출력되게 하고 싶은 것이다.
```javascript
const a = (callback) => {
  setTimeout(()=> {
    console.log(1)
    callback()
  }, 1000)
}

const b = () => console.log(2);

a(() => {
  b()
});

```
 a함수 내부에 익명함수가 들어가게 되고 이것은 callback이라는 이름의 매개변수가 받아서 ```console.log(1)``` 다음 부분에서 실행이 된다.

즉 ```console.log(1)```이 실행된 다음에 b함수가 실행될 수 있는 것이다.

이렇게 실행을 해보면 우리가 원하는 대로 1이 먼저 출력되고 2가 출력된다.

이번에는 숫자3도 출력해보자.
b함수를 비동기 방식으로 바꾸고 함수 c를 추가한다.

```javascript
const a = (callback) => {
  setTimeout(()=> {
    console.log(1)
    callback()
  }, 1000)
}

const b = () => {
  setTimeout(() => {
    console.log(2)
  })
}
const c = () => console.log(3)

a(() => {
  b()
});
c()
```
이렇게 작성한다면 제일 먼저 3이 출력되고 그 다음 1과 2가 출력된다.

우리는 함수 호출 순서와 결과를 일치시키고 싶다.

그러려면

```javascript
const a = (callback) => {
  setTimeout(()=> {
    console.log(1)
    callback()
  }, 1000)
}

const b = (callback) => {
  setTimeout(() => {
    console.log(2)
    callback()
  },1000)
}
const c = () => {
  setTimeout(() => {
    console.log(3)
  },1000)
}

a(() => {
  b(() => {
    c()
  })
});
```
이렇게 코드를 작성하면 1초 간격으로 1, 2, 3이 출력되게 된다.

여기다 숫자 4까지 추가하고 싶다면 어떤식으로 코드를 짜야할까
우선 c함수도 매개변수로 함수를 받아서 이전과 같은 방식으로 작성하면 된다.

```javascript
const a = (callback) => {
  setTimeout(()=> {
    console.log(1)
    callback()
  }, 1000)
}

const b = (callback) => {
  setTimeout(() => {
    console.log(2)
    callback()
  },1000)
}
const c = () => {
  setTimeout(() => {
    console.log(3)
  },1000)
}
const d = () => console.log(4)

a(() => {
  b(() => {
    c(() => {
      d()
    })
  })
})
```

비동기 방식에서 실행 순서를 보장하려면 콜백 패턴을 사용해야하는데 콜백 패텅을 사용하다 보면 점점 복잡해지고 점점 안으로 들어가게된다.

이런 상황을 콜백 지옥이라고 부른다.

이런 문제를 해결하기 위한 개념이 있는데 바로 Promise이다.