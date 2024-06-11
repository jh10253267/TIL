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
  renderSqure(i) {
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

## Props

Props는 Properties로 부모 컴포넌트로부터 자녀 컴포넌트로 데이터등을 전달하는 방법이다.

이는 읽기 전용으로 자녀 컴포넌트 입장에서는 변하지 않는다.

틱텍토 앱에서는 Props를 이용해서 Board컴포넌트에서 Square컴포넌트로 데이터를 전달할 수 있다.

```js
renderSqure(i) {
    return <Sqaure value={i} />;
  }
```

```js
export default class Square extends React.Component {

  render() {
    return (
      <button className="square">
        { this.props.value }
      </button>
    )
  }
}
```

이렇게 사용할 수 있다.

## State

사용자가 칸을 클릭하면 이를 기억하게 만들어 X표시를 넣어야한다.  
State는 컴포넌트에서 무언가를 기억하도록 할 때 사용한다.

state를 사용하기 위해선 생성자를 내부에서 초기화해주어야한다.

```js
// Board component
constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    }
  }
```

```js
handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({ squares : squares})
  }

renderSquare(i) {
  return <Square value={this.state.squares[i]}
  onClick={() => this.handleClick(i)}/>
}
```

```js
{this.renderSquare(0)}
```

각 칸을 클릭하면 onClick prop이 리엑트에게 클릭 이벤트 리스너를 설정하라고 알려주고 버튼을 클릭하면 리엑트는 Squre의 render()함수에 정의된 onClick이벤트 핸들러를 호출한다.

이벤트 핸들러는 this.props.onClick()을 호출한다. Squre의 onClick props는 Board에 정의되어있다. Board에서 Squre로 onclick = {() => this.handelClick(i)}를 전달했기 때문에 Square를 클릭하면 Board의 handleClick(i)를 호출한다.

## 리엑트 불변성 지키기

불변성이란 값이나 상태를 변경할 수 없는 성질을 말한다.

자바스크립트는 원시 타입과 참조 타입이 있다.

원시 타입으로는 Boolean, String, Number, null등이 있고 참조 타입으로는 Object, Array가 있다.

참조 타입의 경우 힙 공간을 사용하고 콜 스택은 개체 및 배열 값이 아닌 메모리에만 힙 메모리 참조 id를 값으로 저장한다.

예를 들어 원시 타입에서 이런식으로 코드를 작성한다면
```js
let username = 'walter';
username = 'john';
```

username walter를 john으로 대체한 것이 아니라 메모리공간에 있는 walter라는 값을 그대로 두고 다른 메모리 공간에 john을 새롭게 할당한 것이다.

반면 참조타입의 경우 배열에 요소를 추가하거나 객체의 속성 값을 변경할 때 콜 스택의 참조 id는 동일하게 유지되고 힙 메모리에서만 변경된다.

이렇게 원본이 변경되면 이 원본 데이터를 참조하고 있는 객체에서 예상치 못한 오류가 발생할 수 있고 화면을 업데이트할 때 불변성을 지켜서 이전 값과 현재 값을 비교해서 변경된 부분을 확인한 후 업데이트하기 때문에 불변성을 지켜줘야한다.

그 방법으로는 새로운 배열을 반환하는 메소드를 사용하는 방법이 있다.

예를 들어 자바스크립트의 map, filter, reduce 등의 메소드는 원본 배열을 가공하여 새로운 배열을 리턴한다.



