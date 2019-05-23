---
title: JPA - ORM
date: 2019-05-19 19:15:45
tags: [JPA, ORM]
---

![images](/images/jpa/jpa.jpg)<br/>

# ORM
ORM(Object Relation Mapping)은 애플리케이션의 **클래스**와 SQL 데이터베이스의 **테이블** 사이의 ​**맵핑 정보를 기술한 메타데이터**​를 사용하여 자바 애플리케이션의 객체를 SQL데이터베이스의 테이블에 **자동으로 영속화​** 해주는 기술입니다.
**Hibernate**나 **JPA**같은 ORM을 사용하는 이유는 도메인 모델을 사용하기 위해서 입니다.<br/>
<br/>

## DB 연동 방식
### JDBC를 직접 사용하는 방식 
```java
try(Connection connection = DriverManager.getConnection(url, username, password)){
    System.out.println("Connection created: "+ connection);
    String sql = "CREATE TABLE ACCOUNT (id int, username varchar(255), password varchar(255));";
    sql = "INSERT INTO ACCOUNT VALUES(1, 'junjang', 'pass');";
    try(PreparedStatement statement = connection.prepareStatement(sql)){
        statement.execute();
    }
}
```
<br/>

### JPA로 도메인 모델을 사용하는 방식
```java
Account account = new Account(junjang, “pass”);
accountRepository.save(account);
```
위 두 코드의 차이가 확연히 느껴지시나요?
JDBC 방식은 비즈니스 로직과 상관없는 코드들을 많이 작성해야하는 반면 JPA를 활용하면 단순하게 연동이 가능합니다.<br/>
그러면 도메인 모델을 사용하면 좋은점에는 뭐가 있을까요? <br/>
<br/>

## 도메인 모델을 사용하면 좋은점
- 객체지향 프로그래밍의 장점을 보여줍니다.
- 각종 디자인 패턴의 사용이 용이합니다.
- 코드 재사용성이 좋습니다.
- 비즈니스 로직 구현 및 테스트가 편리합니다.

<br>

## ORM의 장점
- **생산성** : 매핑을 하면 데이터 입출력이 정말 쉬워집니다.
- **유지보수성** : 코드가 굉장히 간결해지고 코드양이 줄어 유지보수성이 높아집니다.
- **성능** : 객체와 테이블 사이에 **캐시**가 존재하여 중복 혹은 필요없는 데이터 변경 요청이 들어오면 반영하지 않고, 데이터가 변경될 여지가 있을 때만 변경되기 때문에 보다 효율적입니다.
- **벤더 독립성** : Hibernate가 어떠한 데이터베이스에 맞게 SQL을 생성해야 되는지만 알려주면 됩니다. 이유는 ibernate가 데이터베이스 sync를 할때 객체를 영속화 할때 발생하는 SQL만 자동으로 바뀌기 때문입니다.

<br/>

## ORM의 단점
- **높은 학습비용** : SQL, Hibernate, JPA 세가지를 전부 잘 알고 있어야 제대로 활용할 수 있기 때문에 학습하는데 높은 난이도와 많은 시간이 소요됩니다.<br/>
<br/>

이번 주제를 통해서 ORM을 활용한 프레임워크와 기술의 중요성을 알고 학습하여 효율적인 코드를 작성해야겠다는 생각을 가지면 좋겠습니다. :)