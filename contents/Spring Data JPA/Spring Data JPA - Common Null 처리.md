# Spring Data JPA - Common: Null 처리

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

### - Optional

``` java
Optional<Post> findById(Long id);
```

- 단일 값을 사용하는 경우 `Optional`로 처리하는 것을 추천
- 콜렉션은 `Null`을 리턴하지 않고, 비어있는 콜렉션을 리턴
- `Optional` 인터페이스가 제공하는 메서드를 사용해서 검사할 수 있음

<br>

### - Optional 메서드

- `isPresent()` : 값이 있는지 없는지 확인
- `orElse` : 없는 경우 다른 대체적인 인스턴스를 리턴하게 할 수도 있음
- `orElseThrow` : 없는 경우 예외를 던짐