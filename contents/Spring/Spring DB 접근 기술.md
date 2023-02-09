## - 스프링 DB 접근 기술

<hr>


#### 1. H2 DB 설치

[여기](https://www.h2database.com/html/download-archive.html)서 `1.4.200` 버전 설치

```bash
/bin

h2.bat  // 실행하면 아래와 같은 화면이 브라우저로 띄워짐
```

![image-20230206115104923](C:/Users/livem/Desktop/letgodchan0/TIL/contents/Spring/Spring%20%ED%9A%8C%EC%9B%90%20%EA%B4%80%EB%A0%A8%20%EA%B0%84%EB%8B%A8%ED%95%9C%20%EB%B0%B1%EC%97%94%EB%93%9C%20%EA%B0%9C%EB%B0%9C.assets/image-20230206115104923.png)

- 한번 연결하고 이후 `JDBC URL`은 jdbc:h2:tcp://localhost/~/test 로 설정하고 연결한다!

<br>

#### 2. JDBC

```java
// build.gradle

dependencies {
    ...
    implementation 'org.springframework.boot:spring-boot-starter-jdbc' // 자바는 db랑 붙으려면 jdbc 드라이버가 있어야 함
	runtimeOnly 'com.h2database:h2' //
}
```

- db 관련 라이브러리 추가

```properties
//resources/application.properties

spring.datasource.url=jdbc:h2:tcp://localhost/~/test
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
```

> \> 주의!: 스프링부트 2.4부터는 spring.datasource.username=sa 를 꼭 추가해주어야 한다. 그렇지 않으면 Wrong user name or password 오류가 발생한다. 참고로 다음과 같이 마지막에 공백이 들어가면 같은 오류가 발생한다. spring.datasource.username=sa 공백 주의, 공백은 모두 제거해야 한다.

<br>

```java
// SpringConfig
@Configuration
public class SpringConfig {

    private DataSource dataSource;

    ...

    @Bean
    public MemberRepository memberRepository() {
        // 기존 MemoryMemberRepository에서 변경
        return new JdbcMemberRepository(dataSource); 
    }
}
```



![image-20230207105938895](Spring%20DB%20%EC%A0%91%EA%B7%BC%20%EA%B8%B0%EC%88%A0.assets/image-20230207105938895.png)

- `MemberRepository`의 새로운 구현체로 `JdbcMemberRepository`를 만들어서 주입한다. 객체지향의 장점! 아래와 같이 언제든지 새롭게 갈아끼울 수 있음

![image-20230207110414963](Spring%20DB%20%EC%A0%91%EA%B7%BC%20%EA%B8%B0%EC%88%A0.assets/image-20230207110414963.png)

- 개방-폐쇄 원칙(OCP, Open-Closed Principle) 
  - 확장에는 열려있고, 수정, 변경에는 닫혀있다. 
- 스프링의 DI (Dependencies Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있다. 
- 회원을 등록하고 DB에 결과가 잘 입력되는지 확인하자. 
- 데이터를 DB에 저장하므로 스프링 서버를 다시 실행해도 데이터가 안전하게 저장된다.

<BR>

### - 스프링 통합 테스트

스프링 컨테이너와 DB까지 연결한 통합 테스트를 진행해보자.

```java
// test/service/MemberServiceIntegrationTest

@SpringBootTest
@Transactional
class MemberServiceIntegrationTest {
    @Autowired
    MemberService memberService;
    @Autowired
    MemberRepository memberRepository;
    
    @Test
    public void 회원가입() throws Exception {
       ...
    }
    @Test
     public void 중복_회원_예외() throws Exception {
        ...
    }
}
```

- `@SpringBootTest` : 스프링 컨테이너와 테스트를 함께 실행한다. 
- `@Transactional` : 테스트 케이스에 이 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고, 테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.

- 위와 같이 스프링 컨테이너를 띄우면서 테스트를 진행하기도 하지만, 진짜 좋은 테스트는 순수 자바 코드로 단위 테스트를 설계하는게 좋다!

<br>

### - 스프링 JdbcTemplate

- 순수 jdbc와 동일한 환경설정을 하면 된다.

- 스프링 JdbcTemplate과 `MyBatis` 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다. 하지만 SQL은 직접 작성해야 한다.

<br>

#### 스프링 JdbcTemplate 회원 레포지토리

```java
repository/JdbcTemplateMemberRepository
    
public class JdbcTemplateMemberRepository implements MemberRepository{

    private final JdbcTemplate jdbcTemplate;
	
    // 생성자가 딱 하나 있으면 @Autowired 생략할 수 있다!!
    public JdbcTemplateMemberRepository(DataSource dataSource){
        jdbcTemplate = new JdbcTemplate(dataSource);
    }
    ...
}
```

- `private final JdbcTemplate jdbcTemplate;` : dataSource를 주입 받아서 넣어주는 스타일을 권장한다.

<br>

#### JdbcTemplate을 사용하도록 스프링 설정 변경

```java
SpringConfig
    
@Configuration
public class SpringConfig {
    
    ...
        
    @Bean
    public MemberRepository memberRepository() {
        // return new JdbcMemberRepository(dataSource);
        return new JdbcTemplateMemberRepository(dataSource);
    }
}
```



<br>

## JPA

- JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해준다. 
- JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다. 
- JPA를 사용하면 개발 생산성을 크게 높일 수 있다.

<br>

#### build.gradle 파일에 JPA, h2 데이터베이스 관련 라이브러리 추가

```java
build.gradle
    
dependencies{
    ...
    //implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    ...
}
```

`spring-boot-starter-data-jpa` 는 내부에 jdbc 관련 라이브러리를 포함한다. 따라서 jdbc는 제거해도 된다.

<br>

#### 스프링 부트에 JPA 설정 추가

```properties
resources/application.properties

...

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=none
```

- `show-sql` : JPA가 생성하는 SQL을 출력한다. 
- `ddl-auto` : JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 none 를 사용하면 해당 기능을 끈다. 
  - create 를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해준다. 해보자.

<br>

#### JPA 엔티티 매핑

```java
@Entity
public class Member {
    
     @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
     private Long id;
    
     private String name;
    ...
    }
J
```

- `@Entity` 어노테이션 달아주기!
- `@GeneragedValue(strategy = GenerationType.IDENTITY)` : DB에 값을 넣으면 자동으로 id를 생성해주는 것을 Identity 전략이라고 한다. 이를 사용하기 위한 어노테이션!



<br>

#### JPA 회원 레포지토리

```java
repository/JpaMemberRepository

public class JpaMemberRepository implements MemberRepository{

    private final EntityManager em;

    public JpaMemberRepository(EntityManager em){
        this.em = em;
    }

    public Member save(Member member) {
        em.persist(member); // persist 메서드가 저장해줌
        return member;
    }
    
    public Optional<Member> findById(Long id) {
        Member member = em.find(Member.class, id); // find가 조회
        return Optional.ofNullable(member);
    }
    
    public List<Member> findAll() {
        // jpql 방식
        return em.createQuery("select m from Member m", Member.class)
                .getResultList();
    }
    
    public Optional<Member> findByName(String name) {
        List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
                .setParameter("name", name)
                .getResultList();
        return result.stream().findAny();
    }
}
```

- `EntityManager` : Jpa는 얘를 가지고 모든 것이 동작한다. `build.gradle`에서 라이브러리 받으면, 스프링 부트가 자동으로 얘를 생성해주기 때문에 만들어진 것을 주입받기만 하면 된다. 

<br>

#### 서비스에 트랜잭션 추가

```java
service/MemberService
    
@Transactional
public class MemberService()
```

- `org.springframework.transaction.annotation.Transactional` 를 사용하자. 
- 스프링은 해당 클래스의 메서드를 실행할 때 트랜잭션을 시작하고, 메서드가 정상 종료되면 트랜잭션을 커밋한다. 만약 런타임 예외가 발생하면 롤백한다. 
- JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.



<br>

#### JPA를 사용하도록 스프링 설정 변경

```java
SpringConfig
    
@Configuration
public class SpringConfig {

    private DataSource dataSource;
    private EntityManager em; // 추가

    ...

    @Bean
    public MemberRepository memberRepository() {
        return new JpaMemberRepository(em);
    }
}
```



<br>

## 스프링 데이터 JPA

> 스프링 부트와 JPA만 사용해도 개발 생산성이 정말 많이 증가하고, 개발해야할 코드도 확연히 줄어든다. 여기에 스프링 데이터 JPA를 사용하면, 기존의 한계를 넘어 마치 마법처럼, 리포지토리에 구현 클래스 없이 인터페이스 만으로 개발을 완료할 수 있다. 그리고 반복 개발해온 기본 CRUD 기능도 스프링 데이터 JPA가 모두 제공한다. 
>
> 스프링 부트와 JPA라는 기반 위에, 스프링 데이터 JPA라는 환상적인 프레임워크를 더하면 개발이 정말 즐거워진다. 지금까지 조금이라도 단순하고 반복이라 생각했던 개발 코드들이 확연하게 줄어든다. 따라서 개발자는 핵심 비즈니스 로직을 개발하는데, 집중할 수 있다. 실무에서 관계형 데이터베이스를 사용한다면 스프링 데이터 JPA는 이제 선택이 아니라 필수이다.
>
> 라는 김영한님 말씀!

<br>

#### 스프링 데이터 JPA 회원 레포지토리

```java
repository/SpringDataJpaMemberRepository
    
public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository{
    Optional<Member> findByName(String name);
}
```

- 스프링 데이터 JPA가 `JpaRepository`를 받고 있는 인터페이스의 구현체를 자동으로 만들어서 스프링 빈에 등록해준다. 그래서 이거를 그냥 가져다가 아래와 같이 사용하면 된다.

<br>

#### 스프링 데이터 JPA 회원 레포지토리를 사용하도록 스프링 설정 변경

```java
SpringConfig
    
@Configuration
public class SpringConfig {
    
    private final MemberRepository memberRepository;

    @Autowired
    public SpringConfig(MemberRepository memberRepository){
        this.memberRepository = memberRepository;
    }

    @Bean
    public MemberService memberService() {

        return new MemberService(memberRepository);
    }
}
```

- 스프링 컨테이너에서 `MemberRepository`를 찾는데, 등록한게 아직 없다. 근데 위에서  `JpaRepository`를 받는 인터페이스를 만들어 놨기 때문에 스프링 데이터 JPA가 인터페이스에 대한 구현체를 만들어버린다!! 그리고 스프링 빈에 등록하기 때문에 주입만해서 사용하면 되다.
- 스프링 데이터 JPA가 `SpringDataJpaMemberRepository` 를 스프링 빈으로 자동 등록해준다.

<br>

#### 스프링 데이터 JPA 제공 클래스

![image-20230207124810606](Spring%20DB%20%EC%A0%91%EA%B7%BC%20%EA%B8%B0%EC%88%A0.assets/image-20230207124810606-16759078306511.png)

- 인터페이스를 통한 기본적인 CRUD 
- `findByName()` , `findByEmail()` 처럼 메서드 이름 만으로 조회 기능 제공 
- 페이징 기능 자동 제공

> 참고: 실무에서는 JPA와 스프링 데이터 JPA를 기본으로 사용하고, 복잡한 동적 쿼리는 Querydsl이라는 라이브러리를 사용하면 된다. Querydsl을 사용하면 쿼리도 자바 코드로 안전하게 작성할 수 있고, 동적 쿼리도 편리하게 작성할 수 있다. 이 조합으로 해결하기 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리를 사용하거나, 앞서 학습한 스프링 JdbcTemplate를 사용하면 된다.

<br>