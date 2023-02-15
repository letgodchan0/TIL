# Spring Data JPA - Value 타입 매핑

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

### 엔티티 타입과 Value 타입 구분

- 식별자가 있어야 하는가
- 독립적으로 존재해야 하는가

<br>

다른 타입에 종속적인 타입을 **Value** 타입이라고 보면 된다.

- 기본 타입 (`String`, `Date`, `Boolean`, ...)
- **Composite Value** 타입
- Collection Value 타입
  - 기본 타입의 콜렉션
  - 컴포짓 타입의 콜렉션

<br>

### Composite Value 타입 맵핑

```java
# Address

@Embeddable
public class Address {
    private String street;
    private String city;
    private String state;
    private String zipCode;
}
```

```java
# Account
    
@Embedded
@AttributeOverrides({
    @AttributeOverride(name = "street", column = @Column(name = "home_street"))
})
private Address address;
```

- `@Embadable`: **Composite Value** 클래스에 지정하면 해당 클래스를 **Composite Value**로 만듬
- `@Embadded`: **Entity**에서 **Composite Value**로 지정한 클래스를 불러와 정의할 때 사용
- `@AttributeOverrides`: 여러 값을 오버라이딩 하기위한 그룹 어노테이션
- `@AttributeOverride`: 오버라이딩 하기 위해 사용

address 는 entity라고 하기에 조금 애매하다. 주소 자체가 독립적으로 사용할 상황이 아니라면, Account에 속한 data 중에 하나로 취급하는 것이기 때문에, address를 entity라고 보기엔 어렵다.