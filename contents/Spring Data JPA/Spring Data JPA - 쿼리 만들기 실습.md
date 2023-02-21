# Spring Data JPA - 쿼리 만들기 실습

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

### findByCommentContains 메서드 추가

```java
public interface CommentRepository extends MyRepository<Comment, Long>{
    List<Comment> findByCommentContains(String keyword);
}
```

<br>

### 쿼리 테스트

**Spring**의 **S**가 대문자라서 해당하는 **Comment**가 없다고 에러가 남

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

<br>

## IgnoreCase 추가

**upper**를 적용하여 값을 모두 **대문자**로 변경해서 조회하므로 에러가 나지 않음

```java
List<Comment> findByCommentContainsIgnoreCase(String keyword);
```

<br>

## likeCount가 특정 숫자보다 높을 때 조건 추가

```java
List<Comment> findByCommentContainsIgnoreCaseAndLikeCountGreaterThan(String keyword, int likeCount);
```

<br>

### likeCount 값을 초기화

```java
private Integer likeCount = 0;
```

<br>

### likeCount 적용 쿼리 테스트

```java
Comment comment = new Comment();
comment.setLikeCount(1);
comment.setComment("spring data jpa");
commentRepository.save(comment);

List<Comment> comments = commentRepository.findByCommentContainsIgnoreCaseAndLikeCountGreaterThan("Spring", 10);
assertThat(comments.size()).isEqualTo(0);
```

<br>

## Comment를 likeCount 높은 순으로 정렬

```java
List<Comment> findByCommentContainsIgnoreCaseOrderByLikeCountDesc(String keyword);
@Test
public void crud() {
    this.createComment(100, "spring data jpa");
    this.createComment(55, "HIBERNATE SPRING");

    List<Comment> comments = commentRepository.findByCommentContainsIgnoreCaseOrderByLikeCountDesc("Spring");
    assertThat(comments.size()).isEqualTo(2); // 2개의 결과가 있고
    assertThat(comments).first().hasFieldOrPropertyWithValue("likeCount", 100); // 첫번째 값의 likeCount = 100
}

private void createComment(int likeCount, String comment) {
    Comment newComment = new Comment();
    newComment.setLikeCount(likeCount);
    newComment.setComment(comment);
    commentRepository.save(newComment);
}
```

<br>

### ASC 도 테스트

```java
List<Comment> findByCommentContainsIgnoreCaseOrderByLikeCountAsc(String keyword);
List<Comment> comments = commentRepository.findByCommentContainsIgnoreCaseOrderByLikeCountAsc("Spring");
assertThat(comments.size()).isEqualTo(2); // 2개의 결과가 있고
assertThat(comments).first().hasFieldOrPropertyWithValue("likeCount", 55); // 첫번째 값의 likeCount = 55
```

<br>

## Pageable로 동적으로 정렬

```java
Page<Comment> findByCommentContainsIgnoreCase(String keyword, Pageable pageable);
```

<br>

### DESC로 정렬

```java
PageRequest pageRequest = PageRequest.of(0, 10, Sort.by(Sort.Direction.DESC, "LikeCount"));

Page<Comment> comments = commentRepository.findByCommentContainsIgnoreCase("Spring", pageRequest);
assertThat(comments.getNumberOfElements()).isEqualTo(2);
assertThat(comments).first().hasFieldOrPropertyWithValue("likeCount", 100);
```

<br>

## Stream으로 받아오기

```java
Stream<Comment> findByCommentContainsIgnoreCase(String keyword, Pageable pageable);
```

<br>

### Stream 으로 받아 오는 로직

**Stream**은 **Stream**을 받고 난 다음 닫아줘아함

```java
PageRequest pageRequest = PageRequest.of(0, 10, Sort.by(Sort.Direction.DESC, "LikeCount"));
try(Stream<Comment> comments = commentRepository.findByCommentContainsIgnoreCase("Spring", pageRequest)) {
    Comment firstComment = comments.findFirst().get(); //첫번째 값 조회
    assertThat(firstComment.getLikeCount()).isEqualTo(100); //첫번째 likeCount 값은 100
}
```

<br>

## 기본 예제

```java
List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);
// distinct
List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);
// ignoring case
List<Person> findByLastnameIgnoreCase(String lastname);
// ignoring case
List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);
```

<br>

## 정렬

```java
List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
```

<br>

## 페이징

```java
Page<User> findByLastname(String lastname, Pageable pageable);
Slice<User> findByLastname(String lastname, Pageable pageable);
List<User> findByLastname(String lastname, Sort sort);
List<User> findByLastname(String lastname, Pageable pageable);
```

<br>

## 스트리밍

`try-with-resource` 사용할 것 (**Stream**을 다 쓴 다음에 `close()` 해야 함)

```java
Stream<User> readAllByFirstnameNotNull();
```