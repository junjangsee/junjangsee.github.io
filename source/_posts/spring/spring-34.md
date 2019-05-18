---
title: SpringBoot - Actuator의 개념
date: 2019-05-18 17:25:53
tags: [SpringBoot, Actuator, Endpoints]
---

![images](/images/springboot/springboot.png)<br/>

# Actuator
Actuator을 사용하면 스프링 애플리케이션 운영중에 주시할 수 있는 여러가지 유용한 정보들을 제공합니다.
이렇게 얻은 정보들을 **Endpoint**를 통해서 제공해줍니다.<br/>
<br/>

## Actuator 의존성 추가
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
![Actuator](/images/springboot/actuator/ac1.png) 의존성을 추가해줍니다.<br/>
<br/>

## Endpoints
Actuator 의존성을 추가해주면 Actuator Endpoints 정보들이 자동으로 활성화됩니다.
- 다양한 Endpoints를 제공합니다.
- JMX또는 HTTP를 통해 접근가능합니다.
- shutdown을 제외한 모든 Endpoint는 기본적으로 ​활성화​ 상태입니다.

<br/>

## Endpoints 정보들
- `auditevents` : 인증정보(인증정보 획득, 실패) 이벤트
- `beans`: 등록된 빈들
- `conditions` : 어떤 자동 설정이 어떤 조건에 의해서 적용됐는지 안됐는지 정보
- `configprops`: 여기 보이는 것들이 application.properties에 정의가 가능한 것들
- `env`: Spring의 Environment를 Exposes 시켜주고 Environment안에 있는 properties를 보여줌
- `flyway`: flyway migration 정보를 보여줌
- `health`: 이 애플리케이션이 현재 잘 구동 중인지 health 정보를 보여줌
- `httptrace`: 최근 100개의 HTTP 요청과 응답 소요시간등을 보여줌
- `info`: 이 애플리케이션에 관련된 임의의 정보들을 보여줌
- `logger`: 어떤 패키지가 어떤 logging 레벨을 가지고 있는지 또는 그런 logging 레벨들을 운영중에 수정할 수도 있음
- `liquibase`: flyway와 비슷한 데이터베이스 migration liquibase가 의존성이 있는 경우에만 보여줌
- `metrics`: 애플리케이션에 핵심이 되는 정보들 이 애플리케이션이 사용하는 메모리, 
CPU가 어느정도 되는지, 그런 정보들을 여러 모니터링 애플리케이션 사용할 수 있는 공통의 포멧으로 만들어서 제공해줌 다른 모니터링 툴과 연동해서 사용할 수 있어서 다른 모니터링 툴에서 알림을 받거나하는 등으로 활용가능 아주 유용한 정보를 담고있는 Endpoint 정보
- `mappings`: 컨트롤러 맵핑정보를 보여줌
- `scheduledtasks`: scheduled 애노테이션으로 정의해놓은 scheduled tasks 정보를 보여줌
- `sessions`: Spring Session 관련된 Endpoint
- `shutdown`: 애플리케이션을 끌 수 있는 Endpoint 비활성화 되어있음 하지만 공개가 되어있어 동작함
- `threaddump`: threaddump를 뜰 수 있음
- `heapdump`: heapdump(현재 상태 메모리에 무엇이 들어있는지)를 뜰 수 있음
- `jolokia`: JMX beans를 HTTP View에서도 볼 수 있음
  JMX beans는 Java Management beans이라고 하며 JMX 표준에 준하는 bean을 만들면
  애플리케이션 밖에서 그 빈의 Operation들을 호출할 수 있음
  기본적으로 위의 모든 Endpoints 들은 JMX `Mbeans`로 등록되므로 전부 `Mbeans`로 노출이 됨
  그 외에도 추가로 JMX bean에 충족하는 빈에 애플리케이션을 만들어서 등록할 수 있음
  그런 것들도 jolokia를 통해서 HTTP를 통해서 접근이 가능해짐
- `logfile`: logfile에 해당하는 것들도 볼 수 있음
- `prometheus`: metrics 정보를 Prometheus server에서 캡쳐해갈 수 있는 형태로 변환해줌

참고자료 : [스프링공식문서](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints)<br/>
<br/>
## Actuator 접근
> http://localhost:8080/actuator

위와같이 HTTP로 접근하면 Spring HATEOAS(Hypermedia as the Engine of Application State)를 통해 접근가능한 링크 정보를 리턴해줍니다.<br/>
![Actuator](/images/springboot/actuator/ac3.png) http를 통해 접근할 경우 공개된 정보는 `health`, `info` 두가지 밖에 없는데 그 이유는 Endpoint의 Web 부분은 보안이슈 문제로 위 두가지의 정보만 공개한 상태입니다.
때문에 추가로 보기 위해선 활성화 설정을 해야합니다.<br/>
활성화 하는 방법에 대해선 추후 포스팅에서 알아보겠습니다.<br/>