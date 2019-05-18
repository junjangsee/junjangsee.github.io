---
title: SpringBoot - 생성하기
date: 2019-04-03 10:28:37
tags: SpringBoot
---

![images](/images/springboot/springboot.png)<br/>

# SpringBoot 생성하기

## 개발 환경
- Mac OS
- IDE : IntelliJ IDEA Ultimate
- Maven
- Terminal
- Java 8 
<br/>

### 프로젝트 생성하기
![start](/images/springboot/spring-start/start1.png) Maven을 사용하기 때문에 Maven을 선택하고 Next를 클릭합니다.<br/>
![start](/images/springboot/spring-start/start2.png) GroupId에는 패키지명, ArtifactId는 프로젝트명을 작성합니다.<br/>
![start](/images/springboot/spring-start/start3.png) 경로를 나타내는 곳입니다. 수정할 것이 없다면 Finish 클릭합니다.<br/>
![start](/images/springboot/spring-start/start4.png) 프로젝트를 생성하면 우하단에 창이 뜰텐데 **Enable Auto-Import**를 클릭해주면 자동으로 Import 해줍니다.<br/>
<br/>
### 기본적인 의존성 주입하기
기본적인 의존성 주입을 하기 위해 [스프링 레퍼런스 가이드 홈페이지](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-introducing-spring-boot)를 참조합니다.
![start](/images/springboot/spring-start/start5.png) pom.xml에 기본적으로 들어가는 의존성들이며 추후에 설명하도록 하겠습니다.
간단히 말하면 parent는 부모의 부모 느낌으로 의존성 관리를 위해 사용합니다.<br/>
![start](/images/springboot/spring-start/start6.png) 아래와 같이 패키지 및 클래스를 만듭니다.
물론 패키지명과 클래스명은 개발자가 임의로 넣어도 됩니다.<br/>

```
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
![start](/images/springboot/spring-start/start7.png) 만든 클래스에 위와 같은 코드를 작성하여 부트가 실행할 수 있도록 해줍니다.
<br/>
![start](/images/springboot/spring-start/start8.png) 실행이 준비되었다면 마우스 우버튼으로 Run을 합니다.<br/>
![start](/images/springboot/spring-start/start9.png) Tomcat 8080 포트로 구동 된 것을 확인할 수 있습니다.<br/>
![start](/images/springboot/spring-start/start10.png) 8080으로 접속하면 에러나 나지만 구동된 것을 확인할 수 있습니다.<br/>
<br/>
### jar파일 생성 및 실행시키기
```
mvn package
```
위 명령어로 jar 파일을 생성할 수도 있습니다.<br/>
```
java -jar target/spring-boot-getting-started-1.0-SNAPSHOT.jar
```
그리고 위 명령어로 jar파일을 실행 시키면 위와 동일한 값이 출력됩니다.<br/>
<br/>
### Spring Initializr로 프로젝트 생성하기
[Spring Initializr](https://start.spring.io/) 주소로 접속하여 지금까지 했던 내용을 자동으로 설정되도록 생성할 수 있습니다.<br/>
![start](/images/springboot/spring-start/start11.png) 위에 자신이 원하는 항목을 체크하여 생성하고 IDE에서 open하면 자동으로 설정이 완료됩니다.