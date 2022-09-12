# 🌱 정규화(Normalization)

[참고](https://gyoogle.dev/blog/computer-science/data-base/Normalization.html)

> 데이터의 중복을 줄이고 무결성을 향상, DB 저장 용량 효율적 관리 등 여러 목적을 달성하기 위해 관계형 데이터베이스를 정규화된 형태로 재디자인 하는 것!

##### 무결성 : 데이터의 정확성, 일관성, 유효성이 유지되는 것

<br>

### 목적

- 데이터의 중복을 없애면서 불필요한 데이터를 최소화시킨다.
- 무결성을 지키고, 이상 현상을 방지한다.
- 테이블 구성을 논리적이고 직관적으로 할 수 있다.
- 데이터베이스 구조 확장에 용이해진다.



<br>

## 제 1 정규화(1NF)

> 테이블 컬럼이 원자값(하나의 값)을 갖도록 테이블을 분리시키는 것

- 어떤 릴레이션에 속한 모든 도메인이 원자값만으로 되어 있어야 한다.
- 모든 속성에 반복되는 그룹이 나타나지 않는다.
- 기본키를 사용하여 관련 데이터의 각 집합을 고유하게 식별할 수 있어야 한다.

#### Customer 테이블

| Customer ID | First Name | Surname | Telephone Number             |
| ----------- | ---------- | ------- | ---------------------------- |
| 123         | Pooja      | Singh   | 456-456-4567, 192-122-1111   |
| 456         | San        | Zhang   | 182-929-2929, (555) 403-1659 |
| 789         | John       | Doe     | 759-808-1423                 |

현재 테이블은 전화번호를 여러개 가지고 있어 원자값이 아니기 때문에 1NF에 맞추기 위해 아래와 같이 분리할 수 있다.

| Customer ID | First Name | Surname | Telephone Number |
| ----------- | ---------- | ------- | ---------------- |
| 123         | Pooja      | Singh   | 456-456-4567     |
| 123         | Pooja      | Singh   | 192-122-1111     |
| 456         | San        | Zhang   | 182-929-2929     |
| 456         | San        | Zhang   | (555) 403-1659   |
| 789         | John       | Doe     | 759-808-1423     |

<br>

## 제 2정규화(2NF)

> 테이블의 모든 컬럼이 완전 함수적 종속을 만족해야한다. 쉽게 말하면, 테이블에서 기본키가 복합키(키1, 키2)로 묶여있을 때, 두 키 중 하나의 키만으로 다른 컬럼을 결정지을 수 있으면 안된다. 
>
> 기본키의 부분집합 키가 결정자가 되어선 안되다는 것

결정자 : 어떤 컬럼 값(=속성 값)은 다른 속성 값을 고유하게 결정지을 수 있는데 이러 속성을 결정자라고 한다.

#### Eletric Toothbrush 테이블

| Manufacturer | Model        | Model Full Name        | Manufacturer Country |
| ------------ | ------------ | ---------------------- | -------------------- |
| Forte        | X-Prime      | Forte X-Prime          | Italy                |
| Forte        | Ultraclean   | Forte Ultraclean       | Italy                |
| Dent-o-Fresh | EZbrush      | Dent-o-Fresh EZbrush   | USA                  |
| Kobayashi    | X-PrimeST-60 | Kobayashi X-PrimeST-60 | Japan                |
| Hoch         | Toothmaster  | Hoch Toothmaster       | Germany              |
| Hoch         | X-Prime      | Hoch X-Prime           | Germany              |

- `Manufacture`와 `Model`이 키가 되어 `Model Full Name`을 알 수 있다.
- `Manufacturer Country`는 `Manufacturer`로 인해 결정된다. (부분 함수 종속)
- 따라서, `Model`과 `Manufacturer Country`는 아무런 연관 관계가 없는 상황이다!

결국 완전 함수적 종속을 충족시키지 못하고 있는 테이블이다. 부분 함수 종속을 해결하기 위해 테이블을 아래와 같이 나누어서 2NF를 만족할 수 있다. 

#### Eletric Toothbrush Manufacturers

| Manufacturer | Manufacturer Country |
| ------------ | -------------------- |
| Forte        | Italy                |
| Dent-o-Fresh | USA                  |
| Kobayashi    | Japan                |
| Hoch         | Germany              |

#### Eletric Toothbrush Models

| Manufacturer | Model        | Model Full Name        |
| ------------ | ------------ | ---------------------- |
| Forte        | X-Prime      | Forte X-Prime          |
| Forte        | Ultraclean   | Forte Ultraclean       |
| Dent-o-Fresh | EZbrush      | Dent-o-Fresh EZbrush   |
| Kobayashi    | X-PrimeST-60 | Kobayashi X-PrimeST-60 |
| Hoch         | Toothmaster  | Hoch Toothmaster       |
| Hoch         | X-Prime      | Hoch X-Prime           |

<br>

## 제 3정규화(3NF)

> 2NF가 진행된 테이블에서 이행적 종속을 없애기 위해 테이블을 분리하는 것
>
> 아래 두가지 조건을 만족시켜야 한다.
>
> - 릴레이션이 2NF에 만족한다.
> - 기본키가 아닌 속성들은 기본키에 의존한다. 

이행적 종속 : A -> B, B -> C이면, A -> C가 성립된다.

#### Tournament Winners

| Tournament           | Year | Winner         | Winner Date of Birth |
| -------------------- | ---- | -------------- | -------------------- |
| Indiana Invitational | 1998 | Al Fredrickson | 21 July 1975         |
| Cleveland Open       | 1999 | Bob Albertson  | 28 September 1968    |
| Des Moines Masters   | 1999 | Al Fredrickson | 21 July 1975         |
| Indiana Invitational | 1999 | Chip Masterson | 14 March 1977        |

현재 테이블에서는 `Tournament`와 `Year`이 기본키다. `Winner`는 이 두 복합키를 통해 결정된다. 하지만 `Winner Date of Birth`는 기본키가 아닌 `Winner`에 의해 결정되고 있다. 이는 3NF를 위반하고 있으므로 아래와 같이 분리해야 한다. 

#### Tournament Winners

| Tournament           | Year | Winner         |
| -------------------- | ---- | -------------- |
| Indiana Invitational | 1998 | Al Fredrickson |
| Cleveland Open       | 1999 | Bob Albertson  |
| Des Moines Masters   | 1999 | Al Fredrickson |
| Indiana Invitational | 1999 | Chip Masterson |

#### Winner Dates of Birth

| Winner         | Date of Birth     |
| -------------- | ----------------- |
| Chip Masterson | 14 March 1977     |
| Al Fredrickson | 21 July 1975      |
| Bob Albertson  | 28 September 1968 |

