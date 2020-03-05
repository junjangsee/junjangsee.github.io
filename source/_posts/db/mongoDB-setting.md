---
title: MongoDB 세팅방법
date: 2020-03-05 23:51:24
tags: [Database, MongoDB]
---

![images](/images/db/mongoDB.png)<br/>

# MongoDB 회원가입

먼저 MongoDB 공식 사이트에 접속합니다. [공식 홈페이지](https://www.mongodb.com/)
<br/>

![setting](/images/db/mon1.png) 화면 중앙에 보이는 `Start free` 버튼을 클릭합니다.<br/>
![setting](/images/db/mon2.png) 이메일, 비밀번호 등 간단한 가입 절차를 걸친 후 하단의 `Get started free`버튼을 클릭합니다.<br/>

# 클러스터 생성

![setting](/images/db/mon3.png) 가입이 되었으면 로그인을 한 후 화면 중앙에 `Build a Cluster`를 클릭합니다.<br/>
![setting](/images/db/mon4.png) 무료로 사용하기 위해 맨 좌측의 Free를 이용합니다.<br/>
![setting](/images/db/mon5.png) AWS와 프리티어인 국가를 선택하는데 현재 위치인 한국과 그나마 가까운 싱가폴을 사용합니다.<br/>
![setting](/images/db/mon6.png) 티어 또한 `M0 Sandbox`를 클릭하여 프리티어로 설정합니다.<br/>
![setting](/images/db/mon7.png)
![setting](/images/db/mon8.png) 클러스터의 제목을 입력하고 마지막으로 클러스터를 생성합니다.<br/><br/>

# 클러스터 Connection Key 받기

![setting](/images/db/mon9.png) 클러스터를 생성하게 되면 1~3분 내로 생성된다는 문구를 확인할 수 있습니다.<br/>
![setting](/images/db/mon10.png) 다 생성되면 위와 같은 화면을 볼 수 있습니다.<br/>
![setting](/images/db/mon11.png) 생성된 클러스터에서 connect를 클릭합니다.<br/>
![setting](/images/db/mon12.png) 두 가지 입력사항이 나오는데 자신의 `IP와 DBuser 계정 생성`입니다. 생성을 해주고 커넥션 메소드를 선택합니다.<br/>
![setting](/images/db/mon13.png) 현재 운영할 어플리케이션을 선택해줍니다.<br/>
![setting](/images/db/mon14.png) 운영하는 환경을 선택하고 하단의 Key를 copy하여 위에서 생성했던 `DBuser 계정`을 Key 안에 넣어준 후 사용하면 됩니다.<br/>
