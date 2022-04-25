# 📒 Java Script - 데이터 타입, 연산자

<br>

## 데이터 타입

- 자바스크립트의 모든 값은 특정한 데이터 타입을 가짐
- 크게 `원시 타입`과 `참조 타입`으로 분류됨

![image-20220425205816639](Java%20Script%20-%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%ED%83%80%EC%9E%85,%20%EC%97%B0%EC%82%B0%EC%9E%90.assets/image-20220425205816639.png)

<hr>

| 원시 타입                              | 참조 타입                              |
| -------------------------------------- | -------------------------------------- |
| 객체(object)가 아닌 기본 타입          | 객체 (object) 타입의 자료형            |
| 변수에 해당 타입의 값이 담김           | 변수에 해당 객체의 참조 값이 담김      |
| 다른 변수에 복사할 때 실제 값이 복사됨 | 다른 변수에 복사할 때 참조 값이 복사됨 |

#### 1. 원시 타입

```javascript
let message = '안녕하세요'	// 1. message 선언 및 할당

let greeting = message	   // 2. greeting에 message 복사
console.log(greeting)	   // 3. '안녕하세요' 출력

message = 'Hello, world'   // 4. message 재할당
console.log(greeting)      // 5. '안녕하세요' 출력
```

#### 2. 참조 타입

```javascript
let message = ['안녕하세요']	   // 1. message 선언 및 할당

let greeting = message	   	  // 2. greeting에 message 복사
console.log(greeting)	   	  // 3. ['안녕하세요'] 출력

message[0] = 'Hello, world'   // 4. message 재할당
console.log(greeting)      	  // 5. ['Hello, world'] 출력
```



## 원시 타입 (Primitive type)

<hr>
💡숫자 (Number) 타입

```javascript
const a = 13			// 양의 정수
const b = -5			// 음의 정수
const c = 3.14			// 실수
const d = 2.998e8		// 거듭 제곱
const e = Infinity		// 양의 무한대
const f = -Infinity		// 음의 무한대
const g = NaN			// 산술 연산 불가
```

- 정수, 실수 구분 없는 하나의 숫자 타입
- 부동소수점 형식을 따름
- NaN (Not-A-Number)
  - 계산 불가능한 경우 반환되는 값, 근데 웃긴게 타입은 number 타입!
  - ex)  'PIZZA' / 1004 => NaN

<br>

### 💡문자열 (String) 타입

```javascript
const firstName = 'Brandan'
const lastName = 'Eich'
const fullName = `${firstName} ${lastName}`
```

- 텍스트 데이터를 나타내는 타입
- 16비트 유니코드 문자의 집합
- 작은따옴표 또는 큰따옴표 모두 가능
- `템플릿 리터럴` (Template Literal)
  - ES6부터 지원
  - 따옴표 대신 backtick(``)으로 표현
  - `${ expression }` 형태로 표현식 삽입 가능, 파이썬의 f스트링!!

<br>

### 💡undefined

```javascript
let firstName
console.log(firstName)	// undefined
```

- 변수의 `값이 없음`을 나타내는 데이터 타입
- 변수 선언 이후 직접 값을 할당하지 않으면, 자동으로 `undefined`가 할당됨

<br>

### 💡null

```javascript
let firstName = null
console.log(firstName)	// null

typeof null	// object
```

- 변수의 `값이 없음`을 의도적으로 표현할 때 사용하는 데이터 타입
- `typeof`연산자 : 자료형 평가를 위한 연산자
  - [참고] null 타입은 `ECMA`에 의해 따라 원시 타입에 속하지만, typeof 연산자의 결과는 객체(object)로 표현

| undefined                                                    | null                                 |
| ------------------------------------------------------------ | ------------------------------------ |
| 빈 값을 표현하기 위한 데이터 타입                            | 빈 값을 표현하기 위한 데이터 타입    |
| 변수 선언 시 아무 값도 할당하지 않으면, 자바스크립트가 자동으로 할당 | 개발자가 의도적으로 필요한 경우 할당 |
| typeof 연산자의 결과는 undefined                             | typeof 연산자의 결과는 object        |

<br>

### 💡Boolean 타입

- 논리적 참 또는 거짓을 나타내는 타입
- `true` 또는 `false` 로 표현
- 조건문 또는 반복문에서 유용하게 사용
  - [참고] 조건문 또는 반복문에서 boolean이 아닌 데이터 타입은 자동 형변환 규칙에 따라 true 또는 false로 변환됨

- 자동 형변환 정리

| 데이터 타입 |    거짓    |        참        |
| :---------: | :--------: | :--------------: |
|  Undefined  | 항상 거짓  |        X         |
|    Null     | 항상 거짓  |        X         |
|   Number    | 0, -0, NaN | 나머지 모든 경우 |
|   String    | 빈 문자열  | 나머지 모든 경우 |
|   Object    |     X      |     항상 참      |





## 연산자

<hr>
### 🔔 할당 연산자

```javascript
let x = 0

x += 10
console.log(x)	// 10

x -= 3
console.log(x)	// 7

x *= 10
console.log(x)	// 70

x /= 10
console.log(x)	// 7

x++				// += 연산자와 동일
console.log(x)	// 8

x--				// -= 연산자와 동일
console.log(x)	// 7
```

- 오른쪽에 있는 피연산자의 평가 결과를 왼쪽 피연산자에 할당하는 연산자
- 다양한 연산자에 대한 단축 연산자 지원
- [참고] Increment 및 Decrement 연산자
  - Increment(++) : 피연산자의 값을 1 증가시키는 연산자
  - Decrement(--) : 피연산자의 값을 1 감소시키는 연산자

<br>

### 🔔 비교 연산자

```javascript
const numOne = 1
const numTwo = 100
console.log(numOne < numTwo)	// true

const charOne = 'a'
const charTwo = 'z'
console.log(charOne < charTwo)	// false
```

- 피연산자들(숫자, 문자, Boolean 등)을 비교하고 결과값을 boolean으로 반환하는 연산자
- 문자열은 유니코드 값을 사용하며 표준 사전 순서를 기반으로 비교
  - ex) 알파벳끼리 비교할경우
    - 알파벳 순서상 후순위가 더 크다
    - 소문자가 대문자보다 더 크다

<br>

### 🔔 동등 비교 연산자(==)

```javascript
const a = 1004
const b = '1004'
console.log(a == b)	// true

const c = 1
const d = true
console.log(c == d)	// true

// 자동 타입 변환 예시
console.log(a + b)	// 10041004
console.log(c + d)	// 2
```

- 두 피연산자가 같은 값으로 평가되는지 비교 후 boolean 값을 반환
- 비교할 때 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교
- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별
- 예상치 못한 결과가 발생할 수 있으므로 특별한 경우를 제외하고 사용하지 않음

<br>

### 🔔 일치 비교 연산자(===)

```javascript
const a = 1004
const b = '1004'
console.log(a === b)	// true

const c = 1
const d = true
console.log(c === d)	// false
```

- 두 피연산자 같은 값으로 평가되는지 비교 후 boolean 값을 반환
- `엄격한 비교`가 이루어지며 암묵적 타입 변환이 발생하지 않음
  - 엄격한 비교 : 두 비교 대상의 타입과 값 모두 같은지 비교하는 방식
- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별

<br>

### 🔔 논리 연산자

```javascript
// and 연산
console.log(true && false)	// fasle
console.log(true && true)	// true
console.log(1 && 0)			// 0
console.log(4 && 7)			// 7
console.log('' && 5)		// ''

// or 연산
console.log(true || false)	// true
console.log(false || false)	// false
console.log(1 || 0)			// 1
console.log(4 || 7)			// 4
console.log('' || 5)		// 5

// not 연산
console.log(!true)			// false
console.log(!'Bongour!')	// false
```

- 세 가지 논리 연산자로 구성
  - and 연산은 `&&` 연산자를 이용
  - or 연산은 `||` 연산자를 이용
  - not 연산은 `!` 연산자를 이용
- 단축 평가 지원
  - ex) `false && true` => false
  - ex) `true || false` => true

<br>

### 🔔 삼항 연산자

```javascript
console.log(true ? 1 : 2)	// 1
console.log(false ? 1 : 2)	// 2

const result = Math.PI > 4 ? 'Yes' : 'No'
console.log(result)	// No
```

- 세 개의 피연산자를 사용하여 조건에 따라 값을 반환하는 연산자
- 가장 왼쪽의 조건식이 참이면 콜론(:) 앞의 값을 사용하고 그렇지 않으면 콜론(:) 뒤의 값을 사용
- 삼항 연산자의 결과 값이기 때문에 변수에 할당 가능
- [참고] 한 줄에 표기하는 것을 권장



## 📮 Quiz

1. 자바 스크립트의 데이터 타입은 크게 원시 타입과 참조 타입으로 분류된다. (T)
2. Number 타입은 0을 제외한 모든 경우 참으로 자동 형변환이 이뤄진다. (F)
   - Number 타입은 0, -0, NaN을 제외한 모든 경우 참으로 형변환 된다.
3. 일치 비교 연산자(===)는 자동 형변환을 통해 타입과 값이 같은지 판별한다. (F)
   - 일치 비교 연산자는 자동 형변환이 일어나지 않는다. 













