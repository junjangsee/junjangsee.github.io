---
title: SpringBoot - RestClient 커스텀
date: 2019-05-18 16:40:43
tags: [SpringBoot, RestClient, RestTemplate, WebClient]
---

![images](/images/springboot/springboot.png)<br/>

# RestClient 커스텀
이번 주제는 저번에 다루었던 RestClient를 커스터마이징 하는 방법에 대해 공부해보려 합니다. 때문에 이전 RestClient 주제에 대한 내용에 대한 이해와 코드가 있어야 합니다. [RestClient](https://junjangsee.github.io/2019/05/16/spring/spring-32/) 포스팅을 참고하셔서 이번 주제를 학습하시면 됩니다.<br/>
<br/>

## HTTP 클라이언트
### WebClient
- 기본으로 Reactor Netty의 HTTP 클라이언트 사용
- 커스터마이징
  - 로컬 커스터마이징
  - 글로벌 커스터마이징
    - WebClientCustomizer
    - 빈 재정의

### RestTemplate
- 기본으로 java.net.HttpURLConnection 사용
- 커스터마이징
  - 로컬 커스터마이징
  - 글로벌 커스터마이징
    - RestTemplateCustomizer
    - 빈 재정의



## WebClient
### 지역 커스터마이징
지역적으로 선언한 webClient에 baseUrl을 설정하고 build하고 하위 Url 요청 시 baseUrl은 제외하고 요청 할 수 있습니다.
```java
@Configuration
public class RestRunner implements ApplicationRunner {

    @Autowired
    WebClient.Builder builder;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        WebClient webClient = builder
                .baseUrl("http://localhost:8080")
                .build();

        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        Mono<String> helloMono = webClient.get().uri("/hello")
                .retrieve()
                .bodyToMono(String.class);

        helloMono.subscribe(s -> {
            System.out.println(s);

            if (stopWatch.isRunning()) {
                stopWatch.stop();
            }

            System.out.println(stopWatch.prettyPrint());
            stopWatch.start();
        });

        Mono<String> worldMono = webClient.get().uri("/world")
                .retrieve()
                .bodyToMono(String.class);

        worldMono.subscribe(s -> {
            System.out.println(s);

            if (stopWatch.isRunning()) {
                stopWatch.stop();
            }

            System.out.println(stopWatch.prettyPrint());
            stopWatch.start();
        });

    }
}
```
builder의 기능을 사용할 때 지역적으로 baseUrl에 고정 Url을 선언하여 각각의 Url에는 고정을 제외한 해당 Url만 선언합니다.<br/>
![RestClient](/images/springboot/resttemplate/rest4.png) baseUrl을 선언하고 해당되는 Url만 선언한 모습입니다.<br/>
<br/>

### 전역 커스터마이징
WebClientCustomizer를 사용하여 전역에서 커스텀한 빈 객체를 사용할 수 있도록 합니다.<br/>
```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public WebClientCustomizer webClientCustomizer() {
        return new WebClientCustomizer() {
            @Override
            public void customize(WebClient.Builder webClientBuilder) {
                webClientBuilder.baseUrl("http://localhost:8080");
            }
        };
    }
}
```
<br/>
![RestClient](/images/springboot/resttemplate/rest6.png) WebClientCustomizer를 빈으로 등록하여 baseUrl을 선언하였습니다.<br/>
![RestClient](/images/springboot/resttemplate/rest5.png) 람다식으로도 표현 가능합니다.<br/>
<br/>

## RestTemplate
RestTemplate이 기존에 사용하던 javaConnection이 아닌 Apache HttpClient로 변경해서 사용하는 방법에 대해 알아보겠습니다. **RestTemplateCustomizer**를 사용해 전역 커스텀을 합니다.<br/> 

### 의존성 추가
```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
</dependency>
```
![RestClient](/images/springboot/resttemplate/rest7.png) org.apache.httpcomponents의 httpclient 의존성을 추가해줍니다.<br/>
<br/>

### Apache HttpClient로 변경
```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public RestTemplateCustomizer restTemplateCustomizer() {
        return new RestTemplateCustomizer() {
            @Override
            public void customize(RestTemplate restTemplate) {
                restTemplate.setRequestFactory(new HttpComponentsClientHttpRequestFactory());
            }
        };
    }
}
```
![RestClient](/images/springboot/resttemplate/rest8.png) RestTemplateCustomizer을 빈 등록하여 커스텀합니다.<br/>
![RestClient](/images/springboot/resttemplate/rest9.png) 람다식으로 표현 가능합니다.<br/>