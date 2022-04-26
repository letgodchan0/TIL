# 📒 JavaScript - 배열

> 배열의 정의와 특징

```javascript
const numbers = [1, 2, 3, 4, 5]

console.log(numbers[0])		// 1
console.log(numbers[-1])	// undefined
console.log(numbers.length)	// 5
```

```javascript
const numbers = [1, 2, 3, 4, 5]

console.log(numbers[numbers.length - 1])	// 5
console.log(numbers[numbers.length - 2])	// 4
console.log(numbers[numbers.length - 3])	// 3
console.log(numbers[numbers.length - 4])	// 2
console.log(numbers[numbers.length - 5])	// 1
```

- 키와 속성들을 담고 있는 참조 타입의 객체
- 순서를 보장하는 특징이 있음
- 주로 대괄호를 이용하여 생성하고, 0을 포함한 양의 정수 인덱스로 특정 값에 접근 가능
- 배열의 길이는 array.length 형태로 접근 가능
  - [참고] - 배열의 마지막 원소는 array.length - 1로 접근

<br>

### 💡 reverse

```javascript
const numbers = [1, 2, 3, 4, 5]
numbers.reverse()
console.log(numbers)	// [5, 4, 3, 2, 1]
```

- 원본 배열의 요소들의 순서를 반대로 정렬

<br>



### 💡 push & pop

```javascript
const numbers = [1, 2, 3, 4, 5]

numbers.push(100)
console.log(numbers)	// [1, 2, 3, 4, 5, 100]

numbers.pop()
console.log(numbers)	// [1, 2, 3, 4, 5]
```

- `push`
  - 배열의 가장 뒤에 요소 추가
- `pop`
  - 배열의 마지막 요소 제거

<br>

### 💡 unshift & shift

```javascript
const numbers = [1, 2, 3, 4, 5]

numbers.unshift(100)
console.log(numbers)	// [100, 1, 2, 3, 4, 5]

numbers.shift()
console.log(numbers)	// [1, 2, 3, 4, 5]
```

- `unshift`
  - 배열의 가장 앞에 요소 추가
- `shift`
  - 배열의 첫번째 요소 제거

<br>

### 💡 includes

```javascript
const numbers = [1, 2, 3, 4, 5]

console.log(numbers.includes(1))	// true
console.log(numbers.includes(100))	// false
```

- 배열에 특정 값이 존재하는지 판별 후 참 또는 거짓 반환

<br>

### 💡 indexOf

```javascript
const numbers = [1, 2, 3, 4, 5]
let result

result = numbers.indexOf(3)	// 2
console.log(result)

result = numbers.indexOf(100)	// -1
console.log(result)
```

- 배열에 특정 값이 존재하는지 확인 후 가장 첫 번째로 찾은 요소의 인덱스 반환
- 만약 해당 값이 없을 경우 -1 반환

<br>

### 💡 join

```javascript
const numbers = [1, 2, 3, 4, 5]
let result

result = numbers.join()	// 1,2,3,4,5
console.log(result)

result = numbers.join('')	// 12345
console.log(result)

result = numbers.join(' ')	// 1 2 3 4 5
console.log(result)

result = numbers.join('-')	// 1-2-3-4-5
console.log(result)
```

- 배열의 모든 요소를 연결하여 반환
- `separator(구분자)`는 선택적으로 지정 가능하며, 생략 시 쉼표를 기본 값으로 사용!

<br>

### 💡 Spread operator

```javascript
const array = [1, 2, 3]
const newArray = [0, ...array, 4]

console.log(newArray)	// [0, 1, 2, 3, 4]
```

- `spread operator(...)`를 사용하면 배열 내부에서 배열 전개 가능.
- ES5까지는 Array.concat() 메서드를 사용. 
- 얕은 복사에 활용 가능

<br>

## 📋 Array Helper Methods

```python
# callback 함수 예시

def add(a, b):
    return a+b

def practice(x, y, func):
    return x + y - func

# 여기서 practice의 인자로 들어가는 add 함수가 callback 함수
practice(4,5, add(a,b))
```

- 배열을 순회하며 특정 로직을 수행하는 메서드
- 메서드 호출 시 인자로 callback 함수*를 받는 것이 특징
  - callback함수 : 어떤 함수의 내부에서 실행될 목적으로 인자로 넘겨 받는 함수를 말함

<br>

### 💡 forEach

```javascript
array.forEach((element, index, array) =>{
    // do something
})

const fruits = ['딸기', '수박', '사과', '체리']

fruits.forEach((fruit, index) => {
    console.log(fruit, index)
    // 딸기 0
    // 수박 1
    // 사과 2
    // 체리 3
})
```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
- 콜백 함수는 3가지 매개변수로 구성
  - element : 배열의 요소
  - index : 배열 요소의 인덱스
  - array : 배열 자체
- 반환 값(return)이 없는 메서드!!!



<br>

### 💡 map

```javascript
array.map((element, index, array) => {
    // do something
})

const numbers = [1, 2, 3, 4, 5]

const doubleNums = numbers.map((num) => {
    return num * 2
})

console.log(doubleNums)	// [2, 4, 6, 7, 10]
```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
- 콜백 함수의 반환 값을 요소로 하는 새로운 배열 반환
- 기존 배열 전체를 다른 형태로 바꿀 때 유용

<br>



### 💡 filter

```javascript
array.filter((element, index, array) => {
    // do something
})

const numbers = [1, 2, 3, 4, 5]

const oddNums = numbers.filter((num, index) => {
    return num % 2
})

console.log(oddNums)	// [1, 3, 5]
```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
- 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환
- 기존 배열의 요소들을 필터링 할 때 유용

<br>

### 💡 reduce

```javascript
array.reduce((acc, element, index, array)) => {
    // do something
}, initiaValue)

const numbers = [1, 2, 3]

const result = numbers.reduce((acc, num) => {
    return acc + num
}, 0)

console.log(result) // 6
```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
- 콜백 함수의 반환 값들을 하나의 값(acc)에 누적 후 반환
- reduce 메서드의 주요 매개변수
  - acc : 이전 callback 함수의 반환 값이 누적되는 변수
  - initialValue(optional) : 최초 callback 함수 호출 시 acc에 할당되는 값, default 값은 배열의 첫번째 값
- [참고] : 빈 배열의 경우 initialValue를 제공하지 않으면 에러 발생 

<br>

### 💡 find

```javascript
array.find((element, index, array)) {
    // do something
}

const avenvers = {
    { name: 'Tony Stark', age: 45 },
      { name: 'Steve Rogers', age: 32 },
      { name: 'Thor', age: 40 },
}

const reuslt = avengers.find(avenger) => {
    return avenger.name === 'Tony Stark'
}

console.log(result)	// {name: 'Tony Stark, age: 45'}
```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
- 콜백 함수의 반환 값이 참이면, 조건을 만족하는 첫번째 요소를 반환
- 찾는 값이 배열에 없으면 undefined 반환

<br>



### 💡 some

```javascript
array.some((element, index, array) => {
    // do something
})

const numbers = [1, 3, 5, 7, 9]

const hasEvenNumber = numbers.some((num) => {
    return num % 2 === 0
})
console.log(hasEvenNumber)	// false

const hasOddNumber = numbers.some((num) => {
    return num & 2
})
console.log(hasOddNumber)	// true
```

- 배열의 요소 중 하나라도 주어진 판별 함수를 통과하면 참을 반환
- 모든 요소가 통과하지 못하면 거짓 반환
- 빈 배열은 항상 거짓 반환, 파이썬 if any

<br>



### 💡 every

```javascript
array.every((element, index, array) => {
    // do something
})

const numbers = [2, 4, 6, 8, 10]

const isEveryNumberEven = numbers.every((num) => {
    return num % 2 === 0
})
console.log(isEveryNumberEven)	// true

const isEveryNumberOdd = numbers.every((num) => {
    return num % 2
})
console.log(isEveryNumberOdd)	// false
```

- 배열의 모든 요소가 주어진 판별 함수를 통과하면 참을 반환
- 하나의 요소라도 통과하지 못하면 거짓 반환
- 빈 배열은 항상 참 반환, 파이썬의 if all

<br>



### 📮 Quiz

1. 배열은 원시 타입에 속하며 음수 인덱싱이 불가능하다는 특징이 있다. (F)
   - 자바스크립트의 배열은 참조 타입에 속한다.
2. forEach 메서드는 배열의 각 요소들을 모아서 새로운 배열을 반환한다. (F)
   - 배열의 각 요소들을 모아서 새로운 배열을 반환하는 메서드는 map 메서드이다.
3. 배열의 가장 마지막에 요소를 추가하는 메서드는 append이다. (F)
   - 배열의 가장 마지막 요소를 추가하는 메서드는 push이다. 