---
title: SpringBoot - 내장 웹 서버
date: 2019-04-17 14:21:42
tags: [SpringBoot, Tomcat]
---

![images](/images/springboot/springboot.png)<br/>

# 내장 웹 서버 이해하기
* SpringBoot는 Spring을 보다 쉽게 사용할 수 있도록 내장 웹 서버를 제공합니다.
* 기본적으로 Tomcat이 내장되어 있지만 어떻게 사용되고 있는지 이해하기 위해 직접 구현해보며 이해합시다.<br/>
<br/>

## Tomcat 라이브러리
![auto](/images/springboot/server/ser1.png) 라이브러리에 위 세개가 내장되어있는 Tomcat입니다.
이 라이브러리를 사용하지 않고 직접 Tomcat을 구동시켜보겠습니다.<br/>
새로운 프로젝트를 생성하고 Application에는 SpringBoot에 관련된 어노테이션을 설정하지 않은 상태로 둔 가정하에 진행합니다.<br/>
![auto](/images/springboot/server/ser2.png) Tomcat을 new 연산자로 생성하고 포트 번호를 지정합니다.
그리고 경로를 지정한 후에 start 명령을 선언합니다.<br/>
정상적으로 구동된다면 오류 없이 Tomcat이 실행된 것을 확인할 수 있습니다.<br/>
<br/>
## 서버 충돌시 조치방법
**하지만!** 서버 충돌 오류가 난다면 포트가 기존에 사용되고 있을 확률이 높습니다.
```
ps ax | grep tomcat
kill -9 번호
```
위 명령어를 통해서 실행된 포트를 지우시고 다시 시도해봅니다.<br/>
<br/>

## Servlet 추가하기
이제 Servlet을 추가하여 구동된 Tomcat을 확인합니다.<br/>
```java
public class Application {

    public static void main(String[] args) throws LifecycleException {
        Tomcat tomcat = new Tomcat();

        Context context = tomcat.addContext("/","/");

        HttpServlet servlet = new HttpServlet() {
            @Override
            protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                PrintWriter writer = resp.getWriter();
                writer.println("<html><head><title>");
                writer.println("Hey, Tomcat");
                writer.println("</title></head>");
                writer.println("<body><h1>Hello Tomcat</h1></body>");
                writer.println("</html>");
            }
        };

        String servletName = "helloServlet";
        tomcat.addServlet("/", servletName, servlet);
        context.addServletMappingDecoded("/hello", servletName);

        tomcat.start();
        tomcat.getServer().await();

    }

}
```
![auto](/images/springboot/server/ser3.png) Get메소드를 생성하여 Hello Tomcat이 정상적으로 출력되는지 확인합니다.<br/>
![auto](/images/springboot/server/ser4.png) 정상적으로 출력되는 것을 확인할 수 있습니다.<br/>
이처럼 자동으로 설정하지 않더라도 직접 Tomcat을 구동할 수도 있습니다. 하지만 이런 역할을 SpringBoot에서 자동으로 해주는데 우리는 **@SpringBootApplication**이 실행될 때  **AutoConfiguration**을 통해 자동으로 설정된다는 것을 인지해야합니다.<br/>
<br/>

## spring.factories 자동설정은 ?
![auto](/images/springboot/server/ser5.png) spring.factories에서 Servlet에 관련된 것을 확인할 수 있습니다.
여기서 DispatcherServlet과 Servlet이 나뉜 이유는 ServletContainer는 매번 달라지지만 DispatcherServlet은 만들어진 Container에 등록만 하면 되기 때문에 별도로 나뉘게 된 것입니다.<br/>
<br/>
# 내장 웹 서버 응용 - 컨테이너, 포트
내장 웹 서버를 이해했다면 이제 활용하는 법을 알아봅시다.<br/>
![auto](/images/springboot/server/ser6.png) 현재 의존성에는 tomcat이 자동 설정을 통해 자동으로 웹 애플리케이션을 만들어주고 있습니다.
그렇다면 자동 설정되는 WAS(Tomcat)을 바꾸려면 어떻게 해야할까요?<br/>
<br/>

## WAS 바꾸는 방법
그냥 의존성을 추가하는 것이 아니라 **exclusion**을 통해 실행시 제외 시키고 WAS(jetty, undertow, etc...)를 추가해야합니다.<br/>
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
	<exclusions>
		<!-- Exclude the Tomcat dependency -->
		<exclusion>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		</exclusion>
	</exclusions>
</dependency>
<!-- Use Jetty instead -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```
![auto](/images/springboot/server/ser7.png) web이 시작할 때 제외를 시켜야 하므로 spring-boot-starter-web에서 tomcat을 제외시킵니다.<br/>
![auto](/images/springboot/server/ser8.png) jetty가 추가된 것을 확인할 수 있습니다.<br/>
![auto](/images/springboot/server/ser9.png) 물론 결과도 jetty로 웹 애플리케이션이 실행됩니다.<br/>
<br/>

## 웹 서버 사용하지 않는 @SpringBootApplication 설정
[스프링 레퍼런스 홈페이지 - 웹서버](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-externalize-configuration) 
아래의 두 방법이 있습니다.<br/>
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication application = new SpringApplication(Application.class);
        application.setWebApplicationType(WebApplicationType.NONE);
        application.run(args);
    }
}
```
@SpringBootApplication에 타입을 NONE으로 설정하는 방법<br/>
<br/>

## 웹 서버 사용하지 않는 application.properties 설정
```
spring.main.web-application-type=none
```
![auto](/images/springboot/server/ser11.png) properties에 타입을 NONE으로 설정하는 방법<br/>
<br/>

## 포트 변경 방법
[스프링 레퍼런스 홈페이지 - 포트변경방법](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-use-short-command-line-arguments)

```
server.port=포트번호
server.port=0
```
![auto](/images/springboot/server/ser12.png) properties를 통해 원하는 포트를 선언할 수 있습니다.
그리고 포트번호를 0으로 주게 되면 사용가능한 포트번호를 알아서 찾아 구동합니다.<br/>
<br/>

## 포트 리스너를 통해 포트번호 알아내기
웹서버가 생성이 되면 servletWebServer가 실행되고 이 이벤트 리스너 핸들러가 호출이 되고 onApplicationEvent 콜백이 실행됩니다.
먼저 포트리스너를 구현하기 위해 클래스를 생성합니다.
```java
@Component
public class PortListener implements ApplicationListener<ServletWebServerInitializedEvent> {
    @Override
    public void onApplicationEvent(ServletWebServerInitializedEvent event) {
      ServletWebServerApplicationContext applicationContext = event.getApplicationContext();
      applicationContext.getWebServer().getPort();
    }
}
```
![auto](/images/springboot/server/ser13.png) 포트번호를 출력한 모습입니다.<br/>
<br/>

## 응답을 압축해서 보내기
[스프링 레퍼런스 홈페이지 - 응답압축](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#how-to-enable-http-response-compression)
```
server.compression.enabled=true
```
위 코드를 통해 응답을 압축해서 보낼 수 있습니다.<br/>
<br/>

# 내장 웹 서버 응용 - HTTPS, HTTP2
실습하기 위해 새로운 프로젝트를 생성합니다.
<br/>

## Key-Store 생성하기
HTTPS를 사용하기 위해서는 Key-Store를 사용해야 합니다.
```
keytool -genkey 
  -alias tomcat 
  -storetype PKCS12 
  -keyalg RSA 
  -keysize 2048 
  -keystore keystore.p12 
  -validity 4000
```
![auto](/images/springboot/server/ht1.png) 
위 명령어 terminal을 통해 입력합니다. 
생성할 때 나오는 항목들은 본인 임의로 작성해도 됩니다.
최종으로 맞는지 물어보면 **y**를 눌러 생성하면 됩니다.<br/>

![auto](/images/springboot/server/ht2.png) 위 처럼 생성된 것을 볼 수 있습니다.<br/>
<br/>
## Key-Store(HTTPS) 실행하기
생성한 Key-Store는 application.properties에서 설정할 수 있습니다.<br/>
```
server.ssl.key-store=keystore.p12
server.ssl.key-store-password=123456
server.ssl.keyStoreType=PKCS12
server.ssl.keyAlias=spring
```
![auto](/images/springboot/server/ht3.png)  server.ssl.key-store=**classpath:**keystore.p12는 keystore의 위치를 찾아낼 때 사용하는 명령어입니다. 지금은 초기폴더에 있어 따로 설정하지 않았지만 다른 곳에 생성하였다면 루트를 설정해주어야 합니다.<br/>
![auto](/images/springboot/server/ht5.png) 
![auto](/images/springboot/server/ht6.png) 앞으로 모두 HTTPS 요청을 붙여야합니다.
그런데 여기서 빨간색으로 오류가 뜨는 것을 볼 수 있습니다.
이유는 만든 인증서가 공인된 인증서가 아니기 때문입니다. 하지만 무시하고 진행하면 값은 출력이 됩니다.<br/>
이렇게 HTTPS를 설정하면 커넥터(Connector)가 하나이기 때문에 다른 설정이 불가능합니다. 이제부터 이를 극복하는 설정법을 알아보도록 하겠습니다.<br/>
<br/>
## Connector(HTTP) 설정하기
```java
   @Bean
    public ServletWebServerFactory serverFactory() {
        TomcatServletWebServerFactory tomcat = new TomcatServletWebServerFactory();
        tomcat.addAdditionalTomcatConnectors(createStandardConnector());
        return tomcat;
    }

    private Connector createStandardConnector() {
        Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
        connector.setPort(8080);
        return connector;
    }
```
![auto](/images/springboot/server/ht7.png) 위 코드를 Application에 작성합니다.
그리고 기존 HTTPS가 적용되어있는 포트는 8443으로 변경해줍니다.<br/>
이렇게 되면 HTTPS도 받고 HTTP도 받게 됩니다.<br/>
<br/>

## HTTP2 사용하기
시작 전 HTTP를 활성화 하기 위한 코드는 삭제합니다.<br/>
```
server.http2.enabled=true
```
위 코드로 HTTP2를 활성화 할 수 있습니다.<br/>
하지만 이건 undertow 서버에서만 해당되는 것이고 tomcat에서는 다릅니다. 각자 사용하는 서버별 설정방법을 확인하시려면 [서버별 HTTP2 설정방법 페이지](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-embedded-web-servers.html#howto-configure-http2-tomcat)로 이동하셔서 실습하시면 되겠습니다.
혹시 Intelli J를 사용하신다면

* File - Project Structure - Project
* File - Project Structure - Modules - Dependencies

위 설정들을 통해 Java 버전을 바꾸어야합니다.<br/>
```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-undertow</artifactId>
        </dependency>
    </dependencies>
```
위 코드는 pom.xml에서 undertow서버를 설정하는 방법입니다.<br/>
http2로 실행시키면 결과가 출력될 것입니다.<br/>
