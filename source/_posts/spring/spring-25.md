---
title: SpringBoot - PostgreSQL
date: 2019-05-13 22:46:44
tags: [SpringBoot, DB, PostgreSQL, Docker]
---

![images](/images/springboot/springboot.png)<br/>

# PostgreSQL
Postgres DB를 사용하는 방법을 알아봅시다.
MySQL과 마찬가지로 Docker로 설치 및 실행할 예정이니 Docker를 설치 하지 않았다면 [Mysql](https://junjangsee.github.io/2019/05/12/spring/spring-24/) 포스팅을 참고해주시기 바랍니다.<br/>
<br/>

## PostgreSQL 의존성 추가
```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
```
![MySQL](/images/springboot/postgres/post1.png) 의존성을 추가해줍니다.<br/>
<br/>

## Docker에 PostgreSQL 설치하기
```bash
docker run -p 5432:5432 -e POSTGRES_PASSWORD=pass -e POSTGRES_USER=junjang -e POSTGRES_DB=springboot --name postgres_boot -d postgres
```
![MySQL](/images/springboot/postgres/post2.png) 
- 도커 5432 포트를 로컬 5432 포트와 연결합니다.
- 비밀번호는 pass로 합니다.
- 사용자는 junjang으로 합니다.
- DB는 springboot를 사용합니다.
- 다운로드 파일은 postgres_boot로 합니다.
- postgres를 다운로드 받는다.

이 순서로 이해하시면 됩니다.<br/>
<br/>

## PostgreSQL 연결

### application.properties 설정
```
spring.datasource.url=jdbc:postgresql://localhost:5432/springboot
spring.datasource.username=junjang
spring.datasource.password=pass
```
<br/>

### Container에 bash 접근하기
```bash
docker exec -i -t postgres_boot bash
```
위 명령어로 bash를 사용하여 postgres를 사용할 수 있도록 합니다.
<br/>

### User 및 DB 연결하기
```bash
psql -U junjang springboot
```
![MySQL](/images/springboot/postgres/post3.png) 최초 설치시 만들었던 username과 DB를 선언하여 연결합니다.<br/>
이 방법이 되지 않는다면 username -> DB 순으로 연결하여야 합니다. 
```bash
su - postgres
psql springboot 
```
위 명령어를 순차적으로 사용하여 접속합니다.
<br/>

### 명령어
#### 데이터베이스 조회
```bash
\list or \l
```
#### 전체 테이블 조회
```bash
\dt
```

#### PostgreSQL 접속 끊기
```bash
\q
```

#### account 테이블 조회
```bash
SELET * FROM account;
```
![MySQL](/images/springboot/postgres/post4.png) 
<br/>
<br/>

## PostgreSQL Runner 클래스
```java
@Component
public class PgSQLRunner implements ApplicationRunner {

    @Autowired
    DataSource dataSource;

    @Autowired
    JdbcTemplate jdbcTemplate;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        try (Connection connection = dataSource.getConnection()) {
            System.out.println(connection.getClass());
            System.out.println(connection.getMetaData().getURL());
            System.out.println(connection.getMetaData().getUserName());

            Statement statement = connection.createStatement();

            String sql = "CREATE TABLE ACCOUNT (ID INTEGER NOT NULL, name VARCHAR(255), PRIMARY KEY (id))";
            statement.executeUpdate(sql);

        }

        jdbcTemplate.execute("INSERT INTO ACCOUNT VALUES(1, 'junjang')");

    }
}
```
테이블을 생성하고 INSERT하는 쿼리를 만드는 클래스를 구현하고 실행합니다. 
이 클래스를 이해하지 못하신다면 [Mysql](https://junjangsee.github.io/2019/05/12/spring/spring-24/) 포스팅을 참고해주시기 바랍니다.<br/>
![MySQL](/images/springboot/postgres/post5.png)
![MySQL](/images/springboot/postgres/post6.png) 테이블과 SELECT문을 통해 값이 들어간 것을 확인할 수 있습니다.<br/>
<br/>

## IntelliJ Database
IntelliJ 는 툴 내에서 DB를 관리할 수 있습니다.<br/>
![MySQL](/images/springboot/postgres/post7.png) 우측 Database에 PostgreSQL을 클릭합니다.<br/> 
![MySQL](/images/springboot/postgres/post8.png) User, Password, Database를 입력하고 Test를 시행하고 이상이 없다면 OK를 클릭하여 추가합니다.<br/>
![MySQL](/images/springboot/postgres/post9.png) 우측에 추가된 것을 확인할 수 있고 테이블을 볼 수 있으며 직접 쿼리를 작성해 값을 출력할 수도 있습니다.<br/>

