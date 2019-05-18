---
title: SpringBoot - SpringApplication
date: 2019-04-30 19:33:28
tags: [SpringBoot, Debug, banner]
---

![images](/images/springboot/springboot.png)<br/>

# Application
기존에 만들어지는 방법으로 실행시키면 스프링부트가 제공하는 기능을 전부 사용하기 어렵습니다.
그래서 인스턴스화 하여 기능을 사용하는 방법에 대해 알아보겠습니다.<br/>
참고 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-spring-application.html#boot-features-spring-application)
```java
public static void main(String[] args) {
        SpringApplication app = new SpringApplication(Application.class);
        app.run();
}
```
![application](/images/springboot/application/ap1.png) 그래서 위와 같은 코드로 app 인스턴스를 생성하여 실행시키는 방법을 사용합니다.<br/>
<br/>

## Debug 사용하기
디버그 모드는 자동설정되는 사항들이 적용되는지 여부를 알려줍니다.
![application](/images/springboot/application/ap2.png) Edit Configuration을 클릭합니다.<br/>
![application](/images/springboot/application/ap3.png) 
-  VM options `-Ddebug`
-  Program arguments `--debug`

이 두가지를 사용하여 디버그 모드를 활성화 시킬 수 있습니다.
두 가지 중 하나만 선언하시면 구동됩니다.<br/>
![application](/images/springboot/application/ap4.png) 디버그 모드가 활용된 모습입니다.<br/>
<br/>

## 배너 사용하기
배너는 스프링부트 실행시 나오는 배너(문구)입니다.
![application](/images/springboot/application/ap5.png) 이 것이 배너!!<br/>
<br/>
### 파일로 배너 만들기
![application](/images/springboot/application/ap6.png) main - resources 경로에 banner.txt를 생성하고 원하는 문구를 넣으면 됩니다.<br/>
![application](/images/springboot/application/ap7.png) 문구를 넣고 실행한 모습입니다.<br/>
![application](/images/springboot/application/ap11.png) 파일을 타경로에 생성하였다면 application.properties에서 classpath를 통해 경로를 지정해주면 됩니다.<br/>
<br/>
### 배너 기능 사용하기
![application](/images/springboot/application/ap8.png) 추가적으로 버전도 나타낼 수 있습니다.
기타 여러가지 기능이 있는데 MANIFEST.MF가 있어야 가능한 기능이 있습니다.(application 버전 등..)
이는 packaging을 통하여 jar파일을 생성해 구동하여야 합니다.<br/>
![application](/images/springboot/application/ap9.png) packaging...
![application](/images/springboot/application/ap10.png) jar를 실행하니 application 버전이 출력됩니다.<br/>
<br/>
### 코드로 배너 만들기
코드로도 배너를 만들 수 있습니다.
```java
app.setBanner(new Banner() {
    @Override
    public void printBanner(Environment environment, Class<?> sourceClass, PrintStream out) {
        out.println("=========================");
        out.println("junjang");
        out.println("=========================");
    }
});
```
생성한 인스턴스를 사용하여 setBanner함수를 사용하여 출력하고싶은 문구를 출력할 수도 있습니다.<br/>
```java
public static void main(String[] args) {
        SpringApplication app = new SpringApplication(Application.class);
        app.setBannerMode(Banner.Mode.OFF);
        app.run();
    }
```
지금까지 생성한 배너를 출력하고 싶지 않다면, setBannerMode를 OFF모드로 변경하여 미출력하게 할 수 있습니다.<br/>
<br/>

## ApplicationEvent 활용하기
Application이 실행될 때 이벤트를 적용하기 위해 사용하는 기능입니다.<br/>
<br/>

### ApplicationStartingEvent 등록하기
![application](/images/springboot/application/ap12.png) 먼저 EventListener 클래스를 생성합니다.<br/>
```java
public class SampleListener implements ApplicationListener<ApplicationStartingEvent> {
    @Override
    public void onApplicationEvent(ApplicationStartingEvent event) {
        System.out.println("=======================");
        System.out.println("Application is Starting");
        System.out.println("=======================");
    }
}
```
![application](/images/springboot/application/ap13.png) 그리고 application 시작시 실행하는 함수를 선언하고 @Component를 통해 빈 등록을 합니다.
그리고 실행을 시키면 나오지 않습니다. ㅠㅠ<br/>
**그 이유**는 ?
ApplicationStartingEvent 는 **ApplicationContext** 가 만들어기 전에 발생하는 이벤트이므로 빈등록 설정을 해도 빈으로 등록할 수 없기 때문입니다.<br/>
<br/>

### addListener 사용하기
```java
app.addListeners(new SampleListener());
```
![application](/images/springboot/application/ap14.png) 그래서 해결 방법으로 시작시 인스턴스를 생성해 실행시켜주는 방법을 사용합니다.<br/>
<br/>

### ApplicationStartedEvent 등록하기
```java
@Component
public class StartedListener implements ApplicationListener<ApplicationStartedEvent> {
    @Override
    public void onApplicationEvent(ApplicationStartedEvent event) {
        System.out.println("===================");
        System.out.println("Application Started");
        System.out.println("===================");
    }
}
```
![application](/images/springboot/application/ap15.png) ApplicationStartedEvent 의 경우 ApplicationContext 가 만들어진 다음에 발생하는 이벤트이므로 빈으로 등록해서 서용 가능합니다.
구동하면 마지막에 출력되는 것을 확인할 수 있습니다.<br/>
<br/>

## WebApplicationType 설정하기
@AutoConfiguration의 기능으로 자동으로 Tomcat이 구동되는데 WEB에 관련된 설정을 변경할 수 있습니다.<br/>
<br/>
#### NONE : WebApplication 사용안함 설정
```java
application.setWebApplicationType(WebApplicationType.NONE);
```

#### SERVLET : SERVLET 사용 설정
```java
application.setWebApplicationType(WebApplicationType.SERVLET);
```

#### REACTIVE : REACTIVE 사용 설정
```java
application.setWebApplicationType(WebApplicationType.REACTIVE);
```
![application](/images/springboot/application/ap16.png) 위 세가지를 통해 변경이 가능합니다.
- SERVELT : Default 혹은 MVC가 존재하면 servlet으로 동작합니다.
- REACTIVE : Webflux가 존재하면 reative로 동작합니다.
- NONE : 둘 다 없으면 none으로 동작합니다.<br/>
<br/>

## Application Arguments 활용하기
`-D` 들어오는 옵션은 JVM 옵션 `--`로 들어오는 옵션은 Program arguments입니다.<br/>
![application](/images/springboot/application/ap17.png) 테스트 하기 위해 입력한 값입니다.<br/>
```java
@Component
public class SampleArguments {

    public SampleArguments(ApplicationArguments arguments) {
        System.out.println("foo: "+arguments.containsOption("foo"));
        System.out.println("bar: "+arguments.containsOption("bar"));
    }
}
```
위 코드를 추가하고 실행시킵니다.<br/>
![application](/images/springboot/application/ap18.png) VM은 false arg는 true가 출력되었습니다.
이걸 console에서도 똑같이 테스트 해보겠습니다.<br/>
<br/>

### console에서 테스트하기
1. Maven Clean 후 패키징합니다.(테스트 제외)
```bash
mvn clean package -DskipTests
```
2. target 폴더에서 java -jar 명령으로 Test
```bash
 java -jar target/jar파일 -Dfoo --bar
```
![application](/images/springboot/application/ap19.png) console도 동일하게 값이 출력됩니다.<br/>
<br/>

## Application 실행 후 추가 실행할 때
어플리케이션을 실행하고 추가로 실행하고 싶은 것이 있을 때 사용하는 방법입니다.<br/>
<br/>

### ApplicationRunner 사용하기
```java
@Component
@Order(1)
public class SampleRunner implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("foo: "+args.containsOption("foo"));
        System.out.println("bar: "+args.containsOption("bar"));
    }
}
```
![application](/images/springboot/application/ap20.png) ApplicationRunner를 구현하여 사용하면 결과가 출력됩니다.
결과는 마찬가지로 `--`로 선언한 arguments만 받아낼 수 있습니다.<br/>
**@Order** 어노테이션은 우선순위를 주는 기능으로 숫자가 낮을수록 순위가 높다는 것을 의미합니다.<br/>
