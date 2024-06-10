# TicTacToe02

## 개요

틱텍토 게임은 총 3*3의 게임판에서 가로 or 세로 or 대각선으로 같은 기호 3개를 연속되도록 놓으면 이기는 게임이다.

두 명의 유저가 턴을 번갈아가면서 갖는다.

## JSX

JSX란 Javascript Syntax Extension의 약자로 자바스크립트의 확장 문법으로 쉽게 말하면 html과 자바스크립트를 함께 사용할 수 있는 문법이다.

예를 들면 이렇게 사용할 수 있다.
```js
const sample = <h1>Hello react!!</h1>;
```

리엑트에선 createElement API를 사용해서 엘리먼트(html 태그 요소)를 생성한 뒤 이 엘리먼트를 인메모리에 저장한다. 그런 다음 ReactDOM render함수를 사용하여 실제 웹 브라우저에 그리게 된다.

```js
const sampleEl = React.createElement('h1', {}, 'Hello react!!');

ReactDOM.render(sampleEl, document.getElementById('sample'));
```

JSX는 createElement를 쉽게 사용하기 위해 사용한다.

JSX를 사용하면 이를 바벨이 createElement로 변경하여 사용한다.

JSX를 사용할 때 주의해야할 점이 몇 가지가 있다.

가장 중요한 것은 컴포넌트에 여러 엘리먼트 요소가 있다면 반드시 부모 요소 하나로 감싸줘야한다는 것이다.

틱텍토앱은 게임 보드에 해당하는 Board, 각각의 칸에 해당하는 9개의 Square 컴포넌트로 이루어져있고 하단에 유저가 둔 수를 기록하여 보여주는 부분으로 나누어져있다.

스퀘어 컴포넌트는 버튼 요소를 렌더링하고 보드 컴포넌트는 사각형 9개를 렌더링하고 App 컴포넌트는 게임판을 렌더링한다.

```js
function App() {
  return (
    <div className="game">
      <div className='game-board'>

      </div>
      <div className='game-info'>
      </div>
    </div>
  );
}

export default App;

```

이제 컴포넌트를 작성해야하는데 src 경로에 바로 작성해도 괜찮지만 components와 같이 폴더를 만들어서 따로 관리하는 것이 편하다.

우선 클래스형으로 작성한 뒤 함수형으로 변경하도록 한다.
컴포넌트.js와 같이 파일을 작성해주고 Component를 상속받아 사용한다.

```js
export default class Square extends React.Component {

  render() {
    return (
      <button className="square">
        Squre Components
      </button>
    )
  }
}
```

직접 코드를 작성해도 되지만 `rcc`를 입력하고 탭 혹은 엔터를 누르면 자동으로 작성된다.

VS코드에서 ES7+ 플러그인을 설치하면 사용할 수 있다.

`rfc`는 함수형 컴포넌트를 만들 수 있다. 

보드에는 총 9개의 컴포넌트가 존재한다.

```js
export default class Board extends Component {
  renderSqure() {
    return <Sqaure />;
  }
  render() {

    return (
      <div>
        <div className="status">Next Player: X, O</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    )
  }
}
```