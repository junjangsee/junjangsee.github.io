---
title: SpringBoot - MVC CORS
date: 2019-05-12 15:59:04
tags: [SpringBoot, MVC, CORS, SOP]
---

![images](/images/springboot/springboot.png)<br/>

# CORS(Cross-Origin Resource Sharing)
SOP(Single-Origin Policy)를 우회하기 위한 기술이며 서로 다른 Origin과 Resource를 공유하기 위한 방법입니다.<br/>
<br/>

## SOP, CORS
- SOP : 같은 Origin끼리만 Resource를 공유합니다.
- CORS : 다른 Origin들과 Resource를 공유합니다.
<br/>
<br/>

## Origin이란?
총 세가지 조합으로서 이 세가지 사항이 전부 만족해야 Resource를 호출할 수 있습니다.
- URI 스키마 (http, https)
- hostname (junjang.me, localhost)
- 포트 (8080, 18080)
<br/>
<br/>

## CORS 실습해보기
실습을 위해선 Server, Client 두 개의 프로젝트를 생성해야 합니다.<br/>

### Server
```java
@SpringBootApplication
@RestController
public class Application {

    @GetMapping("/hello")
    public String hello() {
        return "Hello";
    }

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```
![cors](/images/springboot/cors/cors1.png) Hello를 출력하는 REST API를 생성합니다.<br/>
<br/>

### Client
```
server.port=18080
```
![cors](/images/springboot/cors/cors2.png) 다른 포트를 설정합니다.<br/>
![cors](/images/springboot/cors/cors3.png) 인덱스 페이지를 생성합니다.<br/>
```xml
<dependency>
    <groupId>org.webjars.bower</groupId>
    <artifactId>jquery</artifactId>
    <version>3.3.1</version>
</dependency>
```
![cors](/images/springboot/cors/cors4.png) jQuery를 사용하여 ajax 통신을 하기 위해 의존성을 추가합니다.<br/>
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>
    CORS Client
</h1>
<script src="/webjars/jquery/3.3.1/dist/jquery.min.js"></script>
<script>
    $(function () {
        $.ajax("http://localhost:8080/hello")
            .done(function (msg) {
                alert(msg);
            })
            .fail(function () {
                alert("fail");
            });
    })
</script>
</body>
</html>
```
![cors](/images/springboot/cors/cors5.png) 생성했던 인덱스 페이지에 jQuery를 사용하여 8080서버 포트로 통신이 성공하면 메시지를, 실패하면 fail 경고창을 출력하도록 합니다. 
그리고 클라이언트에서 요청을 합니다.<br/>
![cors](/images/springboot/cors/cors6.png) 당연히 실패가 뜹니다.
server의 Access-Control-Allow-Origin Header 값을 Client에 보내서 매칭이 되어야 하기 때문입니다.<br/>
<br/>

### Server
```java
@CrossOrigin(origins = "http://localhost:18080")
```
![cors](/images/springboot/cors/cors7.png) @CrossOrigin 어노테이션을 통해서 Header 값을 보내 매칭을 시켜주면 됩니다.
이는 메소드 뿐만 아니라 Controller에도 사용할 수 있습니다.<br/>
![cors](/images/springboot/cors/cors8.png) 그렇게 되면 메시지가 Hello로 출력되는 것을 볼 수 있습니다.
매칭이 된 것입니다.<br/>
하지만 메소드가 한 두개가 아니라면 일일히 매칭해주는 것도 소모적인 일이기 때문에 WebMvcConfigurer를 통해 커스텀을 해줍니다.<br/>
<br/>

#### addCorsMappings 
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://localhost:18080");
    }
}

```
![cors](/images/springboot/cors/cors9.png) addCorsMappings을 통해서 전체 REST API에 대한 Origin을 매칭할 수 있습니다.
**addMapping**은 `/**`로 전체 경로를, **allowedOrigins**는 Client의 `주소`를 선언해줍니다.
이렇게 되면 전체 경로에 대해서 한 번에 매칭이 되기 때문에 따로 어노테이션을 선언하지 않아도 됩니다.<br/>