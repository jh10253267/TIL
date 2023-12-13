# 자바스크립트의 객체

## 개요
자바스크립트의 객체는 키와 값으로 이루어진 딕셔너리 자료구조이다.
그 사용법이 간단하고 객체안에 객체가 들어갈 수 있고 객체안에 함수도 들어갈 수 있어서 굉장히 유용하게 사용할 수 있다.

## 사용법
### 선언과 접근

객체는 다음과 같이 선언하고 사용할 수 있다.
```javascript
let obj = {name : "user1", age: "20"};

console.log(obj["name]);
console.log(obj.age);
```
자바스크립트의 객체구조를 본따 서버와 웹 브라우저간에 데이터를 주고받을 때 정의한 JSON이라는 것이 있다.

### 객체의 추가/삭제

```javascript
let obj = {name : "user1", age: "20"};

//추가
obj["key1"] = "test";
//삭제
delete obj.name;
delete obj["key1"];
```

### 객체의 탐색
```javascript
let obj = {name : "user1", age: "20"};

// 방법 1
for(value in obj) {
  console.log(obj[value]);
}
// 방법2
Object.keys(obj).forEach(function(key) {
  console.log(obj[key]);
});
```

## 객체 리터럴
```javascript
let healthObj = {
  name : "달리기",
  lastTime : "PM10:12",
  showHealth : function() {
    console.log(this.name + "님, 오늘은 " + this.lastTime + "에 운동을 하셨네요");
  }
}

healthObj.showHealth();
```
이와 같은 형태를 객체 리터럴이라 부른다.

그러나 이러한 형태의 객체가 여러개 필요하다면?

healthObj1,healthObj2 .....

이런 식으로 형태는 같고 디테일한 내용만 다른 객체들을 여러번 선언해줘야할 것이다.

개발자들이 끔찍히도 싫어하는 코드가 중복되는 문제가 생긴다.

이런 문제를 해결하는 방법은 다른 객체 지향 프로그래밍언어처럼 new 키워드를 이용해 객체를 동적으로 생성하는 방법이다.

```javascript
function Health(name, lastTime) {
  this.name = name;
  this.lastTime = lastTime;
  this.showHealth = function(){...}
}
const h = new Health("달리기", "10:12");

h2 = new Health("걷기", "20:11"); 
```
이런 식으로 사용할 수 있다.

그러나 아직 내부 함수가 새롭게 정의되며 중복되고 있다.

이를 해결할 수 있는 방법이 바로 
> Prototype

## Prototype
모든 자바스크립트 객체는 한 곳을 바라본다. 바로 prototype이라는 공간이다.

위에서 살펴본 것 같이 새로운 객체를 new 키워드를 이용해 생성하면 생성된 모든 객체는 프로토 타입을 바라본다.

![프로토타입](https://cphinf.pstatic.net/mooc/20180305_178/1520239531737NnTs4_JPEG/5-1-1_prototyp.jpeg?type=w760)

다르게 말해 프로토 타입에 있는 정보를 같은 타입의 객체라면 가져다 쓸 수 있다는 의미이다.

우리는 이 공간에 해당 객체에 필요한 정보, 혹은 함수들을 넣어서 객체에서 사용할 수 있다는 것이다.

마치 상속의 느낌이다.

여기서 부터 마치 다른 프로그래밍 언어를 사용하는 것처럼 느껴지기도 한다.

```javascript
function Health(name, lastTime) {
  this.name = name;
  this.lastTime = lastTime;
}

Health.prototype.showHealth = function() {
    console.log(this.name + "," + this.lastTime);
}

const h = new Health("달리기", "10:12");
console.log(h);  
h.showHealth();
```

이전에 객체를 매번 새롭게 정의했다. -> 코드가 중복되는 문제가 생김

그 다음엔 객체를 동적으로 생성했다. -> 같은 함수가 새롭게 정의되는 문제가 생김

 new 키워드로 만들어진 객체들의 공통 기능을 프로토 타입 공간에 선언해서 중복되는 코드를 줄일 수 있게 되었다. -> 문제 해결

최근 자바스크립트에서도 클래스의 개념이 도입되었는데 프로토 타입을 좀더 정돈한 것이라. 새로운 패러다임이라고 보긴 어렵다. 그래도 한 번 배워보자 

다음 편에...