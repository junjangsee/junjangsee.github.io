---
title: SpringBoot - 데이터베이스 Migration(마이그레이션)
date: 2019-05-14 18:49:30
tags: [SpringBoot, DB, Migration, Flyway]
---

![images](/images/springboot/springboot.png)<br/>

# 데이터베이스 Migration(마이그레이션)
스키마 혹은 데이터를 변경할 때 버전관리 형식으로 관리가 가능합니다. 대표적으로 Flyway와 Liquibase가 있습니다. 이중에 Flyway를 대표로 삼아 공부해보겠습니다.<br/>
도중 나오는 코드들은 이전 포스트에서 그대로 가져왔습니다.
코드가 필요하시면 [JPA 연동하기](https://junjangsee.github.io/2019/05/14/spring/spring-27/) 포스팅을 참고하시면 됩니다.
참고자료 : [스프링공식홈페이지](https://docs.spring.io/spring-boot/docs/2.1.1.RELEASE/reference/htmlsingle/#howto-execute-flyway-database-migrations-on-startup), [flyway홈페이지](https://flywaydb.org/)
<br/>

## Flyway 의존성 추가
```xml
        <dependency>
            <groupId>org.flywaydb</groupId>
            <artifactId>flyway-core</artifactId>
        </dependency>
```
![JPA](/images/springboot/mygration/fly1.png) 의존성을 추가합니다.<br/>
<br/>

## 마이그레이션

### 마이그레이션 디렉토리
- **/src/resources에 db/migration** 경로를 생성합니다.
- spring.flyway.locations 을 활용하여도 됩니다.

<br/>

### 마이그레이션 파일 이름
- V숫자__이름.sql (언더바(_) 두개입니다.)
- V는 꼭 대문자로
- 숫자는 순차적으로 (타임스탬프 권장)
- 이름은 가능한 서술적으로 작성

<br/>

### 마이그레이션 사용
```sql
drop table if exists account;
drop sequence if exists hibernate_sequence;
create sequence hibernate_sequence start with 1 increment by 1;
create table account (id bigint not null, email varchar(255), password varchar(255), username varchar(255), primary key (id));
```
![JPA](/images/springboot/mygration/fly4.png) JPA 연동시 출력됐던 스키마를 sql문 안에 선언합니다. 그리고 애플리케이션을 실행합니다.<br/>
ps. 제가 선택한 postgres는 문법이 다르기 때문에 위 코드로 수정하였습니다.<br/>
![JPA](/images/springboot/mygration/fly3.png) flyway가 실행되면서 테이블이 account와 함께 flyway_schema_history가 함께 생성되었습니다. 이 테이블에는 앞으로 추가할 쿼리에 대해서 순서대로 기록해줍니다.<br/>
<br/>

### Entity 클래스에 컬럼이 추가되면?
```java
@Entity
public class Account {

    @Id
    @GeneratedValue
    private Long id;
    private String username;
    private String password;
    private String email;
    private boolean active;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public boolean isActive() {
        return active;
    }

    public void setActive(boolean active) {
        this.active = active;
    }


    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Account account = (Account) o;
        return active == account.active &&
                Objects.equals(id, account.id) &&
                Objects.equals(username, account.username) &&
                Objects.equals(password, account.password) &&
                Objects.equals(email, account.email);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, username, password, email, active);
    }
}
```
기존 email까지 있었던 컬럼에서 boolean 타입인 active를 추가했습니다. 그리고 **spring.jpa.hibernate.ddl-auto=validate** 로 Entity 클래스와 운영 DB와 연동 되는지 확인하겠습니다.<br/>
두 번째 쿼리니 V2로 동일 경로에 만들어준 후 실행합니다.(절대 첫 번째 sql파일은 손대지 않습니다.)
```sql
ALTER TABLE account ADD COLUMN active BOOLEAN;
```
![JPA](/images/springboot/mygration/fly2.png) 테이블에 컬럼이 추가된 것을 알 수 있습니다.
만약 V2 쿼리 없이 validate 했다면 Entity와 DB와의 연동은 되지 않았을 것입니다.
이유는 active에 해당되는 DB 컬럼이 없기 때문입니다.<br/>