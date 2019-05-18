---
title: SpringBoot - MVC HttpMessageConverters
date: 2019-05-08 18:52:52
tags: [SpringBoot, MVC, RequestBody, ResponseBody]
---

![images](/images/springboot/springboot.png)<br/>

# HttpMessageConverters
SpringFrameWork에서 제공하는 인터페이스 SpringMVC 의 일부분으로 **HTTP 요청 본문**을 **객체**로 변경하거나, **객체**를 **HTTP 응답 본문**으로 변경할 때 사용합니다. 예제를 보면서 공부해보겠습니다.<br/>
<br/>

## HttpMessageConverters에 사용되는 어노테이션
함께 사용하는 어노테이션 2가지가 있습니다.
- @RequestBody
- @ResponseBody
 - @Controller에서는 반드시 붙여야됩니다.
 - @RestController는 생략해도됩니다.
 - 자동으로 설정 뷰 네임 리졸버를 안타고 바로 MessageConverter 타고 응답 본문으로 내용이 들어갑니다.
<br/>
<br/>

## Test 클래스

```java
@RunWith(SpringRunner.class)
@WebMvcTest(UserController.class)
public class UserControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void createUser_JSON() throws Exception {
        String userJson = "{\"username\":\"junjang\", \"password\":\"123\"}";

        //요청을 만드는 단계
        mockMvc.perform(post("/users/create")
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .accept(MediaType.APPLICATION_JSON_UTF8)
                .content(userJson))
                    //응답 확인단계
                    .andExpect(status().isOk())
                    .andExpect(jsonPath("$.username", is(equalTo("junjang"))))
                    .andExpect(jsonPath("$.password", is(equalTo("123"))));

    }
}
```
![MVCHttp](/images/springboot/mvchttp/http1.png) JSON 형식으로 들어오는 요청을 응답하는 테스트 코드를 작성합니다.
- accept는 contentType과 함께 적용되므로 현재는 생략 가능하나 써주면 확실해집니다.
- import는 matchers로 합니다.<br/>

여기서 테스트를 실행시키면 어떻게 될까요 ?
바로 **404에러**가 뜹니다.
이유는 post방식으로 경로설정을 하였지만 해당 경로에 대한 **Controller**가 없기 때문입니다.
그러므로 Controller를 구현하러 갑니다.<br/>
<br/>

## Controller 클래스

```java
@RestController
public class UserController {

    @PostMapping("/users/create")
    public User create(){
        return null;
    }
}
```
![MVCHttp](/images/springboot/mvchttp/http2.png) Controller에서 동일 경로로 User를 객체로한 메소드를 생성합니다.(User 클래스만 생성합니다.)<br/>
그리고 테스트를 실행하보면 ?
또 에러가 뜨는데 이번엔 **200에러** 입니다.
이유는 요청은 완료되었지만 JSON으로 넘어가는 값이 없습니다.
그래서 값을 넘기기 위해 **@RequestBody**로 User 객체를 넘겨주어야 합니다.<br/>
<br/>

```java
@RestController
public class UserController {

    @PostMapping("/users/create")
    public User create(@RequestBody User user){
        return user;
    }
}
```
![MVCHttp](/images/springboot/mvchttp/http3.png)  파라미터에 User 객체를 보내는 코드를 작성하고 다시 테스트를 실행했더니..<br/>
이번엔 **500에러**가 등장했습니다.
이유는 User 클래스는 존재하지만 객체를 **바인딩**할 것이 아무것도 없기 때문입니다.
그래서 User 클래스에 바인딩할 것이 필요합니다.<br/>
<br/>

## User 객체 클래스
```java
public class User {
    private Long id;
    private String username;
    private String password;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```
![MVCHttp](/images/springboot/mvchttp/http4.png) 변수를 선언하고 getter, setter를 생성한 후 테스트를 진행하면 이상없이 테스트가 성공한 것을 확인할 수 있습니다.<br/>