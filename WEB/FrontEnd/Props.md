# Props

## 개요

리엑트로 구성한 프론트엔드는 컴포넌트 단위로 구성된다.

이 때 컴포넌트 간 데이터를 전달하여 사용할 수 있는 방법이 바로 Props이다.

## 개념

지금 실습하고있는 Tictactoe를 예로 들면 전체 Board라는 컴포넌트 내부에 9개의 Square 컴포넌트가 있다.

만약 Board에서 데이터를 전달해서 Square에서 사용해야할수도 있다. 이럴 때 Props를 사용할 수 있다.

이는 읽기 전용값으로 자식 컴포넌트에서 변경할 수 없다.

부모 컴포넌트

```js
import React, { Component } from 'react'
import Square from './Square'
import "./Board.css"
export default class Board extends Component {

  renderSquare(i) {
    return <Square value={i}/>
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

자식 컴포넌트
```js
import React from "react";
import "./Square.css"
export default class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    )
  }
}
```