# π Java Script - λ³μμ μλ³μ

<br>



## π‘ μλ³μ μ μμ νΉμ§

- μλ³μλ λ³μλ₯Ό κ΅¬λΆν  μ μλ λ³μλͺμ λ§ν¨
- μλ³μλ λ°λμ λ¬Έμ, λ¬λ¬($) λλ λ°μ€(_)λ‘ μμ
- λμλ¬Έμλ₯Ό κ΅¬λΆνλ©°, ν΄λμ€λͺ μΈμλ λͺ¨λ μλ¬Έμλ‘ μμ
- μμ½μ΄ μ¬μ© λΆκ°λ₯ (for, if, function) 



### μλ³μ μμ± μ€νμΌ

<hr>

- μΉ΄λ© μΌμ΄μ€ 
  - λ³μ, κ°μ²΄, ν¨μμ μ¬μ©
  - λ λ²μ§Έ λ¨μ΄μ μ²« κΈμλΆν° λλ¬Έμ

```javascript
// λ³μ
let dog
let variableName

// κ°μ²΄
const userInfo = { name: 'Chan', age: 20}

// ν¨μ
function add() {}
function getName() {}
```

<br>

- νμ€μΉΌ μΌμ΄μ€
  - ν΄λμ€, μμ±μμ μ¬μ©
  - λͺ¨λ  λ¨μ΄μ μ²« λ²μ§Έ κΈμλ₯Ό λλ¬Έμλ‘ μμ±

```javascript
// ν΄λμ€
class User {
    constructor(options) {
        this.name = options.name
    }
}

// μμ±μ ν¨μ
function User(options) {
    this.name = options.name
}
```

<br>

- λλ¬Έμ μ€λ€μ΄ν¬ μΌμ΄μ€
  - μμ(κ°λ°μμ μλμ μκ΄μμ΄ λ³κ²½λ  κ°λ₯μ±μ΄ μλ κ°)μ μ¬μ©
  - λͺ¨λ  λ¨μ΄ λλ¬Έμ μμ± & λ¨μ΄ μ¬μ΄μ μΈλμ€μ½μ΄ μ½μ

```javascript
// κ°μ΄ λ°λμ§ μμ μμ
const API_KEY = 'my-key'
const PI = Math.PI

// μ¬ν λΉμ΄ μΌμ΄λμ§ μλ λ³μ
const numbers = [1, 2, 3]
```

<br>



## π‘ λ³μ μ μΈ ν€μλ (let, const)

| let                                | const                                   |
| ---------------------------------- | --------------------------------------- |
| μ¬ν λΉ ν  μμ μΈ λ³μ μ μΈ μ μ¬μ© | μ¬ν λΉ ν  μμ μ΄ μλ λ³μ μ μΈ μ μ¬μ© |
| λ³μ μ¬μ μΈ λΆκ°λ₯                 | λ³μ μ¬μ μΈ λΆκ°λ₯                      |
| λΈλ‘ μ€μ½ν                        | λΈλ‘ μ€μ½ν                             |

![image-20220425202950870](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425202950870.png)

![image-20220425203011947](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425203011947.png)

![image-20220425203207414](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425203207414.png)



```javascript
[μ°Έκ³ ] μ μΈ, ν λΉ, μ΄κΈ°ν

let foo				// μ μΈ			=> λ³μλ₯Ό μμ±νλ νμ λλ μμ 
console.log(foo)	// undefined

foo = 11			// ν λΉ			=> μ μΈλ λ³μμ κ°μ μ μ₯νλ νμ λλ μμ 
console.log(foo)	// 11

let bar = 0			// μ μΈ + ν λΉ		=> μ΄κΈ°ν - μ μΈλ λ³μμ μ²μμΌλ‘ κ°μ μ μ₯νλ νμ λλ μμ 
console.log(bar)	// 0
```



## π‘ λ³μ μ μΈ ν€μλ 'var'

- `var`λ‘ μ μΈν λ³μλ μ¬μ μΈ λ° μ¬ν λΉ λͺ¨λ κ°λ₯

  ```javascript
  var number = 10	// 1. μ μΈ λ° μ΄κΈ°κ° ν λΉ
  var number = 50 // 2. μ¬ν λΉ
  
  console.log(number)  // 50
  ```

- ES6 μ΄μ μ λ³μλ₯Ό μ μΈν  λ μ¬μ©λλ ν€μλ

- `νΈμ΄μ€ν` λλ νΉμ±μΌλ‘ μΈν΄ μκΈ°μΉ λͺ»ν λ¬Έμ  λ°μ κ°λ₯

  - λ°λΌμ ES6 μ΄νλΆν°λ var λμ  constμ letμ μ¬μ©νλ κ²μ κΆμ₯

  ![image-20220425204118008](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425204118008.png)

- ν¨μ μ€μ½ν

  ![image-20220425204053859](Java%20Script%20%EB%AC%B8%EB%B2%951.assets/image-20220425204053859.png)



## π? let, const, var λΉκ΅

| ν€μλ | μ¬μ μΈ | μ¬ν λΉ |   μ€μ½ν    |     λΉκ³      |
| :----: | :----: | :----: | :---------: | :----------: |
|  let   |   X    |   O    | λΈλ‘ μ€μ½ν | ES6λΆν° λμ |
| const  |   X    |   X    | λΈλ‘ μ€μ½ν | ES6λΆν° λμ |
|  var   |   O    |   O    | ν¨μ μ€μ½ν |    μ¬μ© x    |

