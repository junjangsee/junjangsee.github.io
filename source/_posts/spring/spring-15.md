---
title: SpringBoot - MVC Static Resource(정적 리소스)
date: 2019-05-08 21:11:25
tags: [SpringBoot, MVC, Resource]
---

![images](/images/springboot/springboot.png)<br/>

# Static Resource(정적 리소스)
클라이언트가 요청하면 기존에 만들어져있는걸 그냥 응답(보내)주면 되는 경우에 사용합니다.
기본적인 Mapping(매핑) 경로는 /** 입니다.<br/>
<br/>

## 기본 리소스 위치
- classpath:/static
- classpath:/public
- classpath:/resources/
- classpath:/META-INF/resources

![StaticResource](/images/springboot/staticresource/sta1.png) 위치에 html 파일을 생성하게 되면 자동으로 Root(기본) 경로로 매핑됩니다.
<br/>
<br/>

## 정적 리소스의 원리
정적 리소스가 어떻게 구동되는지 원리를 알아봅시다.<br/>

![StaticResource](/images/springboot/staticresource/sta2.png) 최초 클라이언트에서 요청을 보냈을 때 **304**가 출력됩니다.<br/>
![StaticResource](/images/springboot/staticresource/sta3.png) 이러한 요청에 대해서 응답을 보내면 **200**이 출력됩니다.<br/>
이런 status는 어떻게 나온걸까요?
이를 알기위해선 **If-Modyfied-Since**, **Last-Modified**의 관계를 알아야합니다.<br/>
<br/>

### status가 304일 때
![StaticResource](/images/springboot/staticresource/sta5.png) **304**는 클라이언트의 요청이 들어오고 그 요청이 가장 **최신**이라면 나타나는 상태입니다.
즉 **If-Modyfied-Since**와 **Last-Modified**가 같을 때를 의미하게 됩니다.
> **If-Modyfied-Since** == **Last-Modified**

<br/>

### status가 200일 때
![StaticResource](/images/springboot/staticresource/sta4.png) **200**은 클라이언트의 요청이 들어온 후 응답을 주게 되면 나타나는 상태입니다.
즉, **If-Modyfied-Since**가 **Last-Modified**보다 빠른 시간이라는 것입니다.
> **If-Modyfied-Since** < **Last-Modified**

만약 여기서 응답을 준 후 페이지를 Reload하게 되면 다시 304 상태로 돌아가게 됩니다.
<br/>
<br/>

## 리소스 위치 변경하기
기본 리소스 위치에서 추가로 등록하는 방법이 있습니다.
<br/>

### spring.mvc.static-path-pattern
기존에 Root로 매핑이 되어있다면 해당 Key를 이용하여 Root를 변경할 수 있습니다.
![StaticResource](/images/springboot/staticresource/sta6.png) properties에 spring.mvc.static-path-pattern을 추가하여 내가 매핑하고 싶은 위치를 선언합니다.<br/>
![StaticResource](/images/springboot/staticresource/sta7.png) 서버재시작 후 해당 경로로 가면 동일한 HTML이 출력됩니다.<br/>
<br/>

### spring.mvc.static-locations
위 Key도 마찬가지로 Root를 바꾸는 것이지만 기존 경로를 전부 제외하고 새롭게 만드는 것이기 떄문에 권장하진 않습니다.<br/>
<br/>

## ResourceHttpRequestHandler를 활용한 커스텀
스프링 부트에서 MVC를 커스텀 할 수 있는 **WebMvcConfigurer**의 **addRersourceHandlers**을 활용하여 커스텀이 가능합니다.
- 기존의 Resource Handler는 건드리지 않고 추가하는 개념입니다.
- 기존의 Resource Handler는 다르게 캐싱전략이 다르므로 따로 **캐싱처리**를 해주어야 합니다.
<br/>

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/m/**") // /m 으로 시작하는 요청이 오면
                .addResourceLocations("classpath:/m/") // classpath 기준으로 m 디렉토리 밑에서 제공
                .setCachePeriod(20); // 20초
    }
}
```
![StaticResource](/images/springboot/staticresource/sta8.png) WebConfig 클래스를 생성하고 WebMvcConfigurer를 구현합니다.
그리고 addRersourceHandlers를 오버라이딩 하여 경로를 선언하고 캐싱을 처리합니다.<br/>
![StaticResource](/images/springboot/staticresource/sta9.png) 위에서 경로를 선언한 대로 동일 경로에 HTML을 생성합니다.<br/>
![StaticResource](/images/springboot/staticresource/sta10.png) 서버 Restart 후 경로를 입력하면 커스텀한 경로에 있는 HTML이 출력됩니다.
물론 기존에 있는 HTML도 출력됩니다.<br/>