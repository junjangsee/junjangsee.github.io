---
title: JPA - Relation Mapping(관계 매핑)
date: 2019-05-23 16:34:57
tags: [JPA, Relation]
---

![images](/images/jpa/jpa.jpg)<br/>

# 관계 매핑
관계 매핑에선 항상 두 엔티티가 존재해야합니다. **주종관계**를 가지고 있으며 반대쪽 레퍼런스를 가진쪽이 주인 관계가 됩니다.<br/>
두 엔티티 클래스가 필요하기 때문에 기존 Account 클래스, 추가적으로 Study 클래스를 생성합니다.
여기서 Study는 Account가 Study를 다수로 생성할 수 있는 개념입니다.<br/>
```java
@Entity
public class Study {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```
![Entity](/images/jpa/relation/re1.png) 
<br/>

## 단방향
- 관계를 정의한 쪽이 그 관계의 주인입니다.(@ManyToOne)
- 기본값은 FK가 생성됩니다.
- 기본값은 조인 테이블을 생성합니다.
- @OneToMany로 매핑시 JOIN 테이블로 매핑됩니다.

<br/>

### @ManyToOne

```java
@Entity
public class Study {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    private Account owner;

    public Account getOwner() {
        return owner;
    }

    public void setOwner(Account owner) {
        this.owner = owner;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```
![Entity](/images/jpa/relation/re2.png) 스터디는 하나의 계정이 복수로 만들 수 있습니다. 그러므로 **@ManyToOne**을 사용합니다.
<br/>
```java
@Component
@Transactional
public class JpaRunner implements ApplicationRunner {

    @PersistenceContext
    EntityManager entityManager;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Account account = new Account();
        account.setUsername("kimjunhyeung");
        account.setPassword("1234");

        Study study = new Study();
        study.setName("Spring Data JPA");
        study.setOwner(account);

        Session session = entityManager.unwrap(Session.class);

        session.save(account);
        session.save(study);
    }
}
```
Study를 Account에 추가하는 코드를 작성합니다.
![Entity](/images/jpa/relation/re4.png) owner라고 값을 주었지만 **Account의 PK**를 참고하는 **owner_id라는 FK**가 생성됩니다.
<br/>
<br/>

### @OneToMany
```java
@Entity
public class Account {

    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    private String password;

    @OneToMany
    private Set<Study> studies = new HashSet<>();
}
```
![Entity](/images/jpa/relation/re5.png) Account는 여러개의 Study를 가질 수 있기 때문에 Set에 담아서 **@OneToMany**를 선언합니다.<br/>
```java
@Component
@Transactional
public class JpaRunner implements ApplicationRunner {

    @PersistenceContext
    EntityManager entityManager;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Account account = new Account();
        account.setUsername("kimjunhyeung");
        account.setPassword("1234");

        Study study = new Study();
        study.setName("Spring Data JPA");

        account.getStudies().add(study);

        Session session = entityManager.unwrap(Session.class);

        session.save(account);
        session.save(study);
    }
}
```
Set에 study를 add해줍니다.
![Entity](/images/jpa/relation/re6.png) 2개가 아닌 3개의 테이블이 생겼는데 account_studies는 **JOIN 테이블**로서 각 테이블의 PK를 가지고 있게 됩니다.
<br/>
<br/>

## 양방향
- FK를 가지고 있는 쪽이 주인입니다. 따라서 기본값은 @ManyToOne 가지고 있는 쪽이 주인입니다.
- 주인이 아닌쪽(@OneToMany쪽)에서 mappedBy 사용해서 관계를 맺고 있는 필드를 설정해야 합니다.

<br/>
### Study 클래스
```java
@Entity
public class Study {

    @Id @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    private Account owner;
}
```
Account를 종속시킨 주인 클래스로서 @ManyToOne을 선언합니다.
<br/>

### Account 클래스
```java
@Entity
public class Account {

    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    private String password;

    @OneToMany(mappedBy = "owner")
    private Set<Study> studies = new HashSet<>();
}
```
![Entity](/images/jpa/relation/re7.png) **mappedBy**를 통해서 종속관계를 설정해주어야 합니다.<br/>
```java
@Component
@Transactional
public class JpaRunner implements ApplicationRunner {

    @PersistenceContext
    EntityManager entityManager;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Account account = new Account();
        account.setUsername("kimjunhyeung");
        account.setPassword("1234");

        Study study = new Study();
        study.setName("Spring Data JPA");

        account.getStudies().add(study); // 종속관계 관계 설정
        study.setOwner(account); //주인이 되는 쪽 관계 설정

        Session session = entityManager.unwrap(Session.class);

        session.save(account);
        session.save(study);
    }
}
```
![Entity](/images/jpa/relation/re8.png) 주인 관계를 설정하는 로직의 작성은 필수이며 종속관계의 로직은 필수는 아닙니다.
그러나 객체지향적으로 활용하기 위해 **주종 모두 관계를 설정**해주는 것이 좋습니다.<br/>
<br/>

### 관계 설정 로직
```java
account.getStudies().add(study);
study.setOwner(account);
```
관계를 설정할 때 사용했던 로직입니다. 결과만 놓고 봤을 때는 주인쪽에 관계를 설정하면 이상 없지만 객체지향이기 때문에 양방향 관계를 설정해주어야합니다. 그래서 항상 같이 다니기 때문에 하나의 메소드로 묶어주면 편리합니다. 이 메소드를 컨비니언스(convinience) 메소드 라고 합니다.<br/>
<br/>

#### addStudy 메소드 생성
```java
public void addStudy(Study study) {
    this.getStudies().add(study);
    study.setOwner(this);
}
```
Account 클래스에서 Study를 추가할 때마다 관계를 설정하는 메소드를 생성합니다.
여기서 this는 account를 가르킵니다.<br/>
<br/>

#### 메소드 활용
```java
account.addStudy(study);
```
애플리케이션 구동시 생성한 메소드만 호출해주면 관계 설정 로직이 구동됩니다.<br/>