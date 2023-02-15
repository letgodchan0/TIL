# Spring Data JPA - 엔티티 매핑

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

도메인 모델을 만들었으면, 테이블의 매핑시키여 하는데 크게 2가지 방법이 있다.

1. `xml` 방식
2. `Annotation` 방식 - 대부분이 이 방식 사용, 소스 코드와 가깝고 가시적인 효과



<br>

### @Entity

- **엔티티**는 객체 세상에서 부르는 이름
- 기본적으로 `@Table` 을 포함하고 있음
- 보통 클래스와 같은 이름을 사용하기 때문에 값을 변경하지 않음

> 아래와 같은 형식으로 이름을 클래스와 다르게 줄 수 있음

```java
@Entity(name = "myAccount")
```

> 엔티티의 이름은 **JPQL**에서 쓰임

<br>

### @Table

- **릴레이션** 세상에서 부르는 이름
- `@Entity`의 이름이 기본값

> 아래와 같은 형식으로 **Table**명을 다르게 줄 수 있다.

```java
@Table(name = "myTable")
```

> 테이블의 이름은 **SQL**에서 쓰임



<br>

### @Id

- 엔티티의 **Primary key**를 맵핑할 때 사용
- 자바의 모든 `primitive` 타입과 그 랩퍼 타입을 사용할 수 있음
  - `Date`랑 `BigDecimal`, `BigInteger`도 사용 가능
- 복합키를 만드는 맵핑하는 방법도 있지만 그건 논외로..!!



<br>

### @GeneratedValue

- 주키의 생성 방법을 매핑하는 애노테이션
- 생성 전략과 생성기를 설정할 수 있다

> 다른 전략을 임의로 지정할 수 있음 보통은 기본값을 사용

```java
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = ...)
```

- 기본 전략은 **AUTO**: 사용하는 **DB**에 따라 적절한 전략 선택
- **TABLE**, **SEQUENCE**, **IDENTITY** 중 하나



<br>

### @Column

- `unique`: `unique` 설정여부 `true`, `false를` 줄 수 있음
- `nullable`: `null` 사용여부 `true`, `false`를 줄 수 있음
- `length`
- `columnDefinition`: 반드시 **SQL**을 사용해 명시해주어야 할때 설정

<br>

### @Temporal

**JPA 2.2** 부터는 `LocalDate`, `LocalDateTime` 나 **Java8**의 새로운 `Date` 객체드를 기본으로 지원하기 때문에 애노테이션을 지정하지 않아도됨

- 현재 **JPA 2.1**까지는 `Date`와 `Calendar`만 지원

<br>

### @Transient

컬럼으로 매핑하고 싶지 않은 멤버 변수에 사용

<br>

### application.properties

```properties
spring.jpa.show-sql=true
```

- **JPA**에서 자동으로 생성해서 처리한 **SQL**을 **Console**에서 볼 수 있음
- 현재는 매핑되는 값은 보이지 않는데 특별한 **Logger**설정에 의해 값도 보이게 할 수 있다

```properties
spring.jpa.properties.hibernate.format_sql=true
```

- 위에서 보여주는 **SQL**좀 더 보기 좋게 **Console**에 출력해줌