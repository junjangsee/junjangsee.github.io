---
title: 자바스크립트(바닐라스크립트) 기본 이론
date: 2020-02-14 13:08:35
tags: [javascript, vanilaScript]
---

![images](/images/javascript/javscript.png)<br/>

# HelloWorld

## 1. CSS 추가하는 방법

```html
<head>
  <title>Something</title>
  <link rel="stylesheet" href="index.css" />
</head>
```

- head 태그 내부에 추가합니다.
- link 태그를 활용하여 href에 `css파일 경로`를 입력합니다.

<br/>

### index.css

```css
body {
  background-color: peru;
}

h1 {
  color: white;
}
```

### 결과

![images](/images/javascript/hello1.png)<br/>

<br/>

## 2. JS 추가하는 방법

```html
<body>
  <h1>This Works!</h1>
</body>
<script src="index.js"></script>
```

- body 태그 밑에 추가합니다.
- script 태그를 활용하여 src에 `js파일 경로`를 입력합니다.

<br/>

### index.js

```javascript
alert("Hello World!");
```

### 결과

![images](/images/javascript/hello2.png)<br/>

<br/>

# variable(변수)

## 1. let

```js
let a = 100;
let b = 100 - 10;
a = 10;

console.log(b, a);
```

- 90, 10
- let은 초기화한 변수값을 다시 할당하는 순간 그 값을 가진 변수가 됩니다.

<br/>

## 2. const

```js
const a = 100;
let b = 100 - 10;
a = 10;

console.log(b, a);
```

- 에러 발생 : VM18:3 Uncaught TypeError: Assignment to constant variable. at <anonymous>:3:3
- const는 상수로서 초기화한 값 그대로를 가지고 있습니다.

<br/>

## 3. var

```js
var a = 100;
var b = 100 - 10;
a = 10;

console.log(b, a);
```

- 90, 10
- var는 let과 비슷하게 초기화한 변수값을 다시 할당하는 순간 그 값을 가진 변수가 됩니다.

<br/>

# 데이터 타입

## 1. String

```js
// String
const name = "junjang";

console.log(name);
```

- junjang
- 문자열을 ""안에 넣고 변수에 넣고싶을 떄 사용합니다.

<br/>

## 2. boolean

```js
const what1 = true;
const what2 = false;

console.log(what1);
console.log(what2);
```

- true, false
- 참, 거짓 값으로 0 혹은 1로 표현됩니다.
- 문자열이 아닌 true, false 를 입력하여 표협합니다.

<br/>

## 3. number

```js
const number = 55.1;

console.log(number);
```

- 55.1
- 숫자를 표현할 떄 사용합니다.

<br/>

# 데이터 정렬

## 1. Array

```js
const someThing = "someThing";
const dayOfWeek = ["mon", "tue", "wed", "thu", "fri", someThing];

console.log(dayOfWeek);
console.log(dayOfWeek[2]);
```

- ["mon", "tue", "wed", "thu", "fri", "someThing"]
- wed
- 문자열을 ""안에 넣고 변수에 넣고싶을 떄 사용합니다.
- 컴퓨터는 0부터 시작하기 때문에 3번 째인 값을 꺼내오고 싶다면 2를 가져옵니다.
- 배열에는 어떤 타입이든 넣어서 출력이 가능합니다.
- 내가 특정 값을 깅거해서 가져오는 부분이 데이터가 많아질 경우 힘들어집니다.

<br/>

## 2. Object

```js
const myInfo = {
  name: "junjang",
  age: 28,
  gender: "Male",
  isHandsome: true
};

console.log(myInfo);
```

- {name: "junjang", age: 28, gender: "Male", isHandsome: true}

<br/>

### 값 가져오기

```js
const myInfo = {
  name: "junjang",
  age: 28,
  gender: "Male",
  isHandsome: true
};

console.log(myInfo.name);
```

- junjang
- 배열보다 편리하게 원하는 값을 가져올 수 있습니다.

<br/>

### 값 바꾸기

```js
const myInfo = {
  name: "junjang",
  age: 28,
  gender: "Male",
  isHandsome: true
};

console.log(myInfo.name);

myInfo.name = "junhyeoung";

console.log(myInfo.name);
```

- junjang, junhyeoung
- myInfo내에 있는 값은 바꿀 수 있지만 myInfo자체는 바꿀 수 없습니다.

<br/>

### Array를 Oject에 넣기

```js
const myInfo = {
  name: "junjang",
  age: 28,
  gender: "Male",
  isHandsome: true,
  favMovies: ["Shutter island", "Inception", "Old boy"]
};

console.log(myInfo.favMovies);
```

- ["Shutter island", "Inception", "Old boy"]
- Object내의 배열이 출력됩니다.

<br/>

### Object에 Array에 Object넣기

```js
const myInfo = {
  name: "junjang",
  age: 28,
  gender: "Male",
  isHandsome: true,
  favMovies: ["Shutter island", "Inception", "Old boy"],
  favFoods: [
    {
      name: "kimchi",
      fatty: false
    },
    {
      name: "hamberger",
      fatty: true
    }
  ]
};

console.log(myInfo.favFoods);
```

- {name: "kimchi", fatty: false}, {name: "hamberger", fatty: true}
- Array 안에 Object를 넣을 수도 있습니다.

<br/>

# function(함수)

## 1. Function

```js
function sayHello(name, age) {
  console.log("my name is", name, "i'm", age);
}

sayHello("junjang", 28);
```

- my name is junjang i'm 28
- function 함수이름(인자값) { 함수 내용 } 공식을 이용하여 함수를 만듭니다.
- 인자값에는 어떤 이름이 들어가도 무방하며 외부 값을 입력받아 사용하기 위해 사용합니다.

<br/>

### 백틱 사용

```js
function sayHello(name, age) {
  console.log(`my name is ${name} i'm ${age}`);
}

sayHello("junjang", 28);
```

- 백틱내에 String과 \${} 문법을 통해서 스마트하게 가능합니다.

<br/>

### return 사용

```js
function sayHello(name, age) {
  return `my name is ${name} i'm ${age}`;
}

const returnFunction = sayHello("junjang", 28);

console.log(returnFunction);
```

- returnFunction은 return 값을 가지기 때문에 sayHello 함수의 return 값을 가지게 됩니다.

<br/>

### Object 사용

```js
const calculator = {
  plus: (a, b) => {
    return a + b;
  }
};

const plus = calculator.plus(5, 5);
console.log(plus);
```

- calculator 객체의 key값을 이용하여 함수를 만들고 return값을 받아 console.log로 출력합니다.

<br/>

# DOM

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Something</title>
    <link rel="stylesheet" href="index.css" />
  </head>
  <body>
    <h1 id="title">This Works!</h1>
  </body>
  <script src="index.js"></script>
</html>
```

```js
const title = document.getElementById("title");
const title = document.querySelector("#title");
title.innerHTML = "Hi! From JS";
title.style.color = "red";
console.dir(title);
```

<br/>

![images](/images/javascript/inner.png)<br/>

- h1에 id를 title로 선언해서 고유값을 줍니다.
- document.getElementById()를 사용해 id값을 가져옵니다.
- innerHTML을 사용하여 글자를 바꿀 수도 있습니다.
- dir함수를 활용하여 DOM에서 수정할 수 있는 것들을 찾아서 style등을 수정할 수도 있습니다.
- querySelector는 해당 고유값을 가진 태그를 찾아줍니다.

<br/>

# event(이벤트)

## WINDOW

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Something</title>
    <link rel="stylesheet" href="index.css" />
  </head>
  <body>
    <h1 id="title">This Works!</h1>
  </body>
  <script src="index.js"></script>
</html>
```

### resize

```js
const title = document.querySelector("#title");

function handleResize() {
  console.log("I have been resized");
}

window.addEventListener("resize", handleResize);
```

<br/>

- window 객체의 addEventListener함수를 사용하여 이벤트를 기다립니다.
- handleResize와 handleResize()의 차이점은 **이벤트를 기다리는가** 혹은 **즉시 실행하는가**의 차이입니다.

<br>

### click

```js
const title = document.querySelector("#title");

function handleClick() {
  title.style.color = "red";
}

title.addEventListener("click", handleClick);
```

- title 객체의 addEventListener함수를 사용하여 이벤트를 기다립니다.
- click시 handleClick함수를 호출하여 red로 바꿉니다.

<br/>

# 조건문

## 1. if / else

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Something</title>
    <link rel="stylesheet" href="index.css" />
  </head>
  <body>
    <h1 id="title">This Works!</h1>
  </body>
  <script src="index.js"></script>
</html>
```

```js
if ("10" === 10) {
  console.log("hi");
} else {
  console.log("ho");
}

if ("10" === "10") {
  console.log("hi");
} else if ("hello" == "hello") {
  console.log("ho");
} else {
  console.log("hu");
}

if (20 > 5 && "junjang" === "junjang") {
  console.log("hi");
} else {
  console.log("hu");
}

if (20 > 5 || "junjang" === "junjang") {
  console.log("hi");
} else {
  console.log("hu");
}

const age = prompt("How old are you?");

if (age > 18) {
  console.log("you can drink");
} else {
  console.log("you can't");
}
```

<br/>

- if / else를 통해 true 값이면 if문을, false 값이면 else문을 처리할 수 있습니다.
- &&(and) 와 ||(or) 을 통해 추가적인 조건을 사용할 수도 있습니다.

<br>

## 2. if / else function

```js
const title = document.querySelector("#title");

const BASE_COLOR = "rgb(52, 73, 94)";
const OTHER_COLOR = "#7f8c8d";

// resize
function handleClick() {
  const currentColor = title.style.color;
  console.log(currentColor);

  if (currentColor === BASE_COLOR) {
    title.style.color = OTHER_COLOR;
  } else {
    title.style.color = BASE_COLOR;
  }
}

function init() {
  title.style.color = BASE_COLOR;
  window.addEventListener("click", handleClick);
}

init();
```

- 색 두가지를 변수에 담고 init 함수를 즉시실행 하여 기본 컬러를 담고 click시 handleClick이 발생하도록 합니다.
- 만약 현재 색이 기본 컬러면 다른 컬러로 바꾸고, 다른 컬러면 기본 컬러로 바꾸는 if / else문 입니다.

<br/>

## 3. if / else classList

```js
const title = document.querySelector("#title");

const CLICKED_CLASS = "clicked";

function handleClick() {
  /* title.classList.toggle(CLICKED_CLASS); */
  const hasClass = title.classList.contains(CLICKED_CLASS);

  if (!hasClass) {
    title.classList.add(CLICKED_CLASS);
  } else {
    title.classList.remove(CLICKED_CLASS);
  }
}

function init() {
  window.addEventListener("click", handleClick);
}

init();
```

- classList를 통해 class를 포함할 떄 추가 혹은 제거를 할 수 있습니다.
- toggle은 위의 handleCheck 함수를 한 번에 처리해줍니다.
- [MDN 공식문서](https://developer.mozilla.org/ko/docs/Web/API/Element)

<br/>
