# 📒 JavaScript - Object (객체)

<br>



### 📚 객체의 정의와 특징

```javascript
const me ={
    name: 'jack',
    phoneNumber: '01012345678',
    'samsung product': {
        buds: 'Galaxy Buds pro',
        galaxy: 'Galaxy s20',
    },
}

console.log(me.name)	// jack
console.log(me.phoneNumber)		// 01012345678
console.log(me['samsung product'])		// {buds: 'Galaxy Buds pro', galaxy: 'Galaxy s20'}
console.log(me['samsung product'].buds)	// Galaxy Buds pro
```

- 객체는 속성의 집합이며, 중괄호 내부에 key와 value의 쌍으로 표현
- `key`는 문자열 타입*만 가능
  - [참고] `key` 이름에 띄어쓰기 등의 구분자가 있으면 따옴표로 묶어서 표현
- `value`는 모든 타입(함수포함) 가능
- 객체 요소 접근은 점 또는 대괄호로 가능
  - [참고] `key` 이름에 띄어쓰기 같은 구분자가 있으면 대괄호 접근만 가능

<br>

### 📚 객체와 메서드

```javascript
const me = {
    firstName: 'John',
    lastName: 'Doe',
    
    fullName: this.firstName + this.lastName,
    
    getFullName: function() {
        return this.firstName + this.lastName
    }
}
```

- 메서드는 객체의 속성이 참조하는 함수
- `객체.메서드명() 으로 호출 가능`
- 메서드 내부에서는 `this` 키워드가 객체를 의미함
  - fullName은 메서드가 아니기 때문에 정상출력되지 않음(NaN)
  - getFullName은 메서드이기 때문에 해당 객체의 firstName과 lastName을 정상적으로 이어서 반환

<br>

## 📩 객체 관련 ES6 문법 익히기

<br>

### 📚 ES6 문법 (1) - 속성명 축약

```javascript
const books= ['Learning JS', 'learning Python']
const magazines = ['Vogue', 'Science']

//ES6+
const bookShop = {
    books,			// books : books,
    magazines,		// magazines: magazines, ES5 형식
}
console.log(bookShop)
```

- 객체를 정의할 때 `key`와 할당하는 변수의 이름이 같으면 예시와 같이 축약 가능

### 📚 ES6 문법 (2) - 메서드명 축약 (shorthand)

```javascript
const obj = {
    // greeting: function()
    greeting(){
        console.log('Hi')
    }
}
obj.greeting()	// Hi
```

- 메서드 선언 시 function 키워드 생략 가능!!

<br>

### 📚 ES6 문법 (3) - 계산된 속성

```javascript
const key = 'regions'
const value = ['서울', '광주', '대전', '구미']

const nation = {
    [key]: value,
}

console.log(nation)				// { regions: Array(4) }
console.log(nation.regions)		// ['서울', '광주', '대전', '구미']
```

- 객체를 정의할 때 key의 이름을 표현식으로 이용하여 동적으로 생성 가능

<br>

### 📚 ES6 문법 (4) - 구조 분해 할당

```javascript
const userInfo = {
    name: 'kim',
    userId: 'play',
    email: 'play@gmail.com'
}

const name = userInfo.name		=>	const { name } = userInfo.name
const userId = userInfo.userId	=>	const { userId } = userInfo.userId
const email = userInfo.email	=>	const { email } = userInfo.email

// 여러개도 가능
const {name, userId} = userInfo
```

<br>

### 📚 ES6 문법 (5) - Spread operator

```js
const obj = {b: 2, c: 3, d: 4}
const newObj = {a: 1, ...obj, e: 5}

console.log(newObj)	// {a:1, b:2, c:3, d:4, e:5}
```

<br>

## 📣 Json 

```js
// Object => JSON

const jsonData =JSON.stringify({
    cofee: 'Americano',
    iceCream: 'Cookie and cream',
})

console.log(jsonData)			// {"cofee":"Americano","iceCream":"Cookie and cream"}
console.log(typeof jsonData)	// string
```

- `JSON.stringify()`
  - 자바스크립트 객체를 JSON 타입으로!

```js
// JSON => Object

const jsonData =JSON.stringify({
    cofee: 'Americano',
    iceCream: 'Cookie and cream',
})

const parseData = JSON.parse(jsonData)

console.log(parseData)			// {cofee: 'Americano', iceCream: 'Cookie and cream'}
console.log(typeof parseData)	// object
```

- `JSON.parse()`
  - JSON을 자바스크립트 객체로!

<br>

## 📮 Quiz

1. 객체의 KEY 값은 원시 타입만 사용 가능하다. (F)
   - 객체의 key 값은 문자열 타입만 사용 가능하다.
2. 객체 정의 시 key 값과 key에 할당하는 변수의 이름이 같으면 축약 가능하다. (T)
3.  JSON 데이터는 문자열 타입으로 표현된다. (T)

[참고] - 배열은 인덱스를 키로 갖는 객체!!

<br>

### 💡 this

> class 내부에서는 객체를 가리키지만 기본적으로 최상위 객체인 window를 가리킴

```js
const obj = {
    PI: 3.14
    radiuses: [1, 2, 3, 4, 5],
        printArea: function () {
            this.radiuses.forEach(function (r) {
                console.log(this.PI * r * r)
            }.bind(this))                               
        },
}
```

- `this.radiuses`는 메서드 (객체.메서드명()으로 호출 가능) 소속이기 때문에 정상적으로 접근 가능
- `forEach`의 콜백 함수의 경우 메서드가 아님 (객체.메서드명()으로 호출 불가능)
- 따라서 콜백 함수 내부의 `this`는 `window`가 되어 `this.PI`는 정상적으로 접근 불가능
- 이 콜백 함수 내부에서 `this.PI`에 접근하기 위해서 함수객체 `.bind(this)` 메서드를 사용.
- 이 번거로운 `bind` 과정을 없앤 것이 바로 화살표 함수!!!!!!!!!!!!!!!!!

```js
const obj = {
    PI: 3.14
    radiuses: [1, 2, 3, 4, 5],
        printArea: function () {
            this.radiuses.forEach((r) => {
                console.log(this.PI * r * r)
            })
        },
}
```

- 함수 내부에 this 키워드가 존재할 경우
  - 화살표 함수와 function 키워드로 선언한 함수가 다르게 동작
- 함수 내부에 this 키워드가 존재하지 않을 경우
  - 완전히 동일하게 동작

<br>

## 💡 lodash

```html
<body>
  <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
  <script>
      
      _.sample([1, 2, 3, 4]) // 3 (random 1 element)
      _.sampleSize([1, 2, 3, 4,], 2)	// [2, 3] (random 2 element)
      
      _.reverse([1, 2, 3, 4])	// [4, 3, 2, 1]
      
      _.range(5)		// [0, 1, 2, 3]
      _.range(1, 5)		// [1, 2, 3, 4]
      _.range(1, 5, 2)	// [1, 3]
      
    </script>
</body>
```















