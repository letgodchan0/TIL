# Spring Data JPA - Fetch

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

> 연간관계에 대한 정보를 어떻게 가져올지 설정하는 것.. 지금(Eager)? 나중에(Lazy)?

- `@OneToMany`의 기본값은 Lazy
  - 기본적으로 해당 **Entity**의 정보를 가져올때 **Lazy**가 적용된 `@OneToMany` 관계의 **Entity**의 정보를 가져오지는 않는다.
  - 얼마나 많이 있을 지도 모르고 사용하지도 않을 값들을 다 가져오면 객체에 불필요한 정보를 로딩할 수 있기 때문에!
- `@ManyToOne`의 기본값은 Eager
  - 해당 **Entity**의 정보를 가져올때 **Eager**로 설정된 `@ManyToOne` 관계의 **Entity**의 정보도 같이 가져옴

<br>

Fetch를 잘 조정해야 성능을 향상시킬 수 있다!