---
title: JPA - Value Mapping(밸류 매핑)
date: 2019-05-21 19:22:54
tags: [JPA, Value]
---

![images](/images/jpa/jpa.jpg)<br/>

# 밸류 매핑
Entity와는 다르게 다른 타입에 **종속적인 타입**을 매핑할 때 사용하는 방법입니다.
엔티티 클래스에 종속적인 밸류 클래스를 생성하여 엔티티 클래스가 생성, 삭제 될 때 동일하게 행동하게 됩니다.<br/>
여기서 실습하는 코드는 이전에 학습했던 [프로젝트세팅](https://junjangsee.github.io/2019/05/20/jpa/jpa-03/)과 [엔티티매핑](https://junjangsee.github.io/2019/05/21/jpa/jpa-04/)에 설명되어 있어 참고하시면 됩니다.<br/>
<br/>

## 밸류 타입 종류
- 기본 타입 (String, Date, Boolean, ...)
- Composite Value 타입
- Collection Value 타입
  - 기본 타입의 콜렉션
  - 컴포짓 타입의 콜렉션

이 중에서 Composite Value 타입에 대해 알아보겠습니다.
<br/>

## Composite 밸류 타입
- @Embadable : Composite Value 클래스에 지정하면 해당 클래스를 Composite Value로 만듭니다.
- @Embadded : Entity에서 Composite Value로 지정한 클래스를 불러와 정의할 때 사용합니다.
- @AttributeOverrides : 여러 값을 오버라이딩 하기위한 그룹 어노테이션입니다.
- @AttributeOverride : 오버라이딩 하기 위해 사용합니다.

<br/>

## Value 클래스
```java
@Embeddable
public class Address {

    private String street;

    private String city;

    private String state;

    private String zipCode;
}
```
![Entity](/images/jpa/value/val1.png) Address는 Account에 종속적인 클래스입니다. **@Embeddable**를 선언하여 Composite Value로 지정합니다.<br/>
<br/>

## Entity 클래스
```java
    @Embedded
    private Address address;
```
![Entity](/images/jpa/value/val2.png) Address 객체를 가져와 **@Embedded**를 선언하여 Composite Value로 지정한 클래스를 정의합니다.
그 후 애플리케이션을 실행하면 Account 테이블이 생성 될 때 함께 생성되는 것을 확인할 수 있습니다.<br/>
<br/>

### @AttributeOverrides
여러 값을 오버라이딩 하기위한 그룹 어노테이션으로 매핑시 컬럼명을 바꾸는 등 Column에 대한 설정을 재정의 하고 싶을 떄 사용합니다.<br/>
**@AttributeOverrides**는 여러개의 오버라이딩을 할 수 있게 해주고 내부에 **@AttributeOverride**를 통해 복수개의 변수를 설정할 수 있습니다.<br/>

```java
@Embedded
    @AttributeOverrides({
            @AttributeOverride(name = "street", column = @Column(name = "home_street"))
    })
    private Address address;
```

Value 클래스에 있는 street이라는 변수를 home_street으로 변경하여 생성하도록 합니다.<br/>
![Entity](/images/jpa/value/val3.png) 애플리케이션 실행 시 street이 home_street으로 생성된 것을 확인할 수 있습니다.<br/>