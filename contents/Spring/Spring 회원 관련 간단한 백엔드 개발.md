

# 회원 관련 간단한 백엔드 개발

> 회원 관련 간단한 백엔드 개발 및 테스트 코드 작성 과정 기술해보기!

[소스 코드]()

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

#### 테스트 코드 간결하게

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

<br>

#### assertEquals()

```java
// when
Member result = repository.findByName("spring1").get();

// then
Assertions.assertThat(result).isEqualTo(member1);
```

- member1이라고 주어진 객체가 DB 상의 result와 동일한제 테스트 해주는 메서드

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



### Java 문법

public, private, static, final, 람다 뭔지 정확히