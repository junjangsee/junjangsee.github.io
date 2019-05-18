---
title: SpringBoot - MVC Index, Favicon(인덱스, 파비콘)
date: 2019-05-09 19:02:17
tags: [SpringBoot, MVC, Index, Favicon]
---

![images](/images/springboot/springboot.png)<br/>

# Index(인덱스)
인덱스는 `/`로 요청했을 때 호출되는 페이지를 말합니다.
SpringBoot에서는 Error Handler가 내장 인덱스 페이지를 보여줍니다. 이 페이지를 커스텀 시켜보겠습니다.<br/>
![IndexFavicon](/images/springboot/indexfavicon/in1.png) 기본 리소스 위치에 index.html이 있으면 자동으로 매핑하여 출력해줍니다.
혹시 기본 리소스 위치가 기억이 안난다면 [정적리소스](https://junjangsee.github.io/2019/05/08/spring/spring-15/)를 참고해주시면 됩니다.<br/>
![IndexFavicon](/images/springboot/indexfavicon/in2.png) index.html에 있는 제목이 출력된 모습입니다.<br/>
<br/>

# Favicon(파비콘)
파비콘은 웹페이지 로드시 함께 출력되는 좌측에 이미지를 말합니다.
- 파비콘 만드는 홈페이지 : [파비콘 만들기](https://favicon.io/)
- 파비콘 위치 : 기본 리소스 위치
<br/>

![IndexFavicon](/images/springboot/indexfavicon/in3.png) 스프링 파비콘에서 내가 커스텀한 파비콘으로 바뀐 것을 알 수 있습니다.
혹시나 파비콘이 바뀌지 않는다면 [파비콘 오류시 해결방법](https://stackoverflow.com/questions/2208933/how-do-i-force-a-favicon-refresh)을 참고하시면 됩니다.<br/>