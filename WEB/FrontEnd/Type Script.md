# Type Script

## 개요
세상에는 여러가지 프로그래밍 언어가 있다. 그 언어마다의 용도와 특성이 모두 다른데 여러가지 언어를 경험해 봤다면 자유로운 형식의 언어와 스트릭한 형식의 언어로 나눠볼 수 있을것이다.

예를 들어 c언어를 사용할 때 6줄의 코드를 작성해서 콘솔창에 Hello World를 출력했고 파이썬을 사용할 땐 한줄의 코드만 작성하면 된다.

자바스크립트는 비교적 자유로운 형식의 언어에 속한다.

장점으로는 배우기 쉽고 사용할 때도 자유롭게 코드를 작성할 수 있다는 것이 있다. 그러나 이런 자유로움이 반대로 단점으로 작용하기도 한다.

## 필요성
예를 들어 다음과 같은 덧셈 함수가 있다고 하자.
```javascript
function sum(num1, num2) {
  return num1 + num2;
}
```

```javascript
sum(10, 20); // 30
sum('10', '20'); //1020
```
그러나 우리가 원하는 결과는 첫번째 결과처럼 숫자 두 개를 더하는 것이였는데 문자열 두개를 이어버리는 연산이 수행되어버렸다.

이렇게 타입이 정수형이 아닌 문자형을 입력해도 오류를 발생시키지 않고 작동한다.


의도치 않은 입력값이 들어갔고 그로인해 의도치 않은 동작이 수행된다.

타입스크립트를 사용하면 이러한 오류를 사전에 방지할 수 있다.

## 정적 타입의 컴파일 언어
* 자바스크립트(동적 타입) - 런타임에서 동작할 때 타입오류를 확인한다.
* 타입스크립트(정적 타입) - 코드 작성단계에서 타입오류 확인
  자바스크립트로 변환(컴파일)후 브라우저나 Node.js환경에서 동작한다.

타입스크립트는 코드 작성 단계에서 타입오류를 확인하고 자바스크립트로 변환되어 동작하는 정적타입의 컴파일 언어라고 할 수 있겠다.

타입스크립트는 자바스크립트의 슈퍼셋으로 완벽하게 호환된다.
슈퍼셋은 기본적으로 자바스크립트의 기능을 모두 가지고있고 추가적인 기능을 포함하고있다는 의미이다.

오늘날 우리가 사용하는 대부분의 라이브러리나 프레임워크는 타입스크립트를 지원한다. 범용적으로 사용할 수 있다는 의미이다.


## Let's get started!

### 개발 환경 구성
우선 Node.js가 설치되어있어야하니 먼저 설치하기.  
[Node.js](https://nodejs.org/en)

1. vs코드 상에서 비어있는 폴더를 하나 생성한다.
2. 터미널을 열어서 `npm init -y`명령어를 입력한다.
3. `npm i -D parcel typescript`명령어를 입력한다.
4. 여기까지 잘 진행이 되었다면 pakage.json가 생성되고 의존성들이 들어가있을 것이다.
5.  pakage.json의 스크립트 부분을 다음과 같이 변경한다.
  ```
  "scripts": {
      "dev": "parcel ./index.html",
      "build": "parcel build ./index.html"
    },
  ```
6. tsconfig.json파일을 만들고 내용을 채워넣는다.
  ```
  {
    "compilerOptions": {
      "target": "ES2015",//변환할 자바스크립트의 버전
      "module": "ESNext",//자바스크립트(ESNext)로 변환
      "moduleResolution": "Node",
      "esModuleInterop": true,//자바스크립트는 ESM방식을 사용하지만 nodejs에선 common방식을 사용한다. 호환여부 true
      "lib": ["ESNext", "DOM"],//타입스크립트에서 자바스크립트로 컴파일될 때 내부적으로 사용되는 라이브러리 지정
      "strict": true//타입스크립트 문법 엄격히 적용
    },
    "include": [
      "src/**/*.ts"//어느 부분에서 타입스크립트 파일을 찾을 수 있는지
    ],
    "exclude": [
      "node_modules"//컴파일시 제외할 경로를 명시
      //해당 경로는 패키지를 보관하기 위한 용도로 컴파일을 위한 용도는 아니기에 제외
    ]
  }
  ```
7. 소스파일 폴더(src)를 만들고 그 안에 main.ts파일을 만든다.  
8. index.html 파일을 생성하고 밑의 스크립트를 추가한다.
  ```
  <script type="module" defer src="./src/main.ts"></script>
  ```


이제 타입스크립트 파일에서 타입스크립트를 작성해볼 수 있다.

간단한 예제를 살펴보자면
```javascript
let hello = 123 // 기존의 자바스크립트 방식 아무런 오류가 나지 않는다.
```
```javascript
//콜론과 자료형을 명시해서 타입을 보장할 수 있다.

let hello: String = 123; 

//오류 발생
//'number' 형식은 'String' 형식에 할당할 수 없습니다.
```

이렇게 작성된 타입 스크립트가 브라우저에서 동작하려면 자바스크립트로 변환되어야한다.

앞에서 의존성으로 추가한 파슬 번들러가 변환역할까지 해줄 수 있다.

이부분도 실습을 해보자면

다음의 명령어로 개발 서버를 오픈할 수 있다.

```
npm run dev
```
실행시키면
```
Server running at http://localhost:1234
```
이런 문구가 나오며 서버가 실행된다.

그리고 파슬 번들러는 결과를 만들 때 dist라는 폴더가 생성되는데 열어보면 사용할 수 있는 Index.html파일이 있고 내부 자바스크립트 부분을 보면 자동으로 자바스크립트 파일로 변환되어 적용되고 있는 것을 볼 수 있다.

```html
<script defer="" src="/index.b7a05eb9.js"></script>
```

그리고 이렇게 변환된 js파일로 한 번 이동해보면 다양한 내용들이 보이는데 여기서 우리가 작성한 코드를 검색해보면 
```javascript
let hello = "hello world!";
```
위와 같이 타입스크립트의 문법이 사라지고 자바스크립트 문법으로 변환된 것을 확인할 수 있다.

타입을 체크하긴 하지만 그 사용법이 다른 언어들과 다르게 자유롭다

숫자
----

```javascript
let num: number
let integer: number = 10
let float: number = 3.14
let infinity: number = Infinity
let nan: number = NaN
```

불리언
----

```javascript
let isBoolean: boolean
let isDone: boolean = false
```

널/언디파인드
----

```javascript
let nul: null
let und: undefined
```

배열
----

```javascript
const fruits: string[] = ['Type', 'script]
const numbers: number[] = [1,2,3,4,5]
const union: (String|number)[] = ['Type', 1, 2, 'script']
```

객체
----

타입스크립트로 객체 데이터를 사용할 경우 object라는 키워드를 명시해서 사용하기 보단 밑의 userA처럼 객체의 필드값에 대한 일종의 정의를 해서 쓴다.


```javascript
const obj: object = {}
const arr: object = []
const func: object = function() {}

const userA: {
  name: string
  age: number
  isValid: boolean
  = {
    name: 'user',
    age: 20,
    isValid: true
  }
}

const userB: {
  name: string
  age: number
  isValid: boolean
  = {
    name: 'user',
    age: 20,
    isValid: true
  }
}
```
uesrA와 userB는 형식은 같고 내용이 다르다

이렇게 형식은 같고 내용이 다른 객체들을 여러개 만들어야한다면 중복되는 코드가 생긴다. 개발자들은 중복되는 코드를 매우 싫어하기 때문에 당연히 중복되는 코드들을 일반화하는 방법이 있을거라고 유추할 수 있다.

바로 인터페이스라는 개념인데 다음과 같이 사용할 수 있다.

```javascript
interface User{ 
  name : string,
  age : number,
  isValid : boolean
}

const userA: User={
  name: 'userA',
  age: 20,
  isValid: true
}

const userB: User={
  name: 'userB',
  age: 22,
  isValid: true  
}
```

인터페이스에 선언되지 않은 속성을 새롭게 추가하면 오류가난다.

함수
----

일반적인 화살표 함수의 사용법에서 크게 벗어나지 않는다. 바뀐 부분은 각 파라미터에 대한 타입 정의와 리턴값의 타입을 지정해주는 부분이다.

마치 컴파일 언어들처럼 작성할 수 있다.

```javascript
const add: (x: number, y: number) => number = function(x, y) {
  return x + y
}
//이렇게 작성할 수도 있다.
const add1: function(x: number, y: number): number{
  return x + y
}
// add함수의 리턴형은 number이기 때문에 받는 변수의 타입도 마찬가지로 number여야한다
const a: number = add(1, 2)
```

리턴해주는 값이 없는 경우는 다음과 같이 작성할 수 있다.

```javascript
const sayHello: () => void = function() {
  console.log("Hello~~")
}

const sayHello1: = function(): void {
  console.log("Hello~~")
}

const h: void = sayHello()
```

Any
-----

any 키워드는 모든 타입이 올 수 있다.

```javascript
let hello: any = 'Hello world'
hello = 1234
hello = null
```

그러나 타입을 체크하지 않느다면 타입스크립트를 사용하는 의미가 없다.

따라서 any타입의 사용을 권장하지 않는다.


Unknown
-------

정확하게 어떤 타입이 올지 알 수 없어서 unknown으로 표현하겠다 라는 의미이다.

any와 비슷한 개념같지만 조금 차이가 있다.

우선 any를 살펴보면 다음과 같다.
```javascript
let hello: any = 'Hello world'

const boo: boolean = hello // 문제없음
const num: number = hello // 문제 없음
```

그러나 unknown은 any처럼 다른 타입의 값에 할당할 수 없다.

```javascript
let hello: unknown = 'Hello world'

const boo: boolean = hello // 할당 불가
const num: number = hello // 할당 불가
```

그래서 타입을 특정짓기 힘들 경우에 any를 사용하는 것보다 타입스크립트의 사용이 의미를 가지려면 unknown을 사용하는 것이 좋다. 


Tuple
-----

```javascript
const tuple: [String, number, boolean] = ['a', 1. true]
```

리스트와 비슷한 형태이고 자료형을 지정할 수 있다.


Void
-------

함수에서 리턴값이 없을 경우 반환 타입을 void로 작성할 수 있다.

```javascript
const sayHello1: = function(): void {
  console.log("Hello~~")
}
```

Never
-----

const nev: [] = []
//const nev: [Never] = []

이렇게 아무 타입을 넣어주지 않으면 Never라는 타입이 들어간다. Never타입은 아무값이 들어갈 수 없는 타입이다.

Union
-------
유니온은 타입이 두가지 이상 올 때 사용할 수 있다.

let union: string | number

Interscetion
-----

위에서 봤던 인터페이스의 내용을 보면

```javascript

```

const test: User & Validation = {
  name: 'jun',
  age: 25,
  isValid: true
}

이렇게 사용할 수 있다. 


## 타입 추론

위에서 타입을 매번 지정해줬는데 귀찮다고 느낄수도 있다. 이를 위해 타입스크립트는 타입 추론을 제공한다. 그래서 매번 타입을 지정할 필요 없이 필요한 곳에서만(컴퓨터가 추론하기 힘든부분) 타입을 지정해서 쓸 수 있다.

```javascript
let num = 12
num = 'Hello world' // 할당 불가
```

위의 코드는 number라는 타입을 명시하지 않았다. 그러나 문자열을 재할당 하려고 할 때 에러가난다. 타입스크립트가 num에 초기 할당된 타입이 number라는 것을 추론해서 자동으로 타입을 설정하고있기 때문이다.

조금 더 자세히 알아보면 타입 추론을 하는 일종의 룰이있다.

* 초기화된 변수
* 기본값이 설정된 매개변수
* 반환값이 있는 함수

