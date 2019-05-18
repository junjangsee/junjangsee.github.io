---
title: StudyRoom(SRoom) 프로젝트 - 05(운영 환경 설정하기)
date: 2019-03-25 14:46:18
tags: AWS
---

# SRoom 프로젝트

![images](/images/sroom/studyroom.jpg)<br/>

자동 배포까지 완료하였고 이제 SpringBoot에서 실제 운영 DB를 가지고 있지 않기 때문에 AWS, RDS와 프로젝트를 연동해야 합니다.
그래서 실제 운영 환경 설정을 하겠습니다.
<br/>
### 운영 DB 의존성 추가하기
Maria DB를 추가하기 위해 아래의 코드를 추가합니다.
```
compile("org.mariadb.jdbc:mariadb-java-client")
```
위 코드를 추가했다면 의존성 추가는 끝입니다.
<br/>
### 로컬 YAML(.yml) 파일 수정하기
로컬 환경에서 DB를 다루기 위해 파일을 수정해야 합니다.
내부 환경은 application.yml에서 수정해야 하기 때문에 아래의 코드와 같이 변경합니다.
```
spring:
  profiles:
    active: sroom-db # 기본 환경 선택
  jpa:
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL5InnoDBDialect
        # MySQL 문법으로 출력시켜준다.
# local 환경
---
spring:
  profiles: local
  jpa:
    show-sql: true
    hibernate:
      ddl-auto: create-drop
  h2:
    console:
      enabled: true
# 운영 환경
---
spring.profiles: sroom-db
spring.profiles.include: real-db

server:
  port: 9000
```
![db](/images/sroom/db6.png) 그리고 local에서도 sroom-db 환경을 가지고 있어야 하니 수정합니다.<br/>
real-db는 운영 환경에서 다루는 db명입니다.(차후 추가하니 미리 작성했습니다.)<br/>
<br/>
### YAML(.yml) 파일 생성하기
로컬 뿐만 아니라 운영 환경에서도 DB를 다루어야 하기 때문에 또 다른 작업이 필요합니다.
로컬 및 운영 환경에서 .yml파일을 생성하여 운영 환경을 설정합니다.
```
sudo mkdir app/config/sroom/real-application.yml
```
위 코드로 로컬 및 EC2에 .yml파일을 생성합니다.(경로 동일)<br/>
```
---
spring:
  profiles: real-db
  datasource:
        url: jdbc:mariadb://rds주소:포트명(기본은 3306)/database명
        username: db계정
        password: db계정 비밀번호
        driver-class-name: org.mariadb.jdbc.Driver
```
위 코드를 기준으로 자신의 프로젝트에 맞게 입력합니다.
운영 파일에도 마찬가지 경로에 동일하게 작성합니다.<br/>
![db](/images/sroom/db1.png) 위를 입력한 .yml 파일입니다.<br/>
.yml파일을 읽기 위해서는 서버 실행시 Application에서 가져올 수 있게 설정해주어야 합니다.
```
public static final String APPLICATION_LOCATIONS = "spring.config.location="
            + "classpath:application.yml,"
            + "/app/config/sroom/real-application.yml";

    public static void main(String[] args) {
        new SpringApplicationBuilder(Application.class)
                .properties(APPLICATION_LOCATIONS)
                .run(args);
```
![db](/images/sroom/db5.png) 위 처럼 .yml파일을 실행하도록 설정해줍니다.<br/>
![db](/images/sroom/db2.png) 여기서 운영 환경의 spring.profiles.include는 해당 profiles를 실행 시킬 땐 특정 profiles를 포함시킨다는 뜻입니다.<br/>
이제 DB를 연동한 서버를 실행 시키면 아래와 같은 오류가 뜹니다.
![db](/images/sroom/db3.png) 이는 posts라는 테이블이 존재하지 않는다는 것인데 사실 테이블이 존재한다면 이 오류는 뜨지 않습니다.
저는 테스트를 위해서 jojoldu님의 JPA 세팅을 하여 테스트를 하였으나 필요 없다면 따로 테스트를 하지 않아도 됩니다.
필요하다면 jojoldu님의 블로그를 참고해 주시면 됩니다.
<br/>
**Table com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'DB명.hibernate_sequence' doesn't exist** 라는 오류가 뜬다면 ? 하이버네이트 5버전의 문제이기 때문에 
```
@GeneratedValue(strategy = GenerationType.IDENTITY)
```
위 코드처럼 IDENTITY를 사용해야합니다.<br/>
![db](/images/sroom/db4.png) 최종적으로 테스트한 결과 값입니다.
<br/>
이제 본격적으로 OAuth를 활용한 회원가입을 시작으로 API를 구현하려 합니다.