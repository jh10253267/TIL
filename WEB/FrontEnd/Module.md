# Module

## 개요

모듈이란 특정 데이터들의 집합을 말한다.

이런 데이터들을 묶어서 자바스크립트 파일로 만들 수 있고 이를 자바스크립트 모듈이라고 부른다.

이렇게 작성하고 Import(가져오기), Export(내보내기)를 할 수 있다.

예를 들어 다음과 같이 할 수 있는 것이다.

```js
// module.js
export const hello = 'Hello world';

// main.js
import { hello } from './module.js';
console.log(hello); // Hello world
```

