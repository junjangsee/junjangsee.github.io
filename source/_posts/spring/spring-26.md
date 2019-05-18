---
title: SpringBoot - ORM, JPA의 개념
date: 2019-05-14 14:42:45
tags: [SpringBoot, DB, ORM, JPA]
---

![images](/images/springboot/springboot.png)<br/>

# ORM(Object-Relational Mapping)
객체와 릴레이션을 Mapping(매핑)할 때 발생하는 개념적 불일치를 해결하는 프레임워크입니다.
- 참고자료 : [하이버네이트 공식문서](http://hibernate.org/orm/what-is-an-orm/)<br/>
<br/>

## ORM에서 다루어지는 문제들
- 객체는 크기가 굉장히 다양하지만 테이블은 테이블과 컬럼 밖에 없고 크기가 한정적이다.
- 복잡한 객체의 다양한 크기들을 테이블에 맵핑을 시킬 수 있을 것인가에 대한 해결책을 제공한다.
- 테이블은 상속이 없지만 객체들은 상속이 있음 클래스간에 상속구조를 만들 수 있는데 그런 상속구조를 테이블로는 어떻게 매핑할 것인가
- Relational 에서 식별자는 굉장히 단순하다.
- Object에서는 Identity는 Hashcode? equals Method?
- Object Identity가 같으면 도대체 어떤 Entity가 같아야 우리는 객체가 같다고 해야되는건가?
- 테이블에서는 식별자만 같으면 같은건데 Object에서는 그게 아님 프로퍼티만 같다고 해서 같은건가? 
- ID가 null이면 어떻게 되는거지?

위와 같은 문제들이 다루어집니다.<br/>
<br/>

# JPA(Java Persistence API)
ORM을 손쉽게 구현하도록 만들어줍니다.
- 대부분의 JPA스펙이 Hibernate에 기반해서 만들어집니다.(하지만 모든 기능을 커버하진 않아서 직접 Hibernate에 설정을 다루어야 합니다.)
- ORM을 위한 자바(EE) 표준입니다.<br/>
<br/>

## SpringData JPA
JPA 표준 스펙을 아주쉽게 사용할 수 있도록 SpringData로 추상화 시켜놓은 것입니다.
- JPA 의존성을 추가하면 SpringDataJDBC의 기능을 모두 다 사용할 수 있으면서 부가적으로 JPA, Hibernate, SpringDataJPA 기능을 더 사용할 수 있습니다.
- SDJ(SpringDataJPA) -> JPA -> Hibernate -> Datasource(JDBC) 까지 의존성이 추가됩니다.