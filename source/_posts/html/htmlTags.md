---
title: HTML태그 간단하게 총정리
date: 2020-05-6 22:17:25
tags: [HTML, Tag]
---

![images](/images/html/html.jpg)<br/>

# 규격

HTML 문서를 작성할 때 어떠한 규격을 사용할지 선언해줍니다.

## DOCTYPE

document Type의 줄임말로 문서의 타입을 말합니다.

```html
<!DOCTYPE html>
```

HTML 문서 최상단에 위 태그를 입력하면 브라우저에게 이 문서는 `HTML5 버전` 이라는 것을 명시하는 것입니다.
즉, HTML5 기준으로 맞추어 렌더링을 해달라는 의미입니다.

# 제목(Headings)

어떤 주제에 대한 제목을 나타낼 때 사용합니다. 총 6가지 종류로 표현 가능합니다.<br/>

h1 부터 h6까지 숫자가 커질 수록 폰트가 작아지는 특징을 가지고 있습니다.

```html
<h1>이 태그는 제목입니다.</h1>
<h3>이 태그는 h3을 이용한 제목입니다.</h3>
```

# 문단(Paragraph)

어떤 주제에 대한 내용을 감싸는 용도로 사용합니다.<br/>
p로 감싸서 표현합니다.

```html
<h1>김준형의 Github</h1>

<p>닉네임은 junjang을 사용하고 있으며 개발을 좋아하는 사람입니다.</p>
```

# 강조

어떤 내용을 `강조`하고 싶을 때 사용합니다. 2가지 종류가 있습니다.

## em(Emphasis)

em은 기울기를 주어 강조를 합니다.

```html
<p>현재 저는 em 태그를 사용하여 <em>강조</em> 하고 싶습니다.</p>
```

## strong

strong은 폰트를 두껍게 하여 강조를 합니다.

```html
<p>strong 태그 또한 <strong>강조</strong>를 표현할 때 사용합니다.</p>
```

# 링크(Anchor)

링크는 현재 보고있는 페이지에서 다른 페이지로 `이동`하고 싶을 때 사용합니다. 대표적인 속성 값으로 `href`와 `target`이 있습니다.<br/>
먼저 `href` 속성의 종류를 알아보겠습니다.

## href

페이지를 이동할 대상을 나타냅니다.

### 웹 URL

```html
<a href="https://junjangsee.github.io/">김준형의 블로그</a>
```

### 상대경로

```html
<a href="./click.html">같은 디렉토리 내 페이지로 이동</a>
```

### 페이지 내 이동

```html
<a href="#hello">hello 문단으로 이동</a>

<p id="hello">이 문단으로 이동됩니다.</p>
```

### 메일 작성

```html
<a href="mailto:junjang.see@gmail.com">김준형에게 메일 작성하기</a>
```

### 전화 걸기

```html
<a href="tel:01012345678">클릭시 전화 걸기</a>
```

## target

페이지 이동 시 창의 이동에 대해서 결정합니다.

### \_blank

```html
<a href="" target="_blank">새로운 탭으로 이동</a>
```

# 이미지

이미지를 삽입할 때 사용합니다. 속성값은 `src`와 `alt` 두 가지가 있습니다.

## src

### 상대경로

개발자가 가져오고 싶은 이미지 파일의 경로가 있는 상대경로를 명시합니다.

```html
<img src="../images/junjang.jpg" alt="" />
```

### 이미지 주소

이미지 파일이 있는 URL 주소를 명시합니다.

```html
<img
  src="https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile7.uf.tistory.com%2Fimage%2F24283C3858F778CA2EFABE"
  alt=""
/>
```

## alt

대체 텍스트라는 의미이며 이미지가 로드되지 않을 때 대체로 텍스트를 보여주는 것을 말합니다. 또한 스크린 리더를 통해 읽을 수 있는 페이지를 만들기 위해 사용됩니다.

```html
<img src="../images/junjang.jpg" alt="김준형의 사진" />
```

# 목록

말 그대로 목록을 표현하고 싶을 때 사용합니다.

## ol

ordered list의 약자로 `순서가 있는` 목록을 표현할 때 사용합니다. 자식 요소는 **무조건 li**만 사용 가능합니다.

```html
<h1>실시간 급상승 검색어</h1>
<ol>
  <li>김준형</li>
  <li>junjang</li>
  <li>다이어트</li>
  <li>스타벅스</li>
  <li>KFC</li>
</ol>
```

## ul

unordered list의 약자로 `순서가 없는` 목록을 표현할 때 사용합니다. 자식 요소는 **무조건 li**만 사용 가능합니다.

```html
<h1>채용 공고</h1>
<ul>
  <li>웹 개발자</li>
  <li>서버 개발자</li>
  <li>프론트 개발자</li>
  <li>데브옵스</li>
  <li>안드로이드 개발자</li>
  <li>IOS 개발자</li>
</ul>
```

### li

list item의 약자로 ol과 ul 내부에 항목을 표현할 때 사용합니다.

# 정의

`용어를 정의`하거나 `key-value`로 정보를 제공할 때 사용됩니다. 여기서 key-value는 **key는 value** 즉, 이름은 김준형, 닉네임은 junjang 식의 형태를 말합니다.

## dl

Description List의 약자로 이제부터 정의를 할 것이라는 것을 **최초 선언**할 때 사용합니다. **dt, dd, div**를 사용하기 위해서는 꼭 사용해주어야 합니다.

### dt

description term의 약자로 key-value에서 `key`를 말합니다. 즉 어떤 값을 정의하기 전 그에 대한 `이름을 부여`하는 개념이라고 생각하면 됩니다.

#### dfn

dt내에서 구체적으로 key를 선언하고 싶다면 dfn을 사용하여 선언합니다.

### dd

description data의 약자로 key-value에서 `value`를 말합니다. dt에서 선언한 이름에 대한 `값을 부여`하는 개념입니다.

```html
<dl>
  <dt>김준형</dt>
  <dd>프론트엔드 개발을 꿈꾸는 개발 지망생입니다.</dd>
</dl>
```

```html
<dl>
  <dt>
    <dfn>junjang</dfn>
  </dt>
  <dd>1. 백엔드 개발 경험이 있음</dd>
  <dd>2. 경기도 안양에 거주하고 있음</dd>
</dl>
```

```html
<dl>
  <dt>김준형</dt>
  <dt>junjang</dt>
  <dd>프론트엔드 개발을 꿈꾸는 개발 지망생입니다.</dd>
</dl>
```

```html
<dl>
  <dt>김준형</dt>
  <dd>개인 블로그입니다.</dd>
  <dd>
    <a href="https://junjangsee.github.io/">김준형의 블로그</a>
  </dd>
</dl>
```

```html
<dl>
  <dt>김준형</dt>
  <dt>junjang</dt>
  <dd>개인 블로그입니다.</dd>
  <dd>
    <a href="https://junjangsee.github.io/">김준형의 블로그</a>
  </dd>
</dl>
```

```html
<dl>
  <div>
    <dt>김준형</dt>
    <dd>김준형에 대한 설명</dd>
  </div>
  <div>
    <dt>junjang</dt>
    <dd>junjang에 대한 설명</dd>
  </div>
</dl>
```

# 인용

인용문을 표현할 때 사용합니다.

## blockquote

문단이나 내용 전체를 전부 인용문으로 사용합니다. 속성에는 `cite`를 사용하여 인용문의 출처를 명시할 수도 있습니다.

```html
<blockquote>
  우리가 겪을 수 있는 가장 아름다운 체험은 신비다.<br />
  신비는 모든 참 예술과 과학의 근원이다.
</blockquote>
```

### cite

인용문의 출처를 명시할 때 사용합니다.

```html
<blockquote cite="https://junjangsee.github.io">
  우리가 겪을 수 있는 가장 아름다운 체험은 신비다.<br />
  신비는 모든 참 예술과 과학의 근원이다.
  <cite>- 알버트 아인슈타인</cite>
</blockquote>
```

## q

```html
<p>
  개발 공부를 하고 있을 때 olaf가 와서 말했습니다.
  <q>죽기 직전까지 코딩해라.</q> 저는 그 말을 듣고 소름이 끼쳤습니다.
</p>
```

# 그룹화

블록, 인라인 단위로 그룹화를 해서 보다 편하게 관리 및 스타일링을 하기 위해 사용합니다.

## div

블록 단위로 그룹화를 할 때 사용합니다.

```html
<div>
  <h1>김준형의 github</h1>
  <p>김준형의 저장소입니다.</p>
  <a href="https://junjangsee.github.io/">김준형의 블로그</a>
</div>
```

## span

인라인 단위로 그룹화를 할 때 사용합니다.

```html
<p>
  제 이름은 김준형입니다. 나이는 28이고 백엔드 개발을 하다가
  <span style="color: pink">프론트엔드</span>에 관심이 생겨 공부중에 있습니다.
</p>
```

# 입력 양식

사용자로부터 어떠한 정보 혹은 데이터를 받기 위해 사용합니다.

## form

브라우저에게 입력 양식을 사용하는 것을 선언합니다. 속성 값으로 `action`과 `method`가 있는데 action에는 `API 주소`를, method에는 `HTTP 메서드`(GET, POST)를 작성합니다.

```html
<form action="/account/id" method="GET"></form>
```

## input

사용자가 정보를 입력할 때 사용하는 태그입니다. `type`속성을 사용하여 input의 종류를 나타내며, 꼭 선언을 해주어야합니다.

```html
<input type="입력 속성" />
```

### text

대체적으로 한 줄로 끝나는 `간단한 텍스트`를 입력 받을 때 사용합니다.

```html
<input type="text" />
```

#### placeholder

`placeholder`는 input내에 **값이 없을 때** 안내될 텍스트를 출력하고 싶을 때 사용합니다.

```html
<input type="text" placeholder="이름을 입력하세요" />
```

#### maxlength

`maxlength`는 input내에 텍스트의 최대 입력 길이를 제한할 때 사용합니다.

```html
<input type="text" maxlength="10" />
```

#### minlength

`minlength`는 input내에 텍스트의 최소 입력 길이를 제한할 때 사용합니다.

```html
<input type="text" minlength="1" />
```

#### required

`required`는 input내에 텍스트를 필수적으로 입력해야할 때 사용합니다.

```html
<input type="text" required />
```

#### disabled

`disabled`는 input내에 텍스트를 입력할 필요가 없을 때 사용합니다.

```html
<input type="text" disabled />
```

#### value

`value`는 input내에 텍스트가 로드 될 때 초기값으로 출력됩니다.

```html
<input type="text" value="김준형" />
```

### email

`이메일`을 입력 받을 때 사용합니다.

```html
<input type="email" placeholder="이메일을 입력하세요" />
```

입력 창에 `@`가 없으면 예외를 뱉어내기 때문에 이메일 형식만 입력이 가능합니다.

### password

`비밀번호`를 입력 받을 때 사용합니다.

```html
<input type="password" minlength="6" />
```

비밀번호를 입력할 때 보안처리가 되어 보여집니다.

### url

`주소`를 입력 받을 때 사용합니다.

```html
<input type="url" placeholder="포르폴리오 URL을 적어주세요" />
```

### number

`숫자`를 입력 받을 때 사용합니다.

```html
<input
  type="number"
  placeholder="나이를 입력하세요 (18세 이상 ~ 100세 이하)"
  min="18"
  max="100"
/>
```

#### min

**숫자**의 `최소값`을 선언할 때 사용합니다.

#### max

**숫자**의 `최대값`을 선언할 때 사용합니다.

### file

`파일`을 입력 받을 때 사용합니다.

```html
<input type="file" />
```

#### accept

파일의 `확장자`를 정해줄 때 사용합니다.

```html
<input type="file" accept=".png, .jpg" />
```

## label

폼 양식에 **이름**을 붙일 떄 사용합니다. label을 사용할 때 주의할 점은 어떤 input의 label인지 확실히 `구분`해주어야 한다는 것입니다. 때문에 속성 `for`을 통해서 어떤 input인지 명시해줍니다.<br/>
여기서 for속성에 들어가는 값은 input의 id를 선언합니다.

```html
<label for="inputId">이름</label>
<input id="inputId" type="text" placeholder="이름을 입력해주세요" />
```

for속성에 input의 id를 선언하고 input의 이름을 붙여줍니다.
이 때 label으로 선언된 `이름`을 클릭하게 되면 해당 input을 focus하게 됩니다.

## radio

여러가지의 항목 중 한 개만 선택하는 버튼을 만들 때 사용합니다.

```html
<label for="americano">아메리카노</label>
<input id="americano" type="radio" />
<label for="latte">카페라떼</label>
<input id="latte" type="radio" />
```

위처럼 radio 버튼을 두 개를 선언하였을 때 두 radio 버튼이 구별이 되지 않고 `동시에 선택`될 것입니다. 이유는 이 두개의 input은 현재 같은 그룹에 있는 radio가 무엇인지 확인할 방법이 없기 때문입니다. <br/>
그래서 `name 속성`을 통해 같은 그룹 소속이란 것을 선언해주어야 합니다.

```html
<label for="americano">아메리카노</label>
<input id="americano" type="radio" name="coffee" />
<label for="latte">카페라떼</label>
<input id="latte" type="radio" name="coffee" />
```

같은 name을 선언하게 되면 하나의 그룹으로 인식하여 단 한개의 input만 가져올 수 있게 됩니다. 그러면 이 radio의 선택된 값은 어떻게 서버로 전송할까요?<br/>

```html
<form action="" method="GET">
  <label for="americano">아메리카노</label>
  <input id="americano" type="radio" name="coffee" />
  <label for="latte">카페라떼</label>
  <input id="latte" type="radio" name="coffee" />
  <button type="submit">
    Submit
  </button>
</form>
```

submit을 통해 전송을 하게 되면 URL에 **서브스트링** 형식으로 `URL?coffee=on`이 추가된 것을 확인할 수 있을 것입니다. <br/>
즉, 서버에 보내는 값은 input의 name을 물고 가는 것을 확인할 수 있습니다.<br/> 하지만 두 개의 input을 submit 하더라도 on값이 넘어가는데 이는 input의 초기값이 없기 때문입니다. 때문에 `value 속성`을 사용하여 초기값을 확실히 선언해주어야 합니다.

```html
<form action="" method="GET">
  <label for="americano">아메리카노</label>
  <input id="americano" type="radio" name="coffee" value="americano" />
  <label for="latte">카페라떼</label>
  <input id="latte" type="radio" name="coffee" value="latte" />
  <button type="submit">
    Submit
  </button>
</form>
```

value를 선언하면 해당 value값을 가져와서 서버에 전송하게 됩니다.

## checkbox

여러가지의 항목 중 다중 선택을 할 수 있는 버튼을 만들 때 사용합니다. radio 버튼과의 속성 값들은 동일하지만 type 속성만 다르게 이해하시면 됩니다.

```html
<h1>사용 가능 언어</h1>
<form action="" method="GET">
  <input id="html" name="lang" value="html" type="checkbox" />
  <label for="html">HTML</label>
  <input id="css" name="lang" value="css" type="checkbox" />
  <label for="css">CSS</label>
  <input id="javascript" name="lang" value="javascript" type="checkbox" />
  <label for="javascript">JavaScript</label>
  <button type="submit">
    Submit
  </button>
</form>
```

name으로 한 그룹인 것을 알려주고, value로 각 input에 대한 초기값을 선언해줍니다.

## select & option

항목들을 박스 안에 나열하여 풀 다운 메뉴를 구성할 때 사용합니다.

```html
<form action="" method="GET">
  <select>
    <option>HTML</option>
    <option>CSS</option>
    <option>JavaScript</option>
  </select>
  <button type="submit">
    Submit
  </button>
</form>
```

submit을 하면 URL에 ?만 추가되고 선택한 값은 반영되지 않은 것을 알 수 있습니다.
이는 input의 checkbox, radio처럼 같은 그룹을 명시하는 `name 속성과, value 속성`을 선언하지 않았기 때문입니다.

```html
<form action="" method="GET">
  <label for="lang">스킬</label>
  <select name="lang" id="lang">
    <option value="html">HTML</option>
    <option value="css">CSS</option>
    <option value="javscript">JavaScript</option>
  </select>
  <button type="submit">
    Submit
  </button>
</form>
```

select의 option의 `자식`들을 그룹화 하기 때문에 **select 태그에 name**을 주어 그룹화를 해주고 각각의 **option 항목**에 대해 `value 속성`을 활용하여 초기값을 부여합니다.<br/>
만약 여러 항목을 선택하고 싶다면 `multiple` 속성을 활용하면 됩니다.

```html
<form action="" method="GET">
  <label for="lang">스킬</label>
  <select multiple name="lang" id="lang">
    <option value="html">HTML</option>
    <option value="css">CSS</option>
    <option value="javscript">JavaScript</option>
  </select>
  <button type="submit">
    Submit
  </button>
</form>
```

## textarea

여러줄에 걸쳐서 텍스트를 입력할 때 사용합니다.

```html
<label for="introduce">자기소개</label>
<textarea id="introduce" placeholder="자기소개를 입력하세요"></textarea>
```

## button

버튼을 통해 제어 혹은 form 제출 시 사용합니다. type 속성을 통해 버튼의 타입을 선언합니다.

### button 타입

버튼을 클릭 했을 때 JavaScript를 통해 제어를 할 때 자주 사용합니다.

```html
<button type="button">버튼</button>
```

### submit 타입

버튼을 클릭 했을 때 form 제출 시 사용됩니다.

```html
<button type="submit">버튼</button>
```

# 표

데이터를 담은 표를 만들 때 사용합니다.

## table

표를 생성하기 위해 `최초 생성`해야하는 태그입니다. tr, th 등 자식을 사용하기 전 필수로 선언해주어야 합니다.

### tr

표의 `가로줄`을 만들 때 사용합니다. tr 하나마다 가로줄 하나가 추가된다고 생각하면 됩니다.

### th

표의 `헤더(셀)`를 만들 떄 사용합니다. 데이터의 `제목` 느낌이라고 생각하면 됩니다.

#### thead

표의 헤더를 브라우저에게 좀 더 `명확하게 명시`하기 위해서 사용합니다.

```html
<thead>
  <tr>
    <th>ID</th>
    <th>이름</th>
    <th>개발분야</th>
  </tr>
</thead>
```

### td

표의 `데이터`를 삽입할 때 사용합니다.

```html
<table>
  <tr>
    <th>ID</th>
    <th>이름</th>
    <th>개발분야</th>
  </tr>
  <tr>
    <td>001</td>
    <td>김준형</td>
    <td>프론트엔드</td>
  </tr>
  <tr>
    <td>002</td>
    <td>junjang</td>
    <td>백엔드</td>
  </tr>
</table>
```

`최초 tr`을 통해 가로 줄을 하나 생성하여 th로 어떤 데이터를 표시할 것인지 `헤더를 선언`합니다.<br/>
`두 번째 tr`에서는 헤더에 해당되는 데이터를 넣기 위해 `td`를 사용하여 헤더에 맞는 `데이터를 순차적`으로 넣습니다. 이 때 헤더의 수와 `항상 동일하게` 맞추어 주어야 합니다.

#### tbody

표의 데이터를 브라우저에게 좀 더 `명확하게 명시`하기 위해서 사용합니다.

```html
<table>
  <thead>
    <tr>
      <th>ID</th>
      <th>이름</th>
      <th>개발분야</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>001</td>
      <td>김준형</td>
      <td>프론트엔드</td>
    </tr>
    <tr>
      <td>002</td>
      <td>junjang</td>
      <td>백엔드</td>
    </tr>
  </tbody>
</table>
```

#### tfoot

표의 결론을 브라우저에게 좀 더 `명확하게 명시`하기 위해서 사용합니다.

# 미디어

텍스트가 아닌 형태의 데이터를 문서에 삽입할 때 사용합니다. 종류로는 **오디오, 비디오**가 있습니다.

## audio

오디오 파일을 첨부할 때 사용합니다.

```html
<audio src="./assets/audios/louder.mp3"></audio>
```

```html
<audio>
  <source src="./assets/audios/louder.wav" type="audio/wav" />
  <source src="./assets/audios/louder.mp3" type="audio/mpeg" />
  <source src="./assets/audios/louder.ogg" type="audio/ogg" />
</audio>
```

위 두가지 방법으로 선언할 수 있는데 이 두 방법의 차이는 audio 태그에 `src 속성`을 통해 **하나만 넣으면** 해당 파일 하나만 적용이 되어 만약 해당 **파일을 지원하지 않는 브라우저의 경우 이용이 제한**되는 반면에 `source 태그`를 통해 여러가지의 파일을 선언하면 **해당 파일을 지원하는 브라우저에 맞게 출력** 됩니다.

## video

비디오 파일을 첨부할 때 사용합니다.

```html
<video src="./assets/videos/movie.mov"></video>
```

```html
<video>
  <source src="./assets/audios/movie.mov" type="video/mp4" />
  <source src="./assets/audios/movie.mp4" type="video/mp4" />
</video>
```

### controls

컨트롤러를 사용합니다.

```html
<audio src="./assets/audios/louder.mp3" controls></audio>
<video src="./assets/videos/movie.mov" controls></video>
```

### autoplay

페이지 로드 시 자동으로 재생합니다.

```html
<audio src="./assets/audios/louder.mp3" autoplay></audio>
<video src="./assets/videos/movie.mov" autoplay></video>
```

### loop

자동으로 반복합니다.

```html
<audio src="./assets/audios/louder.mp3" loop autoplay></audio>
<video src="./assets/videos/movie.mov" loop autoplay></video>
```

## iframe

HTML 문서 내에 embed한 미디어를 사용합니다.

```html
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/wfOOMu1PTmg"
  frameborder="0"
  allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
></iframe>
```

위 처럼 가져온 미디어를 표현할 때 사용합니다.

# 데이터

데이터는 head에 들어가는 태그들입니다.

## title

문서의 `대제목`을 나타냅니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>김준형의 TIL</title>
  </head>
  <body></body>
</html>
```

브라우저의 `탭`에는 김준형의 TIL이 적혀있게 됩니다. title은 `검색 최적화`에 아주 중요한 역할을 하기 때문에 신중하게 작성해야합니다.

## link

`CSS 파일`을 블러들일 때 사용합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>김준형의 TIL</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body></body>
</html>
```

## style

HTML 문서 내에 CSS 코드를 작성할 때 사용합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>김준형의 TIL</title>
    <style>
      h1 {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>junjang</h1>
  </body>
</html>
```

이 방법이 편하다고 느낄 수 있지만, style 태그의 사용을 지양해야하는 이유는 바로 CSS 파일을 가져왔을 때 style 태그를 사용하면 해당 `CSS 내용을 덮어버리기 때문`입니다.<br/>
또한 HTML은 클라이언트에 정보를 전달할 컨텐츠를 구분하여 보여주는 역할이기 때문에 style은 지양하는 것이 좋습니다.

## script

자바스크립트 파일을 HTML에 삽입할 때 사용합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>김준형의 TIL</title>
  </head>
  <body>
    <h1>junjang</h1>
    <script src="./index.js"></script>
  </body>
</html>
```

Body 맨 하단에 선언하여 자바스크립트 파일을 선언합니다.<br/>
또한 script 내에 자바스크립트 문법을 사용할 수 있습니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>김준형의 TIL</title>
  </head>
  <body>
    <h1>junjang</h1>
    <script>
      var h1 = document.querySelector("h1");
      h1.addEventListener("click", function (e) {
        this.textContent = "김준형";
      });
    </script>
  </body>
</html>
```

그런데 왜 자바스크립트 코드들을 head에 선언하지 않았을까요?<br/>
브라우저에서는 HTML 문서를 위에서 아래로 훑으며 렌더링을 합니다. **link** 에서 CSS파일을 가져올 때는 로드가 되지 않더라도 `기다리지 않고` 계속 아래로 렌더링을 해나가지만 **script**의 경우는 그렇지 않고 해당 스크립트를 다 처리할 때 까지 브라우저의 `렌더링을 멈추게 하기 때문`에 최하단에 위치시켜 로드시킵니다.

## meta

기타 다른 데이터 태그에서 표현할 수 없는 것들의 정보를 선언합니다. **name 속성**에는 `메타데이터의 종류`, **content 속성**에는 `메타데이터의 값`을 선언합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="author" content="junjang" />
    <meta name="description" content="데이터 관련 태그" />
    <meta name="keywords" content="junjang, 김준형, TIL" />
  </head>
  <body></body>
</html>
```

# 섹션

HTML 문서를 보다 구조적으로 설계할 때 사용합니다. 섹션 태그를 사용할 때는 heading 태그를 반드시 작성해야합니다. 그 이유는 해당 섹션마다의 주제를 나타내야하기 때문입니다.

## header

header는 HTML 문서 내에 섹션 파트의 도입부를 나타내는 역할을 합니다.

```html
<header>
  <h1>
    <a href="#">
      <img src="#" alt="junjang" />
    </a>
  </h1>
</header>
```

header에서 junjang이라는 내용을 가진 제목 역할의 이미지를 클릭하는 예시입니다. header의 주제는 junjang의 내용을 가진 이미지입니다.

## nav

문서간 이동할 수 있는 요소를 포함하는 역할을 합니다.

```html
<nav>
  <h1>Menu</h1>
  <ul>
    <li>
      <a href="#">
        Home
      </a>
    </li>
    <li>
      <a href="#">
        Explore
      </a>
    </li>
    <li>
      <a href="#">
        Notifications
      </a>
    </li>
    <li>
      <a href="#">
        Messages
      </a>
    </li>
    <li>
      <a href="#">
        Bookmarks
      </a>
    </li>
    <li>
      <a href="#">
        Lists
      </a>
    </li>
    <li>
      <a href="#">
        Profile
      </a>
    </li>
    <li>
      <a href="#">
        More
      </a>
    </li>
  </ul>
</nav>
```

ul li로 해당 페이지로 갈 수 있도록 만들었습니다.

## main

본격적으로 본론 컨텐츠가 시작되는 부분을 나타냅니다. 하나의 HTML 문서에 한 번만 사용 가능합니다.<br/>
main은 특이하게도 섹션에 포함되지 않아 heading을 반드시 명시할 필요는 없습니다.

```html
<main>
  <header>
    <h1>Home</h1>
    <button type="button" aria-label="Timeline options">
      <!-- icon -->
    </button>
    <div>
      <h2>Home shows you top Tweets first</h2>
      <!-- icon -->
      <button type="button">
        <strong>See lasted Tweets instead</strong>
        <span>you'll be</span>
      </button>
      <a href="#">
        View content preference
      </a>
    </div>
  </header>
</main>
```

## section

의도가 명확한 역할(논리적으로 완벽한)을 하고 있는 부분에 사용합니다. div의 역할에서 명확한 의도가 추가되었다고 이해하면 됩니다.

```html
<section>
  <h1>what's happening</h1>

  <form action="#" method="POST">
    <img src="#" alt="junjang" />
    <textarea placeholder="what's happening" maxlength="280"></textarea>
    <button type="button" aria-label="Upload files"></button>
    <input type="file" multiple accept="image/*, video/*" />
    <button type="button" aria-label="Search GIFs"></button>
    <button type="button" aria-label="Create a poll"></button>
    <button type="button" aria-label="Choose imoji"></button>

    <strong aria-label="0 out of 280 characters"></strong>
    <button type="button" aria-label="Add another tweet"></button>
    <button type="submit">Tweet</button>
  </form>
</section>
```

위 예시는 트위터에서 트윗을 남기는 역할을 확실히 수행하기 때문에 section으로 분리하였습니다.

## article

뉴스 기사, 블로그와 같은 컨텐츠의 정보가 완결성이 있는 경우 사용합니다. section과 헷갈릴 수 있지만 의도로서와 정보로서의 애매한 차이가 있습니다.

```html
<article>
  <h1>A tweet from junjang</h1>
  <header>
    <a href="#">
      <img src="#" alt="junjang" />
    </a>
    <h2>
      <a href="#">
        김준형
      </a>
    </h2>
    <dl>
      <dt>username</dt>
      <dd>
        <a href="#">
          junjang
        </a>
      </dd>
    </dl>
    <dl>
      <dt>Posted</dt>
      <dd>
        <a href="#">
          Dec 24
        </a>
      </dd>
    </dl>
    <button type="button" aria-label="Options"></button>
    <div>
      <button type="button">
        Show less often
      </button>
      <button type="button">
        Embed Tweet
      </button>
      <button type="button">
        Unfollow @anonymouskim
      </button>
      <button type="button">
        Mute @anonymouskim
      </button>
      <button type="button">
        Block @anonymouskim
      </button>
      <button type="button">
        Report Tweet
      </button>
    </div>
  </header>

  <p>
    영어를 더 잘 하고싶다. 그러나 공부를 하고 싶지는 않다. 내 삶의 모든 것이
    이런 식으로 망해왔다.
  </p>

  <footer>
    <button type="button">
      <span>Tweet your reply</span>
      <strong aria-label="3 replied">3</strong>
    </button>
    <button type="button">
      <span>Retweet</span>
      <strong aria-label="3 retweeted">3</strong>
    </button>
    <div>
      <button type="button">
        Retweet
      </button>
      <button type="button">
        Retweet with comment
      </button>
    </div>
    <button type="button">
      <span>Like this tweet</span>
      <strong aria-label="100 liked">100</strong>
    </button>
    <button type="button">
      <span>Share</span>
    </button>
    <div>
      <button type="button">
        Send via Direct Message
      </button>
      <button type="button">
        Add Tweet to Bookmarks
      </button>
      <button type="button">
        Copy link to Tweet
      </button>
    </div>
  </footer>
</article>
```

하나의 article 내부에 트윗 타임라인 기사를 하나 담은 예제입니다.

## aside

본문 내용과 동떨어진 컨텐츠를 가지고 있는 경우 사용합니다.

```html
<aside>
  <header>
    <h1>
      Worldwide trends
    </h1>
    <button type="button" aria-label="Options"></button>
  </header>

  <ol>
    <li>
      <button type="button" aria-label="Options">
        <!-- Icon -->
      </button>
      <div>
        <button type="button">
          <!-- Icon -->
          This trend is spam
        </button>
      </div>

      <a href="#">
        <span>1 · Trending worldwide</span>
        <strong lang="ko">#junjang</strong>
        <span>100K Tweets</span>
      </a>
    </li>
  </ol>
</aside>
```

## footer

HTML 문서내, 섹션의 하단부를 나타낼 때 사용합니다. 섹션의 특성과 다르게 heading이 꼭 필요하지 않습니다.

```html
<footer>
  <a href="#" target="_blank">
    Terms
  </a>
  <a href="#" target="_blank">
    Privacy policy
  </a>
  <a href="#" target="_blank">
    Cookies
  </a>
  <a href="#" target="_blank">
    Ads info
  </a>
  <button type="button">
    More
  </button>

  <div>
    <a href="#" target="_blank">
      About
    </a>
    <a href="#" target="_blank">
      Status
    </a>
    <a href="#" target="_blank">
      Businesses
    </a>
    <a href="#" target="_blank">
      Developers
    </a>
  </div>

  <span>© 2019 Twitter, Inc.</span>
</footer>
```
