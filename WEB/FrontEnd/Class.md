# Class

## 개요

자바스크립트에는 Prototype이라는 개념이 존재한다.

이는 객체들이 바라보는 공간으로 이 곳에 함수를 선언한다거나 변수를 선언한다거나하면 생성자 함수를 사용해서 만들어진 객체들은 이 함수나 변수를 사용할 수 있다.

마치 자바의 상속과 비슷한 개념이다.

이를 적절히 사용한다면 중복된 코드를 줄이고 객체지향적으로 코드를 작성할 수 있다.

그러나 다소 직관적이지 못하고 다른 언어를 배우다 새롭게 자바스크립트를 배우는 사람들에게는 이해하기 힘든 코드일 수 있다.

따라서 Prototype를 사용하되 좀 더 이해하기 쉬운 코드를 작성하기 위해 ES6에서 도입된 문법이다.

이는 생김새만 바뀐 것으로 내부 동작은 사실 Prototype과 완전히 같다.

## 사용 방법

프로토 타입을 사용할 때
```js
function DoSomething() {

}

DoSomething.prototype.start() = function() {
  console.log("start!!");
}
const doSomething = new DoSomething();
doSomething.start(); // "start!!"
```

클래스를 사용할 때

```js
class DoSomething {
  constructor() {

  }
  start() {
    console.log("start!!");
  }
}
```

약간 자바스럽다고 느껴진다.

클래스로 작성했을 때 start라는 함수를 정의한 것은 `DoSomething.prototype.start() = function() {
  console.log("start!!");
}`에 대응된다.

## Getter/Setter

Getter란 클래스에서 값을 얻어오기 위한 함수이고 Setter란 클래스 내부에 값을 설정하기 위한 방법이다.

```js
class DoSomething {
  constructor(first, last) {
    this.firstName = first;
    this.lastName = last;
    this.fullName = `${first} ${last}`;
  }
  start() {
    console.log("start!!");
  }
}

const doSomething = new DoSomething('first', 'last');
console.log(doSomething);
doSomething.firstName = 'last';
console.log(doSomething);
```

이렇게 작성하고 출력 값을 보면 firstName속성의 값을 변경했음에도 적용이 되지 않는 모습을 볼 수 있다.

이는 생성자 함수로 클래스를 호출했을 때 최초로 설정되도록 했기 때문이고 코드를 작성하며 내부의 값을 가져오거나 변경하고 싶은 경우가 있을 것이다. 이럴 때 사용하는 것이 바로 Getter/Setter 메소드이다.

```js
class DoSomething {
  constructor(first, last) {
    this.firstName = first;
    this.lastName = last;
    this.fullName = `${first} ${last}`;
  }
  getFullName() {
    return `${this.firstName} ${this.lastName}`
  }
  start() {
    console.log("start!!");
  }
}
```

이렇게 작성할 수 있다. 그러나 Getter/Setter는 꽤 자주쓰이기 때문에 이를 위한 문법이 존재한다.

```js
class DoSomething {
  constructor(first, last) {
    this.firstName = first;
    this.lastName = last;
    this.fullName = `${first} ${last}`;
  }
  get fullName() {
    return `${this.firstName} ${this.lastName}`
  }
  start() {
    console.log("start!!");
  }
}

const doSomething = new DoSomething('first', 'last');
console.log(doSomething.fullName);
```

마치 객체의 속성에 접근하듯이 사용하면 get 키워드가 붙은 함수가 호출된다.

이와 마찬가지로 set 키워드를 붙이면 Setter를 작성할 수 있다.

```js
class DoSomething {
  constructor(first, last) {
    this.firstName = first;
    this.lastName = last;
    this.fullName = `${first} ${last}`;
  }
  get fullName() {
    return `${this.firstName} ${this.lastName}`
  }
  set fullName(value) {
    this.fullName = value;
  }
  start() {
    console.log("start!!");
  }
}

const doSomething = new DoSomething('first', 'last');
doSomething.fullName = 'test';
console.log(doSomething);
```


