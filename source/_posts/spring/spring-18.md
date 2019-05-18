---
title: SpringBoot - MVC Thymeleaf
date: 2019-05-09 22:17:03
tags: [SpringBoot, MVC, Thymeleaf, JSP]
---

![images](/images/springboot/springboot.png)<br/>

# Thymeleaf(타임리프) 
MVC에서 동적으로 컨텐츠를 생성하여 뷰로 출력시키는 역할을 하는 템플릿 엔진입니다.
- 템플릿 위치 : /src/main/resources/​template/
- 홈페이지 : [타임리프 홈페이지](https://www.thymeleaf.org/)
- 5분 타임리프 익히기 : [5분 타임리프](https://www.thymeleaf.org/doc/articles/standarddialect5minutes.html)<br/>
<br/>

## JSP를 지양하는 이유
- JSP는 자동설정을 지원하지 않는데다가 SpringBoot가 지향하는 독립적으로 실행가능한 임베디드 톰캣으로 빠르고 쉽게 개발하고 배포하는 의도와 다릅니다.
- WAR파일로 패키징하여야 합니다.(물론 해결 방법은 있습니다.)
- JBOSS에서 만든 가장 최신 Servlet 엔진 Undertow는 JSP를 지원하지 않습니다.
참고문서 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-jsp-limitations)
<br/>
<br/>

## 타임리프 의존성 추가하기
타임리프를 사용하기 위해선 의존성을 추가해주어야 합니다.<br/>
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
![Thymeleaf](/images/springboot/thymeleaf/tym1.png) 의존성을 추가한 모습입니다.<br/>
<br/>

## 테스트 클래스
뷰로 출력하기 Controller를 테스트합니다.<br/>
```java
@RunWith(SpringRunner.class)
@WebMvcTest(SampleController.class)
public class SampleControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void hello() throws Exception {
        // 요청 "/hello"
        // 응답 모델 name : junjang
        // 응답 뷰 이름 : hello
        mockMvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andDo(print())
                .andExpect(view().name("hello"))
                .andExpect(model().attribute("name", is("junjang")));
    }
}
```
![Thymeleaf](/images/springboot/thymeleaf/tym2.png) 먼저 단순하게 응답시 Model과 View 이름을 확인하기 위한 코드입니다.
테스트를 실행시 **404에러**가 뜰텐데 이는 Controller에 명시한 경로가 없기 때문입니다.
Controller를 구현하러 이동합니다.<br/>
<br/>

### Controller 클래스

```java
@Controller
public class SampleController {

    @GetMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("name", "junjang");
        return "hello";
    }
}
```
![Thymeleaf](/images/springboot/thymeleaf/tym3.png) **@Controller**를 통해 View에 **Model**로 junjang이란 name의 **데이터**를 **hello**라는 뷰 이름으로 보냅니다.
테스트를 실행하면 템플릿인 hello가 없다는 오류가 출력됩니다. 아직 우리는 hello.html을 만들지 않았기 때문에 이제 만들러 가봅시다.<br/>
![Thymeleaf](/images/springboot/thymeleaf/tym4.png) /src/main/resources/​template/ 경로에 hello.html을 생성합니다.<br/>
<br/>

## 테스트 클래스

```java
    @Test
    public void hello() throws Exception {
        // 요청 : "/hello"
        // 응답 모델 name : junjang
        // 응답 뷰 이름 : hello

        mockMvc.perform (get ("/hello"))
                // 상태 테스트
                .andExpect(status().isOk())
                // 상태 출력하기
                .andDo(print())
                // 뷰 이름이 일치하는지 테스트
                .andExpect(view().name("hello"))
                // 값이 보내졌는지 테스트
                .andExpect(model().attribute("name", is("junjang")))
                // junjang이 view에 포함되어 있는지 테스트
                .andExpect(content().string(containsString("junjang")));
    }

```
![Thymeleaf](/images/springboot/thymeleaf/tym5.png) 이번엔 junjang이란 값이 Model을 통해 넘어왔는데 그 문자열 값이 포함되어 있는지 테스트 코드를 작성 후 실행합니다.
그랬더니 값이 포함이 되어 있지 않다는 에러가 뜹니다.
이제 값을 포함 시키러 가보겠습니다.<br/>
<br/>

## HTML에 타임리프 적용하기
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1 th:text="${name}">Name</h1>

</body>
</html>

```
![Thymeleaf](/images/springboot/thymeleaf/tym6.png) 타임리프 주소를 추가하면 `th`를 사용할 수 있습니다.
`th`에 맞는 문법을 사용하여 name을 출력하면 됩니다.<br/>
![Thymeleaf](/images/springboot/thymeleaf/tym7.png) junjang이라는 name 데이터가 뷰에 넘어와 출력된 것을 확인할 수 있습니다.<br/>
