# Spring Data JPA - 프로젝트 세팅

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

[이거 보고](https://github.com/letgodchan0/TIL/blob/main/contents/Spring/Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.md) 스프링 부트 프로젝트 세팅을 먼저 하자!

<br>

#### 1. JPA 의존성 추가

```java
# build.gradle

dependencies {
    ...
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa' // JPA
}
```

<br>

#### 2. DB 연결

```java
# build.gradle

dependencies {
    ...
    runtimeOnly 'mysql:mysql-connector-j'  // mysql
}
```

```properties
resources/application.properties

spring.jpa.hibernate.ddl-auto=create
spring.jpa.generate-ddl=true
spring.datasource.url=jdbc:mysql://localhost:3306/springdatajpa?serverTimezone=UTC&characterEncoding=UTF-8
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=admin
spring.datasource.password=admin
```

- JPA DDL 자동 설정 옵션

  - `create` : 스키마와 데이터가 매번 새롭게 생성된다. (개발시 적합)
  - `update` : 스키마와 데이터를 유지하면서, 스키마나 데이터에 대한 변경사항을 적용한다. (스키마 변경시 이전 스키마가 남아있음!)
  - `create-drop` : 스키마와 데이터가 매번 새롭게 생성되고 종료시 제거된다.
  - `validate`  : 스키마를 검증만 해준다. 검증했을 때, 객체와 릴레이션 정보가 매핑되지 않으면, 구동 시 바로 에러가 난다. (운영시 적합)

- `springdatajpa`라는 스키마가 만들어져 있어야 한다.

- mysql user 권한 주기

  ```sql
  # 전체 DB에 전체 권한 추가
  grant all on *.* to test@localhost;
  
  # 특정 DB(mydb)에 대한 전체 권한 추가
  grant all on mydb.* to test@localhost;
  ```

  

<br>

#### 3. 엔티티 매핑

```java
# entity/Account

@Entity
public class Account {
    @Id @GeneratedValue
    private Long id;
    private String username;
    private String password;

    // alit + insert 누르면, getter and setter 한번에!
}
```

- `@Entity` 
  - “엔티티”는 객체 세상에서 부르는 이름. 
  - 보통 클래스와 같은 이름을 사용하기 때문에 값을 변경하지 않음. 
  - 엔티티의 이름은 JQL에서 쓰임. 
- `@Table` 
  -  “릴레이션" 세상에서 부르는 이름. 
  - @Entity의 이름이 기본값. 
  - 테이블의 이름은 SQL에서 쓰임.

- `@Id`: 데이터베이스 주키(Primary Key)에 맵핑
- `@GeneratedValue`: 자동 생성 설정
- `@Entity`: 클래스명에 해당되는 테이블에 맵핑

<br>

#### JpaRunner 생성

```java
# JpaRunner
    
@Component
@Transactional
public class JpaRunner implements ApplicationRunner {

    @PersistenceContext
    EntityManager entityManager;
    @Override
    public void run(ApplicationArguments args) throws Exception {
        Account account = new Account();
        account.setUsername("member1");
        account.setPassword("member1");

        entityManager.persist(account);
    }
}
```

- `@PersistenceContext`를 통해서 **JPA**의 핵심인 **EntityManager**를 주입 받음
- 이 클래스를 통해서 **Entity**들을 영속화`(데이터베이스에 저장)` 할 수 있음
- **JPA**와 관련된 모든 **Operation**들은 한 **Transaction** 안에서 일어나야함
- **Spring**에서 제공하는 `@Transactional`을 사용 클래스,메서드에 적용할 수 있음

<br>

#### Hibernate로 처리

```java
@Component
@Transactional
public class JpaRunner implements ApplicationRunner {

    @PersistenceContext
    EntityManager entityManager;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Account account = new Account();
        account.setUsername("member1");
        account.setPassword("member1");

        Session session = entityManager.unwrap(Session.class);
        session.save(account);
    }
}
```

- **JPA**가 **hibernate**를 사용하므로 **hibernate**도 사용할 수 있음
- **Hibernate**의 가장 핵심적인 **API**인 **Session**을 활용해 저장할 수 있다







