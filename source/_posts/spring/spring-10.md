---
title: SpringBoot - Test(테스트)
date: 2019-05-05 23:43:45
tags: [SpringBoot, TDD, MockMVC, WebTestClient]
---

![images](/images/springboot/springboot.png)<br/>

# Test(테스트)
코드를 작성하고 해당 코드에 대해서 테스트가 필요할 때 사용하는 방법들을 공부해보겠습니다.
**TDD(테스트 주도형 개발)**가 중요한 만큼 테스트의 중요성이 높으니 코딩시 꼭 테스트하는 습관을 길러보도록 합시다.<br/>
<br/>

## 테스트 환경 구성하기
새로운 패키지에 Controller, Service 클래스를 생성합니다.<br/>
#### SampleController.class
```java
@RestController
public class SampleController {

    @Autowired
    private SampleService sampleService;

    @GetMapping("/hello")
    public String hello() {
        return "hello " + sampleService.getName();
    }
}
```

#### SampleService.class
```java
@Service
public class SampleService {
    public String getName() {
        return "junjang";
    }
}
```
![test](/images/springboot/test/te1.png) Service를 주입받고 hello 메소드를 구현합니다.<br/>
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```
![test](/images/springboot/test/te2.png) 테스트를 위해선 꼭 테스트 의존성이 추가되어 있어야 합니다.
pom.xml에 추가합니다.<br/>
그리고 테스트를 하기 위해 테스트클래스를 따로 만들어주어야 합니다.<br/>
<br/>

#### SampleControllerTest.class
```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ApplicationTests {

    @Test
    public void contextLoads() {
    }

}
```
기본적인 테스트 클래스의 구조는 위 코드와 같습니다.<br/>
<br/>

## 테스트를 하는 4가지 방법
### 1. MockMVC 
MockMVC ?
- 내장 Tomcat을 **생략한** 테스트입니다.
- 서블릿을 Mocking한 것이 구동됩니다.
<br/>

```java
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)
@AutoConfigureMockMvc
public class SampleControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void hello() throws Exception {
        mockMvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andExpect(content().string("hello junjang"))
                .andDo(print());

    }
```
![test](/images/springboot/test/te3.png) 
- **@SpringBootTest**에 환경을 **MOCK**으로 설정합니다.
- **@AutoConfigureMockMvc**를 추가하여야만 MockMVC를 사용할 수 있습니다.
- **@Autowired**로 MockMVC 의존성을 가져옵니다.
- **Get**방식으로 hello 경로로 status 200을 출력하고 문자열을 확인하여 출력합니다.

테스트를 실행하면 테스트로그가 결과값으로 출력됩니다.<br/>
<br/>

#### RANDOM_PORT
RANDOM_PORT ?
- 내장 Tomcat을 **포함한** 테스트입니다.
- 사용 가능한 포트를 자동으로 구동시켜줍니다.

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
```
<br/>
<br/>

### 2. TestRestTemplate
또 다른 테스트 방법입니다.<br/>

```java
import static org.assertj.core.api.Assertions.assertThat;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class SampleControllerTest {

    @Autowired
    TestRestTemplate testRestTemplate;

    @Test
    public void hello() throws Exception {
        String result = testRestTemplate.getForObject("/hello", String.class);
        assertThat(result).isEqualTo("hello junjang");
    }

}
```
![test](/images/springboot/test/te4.png) 
- @Autowired로 TestRestTemplate를 주입받습니다.
- 경로와 문자열을 result 변수에 선언하고 assertThat으로 비교합니다.<br/>
<br/>

#### @MockBean 
**@SpringBootTest**는 **@SpringBootApplication**이 구동할 때 등록하는 모든 빈을 전부 등록한 후 테스트를 하게 됩니다.
그렇게 되면 테스트가 너무 **무거워**지고 **효율성**이 떨어지기 됩니다.
그래서 **@MockBean**을 사용하여 필요한 빈을 Mock의 객체로 교체하여 해당 객체만 테스트시 적용되므로 **가볍고** **간편**하게 테스트가 가능합니다.<br/>

```java
import static org.assertj.core.api.Assertions.assertThat;
import static org.mockito.Mockito.when;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class SampleControllerTest {

    @Autowired
    TestRestTemplate testRestTemplate;

    @MockBean
    SampleService mockSampleService;

    @Test
    public void hello() throws Exception {
        when(mockSampleService.getName()).thenReturn("kimjunhyeung");
        String result = testRestTemplate.getForObject("/hello", String.class);
        assertThat(result).isEqualTo("hello kimjunhyeung");
    }

}
```
- Controller에서 사용하는 Service를 MockBean으로 주입합니다.
- when 함수를 사용하여 MockBean에 등록된 메소드를 return합니다.
- MockBean의 객체는 Mock에 등록된 빈이기 때문에 result도 받는 값이 MockBean의 return값과 동일해야 합니다.
<br/>
<br/>

### 3. WebTestClient
- Spring5의 WebFlux에 추가된 RestClient 중 하나로 Asynchronous(비동기식) 방식입니다.
(기존의 RestClient는 Synchronous(동기식))
- 사용하려면 webflux 의존성을 추가해야합니다.
- REST방식을 테스트하기에 매우 용이합니다.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```
![test](/images/springboot/test/te5.png) webflux 의존성을 추가합니다.<br/>
![test](/images/springboot/test/te6.png) 
- @Autowired를 통해 WebTestClient를 주입받습니다.
- 주입받은 WebTestClient로 테스트 합니다.<br/>
<br/>

### 4. 슬라이스 테스트
레이어 별로 잘라서 원하는 부분만 테스트하고 싶을 때 사용하며  가볍게 테스트 할 수 있습니다.
- JsonTest 
참고자료 : [스프링공식문서 - JsonTest](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-json-tests)
- WebMVCTest
참고자료 : [스프링공식문서 - WebMvcTest](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-testing-spring-boot-applications-testing-autoconfigured-mvc-tests)
 - @Controller, @ControllerAdvice, @JsonComponent, Converter, GenericConverter, Filter, WebMvcConfigurer, HandlerMethodArgumentResolver 만 빈 으로 등록됩니다.
 - 사용할 빈이 있다면 **@MockBean**으로 만들어서 다 채워줘야 함 그래야 Controller이 필요한 빈을 주입받을 수 있습니다.
 - @WebMvcTest는 MockMvc로 테스트 해야합니다.
- WebFluxTest
- DataJPATest
- etc..
<br/>

#### OutputCapture
- 로그를 비롯해서 Console에 찍히는 모든 것을 다 캡쳐합니다.
- 로그 메세지가 어떻게 찍히는지 테스트 해볼 수 있습니다.
<br/>

```java
Logger logger = LoggerFactory.getLogger(SampleController.class);

    @Autowired
    private SampleService sampleService;

    @GetMapping("/hello")
    public String hello() {
        logger.info("baeksu");
        System.out.println("job");
        return "hello " + sampleService.getName();
    }
```
hello 메소드에 logger, System.out.println를 선언합니다.<br/>
```java
@Rule
    public OutputCapture outputCapture = new OutputCapture();

    @Autowired
    WebTestClient webTestClient;

    @MockBean
    SampleService mockSampleService;

    @Test
    public void hello() throws Exception {
        when(mockSampleService.getName()).thenReturn("kimjunhyeung");

        webTestClient.get().uri("/hello").exchange().expectStatus().isOk()
                .expectBody(String.class).isEqualTo("hello kimjunhyeung");

        assertThat(outputCapture.toString())
                .contains("baeksu")
                .contains("job");
    }

```
- @Rule을 사용하여 OutputCapture 객체를 생성합니다.(public 항상 사용)
- outputCapture 함수를 사용하여 hello 메소드에 포함된 단어를 선언합니다.<br/>
<br/>

![test](/images/springboot/test/te7.png) 이상 없이 두 문자열이 테스트에 통과되고 출력된 것을 볼 수 있습니다.<br/>