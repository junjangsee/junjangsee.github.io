---
title: StudyRoom(SRoom) 프로젝트 - 01(SpringBoot 생성하기)
date: 2019-03-16 23:03:16
tags: SpringBoot
---


# SRoom 프로젝트

![images](/images/sroom/studyroom.jpg)<br/>

## SRoom 프로젝트를 시작한 이유

개발을 학습하기 위해 스터디를 해오면서 SNS를 통해 의사소통을 하다보니
스터디에 대한 목표 공유나 일정 공유가 원활하게 이루어지지 않아서 불편함이 많았습니다.
그래서 이러한 고민을 없애보자는 취지에서 만들게 되었습니다.
처음으로 SpringBoot를 활용하여 만들다 보니 jojoldu님의 정보를 참고하여 제작할 예정입니다.
<br>

## 개발 환경

- Mac OS
- IDE : IntelliJ IDEA Ultimate
- Gradle
- Terminal
- Java 8 
- SpringBoot 2.1.3
<br>

## SpringBoot Module(Project) 생성하기

SpringBoot & Gradle을 사용하여 생성을 하였습니다.
Gradle을 사용한 이유는 4점대 이후로 대폭 상승한 빌드속도 때문

![spring](/images/sroom/start1.png) Next를 클릭<br>
![spring](/images/sroom/start2.png) sroom으로 선정하고 Gragle 타입으로 선택 후 자바 버전은 8을 선택하였습니다.<br>
![spring](/images/sroom/start3.png) Lombok, Web, H2, JPA, MySql, Actuator를 선택
이번 프로젝트에서 JPA를 활용해볼 것이기 때문에 JPA까지 선택하였습니다.<br>
![spring](/images/sroom/start4.png) 마지막으로 Finish를 눌러 생성합니다.
<br/>

## 생성된 SpringBoot에 HelloWorld 출력하기

![spring](/images/sroom/start5.png) 생성이 되었다면 위와같은 폴더들이 있습니다. 저는 가독성을 위하여 Application으로 파일명을 바꾸었습니다.<br>
![spring](/images/sroom/hello.png) 그리고 기존 Spring의 방식과 동일하게 Controller를 생성하는데 이 때 JSON으로 보내주기 위하여 RestController를 사용하였습니다.
<br/>

## org.apache.catalina.LifecycleException: Protocol handler start failed 에러가 뜬다면?

![spring](/images/sroom/error.png) 이 에러는 내장된 Tomcat을 사용할 때 이미 사용된 port를 재사용 할 때 나타나는 오류입니다.<br/>
해결방법은 resource 폴더에 application.properties 파일 내에서 포트를 바꾸어주면 됩니다.<br>
![spring](/images/sroom/portchange.png) server.port='사용할 포트 번호' 를 통해 바꾸어준 모습입니다.<br/><br/>
그리고 띄워진 결과
![spring](/images/sroom/end.png)<br/>

## Git Repository에 등록

![spring](/images/sroom/gitclone.png) 새로운 Git Repository를 생성하여 Terminal 명령어를 통해 Push하였습니다.<br>
다음 시간엔 AWS를 활용하여 EC2 & RDS를 구축해보겠습니다.