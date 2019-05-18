---
title: StudyRoom(SRoom) 프로젝트 - 06(Kakao Rest API를 활용한 계정 연동하기 - Kakao Developer 가입 및 설정)
date: 2019-04-02 15:43:21
tags: OAuth
---

# SRoom 프로젝트

![images](/images/sroom/studyroom.jpg)<br/>

운영 DB까지 연동이 된 것을 확인 했으니 이제 본격적으로 기능을 구현하겠습니다. 먼저 주요 기능들을 구현하기 전에 회원가입 및 로그인이 되어야겠습니다.<br/>
이번 프로젝트에서는 소셜네트워크의 API를 활용하여 계정을 연동하여 가입할 수 있도록 만들어보려고 합니다.
그래서 우리나라에서 제일 활성화된 카카오를 필두로 하여 점점 늘려나갈 계획입니다.
참고한 블로그 : [Alpreah님의 블로그](https://alpreah.tistory.com/121?category=844976)
<br/>
### Kakao developers 가입하기
카카오 API를 사용하려면 Kakao Developers에 가입하여 절차를 거쳐야합니다.
먼저 [Kakao Developers 홈페이지](https://developers.kakao.com/) 에 들어가서 회원가입을 진행합니다.
<br/>
### 앱 만들기
![kakao](/images/oauth/ka1.png) 자신의 프로젝트 명과 회사명을 작성 후 계속 진행합니다.<br/>
![kakao](/images/oauth/ka2.png) 계속하기를 클릭합니다.<br/>
![kakao](/images/oauth/ka3.png) 위 사진이 출력되는데 여기서 REST API key를 확인 및 저장하고 개발가이드를 클릭합니다.<br/>
![kakao](/images/oauth/ka4.png) 사용자 관리 부분의 REST API 개발가이드를 클릭합니다.<br/>
![kakao](/images/oauth/ka5.png) 사용자 관리 중 먼저 로그인을 구현하겠습니다<br/>
![kakao](/images/oauth/ka6.png) Request 부분을 보면 GET 방식으로 요청을 해야하는 코드와 요청할 HOST가 있습니다.
```
http://kauth.kakao.com/oauth/authorize?client_id={app_key}&redirect_uri={redirect_uri}&response_type=code
```
최종적으로 위와 같은 경로로 요청하면 됩니다.
그러면 redirect_uri를 설정해봅시다.
<br/>
### redirect_uri 설정하기
![kakao](/images/oauth/ka7.png) 자신이 설정했던 프로젝트 이름을 클릭합니다<br/>
![kakao](/images/oauth/ka8.png) 카카오 로그인 부분이 활성화가 안된 부분인데 '사용자 관리'부분을 클릭합니다<br/>
![kakao](/images/oauth/ka9.png) 사용자 관리를 활성화 시킵니다<br/>
![kakao](/images/oauth/ka10.png) 수집할 대상을 선택하고 수집 목적을 작성합니다.
자신의 프로젝트에 맞는 항목을 활성화 시키고 저장을 누릅니다.<br/>
![kakao](/images/oauth/ka11.png) 일반 탭에서 플랫폼 추가를 클릭합니다.<br/>
![kakao](/images/oauth/ka12.png) 현재 제가 진행하는 프로젝트는 웹이기 때문에 선택하였습니다.
그리고 도메인이 아직 없기 때문에 localhost로 설정하였습니다.<br/>
![kakao](/images/oauth/ka13.png) Redirect Path는 자신의 최상위 폴더에서 default값인 oauth를 지정하였습니다.<br/>
이렇게 Kakao Developer 설정이 끝이났습니다.
다음엔 설정한 정보들을 가지고 리다이렉션을 구현해보겠습니다.