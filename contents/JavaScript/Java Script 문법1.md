# 📒 Java Script - 변수와 식별자

<br>



## 💡 식별자 정의와 특징

- 식별자는 변수를 구분할 수 있는 변수명을 말함
- 식별자는 반드시 문자, 달러($) 또는 밑줄(_)로 시작
- 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작
- 예약어 사용 불가능 (for, if, function) 



### 식별자 작성 스타일

<hr>

- 카멜 케이스 
  - 변수, 객체, 함수에 사용
  - 두 번째 단어의 첫 글자부터 대문자

```javascript
// 변수
let dog
let variableName

// 객체
const userInfo = { name: 'Chan', age: 20}

// 함수
function add() {}
function getName() {}
```

<br>

- 파스칼 케이스
  - 클래스, 생성자에 사용
  - 모든 단어의 첫 번째 글자를 대문자로 작성

```javascript
// 클래스
class User {
    constructor(options) {
        this.name = options.name
    }
}

// 생성자 함수
function User(options) {
    this.name = options.name
}
```

<br>

- 대문자 스네이크 케이스
  - 상수(개발자의 의도와 상관없이 변경될 가능성이 없는 값)에 사용
  - 모든 단어 대문자 작성 & 단어 사이에 언더스코어 삽입

```javascript
// 값이 바뀌지 않을 상수
const API_KEY = 'my-key'
const PI = Math.PI

// 재할당이 일어나지 않는 변수
const numbers = [1, 2, 3]
```

<br>



## 💡 변수 선언 키워드 (let, const)

| let                                | const                                   |
| ---------------------------------- | --------------------------------------- |
| 재할당 할 예정인 변수 선언 시 사용 | 재할당 할 예정이 없는 변수 선언 시 사용 |
| 변수 재선언 불가능                 | 변수 재선언 불가능                      |
| 블록 스코프                        | 블록 스코프                             |

![image-20220425202950870](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425202950870.png)

![image-20220425203011947](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425203011947.png)

![image-20220425203207414](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425203207414.png)



```javascript
[참고] 선언, 할당, 초기화

let foo				// 선언			=> 변수를 생성하는 행위 또는 시점
console.log(foo)	// undefined

foo = 11			// 할당			=> 선언된 변수에 값을 저장하는 행위 또는 시점
console.log(foo)	// 11

let bar = 0			// 선언 + 할당		=> 초기화 - 선언된 변수에 처음으로 값을 저장하는 행위 또는 시점
console.log(bar)	// 0
```



## 💡 변수 선언 키워드 'var'

- `var`로 선언한 변수는 재선언 및 재할당 모두 가능

  ```javascript
  var number = 10	// 1. 선언 및 초기값 할당
  var number = 50 // 2. 재할당
  
  console.log(number)  // 50
  ```

- ES6 이전에 변수를 선언할 때 사용되던 키워드

- `호이스팅` 되는 특성으로 인해 예기치 못한 문제 발생 가능

  - 따라서 ES6 이후부터는 var 대신 const와 let을 사용하는 것을 권장

  ![image-20220425204118008](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425204118008.png)

- 함수 스코프

  ![image-20220425204053859](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425204053859.png)



## 📮 let, const, var 비교

| 키워드 | 재선언 | 재할당 |   스코프    |     비고     |
| :----: | :----: | :----: | :---------: | :----------: |
|  let   |   X    |   O    | 블록 스코프 | ES6부터 도입 |
| const  |   X    |   X    | 블록 스코프 | ES6부터 도입 |
|  var   |   O    |   O    | 함수 스코프 |    사용 x    |

