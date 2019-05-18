---
title: SpringBoot - MVC ExceptionHandler
date: 2019-05-10 18:19:24
tags: [SpringBoot, MVC, Exception]
---

![images](/images/springboot/springboot.png)<br/>

# ExceptionHandler
기본적으로 등록되어있는 ErrorHandler가 메시지를 출력해주지만 직접 커스텀하여 에러 메시지를 출력할 수도 있습니다.<br/>
![ExceptionHandler](/images/springboot/Exceptionhandler/err1.png) 위 사진이 기본적으로 등록된 에러 메시지입니다.<br/>
![ExceptionHandler](/images/springboot/Exceptionhandler/err2.png) curl 명령어로 보면 JSON 형식으로 오류가 나는 것을 확인 가능합니다.
- BasicErrorController : 기본 예외 처리를 합니다.
<br/>
<br/>

## 핸들러 실습 해보기
### Controller 클래스
```java
@Controller
public class SampleController {

    @GetMapping("/hello")
    public String hello() {
        throw new SampleException();
    }
}
```
![ExceptionHandler](/images/springboot/Exceptionhandler/err3.png) 먼저 Hello 요청을 보낼 때 SampleException 클래스를 통해 에러메시지를 출력하기 위해 에러를 던집니다.<br/>
<br/>

### SampleException 클래스
![ExceptionHandler](/images/springboot/Exceptionhandler/err4.png) **RuntimeException**을 상속받아 실행시 에러를 발생시키도록 합니다.<br/>
<br/>

### AppError 클래스
```java
public class AppError {

    private String message;

    private String reason;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public String getReason() {
        return reason;
    }

    public void setReason(String reason) {
        this.reason = reason;
    }
}
```
![ExceptionHandler](/images/springboot/Exceptionhandler/err5.png) 메시지와 에러 이유를 나타낼 수 있도록 변수 및 getter, setter를 선언합니다.<br/>
<br/>

### Controller 클래스
```java
@Controller
public class SampleController {

    @GetMapping("/hello")
    public String hello() {
        throw new SampleException();
    }

    
    // 메소드 파라미터로 해당하는 Exception 정보를 받아 올 수 있습니다.
    @ExceptionHandler(SampleException.class)
    public @ResponseBody AppError sampleError(SampleException e) {
    // AppError: App에서 만든 커스텀한 에러정보를 담고 있는 클래스를 생성합니다.
        AppError appError = new AppError();
        appError.setMessage("error.app.time");
        appError.setReason("timeOut");
        return appError;
    }
}
```
![ExceptionHandler](/images/springboot/Exceptionhandler/err6.png) @ExceptionHandler를 활용하여 RuntimeException시 발생하는 에러를 커스텀 하기 위해 선언을 해줍니다.
그리고 AppError클래스의 메시지, 에러이유를 정의해주고 return합니다.<br/>
![ExceptionHandler](/images/springboot/Exceptionhandler/err7.png) /hello 경로 이동시 직접 선언했던 에러 메시지가 출력됩니다.<br/>
<br/>

## 에러 페이지 커스텀하기
- 참고 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-error-handling-custom-error-pages)
- 경로 : src/main/resources/static/error/
- 경로에 상태코드와 동일한 에러페이지를 만들어야 동작합니다. 혹은 첫 번째 숫자만 명시합니다.
 - 404.html
 - 5xx.html
 - ErrorViewResolver 구현
<br/>

![ExceptionHandler](/images/springboot/Exceptionhandler/err8.png) 경로에 404에러 및 5xx에러 발생시 나타내는 페이지를 커스텀 합니다.<br/>
![ExceptionHandler](/images/springboot/Exceptionhandler/err9.png) Root로 접근했을 경우 따로 페이지가 없기 때문에 404에러가 발생하여 커스텀한 페이지가 출력된 것을 확인할 수 있습니다.<br/>