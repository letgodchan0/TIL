

# 회원 관련 간단한 백엔드 개발



`alt` + `insert`  -> Getter and Setter로 하면 걍 만들어준다. 인텔리제이 ㄷㄷ





optional : ㅇ가져온느 값이 null일 수 있으니까, null을 그대로 반환하는 방법 대신 optional로 감싸서 반환할려구

​	Optional.ofNullable(store.get(id)) 이런식으로 null이 올 수도 있는 것



interface 상속받는 구현체 생성할 때도 `alt` `insert`하면 method 자동으로 만들어줌 ㄷㄷ

```java
// 자바 람다인, filter로 이름 같은애 찾으면 반환, findAny()가 하나라도 찾으면 반환
store.values().stream()
    .filter(member -> member.getName().equals(name))
    .findAny()
```



테스트 케이스 작성



public, private, static, final 뭔지 정확히



중간에 enter 치고 싶을때 `ctrl` + `shift` +`enter`



테스트 방법 중 Assertions.assertEquals()



```java
// shift + f6 하면 그 member2가 다 드래그 되서 한번에 바꿀 수 있음
Member member2 = new Member();
member2.setName("spring1");
repository.save(member2);
```



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

