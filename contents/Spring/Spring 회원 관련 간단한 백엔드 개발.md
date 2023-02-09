

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
