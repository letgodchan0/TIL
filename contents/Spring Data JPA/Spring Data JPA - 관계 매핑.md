# Spring Data JPA - 관계 매핑

[인프런 백기선님 참고](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)

<hr>

<br>

#### 관계에는 항상 두 엔티티가 존재한다.

- 둘 중 하나는 그 관계의 주인(owning)이고 
- 다른 쪽은 종속된(non-owning) 쪽.
- 해당 관계의 반대쪽 레퍼런스를 가지고 있는 쪽이 주인. 

<br>

#### 단방향에서의 관계의 주인은 명확하다. 

- 관계를 정의한 쪽이 그 관계의 주인! 

<br>

#### 단방향 @ManyToOne 

- 기본값은 FK 생성 

<br>

##### @ManyToOne 예시

```java
# entity/study

@Entity
public class Study {

    @Id @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    private Account owner;
}
```

- Study 테이블안에 **Account** 테이블의 **PK**를 참조하는 **FK** 컬럼을 생성해서 가지고 있게 된다.
- `owner`라고 줬지만 `owner_id`라고 생성이 되고 `owner_id`에 대한 **constraints** 가 **foreign key**로 잡힌다.



<br>

#### 단방향 @OneToMany 

- 기본값은 조인 테이블 생성 

<br>

##### - @OneToMany 예시

```java
# Account

@Entity
public class Account {

    @Id
    @GeneratedValue
    private Long id;
    @Column(nullable = false, unique = true)
    private String username;
    private String password;

    @OneToMany
    private Set<Study> studies = new HashSet<>();
}
```

- `account_studies` 라는 **Mapping** 테이블을 만든다.



<br>

#### 양방향 

- FK 가지고 있는 쪽이 오너 따라서 기본값은 @ManyToOne 가지고 있는 쪽이 주인. 
- 주인이 아닌쪽(@OneToMany쪽)에서 mappedBy 사용해서 관계를 맺고 있는 필드를 설정해야 한다.

<br>

#### 양방향 

- @ManyToOne (이쪽이 주인) 
-  @OneToMany(mappedBy) 
- 주인한테 관계를 설정해야 DB에 반영이 된다.

<br>

##### - 양방향 예시

```java
# Study

@Entity
public class Study {

    @Id @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    private Account owner;
}
```

```java
# Account

@Entity
public class Account {

    @Id
    @GeneratedValue
    private Long id;
    @Column(nullable = false, unique = true)
    private String username;
    private String password;

    @OneToMany(mappedBy = "owner")
    private Set<Study> studies = new HashSet<>();
}
```

- 관계를 정의한 필드를 `@OneToMany(mappedBy = "owner")` 이렇게 설정해줘야한다.
- **Study**의 **Account**가 `@ManyToOne`로 지정되어있어서 **Owner**이라는 것을 **Account studiesdp mappedBy**로 지정해서 알려줘야한다.
- **Account**가 **Study**에 종속된 관계로 적용됨 별도의 맵핑 테이블을 생성하지 않음
- 양방향 시에는 **Foreign Key**를 가진쪽이 **Owner** 임
- 가장 기본적인 양방향 맵핑방법