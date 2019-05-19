---
title: JPA - 관계형 데이터베이스와 자바
date: 2019-05-18 20:20:49
tags: [JPA, RDS, Java]
---

![images](/images/jpa/jpa.jpg)<br/>

# 관계형 데이터베이스와 자바
JPA를 공부하기 전 데이터베이스와 자바의 관계를 알아보며 JPA가 탄생한 배경에 대해서 알아보겠습니다.<br/>
<br/>

## JDBC
JDBC는 데이터베이스와 자바를 연결하는 **고리**의 역할을 합니다.
JDBC를 활용하여 아래와 같은 기능을 통해 연결합니다.
- DataSource / DriverManager
- Connection
- PreparedStatement
<br/>
<br/>

## JDBC 연동하기
저는 **postgresql**를 Docker를 활용해 실행하고 연동하겠습니다. 만약 다른 DB를 사용하셔도 상관은 없지만 sql문법과 Docker 명령어가 조금 다를 수 있습니다.<br/>
- Docker 다운로드 
 - [맥](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
 - [윈도우](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
- postgresql 사용법 : [postgresql 포스팅](https://junjangsee.github.io/2019/05/13/spring/spring-25/)
- MySQL 사용법 : [MySQL 포스팅](https://junjangsee.github.io/2019/05/12/spring/spring-24/)
<br/>

### docker로 PostgreSQL 사용하기
docker로 PostgreSQL을 설치 및 실행하는 방법은 위 주소에서 설명했으니 생략하도록 하겠습니다.
![RDS](/images/jpa/rds/jrds1.png) docker를 통해 PostgreSQL을 접속합니다.<br/>
<br/>

### PostgreSQL 드라이버 의존성 추가
```xml
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.2.5</version>
        </dependency>
```
![RDS](/images/jpa/rds/jrds2.png) JDBC를 연결하려면 버가 필요하여 드라이버 의존성을 추가합니다.<br/>
![RDS](/images/jpa/rds/jrds3.png) 이제 새로운 패키지와 클래스를 만들어 main 메소드에 연동을 해보겠습니다.<br/>
<br/>

### 연동 로직 구현
```java
public class Application {

    public static void main(String[] args) throws SQLException {
        String url = "jdbc:postgresql://localhost:5432/springboot";
        String username = "junjang";
        String password = "pass";

        try(Connection connection = DriverManager.getConnection(url, username, password)){
            System.out.println("Connection created: "+ connection);
        }
    }
}
```
![RDS](/images/jpa/rds/jrds6.png) JDBC와 연동하는데는 총 3가지의 정보(Url, username, password)가 필요합니다.
PostgreSQL을 설치 및 실행할 때 위 정보를 가지고 실행했기 때문에 그 정보로 선언했고 Connection 객체를 통해 연동하였습니다.
결과적으로 잘 연동된 것을 확인할 수 있습니다.<br/>
그리고 try with resource 문법을 사용하여 괄호 안 로직이 실행되면 그 괄호에 해당되는 로직이 실행되게 구현합니다.<br/>
<br/>

#### try with resource 문법이 에러가 난다면?
Java 8 부터 도입된 문법이므로 프로젝트가 사용하는 JDK의 버전이 낮을 수도 있습니다. Intellij 기준 해결 방법입니다.<br/>
![RDS](/images/jpa/rds/jrds4.png) Modules의 언어 레벨을 바꾸고 적용합니다.<br/>
![RDS](/images/jpa/rds/jrds5.png) Java Compiler의 박스 세 부분을 8버전 이상으로 통일시킵니다.
위 두가지 부분을 수정하면 정상적으로 컴파일이 될 것입니다.<br/>
<br/>

### 테이블 넣어보기
RDS와 연동이 되었으니 DB에 테이블 넣어보겠습니다.<br/>
```java
public class Application {

    public static void main(String[] args) throws SQLException {
        String url = "jdbc:postgresql://localhost:5432/springboot";
        String username = "junjang";
        String password = "pass";

        try(Connection connection = DriverManager.getConnection(url, username, password)){
            System.out.println("Connection created: "+ connection);
            String sql = "CREATE TABLE ACCOUNT (id int, username varchar(255), password varchar(255));";

            sql = "INSERT INTO ACCOUNT VALUES(1, 'junjang', 'pass');";

            try(PreparedStatement statement = connection.prepareStatement(sql)){
                statement.execute();
            }
        }
    }
}
```
![RDS](/images/jpa/rds/jrds7.png) sql문을 DDL로 작성한 변수를 PreparedStatement 객체를 사용하여 execute합니다.<br/>
![RDS](/images/jpa/rds/jrds8.png) 테이블을 조회해보면 방금 생성한 ACCOUNT 스키마가 추가된 것을 볼 수 있습니다.<br/>
<br/>

### 데이터 넣어보기
이번엔 생성된 테이블에 데이터를 넣어보겠습니다.<br/>
```java
public class Application {

    public static void main(String[] args) throws SQLException {
        String url = "jdbc:postgresql://localhost:5432/springboot";
        String username = "junjang";
        String password = "pass";

        try(Connection connection = DriverManager.getConnection(url, username, password)){
            System.out.println("Connection created: "+ connection);
            String sql = "INSERT INTO ACCOUNT VALUES(1, 'junjang', 'pass');";

            try(PreparedStatement statement = connection.prepareStatement(sql)){
                statement.execute();
            }
        }
    }
}
```
![RDS](/images/jpa/rds/jrds9.png) sql문을 DML로 변경하였습니다.<br/>
![RDS](/images/jpa/rds/jrds10.png) insert된 데이터를 볼 수 있습니다.<br/>
<br/>

## JDBC를 사용시 문제점
위에서 예제코드는 정말 단순하게 작성하였지만 실제 프로젝트에서는 여러가지 제약사항이 많습니다.
- SQL을 실행하는 비용이 비쌉니다.(생산 및 유지보수 비용)
- SQL이 데이터베이스 마다 다릅니다.(하나의 DB에 맞는 문법을 사용)
- 스키마를 바꾸면 해당 스키마 코드가 전체 바뀝니다.
- 반복적인 코드가 너무 많습니다.
- 많은 도메인 객체가 필요합니다.

이러한 문제점들이 많아 개발하는데에 시간이 많이 소요됩니다.
그래서 JPA를 활용하면 개발자는 개발에 좀 더 집중할 수 있게 됩니다.<br/>