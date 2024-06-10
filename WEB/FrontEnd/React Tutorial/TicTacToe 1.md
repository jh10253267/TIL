# TicTacToe앱 만들기

## 개요

패스트 캠퍼스 프론트엔드 웹 개발의 모든 것 강의를 들으며 정리해보려한다.

## 프로젝트 생성

로컬에 폴더를 하나 만들고 리엑트 앱을 설치해준다.

```
npx create-react-app ./
```

위의 명령어는 현재 터미널이 열려있는 경로에 리엑트 프로젝트를 생성하는 명령어다.

조금 기다리면 리엑트 앱이 설치된다.

프로젝트 파일을 좀 살펴보면 다음 경로의 파일들이 있다.

```js
public/index.html
src/index.js
```

부분이 보일 것이다. 이 파일들은 각각 페이지 템플릿과 자바스크립트 시작점이 되기 때문에 이들은 이름을 수정하면 안된다.

public 내부의 파일들만 public/index.html에서 사용할 수 있다. index.html 파일 외에도 favicon.ico등의 기본적인 파일들이 있다. 이곳에 Js파일들과 css파일들을 넣어두면 된다.

실습에서 코딩을 하는 방식은 src 폴더 내부에 많은 자바스크립트 파일들을 만들어 작업하는 식이다. 그리고 웹펙을 사용해서 여러 파일의 자바스크립트 코드를 번들링해서 최적화해준다. 

Package.json 파일에는 만들고자 하는 프로젝트에 대한 정보들이 들어있다. 프로젝트의 이름과 버전과 필요한 라이브러리와 버전들이 명시되어있고 앱을 시작할 때 사용할 스크립트, 앱을 빌드할 때 테스트할 때 사용할 스크립트등이 명시되어있다.

script에는 기본적으로 리엑트 앱 실행, 빌드, 테스트 등의 스크립트가 명시되어있고 프로젝트에서 자주 실행해야하는 명령어들을 scripts로 작성해두면 npm명령어로 실행 가능하다.

eslintConfig부분에는 소스코드를 입력할 때 문법이나 코드 포멧을 체크해주는 내용을 적어주면 된다.

```
{
  "name": "tic-tac-toe",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.17.0",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1",
    "web-vitals": "^2.1.4"
  }, 
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

## 앱 실행하기

위에서 설명한 대로 npm으로 실행하는 명령어를 모아놓은 scripts 부분에 start 명령어를 입력하면 앱이 실행되도록 되어있다.

```Js
npm start 
// npm run start
```
둘중 아무거나 입력해서 앱을 실행할 수 있다.

이렇게 하면 앱이 실행되어 원자 모형의 아이콘이 보인다.

이제 코드를 작성해서 작업하면 되는데 페이지 템플릿의 시작은 index.html이기 때문에 index.html 파일을 수정해보자.

리엑트는 컴포넌트 단위로 화면을 그린다. index.html을 살펴보면 `<div id="root"></div>` 부분을 볼 수 있고 이 부분을 렌더링하는 함수부는 index.js내부에 존재한다.

```js
//index.js

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```
코드를 보면 id가 root인 요소를 대상으로 설정해주고 App이라는 컴포넌트를 렌더링 시키고 있다고 이해할 수 있다.

src폴더 내부의 App.js 컴포넌트를 살펴보면 다음과 같다.

```js
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
           <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          ㅇㅇㅇLearn React
        </a>
      </header>
    </div>
  );
}
```

기본 화면에서 출력된 문구들이 여기 App.js의 App 함수 내부에 작성되어있는 것을 발견할 수 있다. 그리고 문구의 한 부분을 수정해보면 아까 실행시킨 리엑트 앱 화면에서 변화를 확인할 수 있다.


