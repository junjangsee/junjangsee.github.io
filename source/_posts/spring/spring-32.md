---
title: SpringBoot - RestClient
date: 2019-05-16 20:43:59
tags: [SpringBoot, RestClient, RestTemplate, WebClient]
---

![images](/images/springboot/springboot.png)<br/>

# RestClient
Rest를 클라이언트에서 요청하였을 때 어떻게 응답할지 결정하는  방식입니다.
RestClient에는 두 종류가 있는데 각각의 특징과 사용법을 학습해보겠습니다.
- **RestTemplate**
 - Blocking I/O 기반의 Synchronous(동기식) API입니다.
 - RestTemplateAutoConfiguration에서 자동설정 됩니다.
 - 프로젝트에 **spring-web** 의존성이 있다면 RestTemplate​Builder​를 빈으로 등록합니다.
 - 참고자료 : [스프링공식문서](https://docs.spring.io/spring/docs/current/spring-framework-reference/integration.html#rest-client-access)

- **WebClient**
 - Non-Blocking I/O 기반의 Asynchronous(비동기식) API
 - WebClientAutoConfiguration에서 자동설정 됩니다.
 - 프로젝트에 **spring-webflux** 의존성이 있다면 WebClient.​Builder​를 빈으로 등록합니다.
 - 참고자료 : [스프링공식문서](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html#webflux-client)
 <br/>
 <br/>

 ## RestTemplate 사용하기
 **동기식**으로 응답하는 RestTemplate을 사용해보겠습니다. 
 Web 의존성을 추가한 프로젝트를 생성합니다.<br/>
 ### Controller 클래스
 ```java
@RestController
public class SampleController {

    @GetMapping("/hello")
    public String hello() throws InterruptedException {
        Thread.sleep(5000l);
        return "hello";
    }

    @GetMapping("/world")
    public String world() throws InterruptedException {
        Thread.sleep(3000l);
        return "world";
    }
}
```
![RestClient](/images/springboot/resttemplate/rest1.png) Thread를 활용하여 hello는 5초, world는 3초의 시간을 줍니다.
그리고 클라이언트 입장에서 값을 받기 위해 ApplicationRunner를 사용할 클래스를 생성하고 생성된 클래스에 구현합니다.<br/>
<br/>

### ApplicationRunner 클래스
클라이언트의 요청을 가정하여 두 가지의 방법을 사용해보겠습니다.
<br/>

#### RestTemplate
```java
@Component
public class RestRunner implements ApplicationRunner {

    @Autowired
    RestTemplateBuilder restTemplateBuilder;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        RestTemplate restTemplate = restTemplateBuilder.build();

        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        String helloResult = restTemplate.getForObject("http://localhost:8080/hello", String.class);
        System.out.println(helloResult);

        String worldResult = restTemplate.getForObject("http://localhost:8080/world", String.class);
        System.out.println(worldResult);

        stopWatch.stop();
        System.out.println(stopWatch.prettyPrint());
    }
}
```
RestTemplate를 사용할 떈 RestTemplateBuilder 의존성을 주입받습니다.
그리고 build()함수를 사용하여 변수에 담고 변수의 기능을 사용해서 REST 요청시 String 값을 가져오도록 하고 출력합니다.
![RestClient](/images/springboot/resttemplate/rest2.png) 위처럼 맨 처음 요청한 **hello**의 5초 + 다음 **world** 요청인 3초 = 총 8초 정도의 시간에 응답을 완료했다고 출력됩니다.<br/>
이처럼 RestTemplate은 요청이 들어오면 다음 코드로 넘어가지 않고 완료되면 넘어가는 동기식 응답을 하는 것을 볼 수 있습니다.<br/>
<br/>

#### WebClient
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```
webflux 의존성을 추가해야만 사용할 수 있습니다.
<br/>
```java
@Component
public class RestRunner implements ApplicationRunner {

    @Autowired
    WebClient.Builder builder;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        WebClient webClient = builder.build();

        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        Mono<String> helloMono = webClient.get().uri("http://localhost:8080/hello") 
                .retrieve() 
                .bodyToMono(String.class);

        helloMono.subscribe(s -> {
            System.out.println(s);

            if(stopWatch.isRunning()) { 
                stopWatch.stop();
            }

            System.out.println(stopWatch.prettyPrint());
            stopWatch.start();
        });


        Mono<String> worldMono = webClient.get().uri("http://localhost:8080/world")
                .retrieve()
                .bodyToMono(String.class);

        worldMono.subscribe(s -> {
            System.out.println(s);

            if(stopWatch.isRunning()) {
                stopWatch.stop();
            }

            System.out.println(stopWatch.prettyPrint());
            stopWatch.start();
        });
    }
}
```
WebClient는 WebClient.Builder로 의존성을 주입받습니다.
여기선 **Stream API**를 사용할 것인데 Subscribe하기 전 까지 작동하지 않고 대기하다가 Callback이 오면 로직을 수행합니다.
현재 로직에선 Rest 요청이 들어오면 String을 Mono 타입으로 변경 후 대기하다가 subscribe를 하게되면 출력한다고 생각하면 됩니다.
![RestClient](/images/springboot/resttemplate/rest3.png) 먼저 출력된 world 3초 + hello 2초 = 총 5초가 소요되었고 비동기로 출력된 것을 볼 수 있습니다.<br/>
이처럼 동기식보다 시간적 효율성이 높고 응답 시간도 빨라 개발자들은 보통 WebClient를 선호합니다.<br/>