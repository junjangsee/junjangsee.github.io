---
title: JPA - Entity Mapping(엔티티 매핑)
date: 2019-05-21 17:43:59
tags: [JPA, Entity]
---

![images](/images/jpa/jpa.jpg)<br/>

# 엔티티 매핑
엔티티를 매핑하는 방법에는 XML, 어노테이션 총 2가지의 방식이 있지만 보통 어노테이션 방식으로 사용하게 됩니다.
어노테이션을 활용하여 매핑하는 법에 대해 알아보겠습니다.<br/>
여기서 실습하는 코드는 이전에 학습했던 [프로젝트세팅](https://junjangsee.github.io/2019/05/20/jpa/jpa-03/)에 설명되어 있습니다<br/>
<br/>

## @Entity
엔티티 클래스를 빈으로 등록하여 JPA와 연동할 수 있도록 해줍니다. 기본적으로 **@Table**을 포함하고 있습니다.<br/>

```java
@Entity(name = "users")
```
도메인 클래스명을 다른 클래스명으로 정의하고 테이블을 만들려고 할 때 name을 선언합니다.<br/>
![Entity](/images/jpa/entity/en1.png) 도메인명은 **User**지만 **users**로 테이블이 생성된 모습입니다.<br/>
<br/>

## @Id, @GeneratedValue
- @Id : 엔티티와 주키를 매핑할 때 사용합니다.
 - 자바의 모든 primitive 타입과 타입에 대한 wrapper 클래스를 사용할 수 있습니다.
- @GeneratedValue : 주키의 생성 방법을 매핑할 때 사용합니다.
 - 보통은 AUTO(기본값)를 사용합니다.

![Entity](/images/jpa/entity/en3.png) 
<br/>

## @Column
컬럼에 대한 설정 값을 매핑할 때 사용합니다.
- unique : unique 사용여부를 결정합니다.
- nullable : null 사용여부를 결정합니다.
- columnDefinition : 반드시 SQL문을 작성해야할 때 사용합니다.

![Entity](/images/jpa/entity/en2.png) username은 null이면 안되고 unique해야한다는 매핑을 한 예시입니다.<br/>
<br/>

## @Temporal
날짜를 매핑할 때 사용합니다.
- Java의 Date에 관련된 값을 타입에 맞게 변환해줍니다.

<br/>

## @Transient
도메인에는 선언하였지만 컬럼으로는 매핑되지 않도록 합니다.
<br/>
<br/>

## 엔티티 클래스
지금까지 학습한 어노테이션을 적용한 엔티티 클래스를 보겠습니다.

```java
import javax.persistence.*;
import java.util.Date;

@Entity
public class Account {

    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    private String password;

    @Temporal(TemporalType.TIMESTAMP)
    private Date created = new Date();

    private String yes;

    @Transient
    private String no;

}
```
![Entity](/images/jpa/entity/en4.png) 위 모든 어노테이션을 활용해 보았을때의 클래스 모습입니다.
결과를 보면 TIMESTAMP형식으로 날짜가 들어간 것을 확인할 수 있고 no 변수는 **@Transient**로 인해 컬럼에 매핑되지 않은 것을 볼 수 있습니다.<br/>
<br/>

## SQL을 확인하고 싶다면?
JPA에서 자동으로 생성해서 처리한 SQL을 Console에서 볼 수 있는데 **application.properries**에서 쉽게 설정이 가능합니다.<br/>

```
spring.jpa.show-sql=true
```
위 설정은 SQL이 실행되는 경과를 출력하는 역할을 해줍니다.<br/>

```
spring.jpa.properties.hibernate.format_sql=true
```
위 설정은 위에서 보여주는 SQL을 가독성 높게 출력해줍니다.
그래서 두 설정을 한 후 출력하게 되면..<br/>
![Entity](/images/jpa/entity/en5.png) 위처럼 보기 쉽게 SQL문을 확인할 수 있습니다.<br/>