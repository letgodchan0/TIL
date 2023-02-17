# Spring Data JPA - Common: Repository 인터페이스 정의

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

![image-20230217141724699](C:/Users/livem/AppData/Roaming/Typora/typora-user-images/image-20230217141724699.png)

<br>

> Repository 인터페이스로 공개할 메서드를 직접 일일히 정의하고 싶다면?

#### 1. @RepositoryDefinition (특정 레포지토리)

```java
@RepositoryDefinition(domainClass = Comment.class, idClass = Long.class)
public interface CommentRepository {
    Comment save(Comment comment);
    List<Comment> findAll();
}
```

- 내가 사용할 기능들을 내가 직접 메서드 정의
- 메서드에 해당하는 기능을 스프링 데이터 JPA가 구현할 수 있는 것이라면, 구현해서 기본적으로 제공해준다.

<BR>

#### 2. @NoRepositoryBean (공통 인터페이스 정의)

```java
@NoRepositoryBean
public interface MyRepository<T, ID extends Serializable> extends Repository<T, ID> {
    <E extends T> E save(E entity);
    List<T> findAll();
    long count();
}
```

```java
public interface CommentRepository extends MyRepository<Comment, Long>{
}
```

- 정의한 공통 인터페이스를 상속받아서 사용한다.