---
title: AWS Certificate Manager를 이용한 SSL인증서 발급받기
date: 2019-09-29 17:57:24
tags: [AWS, Certificate Manager, SSL]
---

![images](/images/network/certificate_manager.png)<br/>

# Certificate Manager
Https를 활용하기 위해서는 SSL인증서를 등록해야합니다. 이 때 사설 인증서를 이용하거나, 대리로 발급해주고 관리해주는 기업들을 활용하는 등 여러 방법이 있습니다. 이 방법들은 비용이 발생하게 되는 큰 단점이 있습니다. AWS에서는 `Certificate Manager`를 사용하여 인증서를 **무료**로 발급받고 관리할 수 있습니다.<br/>
<br/>

## 발급 과정
![images](/images/network/ssl1.png) AWS의 Certificate Manager에 접속하여 시작하기를 클릭합니다.<br/>
<br/>

![images](/images/network/ssl2.png) 공인인증서 요청을 통해 인증서를 요청하겠습니다.<br/>
<br/>

![images](/images/network/ssl3.png) 등록한 도메인에 추가를 해야합니다.(즉, 이미 등록된 도메인이 존재해야합니다.)
`*`을 입력하면 해당되는 모든 경로를 포함한다는 의미입니다.<br/>
<br/>

![images](/images/network/ssl4.png) 인증서 발급시 검증 방법을 선택합니다. 저는 DNS를 선택하였습니다.<br/>
<br/>

![images](/images/network/ssl5.png) 확인 및 요청을 클릭합니다.<br/>
<br/>

![images](/images/network/ssl6.png) 요청이 되었다면 Route 53에 CNAME을 등록해줘야 하는데, 화살표를 클릭하여 하단을 보면 자동으로 레코드를 생성할 수 있는 버튼이 있습니다. 클릭해서 쉽게 생성해줍니다.<br/>
<br/>

![images](/images/network/ssl7.png) 성공하면 위와 같은 메시지가 출력됩니다.<br/>
<br/>

![images](/images/network/ssl8.png) SSL 인증서는 생성후 바로 등록되는 것이 아니기 때문에 현재 보류 상태인 것을 확인할 수 있습니다.<br/>
<br/>

![images](/images/network/ssl9.png) 일정 시간(저는 5분 정도 기다렸습니다.)이 지나면 발급이 완료되었다는 상태가 출력됩니다. 이렇게 쉽게 SSL을 발급받을 수 있습니다.<br/>
<br/>
이 발급받은 SSL은 EC2의 `로드밸런싱`에서 사용이 가능합니다. 차후 기회가 된다면 적용하는 방법도 알아보는 시간을 가지겠습니다.<br/>
