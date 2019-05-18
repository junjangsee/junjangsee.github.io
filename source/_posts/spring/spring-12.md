---
title: SpringBoot - MVC Setting
date: 2019-05-07 22:16:12
tags: [SpringBoot, MVC]
---

![images](/images/springboot/springboot.png)<br/>

# SpringBoot MVC
스프링부트는 기존 Spring에서 MVC를 세팅할 때와는 다르게 기본 설정 덕분에 따로 세팅하지 않아도 바로 MVC 개발이 가능합니다.
예제를 만들어보면서 MVC 패턴이 세팅없이 바로 구현 가능한지 테스트 해보겠습니다.<br/>
<br/>

## MVC Exam
전체 완성된 예제 코드는 각 주제 하단에 있으니 참고하시면 되겠습니다.
<br/>

### Test 클래스
![MVCSetting](/images/springboot/mvcsetting/mvcse1.png) 슬라이스 테스트로 Web만 테스트 하기 위해 @WebMvcTest를 활용합니다.<br/>
![MVCSetting](/images/springboot/mvcsetting/mvcse2.png) 테스트할 클래스를 선언해줄 때 동일한 클래스를 만들기 번거로우므로 임의의 클래스명을 붙인 후 자동생성 기능을 이용하여 생성합니다.
단축키(맥 OS기준) : option + enter<br/>
![MVCSetting](/images/springboot/mvcsetting/mvcse3.png) main 패키지에 있는 곳에 생성해줍니다.<br/>
![MVCSetting](/images/springboot/mvcsetting/mvcse4.png) 다시 테스트클래스로 돌아와서 Web은 보통 MockMVC 객체로 테스트 하기 때문에 의존성을 가져옵니다.
그리고 hello 문자열을 200이 출력되게끔 테스트 코드를 작성합니다.<br/>
```java
@RunWith(SpringRunner.class)
@WebMvcTest(UserController.class)
public class UserControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void hello() throws Exception{
        mockMvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andExpect(content().string("hello"));
    }
}
```
테스트 클래스 예제 코드는 위와 같습니다.<br/>
<br/>

### main 클래스
![MVCSetting](/images/springboot/mvcsetting/mvcse5.png) 특별한 것 잆이 Controller가 선언된 클래스입니다.<br/>
```java
@RestController
public class UserController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }

}
```
메인 클래스 예제 코드는 위와 같습니다.<br/>
그 후 테스트를 실행하면 이상없이 테스트가 완료될 것입니다.
이처럼 SpringBoot는 기본적으로 MVC 패턴을 바로 구현할 수 있도록 기본 세팅이 되어있으며 개발자가 개발에만 더 집중할 수 있도록 해줍니다.<br/>
<br/>

## 기본 설정 의존성은 어디에?
![MVCSetting](/images/springboot/mvcsetting/mvcse6.png) 이전 의존성 파트에서 공부했던 WebMvcAutoConfiguration에 세팅이 들어가있습니다.
많은 세팅이 있지만 너무 깊기 때문에 따로 파고들진 않겠습니다.<br/>
<br/>

## MVC 확장하기
MVC 기본 설정 말고도 추가적으로 설정을 하고 싶을 때 사용할 수 있는 방법입니다.<br/>

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
}
```
![MVCSetting](/images/springboot/mvcsetting/mvcse7.png) WebMvcConfigurer를 구현한 클래스 내부에 추가적으로 필요한 설정의 Callback 메소드를 정의하여 사용하면 됩니다.<br/>
<br/>

## MVC 재정의하기
아래와 같이 `@EnableWebMvc` 를 같이 사용하면 모든 스프링 MVC 설정이 다 사라지고 재정의 할 수 있습니다.<br/>

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
}
```