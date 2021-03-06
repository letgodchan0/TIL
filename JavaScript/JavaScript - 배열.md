# π JavaScript - λ°°μ΄

> λ°°μ΄μ μ μμ νΉμ§

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

- ν€μ μμ±λ€μ λ΄κ³  μλ μ°Έμ‘° νμμ κ°μ²΄
- μμλ₯Ό λ³΄μ₯νλ νΉμ§μ΄ μμ
- μ£Όλ‘ λκ΄νΈλ₯Ό μ΄μ©νμ¬ μμ±νκ³ , 0μ ν¬ν¨ν μμ μ μ μΈλ±μ€λ‘ νΉμ  κ°μ μ κ·Ό κ°λ₯
- λ°°μ΄μ κΈΈμ΄λ array.length ννλ‘ μ κ·Ό κ°λ₯
  - [μ°Έκ³ ] - λ°°μ΄μ λ§μ§λ§ μμλ array.length - 1λ‘ μ κ·Ό

<br>

### π‘ reverse

```javascript
const numbers = [1, 2, 3, 4, 5]
numbers.reverse()
console.log(numbers)	// [5, 4, 3, 2, 1]
```

- μλ³Έ λ°°μ΄μ μμλ€μ μμλ₯Ό λ°λλ‘ μ λ ¬

<br>



### π‘ push & pop

```javascript
const numbers = [1, 2, 3, 4, 5]

numbers.push(100)
console.log(numbers)	// [1, 2, 3, 4, 5, 100]

numbers.pop()
console.log(numbers)	// [1, 2, 3, 4, 5]
```

- `push`
  - λ°°μ΄μ κ°μ₯ λ€μ μμ μΆκ°
- `pop`
  - λ°°μ΄μ λ§μ§λ§ μμ μ κ±°

<br>

### π‘ unshift & shift

```javascript
const numbers = [1, 2, 3, 4, 5]

numbers.unshift(100)
console.log(numbers)	// [100, 1, 2, 3, 4, 5]

numbers.shift()
console.log(numbers)	// [1, 2, 3, 4, 5]
```

- `unshift`
  - λ°°μ΄μ κ°μ₯ μμ μμ μΆκ°
- `shift`
  - λ°°μ΄μ μ²«λ²μ§Έ μμ μ κ±°

<br>

### π‘ includes

```javascript
const numbers = [1, 2, 3, 4, 5]

console.log(numbers.includes(1))	// true
console.log(numbers.includes(100))	// false
```

- λ°°μ΄μ νΉμ  κ°μ΄ μ‘΄μ¬νλμ§ νλ³ ν μ°Έ λλ κ±°μ§ λ°ν

<br>

### π‘ indexOf

```javascript
const numbers = [1, 2, 3, 4, 5]
let result

result = numbers.indexOf(3)	// 2
console.log(result)

result = numbers.indexOf(100)	// -1
console.log(result)
```

- λ°°μ΄μ νΉμ  κ°μ΄ μ‘΄μ¬νλμ§ νμΈ ν κ°μ₯ μ²« λ²μ§Έλ‘ μ°Ύμ μμμ μΈλ±μ€ λ°ν
- λ§μ½ ν΄λΉ κ°μ΄ μμ κ²½μ° -1 λ°ν

<br>

### π‘ join

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

- λ°°μ΄μ λͺ¨λ  μμλ₯Ό μ°κ²°νμ¬ λ°ν
- `separator(κ΅¬λΆμ)`λ μ νμ μΌλ‘ μ§μ  κ°λ₯νλ©°, μλ΅ μ μΌνλ₯Ό κΈ°λ³Έ κ°μΌλ‘ μ¬μ©!

<br>

### π‘ Spread operator

```javascript
const array = [1, 2, 3]
const newArray = [0, ...array, 4]

console.log(newArray)	// [0, 1, 2, 3, 4]
```

- `spread operator(...)`λ₯Ό μ¬μ©νλ©΄ λ°°μ΄ λ΄λΆμμ λ°°μ΄ μ κ° κ°λ₯.
- ES5κΉμ§λ Array.concat() λ©μλλ₯Ό μ¬μ©. 
- μμ λ³΅μ¬μ νμ© κ°λ₯

<br>

## π Array Helper Methods

```python
# callback ν¨μ μμ

def add(a, b):
    return a+b

def practice(x, y, func):
    return x + y - func

# μ¬κΈ°μ practiceμ μΈμλ‘ λ€μ΄κ°λ add ν¨μκ° callback ν¨μ
practice(4,5, add(a,b))
```

- λ°°μ΄μ μννλ©° νΉμ  λ‘μ§μ μννλ λ©μλ
- λ©μλ νΈμΆ μ μΈμλ‘ callback ν¨μ*λ₯Ό λ°λ κ²μ΄ νΉμ§
  - callbackν¨μ : μ΄λ€ ν¨μμ λ΄λΆμμ μ€νλ  λͺ©μ μΌλ‘ μΈμλ‘ λκ²¨ λ°λ ν¨μλ₯Ό λ§ν¨

<br>

### π‘ forEach

```javascript
array.forEach((element, index, array) =>{
    // do something
})

const fruits = ['λΈκΈ°', 'μλ°', 'μ¬κ³Ό', 'μ²΄λ¦¬']

fruits.forEach((fruit, index) => {
    console.log(fruit, index)
    // λΈκΈ° 0
    // μλ° 1
    // μ¬κ³Ό 2
    // μ²΄λ¦¬ 3
})
```

- λ°°μ΄μ κ° μμμ λν΄ μ½λ°± ν¨μλ₯Ό ν λ²μ© μ€ν
- μ½λ°± ν¨μλ 3κ°μ§ λ§€κ°λ³μλ‘ κ΅¬μ±
  - element : λ°°μ΄μ μμ
  - index : λ°°μ΄ μμμ μΈλ±μ€
  - array : λ°°μ΄ μμ²΄
- λ°ν κ°(return)μ΄ μλ λ©μλ!!!



<br>

### π‘ map

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

- λ°°μ΄μ κ° μμμ λν΄ μ½λ°± ν¨μλ₯Ό ν λ²μ© μ€ν
- μ½λ°± ν¨μμ λ°ν κ°μ μμλ‘ νλ μλ‘μ΄ λ°°μ΄ λ°ν
- κΈ°μ‘΄ λ°°μ΄ μ μ²΄λ₯Ό λ€λ₯Έ ννλ‘ λ°κΏ λ μ μ©

<br>



### π‘ filter

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

- λ°°μ΄μ κ° μμμ λν΄ μ½λ°± ν¨μλ₯Ό ν λ²μ© μ€ν
- μ½λ°± ν¨μμ λ°ν κ°μ΄ μ°ΈμΈ μμλ€λ§ λͺ¨μμ μλ‘μ΄ λ°°μ΄μ λ°ν
- κΈ°μ‘΄ λ°°μ΄μ μμλ€μ νν°λ§ ν  λ μ μ©

<br>

### π‘ reduce

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

- λ°°μ΄μ κ° μμμ λν΄ μ½λ°± ν¨μλ₯Ό ν λ²μ© μ€ν
- μ½λ°± ν¨μμ λ°ν κ°λ€μ νλμ κ°(acc)μ λμ  ν λ°ν
- reduce λ©μλμ μ£Όμ λ§€κ°λ³μ
  - acc : μ΄μ  callback ν¨μμ λ°ν κ°μ΄ λμ λλ λ³μ
  - initialValue(optional) : μ΅μ΄ callback ν¨μ νΈμΆ μ accμ ν λΉλλ κ°, default κ°μ λ°°μ΄μ μ²«λ²μ§Έ κ°
- [μ°Έκ³ ] : λΉ λ°°μ΄μ κ²½μ° initialValueλ₯Ό μ κ³΅νμ§ μμΌλ©΄ μλ¬ λ°μ 

<br>

### π‘ find

```javascript
array.find((element, index, array)) {
    // do something
}

const avengers = [
    { name: 'Tony Stark', age: 45 },
    { name: 'Steve Rogers', age: 32 },
    { name: 'Thor', age: 40 },
]

const result = avengers.find((avenger) => {
    return avenger.name === 'Tony Stark'
})

console.log(result)	// {name: 'Tony Stark, age: 45'}
```

- λ°°μ΄μ κ° μμμ λν΄ μ½λ°± ν¨μλ₯Ό ν λ²μ© μ€ν
- μ½λ°± ν¨μμ λ°ν κ°μ΄ μ°Έμ΄λ©΄, μ‘°κ±΄μ λ§μ‘±νλ μ²«λ²μ§Έ μμλ₯Ό λ°ν
- μ°Ύλ κ°μ΄ λ°°μ΄μ μμΌλ©΄ undefined λ°ν

<br>



### π‘ some

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

- λ°°μ΄μ μμ μ€ νλλΌλ μ£Όμ΄μ§ νλ³ ν¨μλ₯Ό ν΅κ³Όνλ©΄ μ°Έμ λ°ν
- λͺ¨λ  μμκ° ν΅κ³Όνμ§ λͺ»νλ©΄ κ±°μ§ λ°ν
- λΉ λ°°μ΄μ ν­μ κ±°μ§ λ°ν, νμ΄μ¬ if any

<br>



### π‘ every

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

- λ°°μ΄μ λͺ¨λ  μμκ° μ£Όμ΄μ§ νλ³ ν¨μλ₯Ό ν΅κ³Όνλ©΄ μ°Έμ λ°ν
- νλμ μμλΌλ ν΅κ³Όνμ§ λͺ»νλ©΄ κ±°μ§ λ°ν
- λΉ λ°°μ΄μ ν­μ μ°Έ λ°ν, νμ΄μ¬μ if all

<br>



### π? Quiz

1. λ°°μ΄μ μμ νμμ μνλ©° μμ μΈλ±μ±μ΄ λΆκ°λ₯νλ€λ νΉμ§μ΄ μλ€. (F)
   - μλ°μ€ν¬λ¦½νΈμ λ°°μ΄μ μ°Έμ‘° νμμ μνλ€.
2. forEach λ©μλλ λ°°μ΄μ κ° μμλ€μ λͺ¨μμ μλ‘μ΄ λ°°μ΄μ λ°ννλ€. (F)
   - λ°°μ΄μ κ° μμλ€μ λͺ¨μμ μλ‘μ΄ λ°°μ΄μ λ°ννλ λ©μλλ map λ©μλμ΄λ€.
3. λ°°μ΄μ κ°μ₯ λ§μ§λ§μ μμλ₯Ό μΆκ°νλ λ©μλλ appendμ΄λ€. (F)
   - λ°°μ΄μ κ°μ₯ λ§μ§λ§ μμλ₯Ό μΆκ°νλ λ©μλλ pushμ΄λ€. 