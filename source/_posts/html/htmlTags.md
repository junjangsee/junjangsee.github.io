---
title: HTML - HTML 문서의 기본 구조
date: 2020-01-19 22:17:25
tags: [HTML, Tag]
---

![images](/images/html/html.jpg)<br/>

# 문서의 기본 구조
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UFT-8">
        <meta name="description" content="Web Tutorial">
        <meta name="keyword" content="HTML, CSS">
        <meta name="author" content="junjang">

        <title>웹 프로그래밍 기초</title>
    </head>
    <body>
    </body>
</html>
```
- `<!DOCTYPE html>` : 해당 HTML 문서가 HTML5 언어로 작성되었다고 선언하는 태그입니다.
- `<html>` : HTML 문서의 시작과 끝을 알리는 태그입니다.
- `<head>` : HTML 문서 서문의 시작과 끝을 알리는 태그입니다. 해당 문서에 대한 요약 정보를 담는 부분으로서 간단한 정보를 입력합니다. 책으로 비유하자면 책 소개, 저자 소개, 주요 키워드 등에 해당 되는 부분입니다.
- `<meta>` : HTML 문서의 한 줄 요약, 키워드, 작성자 등 문서의 특장을 작성합니다. <head></head> 내부에 작성하며 일반적으로 charset, name, content 속성을 사용합니다.
 - charset : Character Set의 약자로 언어의 집합을 뜻하며 UTF-8 값을 사용하여 전 세계 모든 언어를 표현합니다.
 - name
  - description : 문서의 한 줄 요약
  - keywords : 주요 키워드
  - author : 문서 작성자 혹은 저작권자
- `<title>` : 문서의 제목을 입력합니다. 만약 입력하지 않으면 HTML 파일명과 확장자가 나타납니다.
- `<body>` : HTML 문서 본문의 시작과 끝을 알리는 태그입니다. 여기에는 웹 사이트의 텍스트와 이미 정보를 입력합니다.