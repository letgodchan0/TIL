# 📒 Java Script - 조건문, 반복문

<br>



## 조건문

- `if` statement
  - 조건 표현식의 결과값을 Boolean 타입으로 변환 후 참/거짓을 판단
- `switch` statement
  - 조건 표현식의 결과값이 어느 값(case)에 해당하는지 판별
  - [참고] 주로 특정 변수의 값에 따라 조건을 분기할 때 활용
    - 조건이 많아질 경우 `if`문 보다 가독성이 나을 수 있음

<br>

#### 💡 if statement

```javascript
const nation = 'Korea'

if (nation == 'Korea') {
  console.log('안녕하세요!')
} else if (nation == 'France') {
  console.log('Bonjour!')
} else {
  console.log('Hello!')
}
```

- 조건은 소괄호(condition) 안에 작성
- 실행할 코드는 중괄호 {} 안에 작성
- 블록 스코프 생성

<br>

#### 💡 switch statement

```javascript
const nation = 'Korea'

switch(nation) {
 	case 'Korea': {
        console.log('안녕하세요!')
        break
 }
    case 'France': {
        console.log('Bonjour!')
        break
    }
    default : {
        console.log('Hello!')
    }
}
```

- 표현식(nation)의 결과값을 이용한 조건문
- 표현식의 결과값과 case문의 오른쪽 값을 비교
- break 및 default문은 선택적으로 사용 가능!
- break문이 없는 경우 break문을 만나거나 default문을 실행할 때 까지 다음 조건문 실행
  - 쉽게 말해서 위에 코드에서 break문이 없다면 모든 case에서 출력이 된다!
- 블록 스코프 생성

<br>



## 반복문

- `while`
- `for`
- `for in`
  - 주로 객채(object)의 속성들을 순회할 때 사용
  - 배열도 순회 가능하지만 인덱스 순으로 순회한다는 보장이 없으므로 권장하지 않음
- `for of`
  - 반복 가능한(iterable) 객체를 순회하며 값을 꺼낼 때 사용
    - 반복 가능한(iterable) 객체 : Array, Map, Set, String 등

<br>

#### 🌻 while

```javascript
let i = 0

while (i < 6) {
    console.log(i)		// 0, 1, 2, 3, 4, 5
    i += 1
}
```

- 조건문이 참(true)인 동안 반복 시행
- 조건은 **소괄호** 안에 작성
- 실행할 코드는 **중괄호** 안에 작성

- 블록 스코프 생성

<br>



#### 🌻 for

```javascript
for (initialization; condition; expression) {
    // do something
}

for (let i = 0; i < 6; i++){
    console.log(i)	// 0, 1, 2, 3, 4, 5
}
```

- 세미콜론(:)으로 구분되는 세 부분으로 구성
- `initialization` - 최초 반복문 진입 시 1회만 실행되는 부분
- `condition` - 매 반복 시행 전 평가되는 부분
- `expression` - 매 반복 시행 이후 평가되는 부분
- 블록 스코프 생성

<br>



#### 🌻 for...in (객체 순회 적합)

```javascript
// object(객체) => key-value로 이루어진 자료구조
const capitals = {
    korea : 'seoul',
    france: 'paris',
    USA : 'washingtion D.C.'
}

for (let capital in capitals) {
    console.log(capital)	// korea, france, USA
}
```

- 객체(object)의 속성(key)들을 순회할 때 사용
- 배열도 순회 가능하지만 권장하지 않음!!!!!
- 실행할 코드는 중괄호 안에 작성
- 블록 스코프 생성

<br>

#### 🌻 for...of (배열 순회 적합)

```javascript
const fruits = ['딸기', '바나나', '메론']

for (let fruit of fruits) {
    fruit = fruit + '!'		// 딸기! 바나나! 메론!
    console.log(fruit)
}

for (const fruit of fruits) {
    // const로 선언해서 fruit 재할당 불가
    console.log(fruit)
}
```

- 반복 가능한(iterable) 객체를 순회하며 값을 꺼낼 때 사용
- 실행할 코드는 중괄호 안에 작성
- 블록 스코프 생성

- [참고] object를 `for...of`로 돌리면 순회가능하지 않기 때문에 에러가 발생한다

  ```javascript
  const capitals = {
      korea : 'seoul',
      france: 'paris',
      USA : 'washingtion D.C.'
  }
  
  for (let capital of capitals) {
      console.log(capital)	
      // Uncaught TypeError: capitals is not iterbale
  }
  ```

  

### 📮 Quiz

1. while문은 break문을 필수적으로 작성해야 한다. (F)
   - while문에서 break문은 선택적으로 작성
2. for...of 구문은 객체(object)의 속성값 순회에 유용하다. (F)
   - for...of 구문은 객체(object) 순회에 사용할 수 없으며, 객체(object)의 속성값 순회에 유용한 구문은 for...in에 해당한다.
3. for...in 구문은 반복 가능한 객체(iterable)의 순회에만 사용된다.
   - for...in은 반복 가능한 객체를 순회 뿐만 아니라 객체(object) 순회에도 사용할 수 있으며, 객체의 속성값 순회에 더 유용하다. 