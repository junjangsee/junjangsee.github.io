---
title: CSS 간단하게 총정리
date: 2020-05-21 22:26:36
tags: [CSS]
---

![images](/images/css/css.png)<br/>

# 선택자

꾸미고자 하는 요소를 지칭할 때 사용합니다.

## Type Selector

HTML 태그 자체를 지칭하는 선택자입니다.

```html
<div>
  <p>
    빨간색 문단이 됩니다.
  </p>
</div>
```

```css
p {
  color: red;
}

div {
  background-color: yellow;
}
```

## Class Selector

요소의 class 값을 지칭하는 선택자입니다. `(.)점`으로 표현합니다.

```html
<div class="card">
  <h1 class="red">
    Hello
    <strong>junjang</strong>
  </h1>

  <p class="red">
    코로나 바이러스로 인해 집에서 공부 중입니다. 하루빨리 정상화 되어 카페도
    가고 스터디도 나가고 싶습니다. 이상입니다.
  </p>
</div>
```

```css
.red {
  color: red;
}
```

red class를 지칭하는 요소는 전부 빨간색으로 바뀔 것입니다.<br/>
만약 한 요소에서 여러 class를 가지고 있다면,

```html
<div class="red font">
  두개의 클래스가 적용된다.
</div>
```

```css
div.red.font {
  color: red;
  font-style: italic;
}
```

선택자를 붙여서 나열함으로서 class를 가지고 있는 요소를 지칭할 수 있습니다.

## Id Selector

요소 단 하나만 지칭하는 선택자입니다. `(#)샾`으로 표현합니다.

```html
<div class="card">
  <h1 id="junjang">
    Hello
    <strong>junjang</strong>
  </h1>

  <p>
    코로나 바이러스로 인해 집에서 공부 중입니다. 하루빨리 정상화 되어 카페도
    가고 스터디도 나가고 싶습니다. 이상입니다.
  </p>
</div>
```

```css
#junjang {
  font-style: italic;
}
```

junjang이라는 id를 가진 요소만 폰트 스타일이 변하게 됩니다.

## Child Combinator

`자식 선택자`로서 나를 품고 있는 부모의 바로 아래 요소를 말합니다. `직계 자손`만 해당되며 `>(부등호)`를 이용하여 표현합니다.

```html
<section>
  <h1>heading</h1>
  <ul>
    <li>
      <h1>Heading li</h1>
      <p>코로나 빨리 끝났으면...</p>
    </li>
    <li>
      <h1>Heading li</h1>
      <p>코로나 빨리 끝났으면...</p>
    </li>
  </ul>
</section>
```

```css
section > h1 {
  color: #ff2929;
}
```

section 직계 자손인 heading 텍스트를 담은 h1만 변합니다.

## Descendant Combinator

`자손 선택자`로서 나를 품은 부모가 또 부모가 있을 경우의 요소를 말합니다. `자식, 자손 모두` 해당되며 `공백`으로 표현합니다.

```html
<section>
  <h1>heading</h1>
  <ul>
    <li>
      <h1>Heading li</h1>
      <p>코로나 빨리 끝났으면...</p>
    </li>
    <li>
      <h1>Heading li</h1>
      <p>코로나 빨리 끝났으면...</p>
    </li>
  </ul>
</section>
```

```css
section h1 {
  color: #ff2929;
}
```

section의 자식, 자손인 모든 h1요소가 변합니다.

## Sibling Combinators

형제 선택자로서 같은 부모에 있는 요소들을 말합니다. `~와 +`로 표현합니다.

```html
<section>
  <h1>heading</h1>
  <ol>
    <li>1</li>
    <li class="active">2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
  </ol>
</section>
```

```css
.active {
  font-weight: 700;
}

.active ~ li {
  color: #ff2929;
}
```

`~`는 active class 이후의 `모든 형제 요소`들을 가리킵니다.

```css
.active {
  font-weight: 700;
}

.active + li {
  color: #ff2929;
}
```

`+`는 active class 이후의 바로 `다음 형제 요소 단 하나`를 가리킵니다.

## Structural Pseudo-classes

가상 클래스 선택자로서 총 세가지의 방법이 있습니다.

### first-child

첫 번째 요소일 때 사용할 수 있습니다.

```html
<section>
  <h1>heading</h1>
  <ol>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
  </ol>
</section>
```

```css
li:first-child {
  color: #0066ff;
}
```

### last-child

마지막 요소일 때 사용할 수 있습니다.

```css
li:last-child {
  color: #ffc82c;
}
```

### nth-child(n)

직접 내가 요소를 선택할 수 있습니다.

```css
li:nth-child(3) {
  color: #ff4949;
}
```

## User Action Structural Pseudo-classes

유저 액션에 따른 가상 클래스 선택자입니다. 총 3가지가 있습니다.

### hover

마우스를 해당 요소에 위치 시킬 때 사용하는 표현입니다.

```html
<section>
  <h1>heading</h1>
  <a href="#">blog</a>
  <input type="email" placeholder="Enter your email" />
</section>
```

```css
a:hover {
  background-color: #7e5bef;
}
```

### active

마우스를 해당 요소를 클릭했을 떄 사용하는 표현입니다.

```css
a:active {
  background-color: #ff4949;
}
```

### focus

마우스를 클릭한 후 focus 형태가 되었을 때 사용하는 표현입니다.

```css
input:focus {
  border-color: #1fb6ff;
}
```

# 박스

HTML은 모두 박스 형태로 구성되어 있습니다. 박스는 `Content, Padding, Border, Margin` 총 4가지로 분리가 되어있습니다.

## Content

컨텐트가 들어있는 곳을 나타냅니다. 가로는 width, 세로는 height로 표현합니다.

```css
.box {
  width: 300px;
  height: 300px;
  background-color: hotpink;
}
```

가로, 세로 각각 300px의 박스입니다.

## Padding

Content, Border 사이의 공간을 나타냅니다.

```css
.box {
  width: 300px;
  height: 300px;
  padding-left: 30px;
  padding-top: 20px;
  background-color: hotpink;
}
```

좌, 상단에 30px, 20px을 주었습니다.

**padding: 상 우 하 좌;** 순으로 각 공간을 명시할 수 있습니다.

## Border

테두리를 나타냅니다. 표현할 때 `굵기, 색상, 스타일`을 명시해줍니다.

```css
.box {
  width: 300px;
  height: 300px;
  padding-left: 30px;
  padding-top: 20px;
  border: 4px #000 solid;
  background-color: hotpink;
}
```

굵기 4px, 색상 #000, 스타일 solid 세 가지를 명시하면 테두리가 표현됩니다.

### Border-radius

테두리의 가장자리를 둥글게 만들어줍니다.

```css
.box {
  width: 300px;
  height: 300px;
  padding-left: 30px;
  padding-top: 20px;
  border: 4px #000 solid;
  border-radius: 5px;
  /* border-radius: 50%; */
  background-color: hotpink;
}
```

px말고도 %로 표현할 수도 있습니다.

## Margin

바깥 여백으로 요소와 요소 사이의 간격을 나타냅니다.

```css
.box {
  width: 300px;
  height: 300px;
  padding-left: 30px;
  padding-top: 20px;
  border: 1px #000 solid;
  background-color: hotpink;
  margin-bottom: 40px;
}
```

요소의 하단에 40px을 사용하여 간격을 넓혀줍니다.

**margin: 상 우 하 좌;** 순으로 각 공간을 명시할 수 있습니다.

## Box-sizing

기본 값은 content-box로 되어있기 때문에 content에서 **padding을 추가**할 경우 padding까지 **함께 계산**된 `width, height`이 표현됩니다.<br/>

```css
.box {
  box-sizing: border-box;
  width: 480px;
  height: 480px;
  background-color: #0066ff;
  color: #fff;
  padding-top: 40px;
  padding-left: 50px;
}
```

그래서 박스 기준을 선언해주면 선언된 박스 기준으로 자동 계산되어 표현됩니다.<br/>

```css
* {
  box-sizing: border-box;
}
```

보통은 이 기준이 사람들이 생각하는 상식이기 때문에 \* 선택자로 먼저 선언한 후 시작합니다.

## Display

박스 타입을 결정지을 떄 사용합니다.

### Block

**지나가지 못하게 막다**라는 의미를 가지고 있습니다. block의 여러가지 특징을 살펴보겠습니다.<br/>

- 따로 width를 선언하지 않은 경우 부모의 content-box의 100%를 가져갑니다.
- 따로 width를 선언한 경우 남은 공간은 margin으로 자동으로 채웁니다.
  - 만약 margin에 대해서 auto값을 주게 되면 자동으로 생긴 margin을 해당 부분에 위치하라는 의미입니다.
- 따로 height을 선언하지 않을 경우 자식 요소의 height의 합을 가져옵니다.
- box-model의 속성들을 모두 사용할 수 있습니다.

```css
.child {
  width: 100px;
  height: 200px;
  background-color: hotpink;
  margin: 0 auto;
}
```

### Inline

**흐르는** 의미를 가지고 있습니다. inline의 여러가지 특징을 살펴보겠습니다.<br/>

- 자동으로 줄바꿈을 지원합니다.
- width, height, padding-top, padding-bottom, border-top, border-bottom, margin-top, margin-bottom을 사용할 수 없습니다.

```css
.child {
  padding-left: 20px;
  padding-right: 30px;
  margin-left: 10px;
  margin-right: 40px;
}
```

### Inline-Block

inline에 block의 능력을 탑재한 특징을 가지고 있습니다.

# 가로배치(Float)

사전적 정의로 붕 뜨다는 의미를 가지고 있습니다. float는 아래와 같은 특성을 가지고 있습니다.

- 부모 요소에서는 붕 뜬 요소를 찾아내지 못합니다.(인지하지 못함)
- float된 대상은 block 속성으로 바뀝니다.
- block 속성을 가지고 있지만 특성은 가지고 있지 않습니다.
- 텍스트는 float된 대상을 캐치합니다.

위 특성을 해결하기 위해 두 가지의 방법을 사용합니다.

## overflow: hidden;

부모에서 float된 요소를 찾아냅니다.

```css
.parent {
  overflow: hidden;
  width: 200px;
  height: 200px;
  line-height: 200px;
  text-align: center;
  color: #fff;
  font-weight: 600px;
}

.red {
  float: left;
  background-color: #ff4949;
}

.yellow {
  float: left;
  background-color: #ffc82c;
}

.blue {
  background-color: #0066ff;
}
```

## Clear

float로 인해 망가진 속성을 바로잡기 위해 사용합니다. left, right, both 총 세 가지를 가지고 있습니다.<br/>
float의 속성이 있다면 같은 블럭 요소에게 clear 속성을 주어 부모에게 float된 요소가 있음을 알립니다. 또한 display가 block인 상태에서만 사용 가능합니다.

```css
.child {
  clear: both;
  display: block;
}
```

## Pseudo-element

HTML을 건들이지 않고 가상의 요소를 생성하는 방법입니다. 꼭 필수로 작성해야 하는 프로퍼티는 content 입니다.

### ::before

```css
.red::before {
  content: "😀";
  background-color: #ff4949;
}
```

### ::after

```css
.red::after {
  content: "😀";
  background-color: #ff4949;
}
```

# flex

정렬을 보다 쉽게 사용할 수 있게 만들어 줍니다. 총 4가지의 순서에 맞춰 사용합니다.

1. flex 선언
2. 가로 혹은 세로 정렬 선언
3. 한 줄인지 혹은 여러 줄인지 선언
4. 나머지 요소로 정렬 선언

## flex 선언

정렬하고자 하는 요소들을 `감싸고 있는 부모`에게 flex를 선언합니다.

```css
.flexbox {
  display: flex;
  /* flex | inline-flex */
}
```

## 정렬 방법 선언

가로 방향으로 할 것인지, 세로 방향으로 할 것인지 정해줍니다.<br/>
`가로`로 정렬하고 싶을 땐 **row**, `세로`는 **column**으로 선언합니다.

```css
.flexbox {
  display: flex;
  flex-direction: row;
  /* row | row-reverse | column | column-reverse */
}
```

### axis

정렬의 기준을 잡았을 경우 가로로 혹은 세로로 정렬을 할 것입니다. 이 떄 가상의 두 줄이 생기게 되는데 이를 바로 `axis`로서 `main과 cross`로 나뉘게 됩니다.<br/>
가로(row)의 경우 main이 좌에서 우로 관통하게 되고, cross는 위에서 아래로 흐릅니다.<br/>
세로(column)의 경우 main이 위에서 아래로 관통하며, cross는 좌에서 우로 흐릅니다.<br/>
쉽게 생각하자면 정렬하는 방향으로 main 선이 움직이며 cross는 그 main 선을 가로 지른다고 생각하면 됩니다.

## 한 줄 혹은 이상 정렬

nowrap은 무조건 한 줄로 정렬하며, wrap은 상황에 따라 한 줄 이상 정렬할 때 사용합니다.

```css
.flexbox {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  /* nowrap | wrap */
}
```

nowrap일 경우 자식의 사이즈를 줄이더라도 무조건 한 줄로 만들어냅니다.<br/>
예를 들어 부모가 600px을 가지고 있고, 자식 3개가 300px을 가지고 있었다면 자식 3개의 px을 200px씩으로 강제로 만들어버린 후 정렬합니다.<br/>
반대로 저렇게까지 정렬한 요소가 없다면 wrap을 선언하여 상황에 따라 정렬을 할 수 있습니다.

## 나머지 요소로 정렬

axis를 활용하여 main에는 `justify-content`, cross에는 `align-items, align-content` 프로퍼티를 사용하여 정렬합니다.

```css
.flexbox {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
  /* flex-start | flex-end | center | space-between | space-around */
  align-items: center;
  /* flex-start | flex-end | center */
}
```

만약 align-content 프로퍼티를 사용할 경우 **flex-wrap**이 `wrap`으로 선언되어야 합니다.

```css
.flexbox {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-content: center;
}
```

wrap으로 정렬하면 각각 컨텐트 별로 axis가 생기게 됩니다.
align-items는 **각각의 cross별**로 정렬을 하기 때문에 이상하게 정렬을 하지만 align-content는 *전체의 cross*로 정렬을 하기 때문에 올바른 정렬이 됩니다.

### 순서 정렬

order 프로퍼티를 자식 요소에 사용하여 정렬이 가능합니다.

```css
.redchild {
  order: 3;
}

.yellowchild {
  order: 1;
}

.bluechild {
  order: 2;
}
```

order속성을 추가하여 노랑, 파랑, 빨강 순으로 나타낼 수 있습니다.

# 위치

요소를 원하는 위치에 자유롭게 위치시키기 위해 사용합니다. 총 4가지의 포지션이 있습니다.<br/>
포지션을 사용할 땐 두 가지로 판단합니다. 첫쨰, 내가 어떤 종류의 포지션을 사용하는지 둘째, 무엇을 기준으로 위치시키는지 고려해야합니다.<br/>
또한 포지션을 선언했을 때 요소를 움직이는 프로퍼티는 top, right, bottom, left를 사용해서 움직입니다.

## static

모든 요소의 기본 값입니다. 가장 일반적인 요소입니다.

## relative

자기 자신이 원래 있던 자리 값입니다. 원래의 자리를 기억하고 있으며 그 자리를 기준으로 움직이게 됩니다.

```css
.red {
  position: relative;
  top: 20px;
  right: 30px;
}
```

## absolute

**float와 굉장히 비슷한 성격**을 가지고 있습니다. 아래는 absolute의 특성입니다.

- 부모 요소에서는 붕 뜬 요소를 찾아내지 못합니다.(인지하지 못함)
- block 속성으로 바뀝니다.
- block 속성을 가지고 있지만 block의 특성은 가지고 있지 않습니다.

float와 가장 큰 차이점은 부모한테 종속된 것이 아니라 `자신이 종속될 대상 요소`를 **선택**할 수 있다는 것입니다.<br/>
그 기준은 포지션이 `static이 아닌` 요소일 경우 그 요소를 기준으로 종속됩니다.

```css
.grandparent {
  left: 0;
  position: relative;
}

.parent {
  position: static;
}

.red {
  position: absolute;
}
```

parent의 경우 종속 될 수 없기 때문에 기준이 되지 못하며 grandparent의 경우 relative의 경우 기준점이 되기 때문에 왼쪽을 기준으로 위치됩니다.

## fixed

absolute의 특성과 완전 동일한 현상이 발생합니다. 하지만 큰 차이점은 자신의 기준점을 `viewport 기준`으로 위치시키게 됩니다.

```css
.body {
  height: 3000px;
}

.red {
  position: fixed;
}
```

viewport 기준으로 움직이기 때문에 따로 포지션을 줄 필요 없이 위치하게 됩니다.

## z-index

위치된 요소들의 수직의 위치를 알려주는 역할을 합니다.

```css
.red {
  position: absolute;
  z-index: 2;
}

.yellow {
  position: absolute;
  z-index: 1;
}
```

yellow보다 red가 더 상위에 있어야 할 경우 정수로 높은 수를 선언하여 더 위에 표현할 수 있습니다.

# 반응형

웹 뿐만 아니라 디바이스별로 전부 지원할 때 사용합니다.<br/>
HTML에서는 META의 viewport를 사용하여야만 하며 CSS에서는 Media Query를 사용하여 반응형 웹을 설계할 수 있습니다.<br/>
보통 가장 작은 모바일부터 시작하여 큰 순으로 만들어 나갑니다.

## @media

@media를 이용하여 선언하며 width의 조건을 명시하여 해당 조건이 만족하면 명시한 스타일을 적용시키는 방식입니다.

```css
@media screen and (min-width: 768px) {
  /* do something... */
}
```

위 구조로 되어있습니다.

```css
@media screen and (min-width: 576px) {
  /* CSS 선언 */
  .box {
    background-color: #ff5216;
  }

  .box::after {
    content: "LandScape Phone";
  }
}
```

최소 너비가 576px 이하일 때 해당되는 내부의 CSS가 적용됩니다.

# 텍스트(Typography)

텍스트를 디자인 할 때 사용합니다.

## font-size

font-size로 나타내며 `폰트의 크기`를 표현합니다. **px, em, rem**으로 표현됩니다.

### px

절대단위로 표현합니다.

```css
.text {
  font-size: 16px;
}
```

### em

상대단위로서 어떤 값을 기준으로 하냐에 따라 크기가 달라집니다. 실제로 적용된 폰트 사이즈를 1em으로 판단합니다.

```css
body {
  width: 20em;
}

p {
  font-size: 20px;
}
```

여기서 body의 width는 400px이 됩니다. 1em이 실제로 적용된 크기인 20px이기 때문에 20 \* 20 = 400이기 때문입니다.<br/>
조금 불안정하게 폰트 크기를 결정하기 때문에 많이 사용되는 방법은 아닙니다.

### rem

rem은 root(HTML)를 기준으로 삼아 폰트 크기를 결정합니다.

```css
html {
  font-size: 20px;
}

p {
  font-size: 3rem;
}
```

p에서의 폰트 크기는 60px이 될 것입니다. 계산법은 em과 동일합니다.

## line-height

텍스트의 `줄 높이`를 결정합니다. **px, em, rem**으로 표현됩니다.<br/>
보통은 폰트 크기의 비례하여 줄간격을 명시하기 때문에 `em`을 자주 사용합니다.

```css
html {
  font-size: 20px;
}

p {
  font-size: 1rem;
  line-height: 1;
}
```

p의 현재 폰트 크기는 20px이며 줄 간격은 `em을 생략`한 크기로 적어주는 것이 관례입니다. 만약 px, rem으로 사용시에는 꼭 명시해주어야 합니다.

## letter-spacing

글자와 글자 사이의 `간격(자간)`을 줄 때 사용합니다. **px, em**으로 표현됩니다.<br/>
보통은 폰트 크기의 비례하여 줄간격을 명시하기 때문에 `em`을 자주 사용합니다.

```css
html {
  font-size: 20px;
}

p {
  font-size: 1rem;
  line-height: 1;
  letter-spacing: -0.01em;
}
```

p의 20px에서 1퍼센트 줄이고 싶을 경우의 예시입니다.

## font-family

폰트 `서체`를 표현할 떄 사용합니다.

```css
.text {
  font-family: "Poppins";
  font-family: "Poppins", sans-serif;
  font-family: "Poppins", "Roboto", sans-serif;
}
```

위 세가지 패턴 예시로 알아보면, 첫 번째는 Poppins 라는 폰트를 사용해,
두 번째는 Poppins를 사용하되 만약 없으면 sans-serif 계열의 폰트를 사용해,
세 번째는 Poppins를 사용하되 없으면 Roboto를 그마저 없으면 sans-serif 계열의 폰트를 사용해
라는 의미입니다.

## font-weight

폰트의 굵기를 표현할 때 사용합니다.

```css
.text {
  font-weight: 400;
  /* 100 | 200 | 300 | 400| 500 | 600 | 700 | 800 | 900 */
}
```

100 부터 900까지 표현이 가능하며 100단위로 사용해야합니다.<br/>
보통 `400의 경우는 보통 크기`로 사용하며 `700의 경우는 볼드 크기`로 사용합니다.

## color

`폰트의 색`을 표현할 때 사용합니다. **hex, rgb, rgba**로 표현 가능합니다.

```css
.text {
  font-weight: 400;
  color: hotpink;
}
```

### hex

#을 사용하여 숫자, 문자 조합으로 색을 표현합니다.

```css
.text {
  font-weight: 400;
  color: #0066ff;
}
```

### rgb

rgb(r, g, b);를 사용하여 색을 표현합니다.

```css
.text {
  font-weight: 400;
  color: rgb(0, 102, 255);
}
```

### rgba

rgba(r, g, b, a);를 사용하여 색을 표현합니다. a는 투명도를 나타냅니다.<br/>
`0과 1`로 표현하며 0에 가까울수록 `투명`해지며 1에 가까울수록 `불투명`해집니다.

```css
.text {
  font-weight: 400;
  color: rgba(0, 102, 255, 1);
}
```

## text-align

`텍스트를 정렬`할 때 사용합니다. **left, center, right**로 정렬합니다.

```css
.text {
  font-weight: 400;
  color: rgba(0, 102, 255, 1);
  text-align: center;
}
```

텍스트를 가운데 정렬하였습니다.

## text-indent

`텍스트를 들여쓰기`할 떄 사용합니다.

```css
.text {
  font-weight: 400;
  color: rgba(0, 102, 255, 1);
  text-indent: 5px;
}
```

5px 정도 들여쓰기를 하였습니다. 여기에는 음수도 사용하여 밖으로 들여쓰기도 가능합니다.

## text-transform

`텍스트를 변형`시킬 때 사용합니다. **none, capitalize, uppercase, lowercase**로 표현합니다.

- none: 아무런 변형 없음
- capitalize: 첫 번째 글자만 대문자로 변형
- uppercase: 전체 대문자로 변경
- lowercase: 전체 소문자로 변경

```css
.text {
  font-weight: 400;
  color: rgba(0, 102, 255, 1);
  text-transform: capitalize;
}
```

첫 번째 글자를 대문자로 표현하였습니다.

## text-decoration

`텍스트의 줄을 꾸밀`때 사용합니다. **none, underline, line-through, overline**으로 표현합니다.

- none: 줄이 없는 효과
- underline: 밑줄 효과
- line-through: 취소선 효과
- overline: 윗줄 효과

```css
.text a {
  color: #0066ff;
  text-decoration: none;
}
```

보통은 a의 기본 밑줄을 없애는 용도로 사용됩니다.

## font-style

`폰트의 스타일`을 줄 떄 사용합니다. **normal, italic, oblique**로 표현합니다.

- normal: 기본 효과
- italic: 기울임 효과
- oblique: 기울임 효과

```css
.text em {
  color: #0066ff;
  text-decoration: none;
  font-style: normal;
}
```

보통 강조의 텍스트를 남길 때 em의 경우 italic이 기본으로 되어있어 필요가 없는 경우 normal로 변경합니다.

# 폰트

웹을 배포하여 서버에 올렸을 때, 개발자는 있는 폰트가 만약 사용자에게 없는 경우엔 기본 폰트로 변경되어 표현되게 됩니다.<br/>
폰트를 올바르게 제공하는 방법을 알아보겠습니다.

## 가져다 사용하는 방법

웹에서 제공되는 폰트의 링크를 가져와서 head에 명시해주고, CSS에선 font-family 프로퍼티로 해당 폰트를 선언합니다.

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Font</title>
  <link
    href="https://fonts.googleapis.com/css2?family=Libre+Baskerville&display=swap"
    rel="stylesheet"
  />
  <link rel="stylesheet" href="./style.css" />
</head>
```

```css
body {
  font-family: "Libre Baskerville", serif;
}
```

body에 적용을 시키면 모든 요소에 함께 적용이 됩니다.

## 직접 제공하는 방법

파일을 다운로드 하여 직접 제공합니다.

```css
@font-face {
  font-family: "Libre Baskerville";
  font-style: normal;
  font-weight: 400;
  src: url("webfont.eot"); /* IE9 Compat Modes */
  src: url("webfont.eot?#iefix") format("embedded-opentype"), /* IE6-IE8 */
      url("webfont.woff2") format("woff2"),
    /* Super Modern Browsers */ url("webfont.woff") format("woff"), /* Pretty Modern Browsers */
      url("webfont.ttf") format("truetype"),
    /* Safari, Android, iOS */ url("webfont.svg#svgFontName") format("svg"); /* Legacy iOS */
}
```

@font-face에 font-family 프로퍼티를 사용하여 개발자가 제공할 폰트 명을 임의로 정해줍니다.<br/>
각각의 파일별로 지원하는 브라우저와 디바이스에 따라서 url과 format을 명시하여 해당 브라우저 혹은 디바이스에는 해당 파일을 가져갈 수 있도록 해주어야 합니다.

```css
@import url("./fonts.css");
```

@font-face를 선언한 CSS 파일을 HTML에서 불러온 CSS 파일에 import하여 사용할 수 있습니다.

# 배경

배경과 관련된 속성을 사용합니다.

## background-color

배경의 `색상`을 적용할 때 사용합니다.

### hex

#을 사용하여 숫자, 문자 조합으로 색을 표현합니다.

```css
.bg {
  width: 300px;
  height: 300px;
  background-color: #0066ff;
}
```

### rgb

rgb(r, g, b);를 사용하여 색을 표현합니다.

```css
.bg {
  width: 300px;
  height: 300px;
  background-color: rgb(0, 102, 255);
}
```

### rgba

rgba(r, g, b, a);를 사용하여 색을 표현합니다. a는 투명도를 나타냅니다.<br/>
`0과 1`로 표현하며 0에 가까울수록 `투명`해지며 1에 가까울수록 `불투명`해집니다.

```css
.bg {
  width: 300px;
  height: 300px;
  background-color: rgba(0, 102, 255, 1);
}
```

## background-image

배경으로 `이미지`를 삽입할 때 사용합니다. `url()`을 반드시 사용하여야 합니다.

### 자체 이미지 사용

```css
.bg {
  width: 300px;
  height: 300px;
  background-image: url("./assets/hello.jpg");
}
```

현재 프로젝트의 폴더의 경로를 찾아서 확장자명까지 표시합니다.

### 이미지 주소 사용

```css
.bg {
  width: 300px;
  height: 300px;
  background-image: url("https://www.image.com/2");
}
```

이미지의 원본 주소를 찾아서 표시합니다.

## background-repeat

이미지를 삽입 했을 때 `반복`을 할 것인지 여부를 결정합니다.

```css
.bg {
  width: 300px;
  height: 300px;
  background-image: url("./assets/hello.jpg");
  background-repeat: repeat;
  /* repeat | no-repeat */
}
```

repeat은 반복, no-repeat은 미반복입니다.

## background-size

배경 이미지의 `사이즈`를 정할 때 사용합니다. **contain, cover, custom**으로 표현합니다.

- contain: 원본 이미지 그대로 박스 사이즈에 맞게 삽입
- cover: 박스 사이즈에 맞춰서 이미지가 잘린 후 삽입
- custom: 가로 세로에 맞추어 삽입

```css
.bg {
  width: 300px;
  height: 300px;
  background-size: contain;
  /* contain | cover | custom */
  background-image: url("./assets/hello.jpg");
  background-repeat: no-repeat;
}
```

## background-position

배경 이미지의 `위치`를 정할 때 사용합니다. x, y축을 명시합니다.

```css
.bg {
  width: 300px;
  height: 300px;
  background-position: center center;
  background-size: contain;
  background-image: url("./assets/hello.jpg");
  background-repeat: no-repeat;
}
```

가로 세로 순으로 자신이 원하는 위치를 선언할 수 있습니다.

# 애니메이션

CSS에 애니메이션 효과를 줄 떄 사용합니다.

## transition

`전환`을 자연스럽고 부드럽게 처리할 때 사용합니다.

- property: 변화가 일어날 속성에 대해 명시
- duration: ms, s 단위로 지속 시간을 명시
- timing-function: 변화의 속도를 명시
  - ease-in: 나중에 빠르게
  - ease-out: 처음에 빠르게
  - ease-in-out: 처음과 나중을 빠르게
  - cubic-bezier(): 사용자 설정 변화
- delay: 변화의 지연을 명시

```css
.box {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 300px;
  height: 300px;
  border-radius: 5px;
  font-size: 20px;
  font-weight: 500;
  color: #fff;
  background-color: #0066ff;
  transition: all 2500ms cubic-bezier(0.075, 0.82, 0.165, 1) 1000ms;
}

.box.active {
  font-size: 30px;
  background-color: #ff4949;
}
```

## 애니메이션

`@keyframes`을 사용하여 애니메이션을 선언합니다.

- name: 애니메이션 이름을 명시
- duration: ms, s 단위로 지속 시간을 명시
- timing-function: 변화의 속도를 명시
  - ease-in: 나중에 빠르게
  - ease-out: 처음에 빠르게
  - ease-in-out: 처음과 나중을 빠르게
  - cubic-bezier(): 사용자 설정 변화
- delay: 변화의 지연을 명시
- iteration-count: 반복 횟수를 명시
- direction: 애니메이션 진행 방향 명시

```css
@keyframes name {
  from {
    /* do something... */
  }

  to {
    /* do something... */
  }
}
```

위 구조를 기본으로 하며 name에는 애니메이션 이름을 선언하게 됩니다.

```css
@keyframes name {
  0% {
    /* do something... */
  }

  50% {
    /* do something... */
  }

  90% {
    /* do something... */
  }
}
```

위 처럼 진행률 별로 애니메이션을 작성할 수도 있습니다.

```css
.box {
  position: relative;
  width: 300px;
  height: 300px;
  background-color: #0066ff;
  animation-name: move-box;
  animation-duration: 2000ms;
  animation-delay: 1000ms;
  animation-timing-function: ease-in-out;
  animation-iteration-count: 3;
  animation-direction: alternate;
}

@keyframes move-box {
  from {
    top: 0;
    background-color: #0066ff;
  }

  to {
    top: 200px;
    background-color: #ff4949;
  }
}
```

선언한 애니메이션은 사용하고싶은 요소의 `animation-name`에 선언하여 줍니다.<br/>
`animation-iteration-count`는 총 반복할 횟수를 나타내고 `animation-direction: alternate;`은 어색하지 않게 애니메이션의 시작과 끝을 왔다갔다 해주는 역할을 합니다.
이 두 속성을 제외하면 기타 나머지 속성들은 transition과 동일합니다.<br/>
MDN 문서를 활용하면 더 많은 프로퍼티와 활용법을 알 수 있습니다.

# 박스 스타일링

박스를 스타일 할 떄 사용합니다.

## box-shadow

총 `5가지`의 프로퍼티를 순서에 맞춰서 구현해주어야 합니다. 생략의 경우는 blur와 spread를 생략할 수 있습니다.

### h-offset

x축(가로)을 선언합니다.

### v-offset

y축(세로)을 선언합니다.

### blur

흐린 정도를 표현합니다.

### spread

그림자 사이즈를 표현합니다.

### color

색상을 표현합니다.

```css
.button {
  background-color: #ff4949;
  box-shadow: 0 10px 16px 0 rgba(255, 73, 73, 0.35);
}
```

# 나머지 처리

정해진 요소의 크기 이외에 남은 컨텐트가 있을 경우 이를 처리합니다.

## overflow

처리하는 방법에는 4가지가 있습니다.

### visible

기본 요소로서 그냥 남는 컨텐트를 건드리지 않습니다.

### auto

컨텐트가 남으면 알아서 `책임을 전가`하여 처리하게 합니다.

```css
.p {
  width: 500px;
  height: 400px;
  background-color: #0066ff;
  overflow: auto;
}
```

### scroll

컨텐트가 남으면 `스크롤`로 처리합니다.

```css
.p {
  width: 500px;
  height: 400px;
  background-color: #0066ff;
  overflow: scroll;
}
```

### hidden

컨텐트가 남으면 `숨겨서` 처리합니다.

```css
.p {
  width: 500px;
  height: 400px;
  background-color: #0066ff;
  overflow: hidden;
}
```

# 요소변형

요소를 2D, 3D에서 변형할 때 사용합니다.

## transform

가장 많이 사용하는 3가지에 대해 알아보겠습니다. 위치시킬 때 **기존에 있던 위치**를 기억하고 있는 것이 특징입니다.

### translate(x, y)

요소를 내가 원하는 방향으로 `위치`시킬 때 사용합니다.

```css
.box {
  width: 300px;
  height: 300px;
  border-radius: 20px;
  background-color: #0066ff;
  transform: translate(40px, 50px);
}
```

우로 40px, 좌로 50px 움직입니다.<br/>
여기서 축을 `%`로 할 경우 **자기 자신**을 기준으로 적용됩니다.

### scale(x, y)

`사이즈를 증감` 시킬 때 사용합니다. n은 1이 자기 자신의 사이즈이며 증감 할 수록 1을 기준으로 크기가 조절됩니다.

```css
.box {
  width: 300px;
  height: 300px;
  border-radius: 20px;
  background-color: #0066ff;
  transform: scale(2);
}
```

box의 사이즈가 2배로 커집니다. **scale(x, y)**로 상하좌우 사이즈를 조절할 수도 있습니다.

### rotate(Ndeg)

요소의 `각도를 회전`시킬 때 사용합니다. deg를 항상 함께 선언해주어야 동작합니다.

```css
.box {
  width: 300px;
  height: 300px;
  border-radius: 20px;
  background-color: #0066ff;
  transform: rotate(45deg);
}
```
