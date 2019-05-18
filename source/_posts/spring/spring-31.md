---
title: SpringBoot - Security 커스터마이징
date: 2019-05-16 17:17:25
tags: [SpringBoot, Security, UserDetailService, passwordEncoder]
---

![images](/images/springboot/springboot.png)<br/>

# Security 커스터마이징
시큐리티를 커스텀하여 내가 원하는 페이지에서만 로그인을 할 수 있도록 구현하는 방법을 학습하겠습니다. 이 포스팅은 이전에 [Security의 개념과 테스트](https://junjangsee.github.io/2019/05/14/spring/spring-30/)에 작성된 코드의 이해와 내용이 꼭 필요하니 참고해주시면 되겠습니다.<br/>
- 시작 전 세팅되어있어야 할 것들 : Controller 클래스, HTML , thymeleaf, security 의존성 추가

<br/>

## WebSecurityConfigurerAdapter
Config 패키지 내에 SecurityConfig 클래스를 생성하여 **WebSecurityConfigurerAdapter**를 상속받습니다.<br/>
![Security](/images/springboot/security/se17.png) 상속받은 후 **configure** 메소드를 오버라이딩 합니다.<br/>
<br/>

### configure 메소드 구현
```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                // index와 hello는 인증없이 접근이 가능합니다.
                .antMatchers("/","/hello").permitAll() 
                // 이외 요청은 formLogin을 요청하고 아니면 Basic을 요청합니다.
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .and()
            .httpBasic();
    }
}
```
![Security](/images/springboot/security/se18.png) 여기까지는 My에 접속할 때 로그인을 유도하도록 한 작업입니다.<br/> 
<br/>

## UserDetailsService
스프링 부트에서 자동으로 유저를 만들어주는 것이 아닌 직접 유저를 관리 할 수 있도록 하는 인터페이스 입니다.
참고자료 : [스프링공식문서](https://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/#jc-authentication-userdetailsservice)<br/>
그렇게 하기 위해선 DB세팅과 구현할 서비스 클래스가 필요합니다.
먼저 데이터베이스 관련 의존성 주입을 해줍니다.
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
</dependency>
```
jpa를 사용하여 데이터관리를 할 것이고 임베디드 데이터가 필요하기 때문에 h2를 추가하겠습니다.<br/>
### Account 클래스 생성
```java
@Entity
public class Account {

    @Id @GeneratedValue
    private Long id;
    private String username;
    private String password;

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
}
```
<br/>

### AccountRepository 인터페이스 생성
```java
public interface AccountRepository extends JpaRepository<Account, Long> {
}
```
<br/>

### Service 클래스 생성
UserDetailsService를 구현하기 위해 클래스를 생성합니다.<br/>
![Security](/images/springboot/security/se19.png) 최종적으로 위 세가지를 생성하여야 합니다.<br/> 
<br/>

### Service 구현
지금 구현할 것은 데이터에 입력받을 username, password를 연동한 Repository에 추가하는 기능을 구현할 것입니다.<br/>

```java
@Service
public class AccountService {

    @Autowired
    private AccountRepository accountRepository;

    public Account createAccount(String username, String password) {
        Account account = new Account();
        account.setUsername(username);
        account.setPassword(password);
        return accountRepository.save(account);
    }
```
![Security](/images/springboot/security/se20.png) jpa와 연동한 Repository를 주입받고 Account 타입으로 username, password를 파라미터값으로 받아옵니다. 받아온 값을 set 해주고 save로 추가한 account를 저장한 후 return 해줍니다.<br/>
현재까지는 스프링 부트가 자동으로 생성한 username, password를 가지고 로그인을 했었습니다.
이제 그 정보를 받지 않고 추가한 값으로 로그인 처리가 되도록 해볼 것입니다.
그러기 위해선 **UserDetailService** 를 구현해야만 합니다.<br/>
<br/>

## UserDetailService 구현
이전까지 UserDetailService를 구현하기 위한 세팅을 마쳤습니다. 이제 직접 구현하여 새로운 Account 정보로 로그인이 될 수 있게 구현해보겠습니다.<br/>

```java
@Service
public class AccountService implements UserDetailsService {

    @Autowired
    private AccountRepository accountRepository;

    public Account createAccount(String username, String password) {
        Account account = new Account();
        account.setUsername(username);
        account.setPassword(password);
        return accountRepository.save(account);

        @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        return null;
    }
    }
```
![Security](/images/springboot/security/se21.png) 먼저 implements로 UserDetailService를 구현합니다. 그리고 유저 정보를 가져오는 메소드를 오버라이딩합니다.<br/>
ps. 여기서 구현시 어디에 구현하던 상관은 없지만 꼭 **빈**으로 등록되어있어야 자동으로 유저가 생성되지 않습니다.<br/>
<br/>

### loadUserByUsername 인터페이스 구현
위 인터페이스에서는 username을 가져와 UserDetails에서 유저 정보를 확인합니다. 여기엔 password가 들어있는데 이때 입력한 password가 같은지 확인하고 그에 맞는 조치를 취하게 할 수 있습니다.<br/> 
```java
@Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        Optional<Account> byUsername = accountRepository.findByUsername(username);
        Account account = byUsername.orElseThrow(() -> new UsernameNotFoundException(username));
        return new User(account.getUsername(), account.getPassword(), authorities());
    }

    private Collection<? extends GrantedAuthority> authorities() {
        return Arrays.asList(new SimpleGrantedAuthority("ROLE_USER"));
    }
```
![Security](/images/springboot/security/se23.png) Repository에 저장된 username을 찾기 위해 새로운 메소드를 정의하고 찾은 username을 byUsername에 담습니다.(이 부분을 모르시겠다면 [JPA 연동하기 - 운영 DB 테스트](https://junjangsee.github.io/2019/05/14/spring/spring-27/)부분을 참고해주시면 됩니다.)<br/>
만약, username이 없다면 에러를 던지고 있으면 account에 넣게 됩니다.
이제 이러한 유저의 정보를 가지고 로그인 판단을 해야합니다. 그 판단은 **UserDetails**의 정보로 하게됩니다.
(현재 실습의 경우엔 Account의 정보가 되겠습니다.)<br/>
**UserDetails**에 담고있는 유저의 정보를 User()로 가져올 수 있습니다. 그리고 이 유저가 어떤 권한을 가졌는지 알아보기 위해 authorities 메소드를 생성하여 ROLE_USER 이라는 권한을 가진 유저라고 임의 설정하고 최종적으로 return하게 됩니다.<br/>
결론적으로 말하면 로그인 한 username을 UserDetails에 있는 정보와 대조해서 맞으면 로그인, 틀릴시 에러를 출력하게 됩니다.<br/>
여기까지 왔다면 이제 자동으로 생성되는 유저의 정보가 아닌 입력한 정보로 로그인을 판별하게 됩니다.
하지만 아직 로그인이 통과될 유저정보가 없기 때문에 **ApplicationRunner**를 통해 정보를 만들도록 하겠습니다.<br/>
<br/>

## 로그인 정보 구현
**ApplicationRunner**는 애플리케이션이 실행될 때 함께 실행됩니다.<br/>
```java
@Component
public class AccountRunner implements ApplicationRunner {

    @Autowired
    AccountService accountService;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Account junjang = accountService.createAccount("junjang", "1234");
        System.out.println(junjang.getUsername() + " password : " + junjang.getPassword());
    }
}
```
![Security](/images/springboot/security/se24.png) Service에서 구현했던 유저 생성메소드를 사용하여 정보를 추가합니다. 
그리고 애플리케이션을 재실행 하고 로그인을 해보면..?
```bash
ava.lang.IllegalArgumentException: There is no PasswordEncoder mapped for the id "null"
```
위의 오류가 뜨면서 로그인이 되지 않습니다.
내 정보도 생성했고 그 정보로 똑같이 입력했는데 왜 로그인이 안될까요?<br/>
그 이유는 **PasswordEncoder**의 **보안** 때문입니다.
이 보안을 설정하는 법에 대해서 알아보겠습니다.<br/>
<br/> 

## PasswordEncoder
password를 바로 DB에 저장하면 큰 시큐리티이슈에 걸리기 때문에 꼭 인코딩의 작업이 필요합니다.
참고자료 : [스프링공식문서](https://docs.spring.io/spring-security/site/docs/current/reference/htmlsingle/#core-services-password-encoding)<br/>
<br/>

### NoOpPasswordEncoder
실제로는 사용하면 안되지만 예외적으로 회피할 수 있는 방법입니다.<br/>
```java
@Bean
public PasswordEncoder passwordEncoder() {
    return NoOpPasswordEncoder.getInstance();
}
```
![Security](/images/springboot/security/se25.png) 커스텀을 구현한 SecurityConfig에 빈으로 등록하면 아무런 인코딩 절차를 거치지 않고 **NoOpPasswordEncoder**로 변환되어 순수한 비밀번호 그 상태가 됩니다.<br/>
<br/>

### PasswordEncoderFactories.createDelegatingPasswordEncoder()
위 방법을 사용하면 스프링시큐리티가 권장하는 방식으로 인코딩이 됩니다.<br/>
```java
@Bean
public PasswordEncoder passwordEncoder() {
    return PasswordEncoderFactories.createDelegatingPasswordEncoder();
}
```
![Security](/images/springboot/security/se26.png) 커스텀을 구현한 SecurityConfig에 빈으로 등록합니다.
이제 이를 직접 사용하기 위해 Service로 이동합니다.<br/> 
```java
@Autowired
private PasswordEncoder passwordEncoder;

public Account createAccount(String username, String password) {
    Account account = new Account();
    account.setUsername(username);
    account.setPassword(passwordEncoder.encode(password));
    return accountRepository.save(account);
}
```
![Security](/images/springboot/security/se27.png) PasswordEncoder를 주입받으면 빈으로 등록한 메소드가 동작하여 password를 입력하면 인코딩이 됩니다.<br/>
<br/>

### passwordEncoder 인코딩 결과
```bash
junjang password : {bcrypt}$2a$10$Mb4Qf8Ajb6PKDHGflWDKfeWLvQjjp8Wm9TeqGIiuyetWzeM44P4Dq
```
![Security](/images/springboot/security/se28.png) 위와 같이 인코딩 된 것을 보실 수 있습니다.<br/>
