# Spring Data JPA - Query

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

### 1. JPQL(HQL)

```java
TypedQuery<Post> query = entityManager.​createQuery​("SELECT p FROM Post As p", Post.class);
List<Post> posts = query.getResultList();
posts.forEach(System.out::println);
```

- Java Persistence Query Language / Hibernate Query Language

- sql과 차이점은 테이터베이스 테이블이 아닌, 엔티티 객체 모델 기반으로 쿼리를 작성한다.
- JPA 또는 하이버네이트가 해당 쿼리를 SQL로 변환해서 실행한다. 
  - [하이버네이트 참고](https://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html#hql)

- **JPA 2.0** 부터는 **Type**을 지정할 수 있고 지정한 **Type**의 리스트로 출력이 된다. 이전에는 **Object Type**으로 나와서 다 변환해야 했다.

<BR>

### 2. Criteria

```java
CriteriaBuilder builder = entityManager.getCriteriaBuilder();
CriteriaQuery<Post> criteria = builder.createQuery(Post.class);
Root<Post> root = criteria.from(Post.class);
criteria.select(root);
List<Post> posts = entityManager.createQuery(criteria).getResultList();
```

- 타입 세이프 쿼리

- [하이버네이트 참고](https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#criteria)

<br>

### 3. Native Query

```java
List<Post> posts = entityManager
                .createNativeQuery("SELECT * FROM Post", Post.class)
                .getResultList();
```

- SQL 쿼리 실행하기
- Typed 메서드가 아니더라도 지정한 Type으로 결과값을 리턴해준다.
- [하이버네이트 참고](https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#sql)