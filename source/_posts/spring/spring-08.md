---
title: SpringBoot - profile(프로파일)
date: 2019-05-05 01:12:07
tags: [SpringBoot, properties, profile]
---

![images](/images/springboot/springboot.png)<br/>

# Profile(프로파일)
프로파일은 특정한 프로파일에서의 클래스의 빈을 등록하고 싶을 때 사용합니다.
<br/>

## 프로파일 선언하기
프로파일을 실습하기 위해 두가지 클래스를 생성합니다.
```java
@Profile("prod")
@Configuration
public class TestConfiguration {

    @Bean
    public String hello() {
        return "hello";
    }
}
```
![profile](/images/springboot/profile/pro1.png) 
```java
@Profile("test")
@Configuration
public class TestConfiguration {

    @Bean
    public String hello() {
        return "hello test";
    }
}
```
![profile](/images/springboot/profile/pro2.png) 생성한 클래스를 빈으로 등록하고 @Profile("프로파일명")을 통해 프로파일 선언을 합니다.
그리고 의존성 주입을 하고 실행을 합니다.<br/>
![profile](/images/springboot/profile/pro3.png)hello 문자열을 찾을 수 없다는 오류를 볼 수 있습니다.
이유는 프로파일 선언만 했지 어떤 프로파일인지 명시한 부분이 없습니다.<br/>
<br/>

## 프로파일 등록하기
```
spring.profiles.active=prod
```
![profile](/images/springboot/profile/pro4.png) 그래서 properties에 어떤 프로파일을 등록할지 선택하여 출력할 수 있습니다.<br/>
![profile](/images/springboot/profile/pro5.png) prod를 등록하여 출력하면 hello가 출력됩니다.<br/>
![profile](/images/springboot/profile/pro6.png) 반대로 test를 등록하여 출력하면 hello test가 출력됩니다.<br/>
<br/>

## 프로파일을 커맨드 라인으로 출력하기
여기서도 마찬가지로 **우선순위**가 적용됩니다.
```bash
mvn clean package -DskipTests
java -jar target/XXX.jar --spring.profiles.active=prod
```
![profile](/images/springboot/profile/pro7.png) 앞서 공부한 것처럼 우선순위가 높기 때문에 test 프로파일을 패키징 했어도 커맨드 라인에서 prod를 선언하였더니 prod 프로파일에 해당되는 값이 출력되었습니다.<br/>
이처럼 프로파일도 우선순위가 적용되는 것을 알 수 있습니다.<br/>
![profile](/images/springboot/profile/pro11.png)그리고 program arguments에 직접 지정하여 리패키징 없이 계속 사용할 수도 있습니다.<br/>
<br/>

## 프로파일을 별도로 관리하기
프로파일을 별도로 관리함으로써 가독성 높고 유지보수가 편리해집니다.
spring.profiles.acive에 따라서 더 우선순위를 가지고 오버라이딩이 되는 특징을 가지고 있습니다.<br/>
application-{profile}.properties 형식으로 생성합니다.<br/>
![profile](/images/springboot/profile/pro8.png) 그리고 각각의 key값에 대한 value를 선언합니다.<br/>
![profile](/images/springboot/profile/pro9.png) 결과를 보시면 알겠지만 우선순위에 따라 기존 properties에 있는 junjang보다 active에 선언된 프로파일이 우선순위가 높은 것을 알 수 있습니다.<br/>
<br/>

## 프로파일에 대한 파일 자체를 포함시키기
이는 프로파일 파일 자체를 포함시킴으로서 포함시킨 프로파일을 출력하면 같이 포함되어 출력되게 합니다.<br/>
동일하게 properties를 생성합니다.
```
spring.profiles.include=proddb
```
![profile](/images/springboot/profile/pro10.png) 새롭게 생성한 파일에 key, value값을 선언하고 include하여 prod 프로파일에 포함시킵니다.<br/> 
![profile](/images/springboot/profile/pro12.png) 출력하면 포함된 proddb 프로파일이 출력된 것을 볼 수 있습니다.<br/> 