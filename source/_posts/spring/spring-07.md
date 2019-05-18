---
title: SpringBoot - Configuration(외부설정)
date: 2019-05-02 20:24:35
tags: [SpringBoot, properties]
---

![images](/images/springboot/springboot.png)<br/>

# 외부 설정을 할 수 있는 종류들
- properties
- YAML
- 환경변수
- 커맨드 라인 아규먼트

위 종류들 중에 properties를 활용하여 외부 설정을 하는 방법에 대해서 공부해보겠습니다.<br/>
<br/>

## properties의 우선 순위
1. **유저 홈 디렉토리에 있는 spring-boot-dev-tools.properties**
2. **테스트에 있는 @TestPropertySource**
3. **@SpringBootTest 애노테이션의 properties 애트리뷰트**
4. **커맨드 라인 아규먼트**
```bash
java -jar springinit-0.0.1-SNAPSHOT.jar --junjang.name=kimjunhyeung
```
5. SPRING_APPLICATION_JSON (환경 변수 또는 시스템 프로티) 에 들어있는 프로퍼티 : 
[참고](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config)
6. ServletConfig 파라미터
7. ServletContext 파라미터
8. java:comp/env JNDI 애트리뷰트
9.  System.getProperties() 자바 시스템 프로퍼티
10. OS 환경 변수
11. RandomValuePropertySource
12. JAR 밖에 있는 특정 프로파일용 application properties
13. JAR 안에 있는 특정 프로파일용 application properties
14. JAR 밖에 있는 application properties
15. **JAR 안에 있는 application properties**
16. @PropertySource
17. 기본 프로퍼티 (SpringApplication.setDefaultProperties)

위 순서는 전체를 외울 필요는 없지만 중요한 순서는 알아두는게 디버깅을 하는데 도움이 됩니다.<br/>
<br/>

## application.properties 우선 순위 
아래의 네가지 위치에 application.properties가 위치할 수 있는데 위치에 따라 우선순위가 달라집니다.
1. file:./config/
2. file:./
3. classpath:/config/
4. classpath:/
<br/>
<br/>

## application.properties 생성 및 선언하기
![configuration](/images/springboot/configuration/co1.png) main - resources에 application.properties를 생성하고 key와 value을 선언합니다.<br/>
<br/>

## 선언한 변수 출력하기(15순위)
![configuration](/images/springboot/configuration/co2.png) 그리고 선언한 value를 출력하기 위해 ApplicationRunner에 @Value 를 사용하여 선언하고 변수를 만들어줍니다. 그리고 override한 run 메소드에 선언하고 출력합니다.<br/>
![configuration](/images/springboot/configuration/co3.png) 내가 선언한 외부 설정이 출력된 것을 볼 수 있습니다.<br/>
<br/>

## 커맨드 라인 출력하기(4순위)
```bash
mvn clean package
java -jar springinit-0.0.1-SNAPSHOT.jar --junjang.name=kimjunhyeung
```
![configuration](/images/springboot/configuration/co4.png) packaging을 한 jar를 커맨드 라인을 선언하고 출력하면 우선순위가 높기 때문에 커맨드 라인 값으로 출력됩니다.<br/>
<br/>

# 외부 설정 테스트하기
테스트를 하기 위해선 test 경로에 있는 class에서 작업을 진행합니다.<br/>
```java
import static org.assertj.core.api.Assertions.assertThat;

@RunWith(SpringRunner.class)
@SpringBootTest
public class ApplicationstartedApplicationTests {

    @Autowired
    Environment environment;

    @Test
    public void contextLoads() {
        assertThat(environment.getProperty("junjang.name"))
                .isEqualTo("junjang");

    }

}
```
![configuration](/images/springboot/configuration/co5.png) Enviroment를 의존성 주입하여 테스트에 사용합니다.
그리고 비교 테스트를 위해선 assertThat 함수가 필요한데 이를 사용하기 위해서는 static으로 import해야 사용할 수 있습니다.
getProperty를 통해 key를 가져오고 isEqualTo 내장함수로 값이 같은지 비교합니다.
이상 없이 테스트가 완료될 것입니다.<br/>
![configuration](/images/springboot/configuration/co6.png) 테스트를 위하여 따로 application.properties를 만들기 위해서는 resources 폴더를 생성하여 내부에 새롭게 만들어주어야 합니다.
intelli J 에서는 위와 같이 만들면 됩니다.<br/>
![configuration](/images/springboot/configuration/co7.png) 테스트 properties에 테스트와 동일한 값을 넣고 비교하면 오류 없이 출력됩니다.<br/>
<br/>

## @SpringBootTest로 출력하기(3순위)
이 어노테이션의 장점은 바로 작성하여 테스트할 수 있어 신속하게 테스트가 가능합니다.<br/>
```java
@SpringBootTest(properties = "junjang.name=junjang2")
```
![configuration](/images/springboot/configuration/co11.png) properties를 바로 비교한 모습입니다.<br/>
<br/>

## @TestPropertySource로 출력하기(2순위)
이 어노테이션의 장점은 테스트시 순위가 가장 높아 확실한 값을 얻을 수 있어 디버깅 하기 용이합니다.<br/>
```java
@TestPropertySource(properties = {"junjang.name=junjang3", "junjang.age=23"})
```
![configuration](/images/springboot/configuration/co12.png) 테스트가 출력된 모습입니다.<br/>
하지만 한 두개의 값이면 다행이지만 수많은 key와 value이 있다면 사실상 main과 test의 properties를 언제나 동일하게 유지할 수 없습니다.
그래서 대표적으로 **2개**의 방법을 사용하여 테스트를 진행합니다.<br/>
<br/>

## 테스트시 properties 편리하게 관리하기

### 1. test 폴더에 있는 application.properties를 삭제한다.
이 방법을 이해하기 위해선 어플리케이션이 실행되는 **원리**를 알아야합니다.<br/>
test에 있는 소스들은 전부 **main**에 있는 소스들에 의해 **오버라이딩**됩니다. 그렇기 때문에 만약 main에 있는 key가 test에 없다면 동일하게 선언해주어야 하는 것입니다.<br/>
때문에 test에 properties를 **없애면** main에 있는 값으로 진행되기 때문에 없어도 되는 것입니다.<br/>
<br/>

### 2. classpath를 지정하여 따로 관리한다.
```java
@TestPropertySource(locations = "classpath:/test.properties")
```
![configuration](/images/springboot/configuration/co13.png) 위처럼 동일한 이름이 아닌 새로운 이름으로 생성하여 classpath를 지정하여 해당 properties만 사용하는 것입니다.<br/>
<br/>
## 기타 기능들 활용하기
- 랜덤
- 플레이스 홀더

참고 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config-random-values)<br/>
<br/>

### 랜덤값 설정하기
외부 설정에도 여러가지 랜덤값을 선언할 수 있습니다.
특이한 점은 사용시 어떤 구간에도 절대 **공백**이 있어서는 안됩니다.<br/>
```
${random.*}
${random.int(0,100)}
```
![configuration](/images/springboot/configuration/co8.png) 숫자에 대해 랜덤값을 출력하고 싶으면 위와 같이 사용합니다.<br/>
![configuration](/images/springboot/configuration/co9.png) 그리고 이름과 동일하게 빈 등록을 해준 후 출력하면..<br/>
![configuration](/images/springboot/configuration/co10.png) 나이가 랜덤으로 출력된 것을 확인할 수 있습니다.<br/>
<br/>

### 플레이스 홀더 활용하기
이미 선언했던 key를 다시 재사용 할 수 있습니다.<br/>
![configuration](/images/springboot/configuration/co14.png) 위처럼 사용이 가능합니다.<br/>
<br/>

## @ConfigurationProperties 생성하기
타입-세이프한 방법으로 같은 Key를 사용하는 값들을 안전하고 편리하게 사용할 수 있도록 해줍니다.<br/>
먼저 새로운 클래스를 생성합니다.
![configuration](/images/springboot/configuration/co17.png) 생성한 클래스에 properties의 사용하는 key값을 변수로 지정하고 getter, setter를 생성합니다.
그리고 @ConfigurationProperties("key값")을 선언합니다.<br/>
여기서 **오류**가 뜰텐데 META 정보를 생성해주는 플러그인을 추가해달라는 의미입니다.
추가하기 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#configuration-metadata-annotation-processor)<br/>
  ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
  </dependency>
  ```
![configuration](/images/springboot/configuration/co18.png) 위 코드를 pom.xml에 추가하면 추가가 된 것입니다.
혹시 gradle을 사용한다면 밑에 나타난 의존성을 추가해주시면 됩니다.<br/>
```java
@SpringBootApplication
@EnableConfigurationProperties(junjangProperties.class)
```
![configuration](/images/springboot/configuration/co19.png) @EnableConfigurationProperties를 통해 class를 가시적으로 나타낼 수 있지만 자동으로 등록할 수 있기 때문에 생략해도 됩니다.<br/>
<br/>
### 자동으로 등록하기
![configuration](/images/springboot/configuration/co20.png) @Component를 통해 빈으로 등록해주기만 하면 자동으로 주입됩니다.<br/>
<br/>

## 등록된 properties클래스 사용하기
클래스를 생성하고 빈으로 등록하였습니다.
이제는 등록된 클래스를 사용할 차례입니다.<br/>
```java
@Autowired
junjangProperties junjangProperties;

@Override
public void run(ApplicationArguments args) throws Exception {
    System.out.println("=======================");
    System.out.println(junjangProperties.getFullName());
    System.out.println(junjangProperties.getAge());
    System.out.println("=======================");
}
```
![configuration](/images/springboot/configuration/co21.png) 의존성 주입을 통해 단순히 꺼내오고 getter를 통해 빼오기만 하면 됩니다.
그리고 출력하면 외부 설정 value가 출력될 것입니다.<br/>
<br/>

### 융통성 있는 바인딩하기
외부 설정간 key값을 꼭 딱딱하게 선언할 필요성이 없습니다.
- context-path (케밥)
- context_path (언드스코어)
- contextPath (캐멀)
- CONTEXTPATH

이 네가지 어느 것을 사용하던지 동일한 value값을 가져옵니다.
![configuration](/images/springboot/configuration/co22.png) `-`로 선언해도 클래스에선 동일한 값을 가져옵니다.<br/>
<br/>

### 프로퍼티 타입 컨버전 @DurationUnit
SpringBoot가 제공하는 독특한 컨버전 타입으로서 시간정보를 받고 싶을 때 아래의 타입을 잘 명시해주면 Annotation을 활용하지 않아도 시간정보를 가져옵니다.<br/>
참고자료 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config-conversion-duration)<br/>
![configuration](/images/springboot/configuration/co23.png) @DurationUnit을 초 단위로 나타내겠다는 의미입니다. 시간을 명시하지 않으면 30초로 나타내게 됩니다.<br/>
![configuration](/images/springboot/configuration/co27.png) 초 단위로 30초가 나타난 것을 알 수 있습니다.
여기서 properties에 초를 명시했다면 해당 초로 출력이 됩니다.<br/>
![configuration](/images/springboot/configuration/co24.png) 위처럼 25초로 입력하면 **PT25s**로 출력됩니다.<br/>
![configuration](/images/springboot/configuration/co25.png) @DurationUnit 어노테이션을 활용하지 않고 사용한다면 타입만 명시하여 사용이 가능합니다.
타입들은 위 공식문서를 참고하여 사용하면 됩니다.<br/>
<br/>

### 프로퍼티 값을 검증하는 @Validated
이 어노테이션을 활용하면 값을 검증할 수 있습니다.
JSR-303으로서 (@NotNull, ...) 등 많은 검증을 할 수 있습니다.
생성한 properties의 클래스에 선언하면 됩니다.<br/>
![configuration](/images/springboot/configuration/co26.png) @Validated를 선언하고 값이 꼭 있어야 하는 변수에 검증을 한 결과 값입니다. 
임의로 name의 value값을 공란으로 두었기 때문에 에러난 상황입니다.
만약 값이 있었다면 테스트 통과가 되었을 것입니다.<br/>
이 방법을 많이 사용하는 이유는 @Value를 사용하였을 때보다 훨씬 많은 기능들을 사용할 수 있습니다.
