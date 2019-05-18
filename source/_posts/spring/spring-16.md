---
title: SpringBoot - MVC WebJar(웹Jar)
date: 2019-05-09 17:46:10
tags: [SpringBoot, MVC, WebJar]
---

![images](/images/springboot/springboot.png)<br/>

# WebJar(웹Jar)
각종 라이브러리를 JAR파일로 추가가 가능하게 해줍니다.
jQuery를 예시로 공부해보겠습니다.
- Mapping(매핑)은 기본적으로 ​/webjars/**​ 입니다.<br/>
<br/>

## jQuery JAR 추가해보기
라이브러리를 가져옵니다. 
[mvnrepository](https://mvnrepository.com/)

![WebJar](/images/springboot/webjar/webj1.png) jQuery의 3.3.1 버전의 jar형식의 라이브러리를 pom.xml에 추가합니다.<br/>
<br/>

## jQuery 사용하기

```html
<script ​src=​"/webjars/jquery/3.3.1/dist/jquery.min.js"​></script>
<script>
    $(function() {
        console.log("ready!");
    });
</script>
```
![WebJar](/images/springboot/webjar/webj2.png) 그리고 HTML에 매핑경로에 맞춰 jar를 추가한 후 간단한 alert 창을 출력합니다.<br/>
![WebJar](/images/springboot/webjar/webj3.png) 정적 리소스 HTML경로로 들어가게 되면 jar가 인식되어 경고창이 뜨는 것을 확인할 수 있습니다.
혹시 정적 리소스에 대해 알고싶으시다면 [정적 리소스](https://junjangsee.github.io/2019/05/08/spring/spring-15/)를 참고해주시면 됩니다.<br/>
하지만 jar를 추가할 때 버전을 포함하여 추가하게 되면 버전이 업데이트 될 때마다 코드를 바꿔야하는 소요가 있습니다. 그 것을 해결할 방법은 없을까요?<br/>
<br/>

## webjars-locator-core 의존성을 추가하여 버전 생략하기

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>webjars-locator-core</artifactId>
    <version>0.36</version>
</dependency>
```
![WebJar](/images/springboot/webjar/webj4.png) 위 의존성을 추가하면 버전을 생략하고 jar파일을 선언할 수 있게됩니다.<br/>
![WebJar](/images/springboot/webjar/webj5.png) 3.3.1 버전을 생략한 모습입니다. 물론 출력도 이상 없습니다.<br/>
