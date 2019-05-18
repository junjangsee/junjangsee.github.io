---
title: SpringBoot - 인메모리 데이터베이스(H2)
date: 2019-05-12 17:08:38
tags: [SpringBoot, DB, H2, JDBC]
---

![images](/images/springboot/springboot.png)<br/>

# 인메모리 데이터베이스
메모리를 사용하여 DB를 조작하는 것을 말합니다.
다양한 종류가 있지만 H2를 활용해서 실습해 볼 것입니다.<br/>
<br/>

## H2, JDBC 의존성 추가하기
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
```
![h2](/images/springboot/h2/con1.png) jdbc의존성을 추가하면 두개의 항목을 자동설정 해줍니다.
- DataSource
- JdbcTemplate 

이 두가지를 사용하여 리소스 반납처리, 에러 확인 등을 가독성 높고 편리하게 사용할 수 있습니다.<br/>
<br/>

## 인메모리 데이터베이스 연결 정보 확인하기
autoconfigure - DataSourceAutoConfiguration - **DataSourceProperties**에 기본 정보가 들어가있습니다.
- URL: “testdb”
- username: “sa”
- password: “”
<br/>
<br/>

## H2 조회하기
총 2가지의 방법이 있습니다.
- spring-boot-devtools 의존성을 추가합니다.
- application.properties에 spring.h2.console.enabled=true를 추가합니다.

1번 방법에 2번이 포함되어 있기 때문에 devtools를 쓰신다면 2번이 필요 없지만 굳이 의존성을 추가하기 싫으시다면 2번을 하셔도 무방합니다.

![h2](/images/springboot/h2/con4.png) <br/>

## H2 사용하기
H2를 사용할 Runner 클래스를 생성합니다.
<br/>

### DataSource
#### Connection
```java
@Component
public class H2Runner implements ApplicationRunner {

    @Autowired
    DataSource dataSource;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Connection connection = dataSource.getConnection();
        
        System.out.println(connection.getMetaData().getURL());
        System.out.println(connection.getMetaData().getUserName());
        
    }
}
```
![h2](/images/springboot/h2/con2.png) ApplicationRunner를 구현하여 서버 실행시 함께 실행되도록 하고 @Component를 통해 빈으로 등록합니다.
DataSource를 주입받고 Connection 정보를 알아보기 위해 URL과 UserName을 가져옵니다.<br/>
서버를 구동하면 DataSourceProperties에 선언되어있는 기본 정보를 확인할 수 있습니다.<br/>
<br/>

#### Statement
```java
@Component
public class H2Runner implements ApplicationRunner {

    @Autowired
    DataSource dataSource;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        try (Connection connection = dataSource.getConnection()) {
            System.out.println(connection.getMetaData().getURL());
            System.out.println(connection.getMetaData().getUserName());

            Statement statement = connection.createStatement();

            String sql = "CREATE TABLE USER (ID INTEGER NOT NULL, name VARCHAR(255), PRIMARY KEY (id))";
            statement.executeUpdate(sql);
        }


    }
}
```
![h2](/images/springboot/h2/con3.png) CreateStatement함수를 이용하여 Statement객체를 받고 **DDL** 쿼리를 선언할 수 있습니다.
그리고 실행시 Update되도록 선언합니다.
그리고 서버를 구동시키고 오류가 나지 않으면 정상적으로 테이블이 생성된 것입니다.<br/>
![h2](/images/springboot/h2/con5.png) localhost:8080/h2-console로 접속하면 위와 같이 기본 정보가 출력되는 것을 확인할 수 있습니다.
기본정보가 일치하다면 접속을 합니다.<br/>
![h2](/images/springboot/h2/con6.png) 접속하면 테이블이 생성된 것을 확인하실 수 있습니다.<br/>
<br/>

### JdbcTemplate
테이블을 생성했으니 이제 데이터를 삽입해야합니다.
그러기 위해서 JdbcTemplate을 통해 안전하고 편하게 데이터를 넣어보겠습니다.
<br/>

```java
@Component
public class H2Runner implements ApplicationRunner {

    @Autowired
    DataSource dataSource;

    @Autowired
    JdbcTemplate jdbcTemplate;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        try (Connection connection = dataSource.getConnection()) {
            System.out.println(connection.getMetaData().getURL());
            System.out.println(connection.getMetaData().getUserName());

            Statement statement = connection.createStatement();

            String sql = "CREATE TABLE USER (ID INTEGER NOT NULL, name VARCHAR(255), PRIMARY KEY (id))";
            statement.executeUpdate(sql);

        }

        jdbcTemplate.execute("INSERT INTO USER VALUES(1, 'junjang')");

    }
}
```
![h2](/images/springboot/h2/con7.png) DataSource와 동일하게 의존성을 주입하고 **execute** 함수를 통해 **DML**을 선언합니다.<br/>
![h2](/images/springboot/h2/con8.png) SELECT문을 활용하여 테이블에 데이터가 잘 들어간 것을 확인하면 됩니다.<br/>