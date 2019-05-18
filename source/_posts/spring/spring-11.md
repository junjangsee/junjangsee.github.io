---
title: SpringBoot - Devtools
date: 2019-05-07 21:43:17
tags: [SpringBoot, Devtools]
---

![images](/images/springboot/springboot.png)<br/>

# Devtools
데브툴즈는 스프링 부트가 제공하는 하나의 옵션입니다.
**의존성을 추가만** 해주면 자동으로 여러 기능들을 제공해줍니다.
하나하나 살펴봅시다. 참고문서 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-devtools-property-defaults)<br/>

## 의존성 추가
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```
위 의존성을 pom.xml에 추가합니다.<br/>
<br/>

## Cache 설정
웹 개발을 할 때 캐시가 적용되어있으면 바뀌지 않는 상황 때문에 개발 환경이 불편했던 적이 있을겁니다.
하지만 이러한 캐시 설정을 자동으로 꺼주어 개발에 더 집중할 수 있도록 해줍니다.
참고 자료 : [캐시삭제코드](https://github.com/spring-projects/spring-boot/blob/v2.1.1.RELEASE/spring-boot-project/spring-boot-devtools/src/main/java/org/springframework/boot/devtools/env/DevToolsPropertyDefaultsPostProcessor.java)<br/>
<br/>

## Auto Restart
개발시 수정된 코드를 build하면 자동으로 서버를 재구동시켜줍니다.
참고문서 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-devtools-restart)<br/>
<br/>

## Live Reload
Chrome같은 브라우저에서 서버가 재시작했을때 화면을 자동으로 갱신해주는 확장 프로그램입니다.
```
spring.devtools.liveload.enabled=false
```
필요가 없다면 위 코드로 자동 갱신을 비활성화 하면 됩니다.<br/>
<br/>

## Global Configuration
외부 설정시 우선순위가 가장 높게 측정됩니다.
참고문서 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-devtools-globalsettings)<br/>
<br/>

## Remote Application
참고문서 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-devtools-remote)
원격에 Application을 구동하고 Local에서 실행하는 기능입니다.
사실장 잘 쓰이지 않는 기능이고 위험하기 때문에 권장하지는 않습니다.<br/>
<br/>