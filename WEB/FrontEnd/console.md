# console

## 개요

어느 프로그래밍언어를 배우더라도 항상 첫 스텝은 내가 볼 수 있는 화면에 `Hello world!!`를 출력해보는 것이다.

내가 자바스크립트를 처음 배울 때도 마찬가지였다. console.log() 를 이용해 문자열을 출력했었다.

console에서 사용할 수 있는 기능들은 log외에도 여러가지가 있는데 이번 기회에 정리해보려한다.

## 종류

* log
* warn
* error
* dir
* count
* countReset

console.log(document.body) // 문자열로 출력
console.warn(document.body) // 노란색으로 출력
console.error(document.body) // 빨간색으로 출력
console.dir(document.body) // 노드를 반환

## count 메소드

count 메소드는 console을 통해서 실행시킬 수 있는 메소드로 콘솔에 메소드 호출의 누적횟수를 출력하거나 초기화 할 수 있다.

다음과 같다.
```js
console.count() //default: 1
console.count('a') //a: 1
console.count('a') //a: 2
console.count('b') //b: 1
console.count('a') //a: 3
console.countReset('a') // 카운트 초기화
console.count('a') //a: 1
```

## time

콘솔에 타이머가 시작해서 종료되기까지의 시간(ms)을 출력한다.

```js
console.time('반복문')

for (let i = 0; i < 10000; i += 1) {
  console.log(i)
}

console.timeEnd('반복문') //반복문: 198.861083984375 ms
```

## trace

메소드 호출 스택을 추적해 출력한다.

```js
function a() {
  function b() {
    function c() {
      console.log('Log!')
      console.trace('Trace!')
    }
    c()
  }
  b()
}
a()

/*
Log!
VM56:5 Trace!
*/
```

# clear

콘솔에 기록된 메시지를 모두 삭제한다.

```js
console.log(1)
console.log(2)
console.log(3)
console.clear()
//콘솔 삭제됨
```

## 서식 문자 치환

c언어에서 많이 사용하는 문자열 포멧팅 같은 느낌이다.

```js
const a = 'The brown fox'
const b = 3
const c = {
  f: 'fox',
  d: 'dog'
}

console.log('%s jumps over the lazy dog %s times.', a, b)
//The brown fox jumps over the lazy dog 3 times.

console.log('%o is Object!', c)
console.log('%cThe brown fox %sjumped over %cthe lazy dog.',
'color: brown; font-family: serif; font-size: 20px;',
'',
'font-size: 18px; color: #FFF; background-color: green; border-radius: 4px;')
```
