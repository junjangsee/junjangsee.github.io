---
title: JPA - Common CustomRepository
date: 2020-01-02 22:00:00
tags: [JPA, Common, Custom, Repository]
---

![images](/images/jpa/jpa.jpg)<br/>

# CustomRepository
쿼리 메소드(쿼리 생성과 쿼리 찾아쓰기)로 해결이 되지 않는 경우 직접 코딩으로 구현 가능하도록 하는 방법입니다. 구현 방법의 순서는 아래와 같습니다.
1. 커스텀 리포지토리 인터페이스 정의하기
2. 인터페이스 구현 클래스 만들기 (기본 접미어는 Impl)
3. 엔티티 리포지토리에 커스텀 리포지토리 인터페이스 추가하기

<br/>

## 1. 커스텀 리포지토리 인터페이스 정의하기
![CustomRepository](/images/jpa/customrepository/cus1.png) Post Entity와 PostRepository를 생성합니다.<br/>
![CustomRepository](/images/jpa/customrepository/cus2.png) jUnit4를 활용하여 의존성 주입이 잘 됐는지 확인하고 잘 되었다면 이상없이 테스트가 통과 될 것입니다.<br/>
```properties
spring.jpa.properties.hibernate.format_sql=true
logging.level.org.hibernate.type.descriptor.sql=trace
```
![CustomRepository](/images/jpa/customrepository/cus3.png) 쿼리를 출력하기 위해 properties에 위의 코드를 추가합니다.<br/>
기존에 만들었던 PostRepository를 커스텀 하기 위해서 인터페이스를 하나 추가로 정의합니다.
![CustomRepository](/images/jpa/customrepository/cus4.png) 정의한 인터페이스에 Post를 찾는 메소드를 정의합니다.<br/> 
<br/>

## 2. 인터페이스 구현 클래스 만들기 (기본 접미어는 Impl)
![CustomRepository](/images/jpa/customrepository/cus6.png) 커스텀한 리파지토리명에서 `Impl`이라는 접미어를 붙여 클래스를 생성하고 커스텀 리파지토리를 구현합니다.<br/>
```java
   @Override
    public List<Post> findMyPost() {
        System.out.println("custom findMyPost");
        return entityManager.createQuery("SELECT p FROM Post AS p", Post.class).getResultList();
    }
```
![CustomRepository](/images/jpa/customrepository/cus7.png) EntityManager를 활용하여 쿼리를 만들고 결과를 리턴하는 메소드로 재정의합니다.<br/>
<br/>

## 3. 엔티티 리포지토리에 커스텀 리포지토리 인터페이스 추가하기
![CustomRepository](/images/jpa/customrepository/cus8.png) 최초에 만들었던 엔티티 리포지토리에 커스텀한 리포지토리를 추가합니다.<br/>
![CustomRepository](/images/jpa/customrepository/cus10.png) 커스텀한 리포지토리의 메소드가 이상없이 테스트를 통과하였습니다!<br/>
<br/>

## 기존 기능 덮어쓰기
JPA가 기본적으로 제공하는 기능을 커스텀 할 수 있습니다. 예시로 delete를 커스텀 해보겠습니다.<br/>
![CustomRepository](/images/jpa/customrepository/cus11.png) 커스텀 리포지토리에서 타입을 선언하고 delete를 정의합니다.<br/>
![CustomRepository](/images/jpa/customrepository/cus12.png) 구현 클래스에서 타입을 Post로 선언하고 delete를 재정의합니다.<br/>
```java
    @Override
    public void delete(Post entity) {
        System.out.println("custom delete");
        entityManager.remove(entity);
    }
```
![CustomRepository](/images/jpa/customrepository/cus13.png) 재정의한 코드입니다.<br/>
![CustomRepository](/images/jpa/customrepository/cus14.png) 테스트로 Post를 삭제해보겠습니다.<br/> 
![CustomRepository](/images/jpa/customrepository/cus15.png) 이상없이 잘 삭제되는 결과를 볼 수 있습니다!<br/>