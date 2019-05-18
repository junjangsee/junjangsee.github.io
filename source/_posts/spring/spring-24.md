---
title: SpringBoot - DBCP, MySQL
date: 2019-05-12 18:52:08
tags: [SpringBoot, DB, DBCP, MySQL, Docker, Hikari]
---

![images](/images/springboot/springboot.png)<br/>

# DBCP
DBCP란 Database Connection Pool의 약자로 Connection을 미리 여러개 만들어 놓고 필요할때 마다 그때그때 가져다 사용하며 SpringBoot는 기본적으로 Hikari CP를 사용하고 있습니다.
- DBCP가 Application 성능에 아주 핵심적인 역할을 하므로 충분히 학습 후 사용해야합니다.
- Connection에 관련된 여러가지 설정을 할 수 있습니다.
- SpringBoot에서 지원하는 DBCP
 - HikariCP​ (기본)
 - Tomcat CP
 - Commons DBCP2<br/>

## Hikari CP
기본 설정인 Hikari의 핵심 내용입니다.
- 참고자료 : [Hikari 사용방법](https://github.com/brettwooldridge/HikariCP#frequently-used)
- application.properties에 Hikari에 대한 설정값을 선언합니다.
- connectionTimeout : 연결시간
- maximumPoolSize : 커넥션 유지 갯수
 - Connection 객체를 몇개를 유지할 것인가에 대한 설정으로 동시에 실행할 수 있는 Connection 수는 CPU Core 갯수와 같습니다. CPU Core를 넘어간 갯수의 객체들은 모두 대기 하면서 타임슬라이싱해서 조금씩 처리합니다.<br/>
<br/>

# MySQL
Docker를 사용하여 Mysql을 다운받은 후 사용하는 방법에 대해 알아봅시다.<br/>

## MySQL 의존성 추가하기
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```
![MySQL](/images/springboot/mysql/mysql1.png) MySQL을 사용하기 위해선 연동을 해야합니다. 이 의존성을 추가한다고 바로 사용할 순 없습니다.<br/>
<br/>

## Docker 다운로드(설치)
맥 : [맥버전 다운로드](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
윈도우 : [윈도우버전 다운로드](https://hub.docker.com/editions/community/docker-ce-desktop-windows) <br/>
각 운영체제에 맞게 Docker 회원가입 후 다운로드 합니다.
Docker에 대한 설명은 차후에 하도록 하고 지금은 MySQL을 설치하는 용도로만 사용해주시면 됩니다.<br/>
<br/>

## MySQL 다운로드(설치)
설치한 Docker를 활용하여 MySQL설치를 해보겠습니다.<br/>

```bash
docker run -p 3306:3306 --name mysql_boot -e MYSQL_ROOT_PASSWORD=1 -e MYSQL_DATABASE=springboot -e MYSQL_USER=junjang -e MYSQL_PASSWORD=pass -d mysql
```
![MySQL](/images/springboot/mysql/mysql2.png) 위 명령어를 실행하면 mysql을 설치와 함께 실행을 합니다.<br/>
- **3306**포트로 서로 이어집니다.
- **mysql_boot**라는 이름으로 설치합니다.
- DB는 **springboot**를 사용합니다.
- 사용자는 **junjang**입니다.
- 비밀번호는 **pass**입니다.
- **mysql**을 설치합니다.

위 순서로 생각하시면 됩니다.<br/>
저같은 경우엔 3306포트를 기존에 사용하고 있어서 최초 3307로 생성하고 연동하였더니 연동이 안되었습니다.
그래서 다시 3306포트를 사용하는 컨테이너, 이미지를 죽이고 3306으로 mysql을 설치했더니 연동되었습니다. 
아마 3306 고유의 포트 역할이 있는 것 같은데 아직 정확한 원인은 찾지 못했습니다.
찾게 되면 추가 포스팅 하겠습니다.<br/>
<br/>

## Docker Container 안에서 bash 실행 및 MySQL 접속하기
```bash
docker exec -i -t mysql_boot bash
mysql -u junjang -p
```
![MySQL](/images/springboot/mysql/mysql3.png) 설치된 MySQL을 Docker에서 bash를 사용하고 접속하는 명령어입니다.<br/>
<br/>

## DB 테이블 확인하기
```bash
show databases;
use springboot;
show tables;
```
![MySQL](/images/springboot/mysql/mysql4.png) 현재 생성한 테이블이 있는지 확인하는 명령어입니다.
아직은 넣은 값이 없기 때문에 테이블이 존재하지 않습니다.<br/>
<br/>

## MySQL 연동하기
이제 MySQL이 구동되는 것 까지 확인했으니 SpringBoot와 연동해야합니다. 
연동은 application.properties의 Key, Value로 설정하고 **url, username, password** 세가지를 선언합니다.<br/>
```
spring.datasource.url=jdbc:mysql://localhost:3306/springboot
spring.datasource.username=junjang
spring.datasource.password=pass
```
```java
@Component
public class MySQLRunner implements ApplicationRunner {

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

            String sql = "CREATE TABLE USER (ID INTEGER NOT NULL, name VARCHAR(255), PRIMARY KEY (id))";
            statement.executeUpdate(sql);

        }

        jdbcTemplate.execute("INSERT INTO USER VALUES(1, 'junjang')");

    }
}
```
위 코드를 작성하고 실행하게 되면..<br/>
ps. 혹시 위 코드가 이해가 되지 않는다면 [인메모리 데이터베이스](https://junjangsee.github.io/2019/05/12/spring/spring-23/)를 읽어주시면 됩니다.<br/>
![MySQL](/images/springboot/mysql/mysql5.png) 테이블에 INSERT된 값을 보실 수 있습니다.<br/>
<br/>

## SSL 오류가 난다면?
```
spring.datasource.url=jdbc:mysql:/localhost:3306/springboot?**useSSL=false** 
```
**?useSSL=false**를 추가합니다.
<br/>
<br/>

## Public Key Retrieval is not allowed 오류가 난다면?
```
spring.datasource.url=jdbc:mysql:/localhost:3306/springboot?useSSL=false&**allowPublicKeyRetrieval=true**
```
**allowPublicKeyRetrieval=true**를 추가합니다.<br/>
<br/>

## MySQL 라이센스 (GPL)
MySQL은 상용 소프트웨어로서 상용 애플리케이션에서 사용하려면 정기 구독을 해야만 합니다. 또한 소스 공개 의무가 있기 때문에 소스를 공개해야만 합니다.<br/>
그래서 MySQL대신에 Maria DB를 사용하는 것도 좋습니다.
GLP2이기 때문에 소스 공개 의무도 없으며 무료입니다.<br/>
<br/>

### Maria DB 설치
```bash
docker run -p 3306:3306 --name mysql_boot -e MYSQL_ROOT_PASSWORD=1 -e MYSQL_DATABASE=springboot -e MYSQL_USER=junjang -e MYSQL_PASSWORD=pass -d mariadb
```
특별하게 다른건 없습니다. 마지막 줄에 mysql 대신 mariadb를 넣어주면 설치 완료입니다.<br/>