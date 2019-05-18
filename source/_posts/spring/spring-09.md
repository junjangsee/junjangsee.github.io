---
title: SpringBoot - logging(로깅)
date: 2019-05-05 21:41:44
tags: [SpringBoot, ​Logback, Log4J2]
---

![images](/images/springboot/springboot.png)<br/>

# logging(로깅)
로그를 출력할 때 사용하는 설정 값들을 공부해보도록 하겠습니다.<br/>
<br/>
## 스프링 부트 기본 로거 설정하기
### 로깅 퍼사드
- 로거를 바꿀 수 있게 해준다.
- 종류 : Commons Logging​, SLF4j

### 로거
- 실제로 로그를 찍는 역할을 한다.
- 종류 : JUL, Log4J2, ​Logback
<br/>

스프링 부트는 Commons Logging을 기본적으로 사용하고 있으며
Commons Logging을 사용해도 SLF4j로 가고 Logback로 가게 됩니다. 결국 **Logback**이 최종적으로 로깅을 합니다.<br/>
<br/>

### Spring 5.x Version
스프링 5점대 버전부터 많은 변경이 있었습니다.
참고 : [스프링공식문서](https://docs.spring.io/spring/docs/5.0.0.RC3/spring-framework-reference/overview.html#overview-logging)

- Spring-JCL(자카르타 커먼스 로깅)이 기본적으로 구현되어 있고 Commins Loggins이 SLF4j 혹은 Log4j2 로 자동으로 보내도록 설정해줍니다.
- pom.xml에 exclusion을 하지 않아도 됩니다.(2.x 부터)
- Log4j2가 있으면 Log4j2, SLF4j만 있으면 SLF4j를 사용합니다.<br/>
<br/>

## 디버그 모드로 로깅하는 방법
- 기본 포맷
- 핵심 라이브러리 디버그(embeded container, Hibernate, Spring Boot)
 - program arguments: --debug
 - JVM Option: -Ddebug
- 전체 디버그
 - program arguments: --trace

![logger](/images/springboot/logger/log1.png)
<br/>

## 로깅을 컬러로 출력하기
컬러풀 하게 출력을 하고싶을 때 사용합니다.
참고자료 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-logging-color-coded-output)<br/>
```
spring.output.ansi.enabled=always
```
위 코드를 properties에 선언해주면 컬러로 출력됩니다.<br/>
<br/>

## 로깅을 파일로 출력하기
로깅을 파일로 만들어서 출력하고 싶을 때 사용합니다.
참고 자료 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-logging-file-output)<br/>
* properties에서 `logging.file` 또는 `logging.path`로 설정합니다.
<br/>

```
logging.path=logs
```
![logger](/images/springboot/logger/log2.png) 위 코드로 logs라는 경로에 로깅 파일을 생성하게 됩니다.<br/>
<br/>

## 로깅 레벨을 조정하기
로그시 로그레벨을 지정하여 사용이 가능합니다.<br/>
<br/>

### properties에서 조정하기
```
logging.level.me.junjang.applicationstarted=DEBUG
```
```java
private Logger logger = LoggerFactory.getLogger(SampleRunner.class);

logger.info(hello);
```
![logger](/images/springboot/logger/log4.png) 디버그로 레벨을 조정하고 SLF4j를 import하여 log를 찍은 결과 
디버그가 출력된 것을 확인할 수 있습니다.<br/>
<br/>

### logback-spring.xml으로 조정하기
logback-spring.xml을 resources에 생성하여 사용합니다.
properties보다 좀더 많은 커스텀 기능을 사용할 수 있습니다.
참고자료 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-custom-log-configuration)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<include resource="org/springframework/boot/logging/logback/base.xml"/>
	<logger name="org.springframework.web" level="DEBUG"/>
</configuration>
```
![logger](/images/springboot/logger/log5.png) logger name에 자신 패키지 경로를 명시하여줍니다.<br/>
<br/>

## 로깅을 Log4j로 변경하기
pom.xml에 기본적인 로깅을 빼고 Log4j로 바꾸는 방법입니다.<br/>
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```
![logger](/images/springboot/logger/log6.png) web의존성에 들어있던 로깅을 exclusion을 사용하여 빼고 새로운 log4j2 의존성을 추가하였습니다.
exclusion을 한 이유는 5점대에서 기본적으로 사용하는 SLF4j를 제거하고 Log4j2를 사용할 것이기 때문에 바꾼 것입니다.
만약 바꾸지 않는다면 따로 exclusion할 필요없이 기본적인 SLF4j를 사용하게 됩니다.<br/>