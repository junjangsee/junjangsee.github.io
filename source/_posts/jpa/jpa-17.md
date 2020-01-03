---
title: JPA - Common QueryDSL
date: 2020-01-03 23:50:35
tags: [JPA, Common, QueryDSL]
---

![images](/images/jpa/jpa.jpg)<br/>

# QueryDSL(Domain Specific Language)
JPA를 사용하다보면 메소드 이름을 통해 자동으로 쿼리를 생성했었습니다.
```java
findByFirstNameIngoreCaseAndLastNameStartsWithIgnoreCase(String firstName, String lastName) 
```
위와 같은 네임을 가지고 있다면 가독성이 너무 떨어질 뿐더러 쿼리가 추가될 때마다 여러 조건들이 추가될 것입니다.
하지만 QueryDSL을 사용하게 되면 `타입세이프`하게 쿼리를 만들어 사용할 수 있습니다.<br/>
홈페이지 : [querydsl](http://www.querydsl.com/)<br/>
<br/>
 
## 연동 방법
### 기본 리포지토리 커스터마이징 안 했을 때
![querydsl](/images/jpa/querydsl/que1.png) 먼저 Account 엔티티와 JpaRepository를 상속받는 Repository를 생성합니다.<br/>
![querydsl](/images/jpa/querydsl/que3.png) 그리고 빈 주입 테스트를 하기 위해 테스트를 실행합니다.
여기서 이상이 없다면 성공하게 됩니다! 이제 `의존성`과 `플러그인`을 추가해보겠습니다.<br/>
#### 의존성 추가
```xml
<dependency>
  <groupId>com.querydsl</groupId>
  <artifactId>querydsl-apt</artifactId>
</dependency>
<dependency>
  <groupId>com.querydsl</groupId>
  <artifactId>querydsl-jpa</artifactId>
</dependency>
```
![querydsl](/images/jpa/querydsl/que4.png) 의존성을 추가해줍니다. QueryDSL은 스프링 부트가 의존성을 관리해주므로 버전을 명시하지 않아도 됩니다. 
`apt`모듈은 QueryDSL이 Entity모델을 보고 Query용 Specific Language(특정 언어)를 만들어 주는 모듈입니다.
<br/>

#### 플러그인 추가
```xml
<plugin>
  <groupId>com.mysema.maven</groupId>
  <artifactId>apt-maven-plugin</artifactId>
  <version>1.1.3</version>
  <executions>
    <execution>
      <goals>
        <goal>process</goal>
      </goals>
      <configuration>
        <outputDirectory>target/generated-sources/java</outputDirectory>
        <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
      </configuration>
    </execution>
  </executions>
</plugin>
```
![querydsl](/images/jpa/querydsl/que5.png) 빌드하면 target/generated-sources/java에 클래스를 생성하겠며, JPA를 사용하겠다는 의미입니다.
[레퍼런스참고](http://www.querydsl.com/static/querydsl/4.0.1/reference/ko-KR/html_single/)<br/>
![querydsl](/images/jpa/querydsl/que6.png) 위와 같이 의존성을 다 추가한 다음 Maven - Lifecycle - compile 클릭해서 빌드하게 되면<br/>
![querydsl](/images/jpa/querydsl/que7.png) 빌드 성공시 플러그인에 선언했던 경로에 생성된 것을 볼 수 있습니다.
<br/>

#### 생성된 클래스가 자동완성이 안된다면?
여기에 생성된 `QAccount` 클래스가 만약 자동완성이 안되는 경우가 생긴다면 어떻게 해야할까요?
![querydsl](/images/jpa/querydsl/que8.png) Project Structure에 들어가 해당 경로를 `Sources`로 바꾸어주면 해결됩니다.<br/>
```java
public interface AccountRepository extends JpaRepository<Account, Long>, QuerydslPredicateExecutor<Account> {
}
```
![querydsl](/images/jpa/querydsl/que9.png) Repository에 `QuerydslPredicateExecutor<T>`를 상속받습니다. 타입에는 엔티티를 넣어주면 됩니다.<br/>
```java
import com.querydsl.core.types.Predicate;

@RunWith(SpringRunner.class)
@DataJpaTest
public class AccountRepositoryTest {

    @Autowired
    AccountRepository accountRepository;

    @Test
    public void crud() {
        Predicate predicate = QAccount.account
                .firstName.containsIgnoreCase("junjang")
                .and(QAccount.account.lastName.startsWith("kim"));

        Optional<Account> one = accountRepository.findOne(predicate);
        assertThat(one).isEmpty();
    }
}
```
![querydsl](/images/jpa/querydsl/que10.png) querydsl predicate를 생성하여 생성된 QAccount 객체로 쿼리를 작성하여 테스트 합니다.<br/>
<br/>

### 기본 리포지토리 커스터마이징 했을 때
기본 구현 리포지토리가 아닌 커스텀한 리포지토리 구현체에 `QuerydslPredicateExecutor<T>`를 상속받고 생성자를 생성해주면 됩니다.