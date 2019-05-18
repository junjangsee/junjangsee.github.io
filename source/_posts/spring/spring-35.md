---
title: SpringBoot - Actuator JMX, HTTP
date: 2019-05-18 18:40:36
tags: [SpringBoot, Actuator, JMX]
---

![images](/images/springboot/springboot.png)<br/>

# Actuator 
이전 포스팅에 이어서 Actuator를 사용하는 방법에 대해 계속 학습해 보겠습니다.<br/>

## JMX
### JConsole
JConsole은 자바에서 지원하는 JMX입니다.
참고자료 : [공식문서](https://docs.oracle.com/javase/7/docs/technotes/guides/management/jconsole.html)<br/>
```bash
jconsole
```
터미널에서 jconsole을 입력합니다.
![Actuator](/images/springboot/actuator/ac4.png) jconsole을 입력하면 위와같은 창이 뜹니다.<br/> 
![Actuator](/images/springboot/actuator/ac5.png) 현재 로컬 상태이니 해당 프로젝트를 클릭하고 Connect합니다.<br/>
![Actuator](/images/springboot/actuator/ac6.png) SSL을 적용하지 않은 상태이므로 Insecure connection을 클릭합니다.<br/>
![Actuator](/images/springboot/actuator/ac7.png) 
접속하면 여러가지 정보들을 확인 할 수 있습니다.<br/>
Endpoints를 확인하기 위해 MBeans에서 애플리케이션을 확인합니다.<br/>
![Actuator](/images/springboot/actuator/ac8.png) 빈을 확인하기 위해 누른 창에서 도저히 사람의 눈으로 확인할 수 없는 긴 영어가 출력됩니다.
전 이걸 읽느니 차라리 **죽음**을 택하겠습니다.<br/>
저같은 개발자가 많았는지 이를 대응하기 위해 **VisualVM**이라는 툴이 나왔습니다.<br/>
<br/>

### VisualVM
기존에는 java에서 제공을 했었는데 java10 부터 제공되지 않아서 별도로 설치해야합니다.<br/>
다운로드 : [홈페이지](https://visualvm.github.io/download.html) 
![Actuator](/images/springboot/actuator/ac9.png) 위 주로소 들어가 각 OS에 맞는 것을 설치합니다.<br/>
![Actuator](/images/springboot/actuator/ac10.png) 실행을 시키면 좌측에 애플리케이션과 우측에 정보들이 나와있습니다. 
jconsole의 가독성에 비하면 많이 발전된 모습이 보입니다.<br/>
기본적으로 VisualVM은 MBean이 없기 때문에 Plugin을 설치해야합니다.<br/>
<br/>

#### MBean Plugin 설치
![Actuator](/images/springboot/actuator/ac11.png) Tools - Plugins을 클릭합니다.<br/>
![Actuator](/images/springboot/actuator/ac12.png) Available Plugins에서 MBeand을 검색 후 - VisualVM 클릭 - install을 클릭하여 설치합니다.<br/>
![Actuator](/images/springboot/actuator/ac13.png) 아까와 동일한 MBeans를 보실 수 있습니다.<br/>
<br/>

## HTTP
기존 HTTP를 통해 확인했을 땐 health와 info를 제외한 대부분의 Endpoint가 ​비공개​ 상태였습니다.
이 비공개 상태를 공개 옵션을 설정해 모두 확인할 수 있습니다.<br/>
<br/>

### management.endpoints.web.exposure.include
```
management.endpoints.web.exposure.include=*
```
위 설정을 application.properties에 하게되면 /actuator로 접속했을 시 전부 출력하게 됩니다.<br/>
![Actuator](/images/springboot/actuator/ac14.png) 여기서 전체 정보 대신 특정 정보를 출력하고 싶다면 Endpoints명을 입력해서 선택 출력도 가능합니다.<br/>
하지만 이런 정보들이 노출되면 보안이슈가 있으므로 **SpringSecurity**를 통해서 **Admin**만 접근하도록 할 필요성이 있습니다.
그 방법은 다음 포스팅에서 이어서 학습하겠습니다.<br/>