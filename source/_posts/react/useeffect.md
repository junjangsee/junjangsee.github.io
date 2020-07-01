---
title: 리액트 훅(Hook) - useEffect
date: 2020-03-04 23:24:27
tags: [react, hook, useEffect, javascript]
---

![images](../../images/react/react.png)<br/>

# useEffect

`useEffect` 는 리액트 컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정 할 수 있는 Hook 입니다. 클래스형 컴포넌트의 componentDidMount 와 componentDidUpdate 를 합친 형태로 볼 수 있습니다.

<br/>

## 예제

```
import React, { useState, useEffect } from "react";
import "./styles.css";

const App = () => {
  const sayHello = () => {
    console.log("hello");
  };
  const [number, setNumber] = useState(0);
  const [aNumber, setAnumber] = useState(0);
  useEffect(sayHello, [number]);
  return (
    <div className="App">
      <h1>Hello</h1>
      <button onClick={() => setNumber(number + 1)}>{number}</button>
      <button onClick={() => setAnumber(aNumber + 1)}>{aNumber}</button>
    </div>
  );
};

export default App;
```

- useEffect를 import 합니다.
- hello를 출력하는 함수, 클릭시 숫자가 증가하는 버튼을 2개 만들었습니다.
- useEffect(함수, [변경값 감지])

<br/>

### 마운트 될 때만 실행하고 싶을 때

만약 useEffect 에서 설정한 함수가 컴포넌트가 화면에 가장 처음 렌더링 될 때만 실행되고 업데이트시 실행 할 필요가 없는 경우엔 함수의 두번째 파라미터로 비어있는 배열을 넣어주시면 됩니다.

```
useEffect(sayHello, [])
```

- 배열에 빈 값을 넣어 최초에 마운트 될 때만 sayHello 함수가 출력됩니다.

<br/>

### 특정 값이 업데이트 될 때만 실행하고 싶을 때

만약 sayHello 함수를 특정 값이 업데이트가 될 때 실행하고 싶다면 배열 안에 업데이트 대상이 되는 값을 넣어주면 됩니다.

```
useEffect(sayHello, [number])
```

- 배열에 number를 넣으면 number 값이 바뀔 때마다 sayHello 함수가 출력됩니다.
