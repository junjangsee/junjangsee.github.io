---
title: SpringBoot - MVC HtmlUnit
date: 2019-05-10 17:40:23
tags: [SpringBoot, MVC, HtmlUnit, WebClient]
---

![images](/images/springboot/springboot.png)<br/>

# HtmlUnit
HTML 뷰의 테스트를 전문적으로 하기 위해서 사용하는 툴 입니다.
- 홈페이지 : [공식홈페이지](http://htmlunit.sourceforge.net/)
- 사용방법 : [공식홈페이지 - 사용방법](http://htmlunit.sourceforge.net/gettingStarted.html)<br/>
<br/>

## 의존성 추가하기
```xml
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>htmlunit-driver</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>net.sourceforge.htmlunit</groupId>
            <artifactId>htmlunit</artifactId>
            <scope>test</scope>
        </dependency>
```
![Thymeleaf](/images/springboot/htmlunit/un1.png) 
두 가지의 의존성을 추가합니다.<br/>
<br/>

## 테스트 클래스

```java
@RunWith(SpringRunner.class)
@WebMvcTest(SampleController.class)
public class SampleControllerTest {

    @Autowired
    WebClient webClient;
    // 의존성을 추가하면 WebClient를 주입받을 수 있습니다.

    @Test
    public void hello() throws Exception {
        HtmlPage page = webClient.getPage("/hello"); // /hello로 요청합니다.
        HtmlHeading1 h1 = page.getFirstByXPath("//h1"); //h1 제일 앞에있는 것 하나만 가져옵니다.
        assertThat(h1.getTextContent()).isEqualToIgnoringCase("junjang"); // 대소문자인것을 무시하고 문자열이 같은지 비교합니다.
    }
}
```
![Thymeleaf](/images/springboot/htmlunit/un2.png) WebClient라는 의존성을 주입받아 HtmlUnit을 통해 테스트를 진행합니다. 
이처럼 MockMvc보다 더 전문적으로 HTML에 대해서 테스트를 할 수 있습니다.

더 많은 사용법을 알고 싶으면 상단에 홈페이지에 자세히 나와있으니 참고하시면 됩니다.<br/>