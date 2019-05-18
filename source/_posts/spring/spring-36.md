---
title: SpringBoot - Actuator Admin
date: 2019-05-18 19:21:18
tags: [SpringBoot, Actuator, Admin]
---

![images](/images/springboot/springboot.png)<br/>

# Actuator Admin
스프링 진영에서 제공하는 프로젝트가 아니고 제3자가 오픈소스로 제공하는 애플리케이션입니다.
보다 편리하게 Actuator를 확인한 수 있습니다.
- Client, Server 두개 프로젝트가 필요합니다.

참고자료 : [Actuator Admin 홈페이지](https://github.com/codecentric/spring-boot-admin)<br/>
<br/>

## Actuator Server 프로젝트
### 의존성 추가
```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-server</artifactId>
    <version>2.1.4</version>
</dependency>
```
![Actuator](/images/springboot/actuator/ac15.png) 의존성을 추가합니다.<br/>
<br/>

### @EnableAdminServer
```java
@SpringBootApplication
@EnableAdminServer
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```
![Actuator](/images/springboot/actuator/ac16.png)
@EnableAdminServer를 애플리케이션 클래스에 추가합니다.<br/>
<br/>

## Actuator Client 프로젝트
### 의존성 추가
```xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-starter-client</artifactId>
    <version>2.1.4</version>
</dependency>
```
![Actuator](/images/springboot/actuator/ac17.png) 의존성을 추가합니다.<br/>
<br/>

### application.properties 설정
```
management.endpoints.web.exposure.include​=​*
spring.boot.admin.client.url​=​http://localhost:8080
server.port=18080
```
![Actuator](/images/springboot/actuator/ac18.png) endpoints를 전부 보여주는 설정, 클라이언트의 주소를 설정, 포트가 겹치면 충돌이 나기 때문에 클라이언트의 포트를 변경하는 설정을 합니다.<br/>
<br/>

## Spring Boot Admin 접속
> localhost:8080/

기본 루트 경로로 **서버 포트**를 입력합니다.<br/>
![Actuator](/images/springboot/actuator/ac19.png) 그러면 구동중인 애플리케이션을 확인할 수 있습니다.
애플리케이션을 클릭합니다.<br/>
![Actuator](/images/springboot/actuator/ac20.png) 아래와 같이 애플리케이션에 대한 정보들을 가독성 좋게 확인할 수 있습니다.
내부의 기능들은 공식 홈페이지에서 참고하시면 되겠습니다.<br/>