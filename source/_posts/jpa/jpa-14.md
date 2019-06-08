---
title: JPA - Common Query
date: 2019-06-02 21:09:32
tags: [JPA, Common, Query]
---

![images](/images/jpa/jpa.jpg)<br/>

# Query
데이터를 다룰 때 사용할 쿼리를 메소드에 적용하는 방법에 대해 학습해보겠습니다. 총 3가지 방법이 있습니다.
- 직접 쿼리 작성하는 방법
- 메소드 이름 분석 
- 위 두가지를 합친 방법

<br/>
## 쿼리 만드는 방법
### 1. 메소드 이름을 분석해서 ​쿼리 만들기​ (CREATE)
메소드 이름을 분석해서 스프링 데이터 JPA(Common)가 자동으로 쿼리를 만들어 줍니다.<br/>
```java
public interface CommentRepository extends MyRepository<Comment, Long>{
    /* Comment title에 Keyword가 들어있는 모든 Comment를 찾아주는 메소드 */
    List<Comment> findByCommentContains(String Keyword);
}
```
![Query](/images/jpa/commonquery/qu1.png) 메소드 이름을 JPA가 지원하는 방식에 맞게 작성하면 자동으로 쿼리를 만들어줍니다.<br/>
<br/>

### 2. 직접 쿼리 작성하는 방법(USE_DECLARED_QUERY)
**@Query** 어노테이션을 사용하여 직접 쿼리를 만듭니다. JPA를 사용할 때는 **JPQL**을 기본값으로 받아들여서 사용합니다.<br/>
```java
public interface CommentRepository extends MyRepository<Comment, Long>{
    /* Comment title에 Keyword가 들어있는 모든 Comment를 찾아주는 메소드 */
    @Query("SELECT c FROM Comment AS c")
    List<Comment> findByCommentContains(String Keyword);
}
```
![Query](/images/jpa/commonquery/qu2.png) Comment 도메인 객체에 keyword가 포함된 Comment를 찾는 메소드를 select하는 쿼리입니다.
위처럼 원하는 메소드에 **@Query**를 선언하여 활용합니다.<br/>
추가적으로 **nativeQuery(SQL)**를 사용하고 싶다면 @Query 어노테이션에 nativeQuery라고 명시(nativeQuery = true)하고 사용하면 됩니다.<br/>
<br/>

### 3. 미리 정의한 쿼리 찾아보고 없으면 만들기(CREATE_IF_NOT_FOUND)
선언이 되어있는 쿼리를 찾아보고 선언이 된 쿼리가 없는 경우 메서드 이름을 분석해서 생성하는 방법입니다.<br/>
<br/>

#### @EnableJpaRepositories
```java
@SpringBootApplication
@EnableJpaRepositories(queryLookupStrategy = QueryLookupStrategy.Key.CREATE_IF_NOT_FOUND)
public class DemospringdataApplication {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```
![Query](/images/jpa/commonquery/qu3.png) 기본적으로 Application에 **@EnableJpaRepositories** 어노테이션에 기본 값으로 **CREATE_IF_NOT_FOUND**가 설정되어 있어 따로 설정할 필요는 없습니다. 하지만 설정은 자신이 원하는 쿼리 설정 방법에 따라서 바꿀 수 있습니다.<br/>
<br/>

## 메소드 이름을 활용한 쿼리 생성법
메소드 이름으로 쿼리를 작성하는 세부적인 방법에 대해서 학습해보겠습니다.

- 리턴타입 {접두어}{도입부}By{프로퍼티 표현식}(조건식)[(And|Or){프로퍼티 표현식}(조건식)]{정렬 조건} (매개변수)

위 순서대로 메소드를 작성하는 원칙으로 하며, 아래 항목에 대한 명령어를 사용하면 됩니다.<br/>
  
| 항목            | 명령어                                                       |
| --------------- | ------------------------------------------------------------ |
| 접두어          | Find, Get, Query, Count, ...                                 |
| 도입부          | Distinct, First(N), Top(N)                                   |
| 프로퍼티 표현식   | Person.Address.ZipCode => find(Person)ByAddress_ZipCode(...) |
| 조건식          | IgnoreCase, Between, LessThan, GreaterThan, Like, Contains, ... |
| 정렬 조건       | OrderBy{프로퍼티}Asc                                         |
| 리턴 타입       | E, Optional<E>, List<E>, Page<E>, Slice<E>, Stream<E>        |
| 매개변수        | Pageable, Sort                                               |
<br/>

### 메소드 분석 쿼리 간단 예시(Paging, Sorting)
#### Paging(페이징) 쿼리
```java
/* 어떠한 post에 들어가 있으면서 그중에서 like(좋아요)가 몇개 이상인 Comment를 조회해오는 메서드 */
Page<Comment> findByLikeCountGreaterThanAndPost(int likeCount, Post post, Pageable pageable);
```
페이징으로 받고 싶으면 Pageable 파라미터를 주어야만 페이지에 대한 정보를 가져옵니다.<br/>

#### Sorting(정렬) 쿼리
```java
/* 최근 생성일 순으로 DESC 정렬해서 TOP 10개만 조회하는 Sorting 적용 예시 */
Page<Comment> findTop10ByLikeCountGreaterThanAndPostOrderByCreatedDesc(int likeCount, Post post, Pageable pageable);
```
도입부의 TopN을 사용해서 정렬된 10개를 뽑아오는 메소드입니다.
하지만 **Pageable**에 Sort(정렬) 관련된 기능이 이미 존재하기 때문에 권장하는 방법은 아닙니다.<br/>
만약 Page에 대한 정렬이 아니라면 아래 방법으로 사용하면 됩니다.<br/>

#### List Sorting(정렬) 쿼리
```java
List<Comment> findByLikeCountGreaterThanAndPost(int likeCount, Post post, Sort sort);
```
List로 선언하고 Sort를 매개변수로 가져오면 됩니다.<br/>
나머지 활용법에 대해서는 밑에서 자세히 다루겠습니다.
<br/>

### No property title found for type Comment! 
위 오류는 메소드 이름으로 분석한 쿼리가 Comment 도메인 객체와 일치하지 않은 에러입니다.
에러를 분석해보면 **title**이라는 프로퍼티가 Comment에서 찾을 수 없다는 것이므로 title이 아닌 **Comment**로 수정하여 실행하면 됩니다.<br/>
자주 볼 수 있는 에러이기 떄문에 익혀두면 좋습니다.<br/>
<br/>

## 메소드 분석 쿼리 예시
위에서 살펴봤던 간단 예시들에서 사용 방법과 대표 에러를 살펴봤습니다. 여기선 명령어들을 활용하는 방법에 대해 학습해보겠습니다.<br/>
<br/>

### IgnoreCase
upper를 적용하여 모든 값을 대문자로 변경해서 조회합니다.<br/>
```java
List<Comment> findByCommentContainsIgnoreCase(String keyword);
```
```java
@RunWith(SpringRunner.class)
@DataJpaTest
public class CommentRepositoryTest {

    @Autowired
    CommentRepository commentRepository;

    @Test
    public void crud() {
        Comment comment = new Comment();
        comment.setComment("spring data jpa");
        commentRepository.save(comment);

        List<Comment> comments = commentRepository.findByCommentContains("Spring");
        assertThat(comments.size()).isEqualTo(1);
    }
}
```
**Spring**으로 대문자 요청을 하더라도 Comment를 찾아냅니다.<br/>
<br/>

### AndLikeCountGreaterThan
likeCount가 특정 Comment보다 더 높은 Comment를 찾아냅니다. Comment 도메인 객체에 Integer likeCount = 0;을 선언합니다.<br/>
```java
private Integer likeCount = 0;
```
```java
List<Comment> findByCommentContainsIgnoreCaseAndLikeCountGreaterThan(String keyword, int likeCount);
```
찾을 제목, likeCount를 매개변수로 선언합니다.
```java
Comment comment = new Comment();
comment.setLikeCount(1);
comment.setComment("spring data jpa");
commentRepository.save(comment);

List<Comment> comments = commentRepository.findByCommentContainsIgnoreCaseAndLikeCountGreaterThan("Spring", 10);
assertThat(comments.size()).isEqualTo(0);
```
10보다 높은 Spring 이라는 Comment를 가진 정보를 찾습니다.
테스트에는 1로 설정하여 테스트 결과는 0이 나올 것입니다.<br/>
<br/>

### DESC
내림차순으로 정렬합니다.<br/>
```java
List<Comment> findByCommentContainsIgnoreCaseOrderByLikeCountDesc(String keyword);
```
정렬이기 때문에 keyword만 매개변수로 가져갑니다.
```java
@Test
public void crud() {
    this.createComment(100, "spring data jpa");
    this.createComment(55, "HIBERNATE SPRING");

    List<Comment> comments = commentRepository.findByCommentContainsIgnoreCaseOrderByLikeCountDesc("Spring");
    assertThat(comments.size()).isEqualTo(2); /* 2개의 결과가 있고 */
    assertThat(comments).first().hasFieldOrPropertyWithValue("likeCount", 100); /* 첫번째 값의 likeCount = 100 */
}

private void createComment(int likeCount, String comment) {
    Comment newComment = new Comment();
    newComment.setLikeCount(likeCount);
    newComment.setComment(comment);
    commentRepository.save(newComment);
}
```
likeCount가 100, 55인 두개의 Comment를 생성한 것을 Descending하여 총 값이 2개이며 likeCount가 100인 정보가 첫 번째가 맞는지 테스트 합니다.<br/>
오름차순의 경우 ASC를 적용하여 첫 번째 값을 **55**로 하면 되겠습니다.<br/>
<br/>

### Pageable
페이징을 통한 방법입니다.<br/>
```java
Page<Comment> findByCommentContainsIgnoreCase(String keyword, Pageable pageable);
```
Pageable을 매개변수로 사용하고 Page 객체를 사용합니다.
```java
 PageRequest pageRequest = PageRequest.of(0, 10, Sort.by(Sort.Direction.DESC, "LikeCount"));

Page<Comment> comments = commentRepository.findByCommentContainsIgnoreCase("Spring", pageRequest);
assertThat(comments.getNumberOfElements()).isEqualTo(2);
assertThat(comments).first().hasFieldOrPropertyWithValue("likeCount", 100);
```
PageRequest 객체를 사용하여 PageRequest.of()를 통해 시작할 페이지수, 총 정보 개수, 정렬 방식, 정렬할 정보를 선언할 수 있습니다.<br/>
<br/>

### Stream
Stream 객체를 사용할 수 있습니다.<br/>
```java
Stream<Comment> findByCommentContainsIgnoreCase(String keyword, Pageable pageable);
```
Stream 객체로 받아옵니다.
```java
PageRequest pageRequest = PageRequest.of(0, 10, Sort.by(Sort.Direction.DESC, "LikeCount"));
try(Stream<Comment> comments = commentRepository.findByCommentContainsIgnoreCase("Spring", pageRequest)) {
    Comment firstComment = comments.findFirst().get(); /* 첫번째 값 조회 */
    assertThat(firstComment.getLikeCount()).isEqualTo(100); /* 첫번째 likeCount 값은 100 */
}
```
Stream은 **try-with-resource**로 받아야 기능을 합니다.<br/>