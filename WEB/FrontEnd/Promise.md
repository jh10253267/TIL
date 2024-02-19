# Promise


## 개요

  이전의 콜백지옥의 복잡함을 해결할 수 있는 방법이 바로 promise이다.

```javascript
const a = () => {
  return new Promise((resolve) => {
    setTimeout(()=> {
    console.log(1)
    resolve();
  }, 1000)
  })
}

const b = () => console.log(2)

a().then(() => {
  b()
})
```

```javascript
const a = () => {
  return new Promise((resolve) => {
    setTimeout(()=> {
    console.log(1)
    resolve();
  }, 1000)
  })
}
const b = () => {
  return new Promise((resolve) => {
    setTimeout(()=> {
    console.log(2);
    resolve();
  }, 1000)
  })
}
const c = () => {
  return new Promise((resolve) => {
    setTimeout(()=> {
    console.log(2);
    resolve();
  }, 1000)
  })
}

a().then(() => {
  b().then(() => {
    c();
  })
})
```

이런식으로 사용할 수 있다.

이전 콜백지옥 패턴보다는 조금 더 깔끔해졌지만 여전히 함수는 안으로 점점 들어간다.


```javascript
a().then(() => {
  return b(); // b 또한 프로미스 객체를 반환한다.
}).then(() => {
  return c();
});
```

이제는 조금 깔끔해졌다.

화살표 함수를 활용해서 한 번 더 정리하면 다음과 같다.

```javascript
a()
  .then(() => b())
  .then(() => {
  c()
  });
```

```javascript
a()
  .then(b)
  .then(c);
  .then(() => console.log('done));
```



