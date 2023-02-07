

# 회원 관련 간단한 백엔드 개발

> 회원 관련 간단한 백엔드 개발 및 테스트 코드 작성 과정 기술해보기!

[소스 코드](https://github.com/letgodchan0/collection/tree/main/NewProject)

### - 백엔드 코드

<br>

#### - domain/Member

```java
private Long id;
private String name;
```

- 두개 만들어 놓고 `alt` + `insert`  -> Getter and Setter 선택하면 바로 만들어준다. 인텔리제이 ㄷㄷ

<br>

#### - repository/MemberRepository

```java
// interface 생성

Member save(Member member);
Optional<Member> findById(Long id);
Optional<Member> findByName(String namd);
List<Member> findAll();
```

- Optional : 가져오는 값이 null일 수 있기 때문에 null을 그대로 반환하는 방법 대신 optional로 감싸서 반환한다.

- Optional.ofNullable(store.get(id)) : 이런식으로 null이 올 수도 있는 것

<br>

#### - repository/MemoryMemberRepository

```java
public class MemoryMemberRepository implements MemberRepository{
	// alt + insert -> method 자동 생성
}
```

- interface 상속받는 구현체 생성할 때도 `alt` +`insert`하면 method 자동으로 만들어줌 ㄷㄷ

```java
// 자바 람다인, filter로 이름 같은애 찾으면 반환, findAny()가 하나라도 찾으면 반환
store.values().stream()
    .filter(member -> member.getName().equals(name))
    .findAny()
```

<br>

### - service/MemberService

```java
Optional<Member> result = memberRepository.findByName(member.getName());
result.ifPresent(m -> {
    throw new IllegalStateException("이미 존재하는 회원입니다.");
});

// 아래와 같이 코드 깔끔하게 변경

memberRepository.findByName(member.getName())
	.ifPresent(m -> {
        throw new IllegalStateException("이미 존재하는 회원입니다.");
    });
```

- `ifPresent`는 result가 null이 아니라 어떤 값이 있으면 밑에 로직이 동작하는 것, optional 이었기 때문에 가능

<br>

### - 테스트 코드

개발한 기능을 실행해서 테스트 할 때 자바의 main 메서드를 통해 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능을 실행한다. 이러한 방법은 준비하고 실행하는데 오래 걸리고, 반복 실행하기 어렵고 여러 테스트를 한번에 실행하기 어렵다는 단점이 있다. 

자바는 `JUint` 이라는 프레임워크로 테스트를 실행해서 이러한 문제를 해결한다.

```java
test/java/newproject/repository/MemoryMemberRepositoryTest


// 테스트 끝날때마다 실행하는 것
@AfterEach
public void afterEach(){
    repository.clearStore();
}
    
```

- `@AfterEach` : 한번에 여러 테스트를 실행하면 메모리 DB에 직전 테스트의 결과가 남을 수 있다. 이렇게 되면 다음 이전 테스트 때문에 다음 테스트가 실패할 가능성이 있다. `@AfterEach` 를 사용하면 각 테스트가 종료될 때 마다 이 기능을 실행한다. 여기서는 메모리 DB에 저장된 데이터를 삭제한다. 
- 테스트는 각각 독립적으로 실행되어야 한다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아니다.

<br>

#### 테스트 코드 

#### assertEquals()

```java
// when
Member result = repository.findByName("spring1").get();

// then
Assertions.assertThat(result).isEqualTo(member1);
```

- member1이라고 주어진 객체가 DB 상의 result와 동일한제 테스트 해주는 메서드

<br>

#### assertThrows()

```java
IllegalStateException e = Assertions.assertThrows(IllegalStateException.class,
                () -> memberService.join(member2));
```

- `() -> memberService.join(member2)` 이 로직을 실행하면 `IllegalStateException.class` 이게 실행되어야 하는 문법

<br>

#### @BeforeEach

```java
@BeforeEach
public void beforeEach() {
    memoryMemberRepository = new MemoryMemberRepository();
    memberService = new MemberService(memoryMemberRepository);
}
```

- 각 테스트 실행 전에 호출된다. 테스트가 서로 영향이 없도록 항상 새로운 객체를 생성하고, 의존관계도 새로 맺어준다.

<br>

#### 단축키 - Shift + F6

```java
// shift + f6 하면 그 member2가 다 드래그 되서 한번에 바꿀 수 있음
Member member2 = new Member();
member2.setName("spring1");
repository.save(member2);
```

<br>

#### 단축키 - Ctrl + Shift + enter

```java
Member member2 = new 커서 Member();
```

- 커서가 여기 일때 위에 치면 바로 아래로 ㅋㅋ



<br>

#### 단축키 - Alt + enter

- static import, Create Test 등등

<br>

## - 스프링 빈과 의존 관계

![image-20230206103043406](Spring%20%ED%9A%8C%EC%9B%90%20%EA%B4%80%EB%A0%A8%20%EA%B0%84%EB%8B%A8%ED%95%9C%20%EB%B0%B1%EC%97%94%EB%93%9C%20%EA%B0%9C%EB%B0%9C.assets/image-20230206103043406.png)

컨트롤러에서 외부 요청을 받고, 서비스에서 비지니스 로직을 실행하고, 레포지토리에서 데이터를 저장하는 정형화된 패턴을 가지고 있다. 

따라서 스프링은 스프링 컨테이너에 스프링 빈을 등록하는데, 이 때 사용하는 어노테이션이 `Component`이다. `@Controller` 컨트롤러가 스프링 빈으로 자동 등록된 이유도 컴포넌트 스캔때문이다. `Component`를 포함하는 다음과 같은 어노테이션도 스프링 빈으로 자동 등록된다. (@Service, @Repository) 

> 참고: 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다(유일하게 하나만 등록해서 공유한다) 따라서 같은 스프링 빈이면 모두 같은 인스턴스다. 설정으로 싱글톤이 아니게 설정할 수 있지만, 특별한 경우를 제외하면 대부분 싱글톤을 사용한다.

<br>

### 컴포넌트 스캔과 자동 의존관계 설정

```java
controller/MemberController
    
@Controller
public class MemberController {
    
    private final MemberService memberService;
    
	@Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

- `@Controller` : 스프링 컨테이너가 이 어노테이션이 있으면 MemberController 객체를 생성하고 관리를 해줌, 이를 스프링 컨테이너에서 스프링 빈이 관리된다고 표현을 한다.
- `@Autowired` : 생성자에 `@Autowired`가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다. 이렇게 객체 의존 관계를 외부에서 넣어주는 것을 DI, 의존성 주입이라고 한다.
  -  이렇게 `@Autowired`를 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입한다. 생성자가 1개만 있으면 생략할 수 있다.

<br>

### 자바 코드로 직접 스프링 빈 등록하기

```java
// newproject
   
@Configuration
public class SpringConfig {
    
 @Bean
 public MemberService memberService() {
 return new MemberService(memberRepository());
 }
    
 @Bean
 public MemberRepository memberRepository() {
return new MemoryMemberRepository();
 }
}
```

- `@Configuration` : 스프링이 뜰 때 얘를 읽고, 어 이거는 스프링 빈에 등록하라는 뜻이네! 하면서 밑에 로직을 호출하면서 스프링 빈에 등록해줌

- 컨트롤러는 어쩔 수 없음 스프링이 관리하기 때문에.. 컴포넌트 스캔 방식으로 등록,

> 참고: 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용한다. 그리고 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록한다.

> 주의: @Autowired 를 통한 DI는 helloController , memberService 등과 같이 스프링이 관리하는 객체에서만 동작한다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않는다.

<br>

## - 회원 관리 예제 | 웹 MVC 개발

![image-20230206111222112](Spring%20%ED%9A%8C%EC%9B%90%20%EA%B4%80%EB%A0%A8%20%EA%B0%84%EB%8B%A8%ED%95%9C%20%EB%B0%B1%EC%97%94%EB%93%9C%20%EA%B0%9C%EB%B0%9C.assets/image-20230206111222112.png)

지금은 기본적으로 `@Controller`가 있는 템플릿이 없기 때문에, 정적파일을 렌더링한다. 하지만 아래와 같이 컨트롤러를 생성하면 스프링이 컨테이너에서 찾기 때문에, 정적 파일까지 가지 않는다!

```java
// controller/HomeController

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(){
        return "home";
    }
}
```



<br>

## - 스프링 DB 접근 기술

<hr>

#### 1. H2 DB 설치

[여기](https://www.h2database.com/html/download-archive.html)서 `1.4.200` 버전 설치

```bash
/bin

h2.bat  // 실행하면 아래와 같은 화면이 브라우저로 띄워짐
```



![image-20230206115104923](Spring%20%ED%9A%8C%EC%9B%90%20%EA%B4%80%EB%A0%A8%20%EA%B0%84%EB%8B%A8%ED%95%9C%20%EB%B0%B1%EC%97%94%EB%93%9C%20%EA%B0%9C%EB%B0%9C.assets/image-20230206115104923.png)

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



![image-20230207105938895](Spring%20%ED%9A%8C%EC%9B%90%20%EA%B4%80%EB%A0%A8%20%EA%B0%84%EB%8B%A8%ED%95%9C%20%EB%B0%B1%EC%97%94%EB%93%9C%20%EA%B0%9C%EB%B0%9C.assets/image-20230207105938895.png)

- `MemberRepository`의 새로운 구현체로 `JdbcMemberRepository`를 만들어서 주입한다. 객체지향의 장점! 아래와 같이 언제든지 새롭게 갈아끼울 수 있음

![image-20230207110414963](Spring%20%ED%9A%8C%EC%9B%90%20%EA%B4%80%EB%A0%A8%20%EA%B0%84%EB%8B%A8%ED%95%9C%20%EB%B0%B1%EC%97%94%EB%93%9C%20%EA%B0%9C%EB%B0%9C.assets/image-20230207110414963.png)

- 개방-폐쇄 원칙(OCP, Open-Closed Principle) 
  - 확장에는 열려있고, 수정, 변경에는 닫혀있다. 
- 스프링의 DI (Dependencies Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있다. 
- 회원을 등록하고 DB에 결과가 잘 입력되는지 확인하자. 
- 데이터를 DB에 저장하므로 스프링 서버를 다시 실행해도 데이터가 안전하게 저장된다.

<BR>

## 스프링 통합 테스트

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

## 스프링 JdbcTemplate

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

![image-20230207124810606](Spring%20%ED%9A%8C%EC%9B%90%20%EA%B4%80%EB%A0%A8%20%EA%B0%84%EB%8B%A8%ED%95%9C%20%EB%B0%B1%EC%97%94%EB%93%9C%20%EA%B0%9C%EB%B0%9C.assets/image-20230207124810606.png)

- 인터페이스를 통한 기본적인 CRUD 
- `findByName()` , `findByEmail()` 처럼 메서드 이름 만으로 조회 기능 제공 
- 페이징 기능 자동 제공

> 참고: 실무에서는 JPA와 스프링 데이터 JPA를 기본으로 사용하고, 복잡한 동적 쿼리는 Querydsl이라는 라이브러리를 사용하면 된다. Querydsl을 사용하면 쿼리도 자바 코드로 안전하게 작성할 수 있고, 동적 쿼리도 편리하게 작성할 수 있다. 이 조합으로 해결하기 어려운 쿼리는 JPA가 제공하는 네이티브 쿼리를 사용하거나, 앞서 학습한 스프링 JdbcTemplate를 사용하면 된다.

<br>

### Java 문법

public, private, static, final, 람다 뭔지 정확히