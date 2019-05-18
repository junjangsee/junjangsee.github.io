---
title: SpringBoot - MVC HATEOAS
date: 2019-05-10 19:22:06
tags: [SpringBoot, MVC, HATEOAS]
---

![images](/images/springboot/springboot.png)<br/>

# HATEOAS(Hypermedia ​A​s ​T​he ​E​ngine ​O​f A​​pplication ​S​tate)
HATEOAS를 구현하기에 편리한 기능들을 제공하는 라이브러리입니다. REST API를 만들 때 사용하는 기능입니다. SpringBoot에서는 단순 의존성 추가 하나만으로 모든 기능을 사용할 수 있습니다.
코딩하는 과정이 늘어 불편하지만 진정한 REST API를 제작시 사용하면 됩니다.<br/>
참고 자료 : [헤이토스 사용 가이드](https://spring.io/guides/gs/rest-hateoas/), [스프링공식문서](https://docs.spring.io/spring-hateoas/docs/current/reference/html/)<br/>
<br/>

## REST ??
> REST에서 R은 Representational입니다. 

- 서버: 현재 리소스와 ​연관된 링크 정보​를 클라이언트에게 제공합니다.
- 클라이언트: ​연관된 링크 정보​를 바탕으로 리소스에 접근합니다.
- 연관된 링크 정보
  - Rel​ation
  - H​ypertext ​Ref​erence<br/>
<br/>

## hateoas 의존성 추가
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>
```
![hateoas](/images/springboot/hateoas/hate1.png) 위 의존성을 추가하면 다른 추가 설정없이 사용이 가능합니다.
직접 테스트를 하면서 활용법을 알아봅시다.<br/>
<br/>

## 테스트 클래스
```java
@RunWith(SpringRunner.class)
@WebMvcTest(SampleController.class)
public class SampleControllerTest {


    @Autowired
    MockMvc mockMvc;

    @Test
    public void hello() throws Exception {
        mockMvc.perform(get("/hello"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(jsonPath("$._links.self").exists());

    }
}
```
![hateoas](/images/springboot/hateoas/hate2.png) JSON 형식으로 받을 테스트를 작성합니다. 
Controller가 없이 테스트를 실행하면 404에러가 뜰 것입니다.<br/>
<br/>

## Hello 클래스
```java
public class Hello {

    private String prefix;

    private String name;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return  prefix + " " + name;
    }
}
```
![hateoas](/images/springboot/hateoas/hate3.png) 객체를 주고받을 클래스를 추가합니다.<br/>
<br/>

## Controller 클래스
```java
@RestController
public class SampleController {

    @GetMapping("/hello")
    public Hello hello() {
        Hello hello = new Hello();
        hello.setPrefix("Hey, ");
        hello.setName("junjang");

        return hello;
    }
}
```
![hateoas](/images/springboot/hateoas/hate4.png) /hello 객체에 값을 set하고 객체를 return합니다. 그리고 테스트를 실행하면 아래와 같은 오류가 출력됩니다.<br/>
![hateoas](/images/springboot/hateoas/hate5.png) **200에러**와 함께 **JsonPath에러**가 뜰 것입니다. 이는 값은 보내지지만 Jsonpath로 가는 작업이 없기 때문입니다.<br/>
```java
@RestController
public class SampleController {

    @GetMapping("/hello")
    public Resource<Hello> hello() {
        Hello hello = new Hello();
        hello.setPrefix("Hey, ");
        hello.setName("junjang");

        //SampleController 클래스에 존재하는 hello라는 메서드에 대한 링크를 만들어서 self라는 릴레이션을 만들어서 추가합니다.
        Resource<Hello> helloResource = new Resource<>(hello);
        helloResource.add(linkTo(methodOn(SampleController.class).hello()).withSelfRel());

        return helloResource;
    }
}
```
![hateoas](/images/springboot/hateoas/hate6.png) hateoas를 import한 Resource 객체를 생성하여 Hello 클래스를 가지고 Controller의 hello 메소드의 링크를 객체에 추가합니다.
여기서 self의 역할은 자기 자신의 주소를 가르키게 됩니다.
그리고 주소를 넣은 객체를 return합니다.<br/>
![hateoas](/images/springboot/hateoas/hate7.png) 주소가 들어간 resource를 return했더니 JSON에 주소까지 포함되어 출력되는 것을 확인할 수 있습니다.<br/> 