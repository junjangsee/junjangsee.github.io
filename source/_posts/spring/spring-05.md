---
title: SpringBoot - JAR
date: 2019-04-25 20:16:23
tags: [SpringBoot, JAR]
---

![images](/images/springboot/springboot.png)<br/>

# JAR 파일 생성하기
JAR를 실행하기 전에 먼저 해당 프로젝트의 JAR 파일을 생성해야합니다.<br/>
```bash
mvn package
```
위 명령어를 실행하면 target폴더에 jar파일이 생성됩니다.<br/>
![jar](/images/springboot/jar/jar1.png) 위 처럼 intelli J 자체 기능을 사용하여 생성할 수 있습니다.<br/>
![jar](/images/springboot/jar/jar2.png) 생성하면 target 하위에 jar파일이 생성된 것을 볼 수 있습니다.<br/>
```bash
java -jar xxx.jar
```
위 명령어로 jar파일을 실행시킵니다.<br/>
혹시 테스트 및 디버그를 포함히지 않고 jar를 생성하려면
```bash
mvn package -DskipTests
```
```bash
-Ddebug
```
위 명령어들을 입력하고 jar를 생성하면 됩니다.<br/>