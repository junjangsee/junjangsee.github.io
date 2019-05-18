---
title: SpringBoot - 데이터베이스 초기화 
date: 2019-05-14 17:30:01
tags: [SpringBoot, DB, Hibernate, JPA]
---

![images](/images/springboot/springboot.png)<br/>

# 데이터베이스 초기화
이번 주제를 실습하기 위해선 이전 포스팅의 코드가 필요합니다.
필요하신 분들은 [JPA 연동하기](https://junjangsee.github.io/2019/05/14/spring/spring-27/) 를 참고해주시면 됩니다.<br/>
<br/>

## JPA를 사용한 데이터베이스 초기화
JPA를 사용한 프로젝트의 경우 해당되는 초기화 방법입니다.
작성해둔 Entity 정보를 바탕으로 스키마를 생성하고 생성된 스키마를 초기화 하는 방법에 대해 알아보겠습니다.<br/>
<br/>

### Application.properties 설정
#### spring.jpa.hibernate.ddl-auto
```
spring.jpa.hibernate.ddl-auto=
```
위 옵션을 통해서 스키마를 운영할 수 있습니다.
- update: 기존의 스키마는 놔두고 추가된 사항만 변경합니다.
- create-drop: 처음에 스키마를 만들고 Application 종료시 스키마를 drop합니다.
- create: 처음에 스키마를 삭제하고 스키마를 새로 만듭니다.
- validate: 현재 Entity 매핑이 릴레이션 DB에 맵핑할 수 있는 상황인지 맵핑이 되는지를 검증합니다.(운영환경에서 사용)
- 테스트 환경에선 필요하지 않은 옵션입니다.(주석처리)
<br/>

만약 Entity 클래스에 새로운 컬럼을 추가하고 update를 하면 어떻게 될까요?
![JPA](/images/springboot/schema/sch5.png) Email이라는 컬럼을 추가하였습니다.<br/>
![JPA](/images/springboot/schema/sch7.png) update를 주고 실행하였더니 기존 컬럼은 그대로 있고 컬럼이 추가된 것을 볼 수 있습니다.
<br/>
그렇다면 Email이란 컬럼을 추가하지 않고 validate를 했다면 어떻게 됐을까요?
![JPA](/images/springboot/schema/sch6.png) Entity에만 존재하고 운영 DB에는 아직 생성되어있지 않으니 email이란 컬럼을 찾지 못했다고 하게 됩니다.<br/>
#### spring.jpa.generate-ddl
```
spring.jpa.generate-ddl=true
```
- DDL 변경을 허용하기 위한 프로퍼티입니다.
- 기본적으로 false로 되어있어 true로 설정 해줘야 동작합니다.
- 테스트 환경에선 필요하지 않은 옵션입니다.(주석처리)

#### spring.jpa.show-sql
```
spring.jpa.show-sql=true
```
- Application 실행시 hibernate 로그(스키마)를 보여줍니다.
- 기본적으로 false로 되어있어 true로 설정 해줘야 동작합니다.

<br/>
<br/>

## SQL 스크립트를 사용한 데이터베이스 초기화
일반적인 데이터베이스를 사용할 때 사용하는 방법입니다.
- 테스트, Application이 구동될 때 마다 스크립트를 통해 데이터베이스를 초기화합니다.
- 스크립트를 만든상태에서는 ddl-auto, generate-ddl 옵션을 구동시켜도 스크립트로 먼저 스키마 초기화를 하므로 에러가 발생하지 않습니다.
- 스크립트 실행 순서는 schema.sql --> data.sql 입니다.

<br/>
### schema.sql
```sql
drop table account if exists
drop sequence if exists hibernate_sequence
create sequence hibernate_sequence start with 1 increment by 1
create table account (id bigint not null, email varchar(255), password varchar(255), username varchar(255), primary key (id))
```
![JPA](/images/springboot/schema/sch1.png)
![JPA](/images/springboot/schema/sch8.png) resource - schema.sql을 생성하여 실행시 스키마를 입력합니다.
- schema.sql 또는 schema-${platform}.sql로 생성합니다.

<br/>
### data.sql
resource - data.sql를 생성하여 스키마를 입력합니다.
- data.sql 또는 data-${platform}.sql

<br/>