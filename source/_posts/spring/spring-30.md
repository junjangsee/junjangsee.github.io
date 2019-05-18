---
title: SpringBoot - Security의 개념과 테스트
date: 2019-05-14 21:26:00
tags: [SpringBoot, Security, Unauthorized]
---

![images](/images/springboot/springboot.png)<br/>

# Security(시큐리티)
스프링에서는 시큐리티를 제공함으로서 여러 방면에서 보안 기능을 지원합니다.
- 웹시큐리티
- 메소드 시큐리티
- 다양한인증방법지원
  - LDAP, 폼 인증, Basic 인증, OAuth, ... 등

참고자료 : [스프링공식문서](https://docs.spring.io/spring-security/site/docs/current/reference/html/)
<br/>

## SpringBoot Security
스프링 부트 자체 시큐리티 자동 설정이 있습니다.
- SecurityAutoConfiguration : 시큐리티에 대한 설정 값들을 자동 실행해줍니다. 만약 사용하지 않고 커스텀을 원한다면 **WebSecurityConfigurerAdapter**를 상속받아 커스텀 할 수 있습니다.
- UserDetailsServiceAutoConfiguration : 자동으로 랜덤한 User(계정)을 생성해 줍니다.

실습을 통해서 시큐리티가 어떻게 작동하는지 학습해보겠습니다.<br/>
<br/>

## Thymeleaf 의존성 추가
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```
![Securituy](/images/springboot/security/se1.png) 타임리프 의존성을 추가하여 View를 구성하겠습니다.
타임리프가 궁금하시다면 [타임리프 포스팅](https://junjangsee.github.io/2019/05/09/spring/spring-18/)을 참고해주시면 되겠습니다.<br/>
<br/>

## Controller 클래스
```java
@Controller
public class HomeController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }

    @GetMapping("/my")
    public String my() {
        return "my";
    }
}
```
![Securituy](/images/springboot/security/se4.png) 
단순한 View로 이동하는 컨트롤러를 구현합니다.<br/>
<br/>

### 단순 View Controller를 커스텀으로 구현하기
별다른 Controller 설정없이 요청이 들어오면 View로 보내는 설정이 하고싶으면 아래와 같이 설정하는 방법도 있습니다. 
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/hello").setViewName("hello");
    }
}
```
![Securituy](/images/springboot/security/se3.png) 하지만 추가 설정 및 도중 구현 목적이 달라질 수도 있기 때문에 Controller에 구현하는 것이 좋겠습니다.<br/> 
<br/>

## View
Controller에 선언한 경로로 이동하기 위해 View를 만듭니다.
경로를 src/resource/templates에 생성합니다.<br/>

### index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Welcome junjang's Blog!!!</h1>
<a href="/hello">hello!</a>
<a href="/my">My Page</a>
</body>
</html>
```

### hello.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Hello</h1>
</body>
</html>
```

### my.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>My</h1>
</body>
</html>
```
![Securituy](/images/springboot/security/se5.png) 완성된 View의 모습입니다.
이제 View를 생성했다면 Controller와 연동이 되었는지 테스트를 해야합니다.<br/>
<br/>

## Test 클래스
```java
@RunWith(SpringRunner.class)
@WebMvcTest(HomeController.class)
public class HomeControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void hello() throws Exception {
        mockMvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andExpect(view().name("hello"))
                
    }

    @Test
    public void my() throws Exception {
        mockMvc.perform(get("/my"))
                .andExpect(status().isOk())
                .andExpect(view().name("my"))
                .andDo(print());
    }
}
```
![Securituy](/images/springboot/security/se6.png) 슬라이스 테스트를 구현하고 각 경로에 따라 상태, view 이름이 일치하는지 테스트합니다.<br/>
테스트가 완료되었다면 애플리케이션을 구동시킵니다.<br/>
![Securituy](/images/springboot/security/se7.png) 
![Securituy](/images/springboot/security/se8.png) 
![Securituy](/images/springboot/security/se9.png) 각 페이지별 이상 없이 출력되는 것을 확인하실 수 있습니다.
이제 애플리케이션까지 이상없이 구동되는 것을 확인 했으니 본격적으로 시큐리티를 적용해보겠습니다.<br/>
<br/>

## Security 적용하기
```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
  </dependency>
```
![Securituy](/images/springboot/security/se10.png)의존성을 추가하는 순간 내장되어있는 자동 설정이 구동합니다.<br/>
<br/>

### Test 클래스
의존성을 추가 후 다시 테스트를 구동하면 **Unauthorized 401에러**가 납니다.<br/>
<br/>

#### Unauthorized 401에러
```bash
MockHttpServletResponse:
           Status = 401
    Error message = Unauthorized
          Headers = {WWW-Authenticate=[Basic realm="Realm"],
          X-Content-Type-Options=[nosniff], X-XSS-Protection=[1; mode=block],
          Cache-Control=[no-cache, no-store, max-age=0, must-revalidate],
          Pragma=[no-cache], Expires=[0], X-Frame-Options=[DENY]}
     Content type = null
             Body = 
    Forwarded URL = null
   Redirected URL = null
          Cookies = []
```
이유는 Basic Authentication의 인증 정보가 없기 때문인데 이를 해결하기 위해 Accept Header를 주어 Form Authentication으로 인증을 할 수 있도록 조치해야합니다.<br/>
```java
@Test
public void hello() throws Exception {
    mockMvc.perform(get("/hello")
            .accept(MediaType.TEXT_HTML))
            .andExpect(status().isOk())
            .andExpect(view().name("hello"))
            ..andDo(print());
}
```
![Securituy](/images/springboot/security/se11.png)accept Header 설정에 임의로 MediaType.TEXT_HTML을 적용하면 form 인증을 합니다.<br/>
<br/>

### 자동설정에 의한 기본 계정 생성
Application이 구동될 때마다 랜덤으로 유저를 생성하여 Form 인증시 사용할 수 있게 해줍니다.
- Username : user
- Password : 랜덤 출력(콘솔)

![Securituy](/images/springboot/security/se12.png)
![Securituy](/images/springboot/security/se13.png) user와 콘솔에 출력된 비밀번호를 입력하면 페이지가 출력됩니다.<br/> 
<br/>

#### 자동설정을 커스텀 하고 싶다면?
```java
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
}
```
![Securituy](/images/springboot/security/se14.png) 위 클래스 안에 직접 커스텀을 할 수 있습니다. 커스텀을 하는 방법은 다음에 더 자세히 알아보도록 하겠습니다.<br/>
여기까지 애플리케이션에 로그인 Form까지 완성되었을 것입니다. 하지만 정작 테스트에서는 계정을 컨트롤 할 수 없기 때문에 에러페이지가 계속 뜰텐데 이를 해결할 방법은 뭐가 있을까요?<br/>
<br/>

### Spring Security test 의존성 추가
참고자료 : [스프링공식문서](https://docs.spring.io/spring-security/site/docs/current/reference/html/test.html)
```xml
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <version>${spring-security.version}</version>
    <scope>test</scope>
</dependency>
```
![Securituy](/images/springboot/security/se15.png) 시큐리티 테스트라는 의존성을 추가해줍니다.<br/>
<br/>

### @WithMockUser 사용하기
**@WithMockUser**는 가짜 유저 정보를 자체적으로 만들어주어 테스트를 진행합니다.
아래 예제처럼 메소드마다 적용할 수도 있지만 컨트롤러 전체에도 적용 가능합니다.<br/>
```java
@RunWith(SpringRunner.class)
@WebMvcTest(HomeController.class)
public class HomeControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    @WithMockUser
    public void hello() throws Exception {
        mockMvc.perform(get("/hello")
        .accept(MediaType.TEXT_HTML))
                .andExpect(status().isOk())
                .andExpect(view().name("hello"))
                .andDo(print());
    }

    @Test
    @WithMockUser
    public void my() throws Exception {
        mockMvc.perform(get("/my"))
                .andExpect(status().isOk())
                .andExpect(view().name("my"))
                .andDo(print());
    }

}
```
![Securituy](/images/springboot/security/se16.png) 만약 유저 정보가 자동으로 생성되었다면 200, 유저 정보가 없다면 401 Unauthorized 에러 메시지를 출력할 것입니다.<br/>
