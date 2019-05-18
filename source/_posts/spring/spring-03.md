---
title: SpringBoot - 자동 설정 관리
date: 2019-04-16 14:26:10
tags: [SpringBoot, SpringBootApplication]
---

![images](/images/springboot/springboot.png)<br/>

# SpringBoot 자동 설정 관리의 개념
SpringBoot를 사용하면서 @SpringBootApplication 어노테이션이 자동으로 어떤 설정을 하는지 알아보겠습니다.<br/>

## @SpringBootApplication ?
![auto](/images/springboot/auto/au1.png) 스프링 부트 application을 생성할 때 @SpringBootApplication 를 통해 실행시킬 것입니다.
이 어노테이션은 사실 하나가 아니라 **3가지**의 어노테이션이 합쳐져 있습니다.
- @SpringBootConfiguration - @Configuration과 같은 Bean을 등록시켜주는 역할
- @ComponentScan - @Component(@Service, @Repository, @Controller, @RestController, @Configuration)에 해당하는 어노테이션을 찾아 Bean 등록을 해주는 역할
- @EnableAutoConfiguration - Tomcat을 구동시켜주는 역할
<br/>

![auto](/images/springboot/auto/au3.png) 그래서 세개의 어노테이션을 선언하고 구동하여도 동일한 결과가 출력됩니다. 웹 서버를 실행하지 않으려면 @EnableAutoConfiguration을 생략해도 사실상 구동에는 문제가 없습니다.
**하지만!** 만약 생략하게 된다면
```java
@Configuration
@ComponentScan
public class SpringinitApplication {

    public static void main(String[] args) {
        SpringApplication application = new SpringApplication(Application.class);
        application.setWebApplicationType(WebApplicationType.NONE);
        application.run(args);
    }

}
```
위와 같은 코드를 통해 웹 서버로서 동작을 하지 않게 만들어 주어야 합니다.
이런 코드를 이해하기 위해서는 spring-boot-autoconfigure 하위 spring.factories 파일의 meta 설정에 관해서 더 살펴보아야 합니다.<br/>
![auto](/images/springboot/auto/au4.png) 라이브러리에 있는 spring-boot-autoconfigure 하위 spring.factories를 클릭하면 @EnableAutoConfiguration에 해당되는 설정 파일들이 보입니다. 이 파일들이 어노테이션이 실행 되면 자동으로 빈이 등록되고 내장 Tomcat을 사용하여 웹 서버가 동작하게 됩니다.<br/>
그런데 웹 서버를 구동시키지 않으려면 왜 추가적으로 코드를 작성해야 할까요? 바로 여기에도 조건이 걸려있기 때문입니다.<br/>
![auto](/images/springboot/auto/au5.png) 위 코드를 보면 @Configuration은 기본적으로 선언이 되어 있는데 밑에 Conditional이 붙은 어노테이션들이 보입니다. 여기서 WebApllication을 살펴보면 SERVLET 타입인데 만약 이 타입이 아니라면 구동을 시키지 않는다는 조건입니다.
그래서 타입을 NONE으로 오버라이딩 해준다면 실행이 되는 것입니다.
대표적인 예시를 하나 들었지만 직접 활용하고 이해하기는 힘들 것이라 생각합니다.
직접 커스텀 해보면서 익혀보도록 하겠습니다.<br/>
<br/>
# SpringBoot 자동 설정 커스텀 해보기
새로운 프로젝트를 생성해줍니다.(기존 프로젝트 유지)
<br/>
## 의존성 추가하기
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-autoconfigure</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-autoconfigure-processor</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.0.3.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
![auto](/images/springboot/auto/au6.png) 위 두가지의 의존성을 버전관리 해주는 spring-boot-dependencies를 추가하여 2.0.3버전으로 유지하였습니다.<br/>
<br/>
## 객체 생성하기
![auto](/images/springboot/auto/au7.png) 객체를 만들고 setter, getter, toString을 만들어줍니다.
클래스명은 Holoman으로 하였습니다.<br/>
![auto](/images/springboot/auto/au16.png) 객체를 자동설정 해주는 설정 파일을 만들고 빈으로 등록할 메소드를 구현합니다.<br/>
<br/>
## spring.factories 생성하기
![auto](/images/springboot/auto/au8.png) main/resources에 **spring.factories**파일을 만들어 @EnableAutoConfiguration 실행시 자동으로 실행되도록 선언합니다.<br/>
<br/>
## build, install 하기
![auto](/images/springboot/auto/au9.png) 이제 다른 프로젝트에서 사용할 수 있도록 build, install을 해야합니다.
Maven에서 LifeCycle의 install을 클릭하여 생성하여 줍니다.
```
mvn install
```
위 명령어를 통해서도 생성이 가능합니다.<br/>
<br/>
## 새로운 프로젝트에 기존 프로젝트 의존성 추가하기
![auto](/images/springboot/auto/au10.png) 생성한 프로젝트의 install을 실행하기 위하여 해당 프로젝트의 의존성을 새로 만든 프로젝트의 pom.xml에 의존성 추가를 해줍니다.<br/>
![auto](/images/springboot/auto/au11.png) 추가를 해주었다면 라이브러리에 의존성이 추가된 것을 알 수 있습니다.
만약 추가가 되지 않았다면 자동실행시킬 프로젝트 **build, install** 문제 혹은 IDE 자체의 **refresh**가 되지 않았을 수 있으니 확인하면 되겠습니다.<br/>
![auto](/images/springboot/auto/au12.png) 내부를 살펴보면 생성했던 프로젝트 그대로를 볼 수 있습니다.
이제 가져온 프로젝트에 등록되어있는 빈이 정상적으로 등록되어있는지 확인해야합니다.<br/>
<br/>
## Bean이 등록되어 있는지 확인하기
![auto](/images/springboot/auto/au13.png) 새로운 클래스를 생성하는데 여기선 ApplicationRunner를 사용합니다.
이건 Argument인자를 받아서 @SpringBootApplication 실행 시 자동으로 실행되는 Bean을 만들고 싶을 때 사용합니다.
가져온 holoman 클래스를 의존성 주입을 시키고 run메소드를 재정의 합니다.<br/>
![auto](/images/springboot/auto/au14.png) 실행시키면 받아온 프로젝트의 등록되어있는 빈을 출력합니다.<br/>
<br/>
## 메소드 재정의 해보기
```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
            SpringApplication application = new SpringApplication(Application.class);
            application.setWebApplicationType(WebApplicationType.NONE);
            application.run(args);
    }

    @Bean
    public Holoman holoman() {
        Holoman holoman = new Holoman();
        holoman.setName("freelifeBlack");
        holoman.setHowLong(60);
        return holoman;
    }
}
```
![auto](/images/springboot/auto/au15.png) 그렇다면 가져온 빈을 현재 프로젝트에서 다시 정의하여 출력한다면 현재 프로젝트의 메소드 값이 나올까요?
답은 **"NO"**입니다.<br/>
이유는 **@SpringBootApplication**에 있는데,
**첫 번째** 실행하는 어노테이션은 **@ComponentScan**으로 현재 프로젝트에서 만든 메소드를 먼저 빈으로 등록합니다.<br/>
**두 번째** 실행하는 어노테이션은 **@EnableAutoConfiguration**으로 기존 프로젝트에서 가져온 빈을 등록하여 덮어쓰게 되어 결과는 변하지 않습니다.<br/>
그렇다면 이런 현상을 바꾸기 위해선 어떻게 해야할지 알아봅시다.
<br/>
## 재정의 덮어쓰기 방지하기
![auto](/images/springboot/auto/au17.png) **@ConditionalOnMissinBean**은 빈 등록이 되어있지 않다면 등록하고 아니면 등록되지 않게끔 하게 합니다.
즉 현재 프로젝트에서 먼저 등록이 된다면 기존 프로젝트의 빈은 등록이 되지 않는다는 것을 의미합니다.<br/>
![auto](/images/springboot/auto/au18.png) 위처럼 먼저 등록을 하였기 때문에 덮어씌워지지 않고 출력되었습니다.
하지만 여기서 또 문제가 발생합니다.
그렇다면 일일이 내가 다 빈 등록 및 어노테이션을 붙여야 하는가 하는 의문이 들겁니다.
그 해결 방법은 바로 application.properties를 사용하는 것입니다.<br/>
<br/>
## application.properties 사용하기
![auto](/images/springboot/auto/au19.png) main/resources경로에 application.properties를 생성합니다.<br/>
![auto](/images/springboot/auto/au20.png) 그리고 생성한 파일을 사용하기 위해서는 전용 객체를 생성해야합니다.
기존 프로젝트에서 **@ConfigurationProperties("prefix")**를 사용하여 application.properties에 선언한 값을 사용할 수 있습니다.<br/>
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```
![auto](/images/springboot/auto/au21.png) 먼저 위 라이브러리를 추가해야 자동완성이 되니 꼭 추가해야합니다.<br/>
![auto](/images/springboot/auto/au22.png) **@EnableConfigurationProperties** 어노테이션을 사용하여 Properties클래스를 선언합니다.
그리고 값을 유동적으로 가져가야 하기 때문에 파라미터 값으로 가져갑니다.
그리고 재정의한 값을 가져가기 위해서 getter를 통해 set합니다.<br/>
![auto](/images/springboot/auto/au23.png) 그리고 내가 출력하고싶은 값을 application.properties에 작성하고 출력합니다.<br/>
![auto](/images/springboot/auto/au24.png) 입력한 값이 정상적으로 출력됩니다.