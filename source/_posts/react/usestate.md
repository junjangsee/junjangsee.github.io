---
title: 리액트 훅(Hook) - useState
date: 2020-03-01 20:40:35
tags: [react, hook, useState, javascript]
---

![images](/images/react/react.png)<br/>

# useState

## 기존 방식

기존에 class를 기반으로한 state를 다루기에는 불편한 점이 많았습니다. `useState`는 functional한 개발을 도와주며 코드를 보다 간결하게 표현할 수 있습니다. 얼마나 차이가 나는지 기존 코드를 먼저 볼까요?

```
import React from "react";
import "./styles.css";

class ClassApp extends React.Component {
  state = {
    item: 1
  };

  incrementItem = () => {
    this.setState(state => {
      return {
        item: this.item + 1
      };
    });
  };

  decrementItem = () => {
    this.setState(state => {
      return {
        item: this.item - 1
      };
    });
  };

  render() {
    const { item } = this.state;
    return (
      <div className="App">
        <h1>Hello {item}</h1>
        <button onClick={this.incrementItem}>Increment</button>
        <button onClick={this.decrementItem}>Decrement</button>
      </div>
    );
  }
}

export default ClassApp;
```

이 방법이 class를 활용한 state 처리 방법이었습니다.<br/>
아래에서는 useState라는 훅(Hook)을 이용해서 얼마나 코드량이 줄고 편리하게 관리할 수 있는지 확인해보겠습니다.

<br/>

## 사용 방법

```
import React, { useState } from "react";
import "./styles.css";

function App() {
  const [item, setItem] = useState();
  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
    </div>
  );
}

export default App;

```

- react의 useState를 import합니다.
- 두 인자를 넣게 되는데 첫 번째 인자는 item값, 두 번째 인자는 set의 역할을 합니다.
- 인자는 어떤 이름이던 상관 없지만 통상 item, setItem과 같은 형식으로 사용합니다.
- useState(초기값); 을 통해서 초기값을 넣어줄 수 있습니다.

<br/>

## 예제

### 초기값 설정하기

```
import React, { useState } from "react";
import "./styles.css";

function App() {
  const [item, setItem] = useState(1);
  return (
    <div className="App">
      <h1>Hello {item}</h1>
      <h2>Start editing to see some magic happen!</h2>
    </div>
  );
}

export default App;
```

- 위 item에는 초기값인 1이 출력됩니다.

<br/>

### 증감 이벤트

```
import React, { useState } from "react";
import "./styles.css";

const App = () => {
  const [item, setItem] = useState(0);
  const incrementItem = () => setItem(item + 1);
  const decrementItem = () => setItem(item - 1);
  return (
    <div className="App">
      <h1>Hello {item}</h1>
      <button onClick={incrementItem}>Increment</button>
      <button onClick={decrementItem}>Decrement</button>
    </div>
  );
};

export default App;
```

- 초기값을 0으로 넣고 증감 함수를 만들어 Click시 이벤트가 일어나도록 하는 예제입니다.

<br/>

### useState 두개 사용

```
import React, { useState } from "react";

const Login = () => {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  return (
    <>
      <label>
        Name:
        <input
          type="text"
          name="name"
          value={name}
          onChange={({ target: { value } }) => setName(value)}
        />
      </label>
      <label>
        Email:
        <input
          type="email"
          name="email"
          value={email}
          onChange={({ target: { value } }) => setEmail(value)}
        />
      </label>
    </>
  );
};

export default Login;
```

- useState를 두개 사용한 예제입니다.
