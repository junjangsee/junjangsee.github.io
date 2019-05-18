---
title: SpringBoot - JPA 연동하기
date: 2019-05-14 15:19:36
tags: [SpringBoot, DB, ORM, JPA]
---

![images](/images/springboot/springboot.png)<br/>

# JPA
JPA를 연동하고 테스트하는 방법을 공부해봅시다.
기본적인 개념에 대해 궁금하시다면 [ORM, JPA의 개념](https://junjangsee.github.io/2019/05/14/spring/spring-26/) 포스팅을 참고해주시면 됩니다.<br/>
<br/>

## JPA 연동 및 테스트
### JPA 의존성 추가
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```
![JPA](/images/springboot/jpa/jpa1.png) 위 의존성을 추가하면 다른 세팅을 건들지 않아도 바로 사용할 수 있습니다.<br/>
<br/>

### Entity 클래스 생성
```java
@Entity
public class Account {

    @Id @GeneratedValue
    private Long ID;

    private String username;

    private String password;

    public Long getID() {
        return ID;
    }

    public void setID(Long ID) {
        this.ID = ID;
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

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Account account = (Account) o;
        return Objects.equals(ID, account.ID) &&
                Objects.equals(username, account.username) &&
                Objects.equals(password, account.password);
    }

    @Override
    public int hashCode() {
        return Objects.hash(ID, username, password);
    }
}
```
![JPA](/images/springboot/jpa/jpa2.png) account 패키지를 생성 후 Account 클래스를 생성합니다. getter, setter, equals, hashcode도 생성해줍니다.
- @Entity : 엔티티 빈으로 등록하는 역할을 합니다.
- @ID : 엔티티의 기본 키를 나타내며 하나 이상 필요합니다.
- @GeneratedValue : 자동으로 생성되도록 합니다. 만약 선언하지 않으면 수동작업을 해주어야 합니다.

<br/>
### Repository 인터페이스 생성
```java
public interface AccountRepository extends JpaRepository<Account, Long> {}
```
![JPA](/images/springboot/jpa/jpa3.png) extends를 통해 JpaRepository를 상속받고 <엔티티타입, 아이디타입>을 선언합니다.
여기까지 했다면 1차 연동은 되었습니다. 이제 테스트를 통해 연동상태를 확인해보겠습니다.<br/>
<br/>

### 테스트 클래스 생성
```java
@RunWith(SpringRunner.class)
@DataJpaTest
public class AccountRepositoryTest {

    @Autowired
    DataSource dataSource;

    @Autowired
    JdbcTemplate jdbcTemplate;

    @Autowired
    AccountRepository accountRepository;

    @Test
    public void di() throws SQLException {
    }
```
![JPA](/images/springboot/jpa/jpa4.png) @DataJpaTest를 통해 슬라이스 테스트를 진행합니다.
jpa를 의존성 추가하였다면 JDBC를 사용한다는 개념은 알고 계실 것입니다.
Repository까지 3개를 의존성 주입 받고 **'비어있는 테스트'**를 진행합니다.<br/>
**'비어있는 테스트'**를 진행하는 이유는 빈으로 등록이 잘 되어있는지, 테스트가 이상없이 실행 되는지 시험해보기 위함입니다.
그런데 테스트를 진행하면 오류가 뜰 것입니다.
이유는 슬라이스 테스트시 꼭 **인메모리 데이터베이스**가 필요합니다. 그렇기 때문에 H2를 추가하고 테스트를 진행하겠습니다.<br/>
인메모리 데이터베이스가 궁금하시다면 [인메모리 데이터베이스](https://junjangsee.github.io/2019/05/12/spring/spring-23/) 포스팅을 참고하시면 됩니다.<br/>
```xml
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>test</scope>
</dependency>
```
![JPA](/images/springboot/jpa/jpa6.png) H2 의존성을 test용도로 추가합니다.
그리고 다시 테스트를 진행합니다.<br/>
![JPA](/images/springboot/jpa/jpa7.png) 테스트가 성공한 것을 볼 수 있습니다.
그렇다면 애플리케이션에서도 성공할까요?
결론은 **실패**입니다. 그 이유는 운영환경 DB가 없기 때문입니다.
그렇기 때문에 운영환경 DB도 연동해야합니다. 저는 postgreSQL을 사용하여 연동하겠습니다.
혹시 postgreSQL이 궁금하시다면 [postgreSQL](https://junjangsee.github.io/2019/05/13/spring/spring-25/) 포스팅을 참고해주시면 됩니다.<br/>
```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
```
![JPA](/images/springboot/jpa/jpa8.png) postgresql 의존성을 추가합니다.
```bash
docker start postgres_boot
```
그리고 Docker를 통해 실행시킵니다.
<br/>
<br/>

#### Application.properties
```
spring.datasource.url=jdbc:postgresql://localhost:5432/springboot
spring.datasource.username=junjang
spring.datasource.password=pass
```
postgresql을 설치시 설정했던 정보를 선언하고 실행하면 아래과 같은 에러가 뜹니다.
<br/>

#### org.postgresql.jdbc.PgConnection.createClob() is not yet implemented 해결법
![JPA](/images/springboot/jpa/jpa9.png) 드라이버가 createClob()라는 Method를 지원하지않아서 에러가 발생한 것입니다. 
```
spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true
```
위 key, value값을 Application.properties에 추가하면 이상없이 구동될 것입니다.
<br/>

### MetaData 테스트
![JPA](/images/springboot/jpa/jpa10.png) 슬라이스 테스트를 진행하면 현재 사용되는 인메모리 데이터베이스의 MetaData가 출력되는 것을 볼 수 있습니다.<br/>
여기서 슬라이스 테스트가 아닌 **@SpringBootTest**를 하면 어떻게 될까요?
바로 운영환경 DB에 대한 정보가 출력됩니다.
그 이유는 애플리케이션을 구동하면 **전체 빈을 등록**하기 때문에 Application.properties에 선언한 정보가 오버라이딩 되어 운영환경 DB로 넘어가기 때문입니다.<br/>
![JPA](/images/springboot/jpa/jpa11.png) 그래서 위와 같은 결과가 출력됩니다.<br/>
<br/>

### 운영 DB 테스트
이제 최종적으로 연동이 되었는지 테스트 합니다.<br/>
```java
@RunWith(SpringRunner.class)
@DataJpaTest
public class AccountRepositoryTest {

    @Autowired
    DataSource dataSource;

    @Autowired
    JdbcTemplate jdbcTemplate;

    @Autowired
    AccountRepository accountRepository;

    @Test
    public void di() throws SQLException {
        Account account = new Account();
        account.setUsername("junjang");
        account.setPassword("pass");

        // save 함수로 account 값을 NewAccount 변수에 저장합니다.
        Account newAccount = accountRepository.save(account);

        assertThat(newAccount).isNotNull();

        // NewAccount에 저장된 값을 existAccount 변수에 담아 이름을 찾습니다.
        Account existingAccount = accountRepository.findByUsername(newAccount.getUsername());

        assertThat(existingAccount).isNotNull();

        // 들어가지 않은 값을 요청합니다.
        Account nonExistingAccount = accountRepository.findByUsername("kimjunhyeung");

        assertThat(nonExistingAccount).isNull();
    }
}
```
![JPA](/images/springboot/jpa/jpa12.png) findByUsername()는 이름을 찾아주는 역할을 하는 메소드로 직접 만든 메소드이기 때문에 Repository에서 해당 메소드를 정의해주어야 합니다.<br/>
```java
public interface AccountRepository extends JpaRepository<Account, Long> {
    Account findByUsername(String username);
}
```
![JPA](/images/springboot/jpa/jpa13.png) Account 객체를 가지는 메소드를 만들어줍니다.
그리고 테스트를 진행하면 이상없이 구동될 것입니다.<br/>
<br/>

#### Optinal 사용하기
```java
public interface AccountRepository extends JpaRepository<Account, Long> {
    Optional<Account> findByUsername(String username);
}
```
![JPA](/images/springboot/jpa/jpa14.png) 
```java
@RunWith(SpringRunner.class)
@DataJpaTest
public class AccountRepositoryTest {

    @Autowired
    DataSource dataSource;

    @Autowired
    JdbcTemplate jdbcTemplate;

    @Autowired
    AccountRepository accountRepository;

    @Test
    public void di() throws SQLException {
        Account account = new Account();
        account.setUsername("junjang");
        account.setPassword("pass");

        // save 함수로 account 값을 NewAccount 변수에 저장합니다.
        Account NewAccount = accountRepository.save(account);

        assertThat(NewAccount).isNotNull();

        // NewAccount에 저장된 값을 existAccount 변수에 담아 이름을 찾습니다.
        Optional<Account> existintAccount = accountRepository.findByUsername(NewAccount.getUsername());

        assertThat(existintAccount).isNotEmpty();

        // 들어가지 않은 값을 요청합니다.
        Optional<Account> nonExistintAccount = accountRepository.findByUsername("kimjunhyeung");

        assertThat(nonExistintAccount).isEmpty();
    }

}
```
![JPA](/images/springboot/jpa/jpa15.png) Optional을 사용하여 좀 더 명확하게 테스트를 진행할 수도 있습니다.<br/>