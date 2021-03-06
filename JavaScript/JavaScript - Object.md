# ๐ JavaScript - Object (๊ฐ์ฒด)

<br>



### ๐ ๊ฐ์ฒด์ ์ ์์ ํน์ง

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

- ๊ฐ์ฒด๋ ์์ฑ์ ์งํฉ์ด๋ฉฐ, ์ค๊ดํธ ๋ด๋ถ์ key์ value์ ์์ผ๋ก ํํ
- `key`๋ ๋ฌธ์์ด ํ์*๋ง ๊ฐ๋ฅ
  - [์ฐธ๊ณ ] `key` ์ด๋ฆ์ ๋์ด์ฐ๊ธฐ ๋ฑ์ ๊ตฌ๋ถ์๊ฐ ์์ผ๋ฉด ๋ฐ์ดํ๋ก ๋ฌถ์ด์ ํํ
- `value`๋ ๋ชจ๋  ํ์(ํจ์ํฌํจ) ๊ฐ๋ฅ
- ๊ฐ์ฒด ์์ ์ ๊ทผ์ ์  ๋๋ ๋๊ดํธ๋ก ๊ฐ๋ฅ
  - [์ฐธ๊ณ ] `key` ์ด๋ฆ์ ๋์ด์ฐ๊ธฐ ๊ฐ์ ๊ตฌ๋ถ์๊ฐ ์์ผ๋ฉด ๋๊ดํธ ์ ๊ทผ๋ง ๊ฐ๋ฅ

<br>

### ๐ ๊ฐ์ฒด์ ๋ฉ์๋

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

- ๋ฉ์๋๋ ๊ฐ์ฒด์ ์์ฑ์ด ์ฐธ์กฐํ๋ ํจ์
- `๊ฐ์ฒด.๋ฉ์๋๋ช() ์ผ๋ก ํธ์ถ ๊ฐ๋ฅ`
- ๋ฉ์๋ ๋ด๋ถ์์๋ `this` ํค์๋๊ฐ ๊ฐ์ฒด๋ฅผ ์๋ฏธํจ
  - fullName์ ๋ฉ์๋๊ฐ ์๋๊ธฐ ๋๋ฌธ์ ์ ์์ถ๋ ฅ๋์ง ์์(NaN)
  - getFullName์ ๋ฉ์๋์ด๊ธฐ ๋๋ฌธ์ ํด๋น ๊ฐ์ฒด์ firstName๊ณผ lastName์ ์ ์์ ์ผ๋ก ์ด์ด์ ๋ฐํ

<br>

## ๐ฉ ๊ฐ์ฒด ๊ด๋ จ ES6 ๋ฌธ๋ฒ ์ตํ๊ธฐ

<br>

### ๐ ES6 ๋ฌธ๋ฒ (1) - ์์ฑ๋ช ์ถ์ฝ

```javascript
const books= ['Learning JS', 'learning Python']
const magazines = ['Vogue', 'Science']

//ES6+
const bookShop = {
    books,			// books : books,
    magazines,		// magazines: magazines, ES5 ํ์
}
console.log(bookShop)
```

- ๊ฐ์ฒด๋ฅผ ์ ์ํ  ๋ `key`์ ํ ๋นํ๋ ๋ณ์์ ์ด๋ฆ์ด ๊ฐ์ผ๋ฉด ์์์ ๊ฐ์ด ์ถ์ฝ ๊ฐ๋ฅ

<br>

### ๐ ES6 ๋ฌธ๋ฒ (2) - ๋ฉ์๋๋ช ์ถ์ฝ (shorthand)

```javascript
const obj = {
    // greeting: function()
    greeting(){
        console.log('Hi')
    }
}
obj.greeting()	// Hi
```

- ๋ฉ์๋ ์ ์ธ ์ function ํค์๋ ์๋ต ๊ฐ๋ฅ!!

<br>

### ๐ ES6 ๋ฌธ๋ฒ (3) - ๊ณ์ฐ๋ ์์ฑ

```javascript
const key = 'regions'
const value = ['์์ธ', '๊ด์ฃผ', '๋์ ', '๊ตฌ๋ฏธ']

const nation = {
    [key]: value,
}

console.log(nation)				// { regions: Array(4) }
console.log(nation.regions)		// ['์์ธ', '๊ด์ฃผ', '๋์ ', '๊ตฌ๋ฏธ']
```

- ๊ฐ์ฒด๋ฅผ ์ ์ํ  ๋ key์ ์ด๋ฆ์ ํํ์์ผ๋ก ์ด์ฉํ์ฌ ๋์ ์ผ๋ก ์์ฑ ๊ฐ๋ฅ

<br>

### ๐ ES6 ๋ฌธ๋ฒ (4) - ๊ตฌ์กฐ ๋ถํด ํ ๋น

```javascript
const userInfo = {
    name: 'kim',
    userId: 'play',
    email: 'play@gmail.com'
}

const name = userInfo.name		=>	const { name } = userInfo.name
const userId = userInfo.userId	=>	const { userId } = userInfo.userId
const email = userInfo.email	=>	const { email } = userInfo.email

// ์ฌ๋ฌ๊ฐ๋ ๊ฐ๋ฅ
const {name, userId} = userInfo
```

```js
const user = {
    name: '๊ต์๋',
    age : '20',
    balance : 100
}

// ์๋ 2๊ฐ๋ ๊ฐ์ ์ฝ๋, ํค์ ๋ณ์๋ช์ด ๊ฐ์์ผ ํจ
const { name } = user
const name = user.name

// ์๋ 2๊ฐ๋ ๋ค๋ฅธ ์ฝ๋
const { erum } = user
const name = user.name

// ๊ทผ๋ฐ ์ด๊ฑด ๊ฐ๋ฅ
const { name, age, balance } = user

// ๊ทผ๋ฐ ์ด๊ฑธ ์ฐ๊ธดํด?? ์๋์ฒ๋ผ ์
function printUser( { name, age, balance }) {
    console.log(age, balance, name)
}
printUser(user)	// 20 100 '๊ต์๋'
```





<br>

### ๐ ES6 ๋ฌธ๋ฒ (5) - Spread operator

```js
const obj = {b: 2, c: 3, d: 4}
const newObj = {a: 1, ...obj, e: 5}

console.log(newObj)	// {a:1, b:2, c:3, d:4, e:5}
```

<br>

## ๐ฃ Json 

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
  - ์๋ฐ์คํฌ๋ฆฝํธ ๊ฐ์ฒด๋ฅผ JSON ํ์์ผ๋ก!

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
  - JSON์ ์๋ฐ์คํฌ๋ฆฝํธ ๊ฐ์ฒด๋ก!

<br>

## ๐ฎ Quiz

1. ๊ฐ์ฒด์ KEY ๊ฐ์ ์์ ํ์๋ง ์ฌ์ฉ ๊ฐ๋ฅํ๋ค. (F)
   - ๊ฐ์ฒด์ key ๊ฐ์ ๋ฌธ์์ด ํ์๋ง ์ฌ์ฉ ๊ฐ๋ฅํ๋ค.
2. ๊ฐ์ฒด ์ ์ ์ key ๊ฐ๊ณผ key์ ํ ๋นํ๋ ๋ณ์์ ์ด๋ฆ์ด ๊ฐ์ผ๋ฉด ์ถ์ฝ ๊ฐ๋ฅํ๋ค. (T)
3.  JSON ๋ฐ์ดํฐ๋ ๋ฌธ์์ด ํ์์ผ๋ก ํํ๋๋ค. (T)

[์ฐธ๊ณ ] - ๋ฐฐ์ด์ ์ธ๋ฑ์ค๋ฅผ ํค๋ก ๊ฐ๋ ๊ฐ์ฒด!!

<br>

### ๐ก this

> class ๋ด๋ถ์์๋ ๊ฐ์ฒด๋ฅผ ๊ฐ๋ฆฌํค์ง๋ง ๊ธฐ๋ณธ์ ์ผ๋ก ์ต์์ ๊ฐ์ฒด์ธ window๋ฅผ ๊ฐ๋ฆฌํด

```js
const obj = {
    PI: 3.14
    radiuses: [1, 2, 3, 4, 5],
        printArea: function () {
            // ์ฌ๊ธฐ this๋ obj
            this.radiuses.forEach(function (r) {
                // ์ฌ๊ธฐ this๋ window
                console.log(this.PI * r * r)
            }.bind(this))                               
        },
}
```

- `this.radiuses`๋ ๋ฉ์๋ (๊ฐ์ฒด.๋ฉ์๋๋ช()์ผ๋ก ํธ์ถ ๊ฐ๋ฅ) ์์์ด๊ธฐ ๋๋ฌธ์ ์ ์์ ์ผ๋ก ์ ๊ทผ ๊ฐ๋ฅ
- `forEach`์ ์ฝ๋ฐฑ ํจ์์ ๊ฒฝ์ฐ ๋ฉ์๋๊ฐ ์๋ (๊ฐ์ฒด.๋ฉ์๋๋ช()์ผ๋ก ํธ์ถ ๋ถ๊ฐ๋ฅ)
- ๋ฐ๋ผ์ ์ฝ๋ฐฑ ํจ์ ๋ด๋ถ์ `this`๋ `window`๊ฐ ๋์ด `this.PI`๋ ์ ์์ ์ผ๋ก ์ ๊ทผ ๋ถ๊ฐ๋ฅ
- ์ด ์ฝ๋ฐฑ ํจ์ ๋ด๋ถ์์ `this.PI`์ ์ ๊ทผํ๊ธฐ ์ํด์ ํจ์๊ฐ์ฒด `.bind(this)` ๋ฉ์๋๋ฅผ ์ฌ์ฉ.
- ์ด ๋ฒ๊ฑฐ๋ก์ด `bind` ๊ณผ์ ์ ์์ค ๊ฒ์ด ๋ฐ๋ก ํ์ดํ ํจ์!!!!!!!!!!!!!!!!!

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

- ํจ์ ๋ด๋ถ์ this ํค์๋๊ฐ ์กด์ฌํ  ๊ฒฝ์ฐ
  - ํ์ดํ ํจ์์ function ํค์๋๋ก ์ ์ธํ ํจ์๊ฐ ๋ค๋ฅด๊ฒ ๋์
- ํจ์ ๋ด๋ถ์ this ํค์๋๊ฐ ์กด์ฌํ์ง ์์ ๊ฒฝ์ฐ
  - ์์ ํ ๋์ผํ๊ฒ ๋์

<br>

```js
// ์์ฝ

function getFullName() {
    return this.firstName + this.lastName
}
// ์ด์ํ์์ this๋ window
getFullName()


const me = {
    firstName: '๊น'.
    lastName: 'ํผ์',
    'getFullName' : getFullName
}
// ๋ฉ์๋์ผ๋ this๋ ๊ฐ์ฒด, ์ฉ(.)์ ํตํด ๋ถ๋ฅผ๋
me.getFullName()

const you = {
    firstNmae: '์ด',
    lastNmae : '์นํจ'
    qwer: getFullName,
}
// ๋ง์ฐฌ๊ฐ์ง๋ก ๋ฉ์๋์ด๊ธฐ ๋๋ฌธ์ this๊ฐ you์
you.qwer()	'์นํจ์ด'
```



## ๐ก lodash

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















