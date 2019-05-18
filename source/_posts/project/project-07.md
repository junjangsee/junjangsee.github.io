---
title: StudyRoom(SRoom) 프로젝트 - 07(Kakao Rest API를 활용한 계정 연동하기 - Redirection 설정)
date: 2019-04-02 17:10:00
tags: OAuth
---

# SRoom 프로젝트

![images](/images/sroom/studyroom.jpg)<br/>

이번엔 설정한 Kakao Developer의 설정을 가지고 리다이렉션 설정을 해볼 것입니다.
만약 설정을 하지 않았다면 [Kakao Developer 가입 및 설정하기](https://junjangsee.github.io/2019/04/02/project/project-06/)를 참고하면 되겠습니다.
<br/>
### 인증코드(Authorization Code) 받아내기
OAuth인증을 위해서는 인증코드가 있어야 합니다.
설정하고 받았던 정보들을 가지고 요청을 보내서 코드를 받아내 보겠습니다.<br/>
![kakao](/images/oauth/log3.png) 위 요청을 하게되면<br/>
![kakao](/images/oauth/log4.png) 위와 같은 결과를 받게 됩니다.<br/>
```
http://kauth.kakao.com/oauth/authorize?client_id={app_key}&redirect_uri={redirect_uri}&response_type=code
```
위 주소에서 **app_key**에는 **REST API**를 넣고,
**redirect_uri**에는 **자신이 설정한 경로**를 입력하면 됩니다. 제 경우에는 **http://localhost/oauth**를 입력하였습니다.<br/>
![kakao](/images/oauth/log1.png) 위와 같이 입력하면 로그인 동의 창이 뜨게 됩니다.<br/>
![kakao](/images/oauth/log2.png) 동의하면 위 주소에서 code를 얻어낼 수 있습니다.<br/>
![kakao](/images/oauth/log5.png) 위 소스로 코드를 가져올 수 있습니다.<br/>
다음엔 Access Token, Refresh Token을 가져와보도록 하겠습니다.