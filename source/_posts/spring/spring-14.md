---
title: SpringBoot - MVC ViewResolver
date: 2019-05-08 20:13:36
tags: [SpringBoot, MVC, xpath]
---

![images](/images/springboot/springboot.png)<br/>

# ViewResolver
client가 요청을 보내면 Accept Header 에 따라 응답이 달라지게 되는데 이러한 요청을 처리하기 위해 사용합니다.
- Accept Header가 없는 경우 **/path?format=pdf** 와 같은 형식으로 알 수 있습니다.
<br/>

## XML 컨버터 사용방법
대표적으로 XML이 기본적으로 제공되어있지 않기 때문에 XML로 응답하기 위한 방법을 공부하도록 하겠습니다.<br/>
<br/>

### Test 클래스
```java
@Test
public void createUser_XML() throws Exception {
    String userJson = "{\"username\":\"junjang\", \"password\":\"123\"}";
    //요청을 만드는 단계
    mockMvc.perform(post("/users/create")
            .contentType(MediaType.APPLICATION_JSON_UTF8)
            .accept(MediaType.APPLICATION_XML)
            .content(userJson))
            //응답 확인단계
                .andExpect(status().isOk())
                .andExpect(xpath("/User/username")
                        .string("junjang"))
                .andExpect(xpath("/User/password")
                        .string("123"));

}
```
![MVCResolver](/images/springboot/mvcresolver/resol1.png) accept에 XML로 응답하기 위해 XML을 선언합니다.
그리고 XML은 JSON과는 다르게 **xpath**를 통해 값을 확인합니다.
그리고 XML로 보내지는지 테스트를 실행합니다.<br/>
혹시 Controller, User 클래스를 참고하실 분은 
[이전포스팅](https://junjangsee.github.io/2019/05/08/spring/spring-13/)을 참고해주시면 됩니다.<br/>
<br/>

### HttpMediaTypeNotAcceptableException
![MVCResolver](/images/springboot/mvcresolver/resol2.png) 하지만 **HttpMediaTypeNotAcceptableException** 에러가 발생합니다.
이유는 **HttpMessageConvertersAutoConfiguration** 에서 JSON은 기본으로 지원하지만 XML은 지원하지 않아 추가적인 의존성이 필요합니다.<br/>
<br/>

### ​jackson-dataformat-xml​ 의존성 추가하기
```xml
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
    <version>2.9.8</version>
</dependency>
```
![MVCResolver](/images/springboot/mvcresolver/resol3.png) 위 의존성을 pom.xml에 추가하면 **HttpMessageConvertersAutoConfiguration**에서 구동하지 않던 XML클래스가 구동하면서 XML로 응답이 가능하게 됩니다.<br/>