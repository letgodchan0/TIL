# π JavaScript - Event

- λ€νΈμν¬ νλμ΄λ μ¬μ©μμμ μνΈμμ© κ°μ μ¬κ±΄μ λ°μμ μλ¦¬κΈ° μν κ°μ²΄
- μ΄λ²€νΈ λ°μ
  - λ§μ°μ€ ν΄λ¦­μ νκ±°λ ν€λ³΄λλ₯Ό λλ₯΄λ λ± μ¬μ©μ νλμΌλ‘ λ°μν  μλ μμ
  - νΉμ  λ©μλλ₯Ό νΈμΆ(Element.click())νμ¬ νλ‘κ·Έλλ°μ μΌλ‘λ λ§λ€μ΄ λΌ μ μμ

<br>

### π addEventListener

1. 'λλ₯Ό λλ¬λ΄' λ²νΌμ λλ μ λ alert λ©μΈμ§ λμ°κΈ°

```js
<button id="my-button">λλ₯Ό λλ¬λ΄!!</button>
<p id="my-paragraph"> </p>

// λλ₯Ό λλ¬λ΄ λ²νΌμ λλ μ λ aler λ©μΈμ§ λμ°κΈ°
const myButton = document.querySelector('#my-button')
myButton.addEventListener('click', alertMessage)
```

2. `form` νκ·Έμ μλ ₯ν λ΄μ©μ λ°λ‘ μ `P` νκ·Έμ λ£κΈ°!

```js
<form action="#">
    <label for="my-text-input">λ΄μ©μ μλ ₯νμΈμ.</label>
	<input id="my-text-input" type="text">
</form>

// from νκ·Έλ‘ λ΄κ° μλ ₯ν λ΄μ©μ λ°λ‘ μμ Pνκ·Έμ λ£κΈ°!
const alertMessage = function () {
    alert('νννν')
}

const myTextInput = document.querySelector('#my-text-input')

myTextInput.addEventListener('input', function (event) {
    const myPtag = document.querySelector('#my-paragraph')
    myPtag.innerText = event.target.value
})
```

3. λ΄κ° μλ ₯νλλ‘ λΌλ²¨ νκ·Έμ μμ λ³κ²½νκΈ°

```js
<h2>Change My Color</h2>
<label for="change-color-input">μνλ μμμ μμ΄λ‘ μλ ₯νμΈμ.</label>
<input id="change-color-input"></input>

// λ΄κ° μλ ₯ν λλ‘ λΌλ²¨ νκ·Έμ μμ λ³κ²½νκΈ°
const h2Tag = document.querySelector('h2')

const onColorInput = function () {
    const userInput = event.target.value
    //console.log(userInput)

    // const h2Tag = document.querySelector('h2') redλΌκ³  νλ©΄ ν¨μκ° 3λ² μ€ννλκ±΄λ° κ΅³μ΄ μλ₯Ό κ·Έλμ μμ μ μ

    h2Tag.style.color = userInput
}

const colorInput = document.querySelector('#change-color-input')
// ν€λ³΄λ μ΄λ²€νΈκ° λ°μνμ λ
colorInput.addEventListener('input', onColorInput)
```



- `EventTarget.addEventListener(type, listenr[, options])`
  - μ§μ ν μ΄λ²€νΈκ° λμμ μ λ¬λ  λλ§λ€ νΈμΆν  ν¨μλ₯Ό μ€μ 
  - μ΄λ²€νΈλ₯Ό μ§μνλ λͺ¨λ  κ°μ²΄(Element, Document, Window λ±)λ₯Ό λμμΌλ‘ μ§μ  κ°λ₯

- `type`
  - λ°μ ν  μ΄λ²€νΈ μ ν (λμλ¬Έμ κ΅¬λΆ λ¬Έμμ΄)
- `listener`
  - μ§μ λ νμμ μ΄λ²€νΈκ° λ°μνμ λ μλ¦Όμ λ°λ κ°μ²΄
  - EventListener μΈν°νμ΄μ€ νΉμ JS function κ°μ²΄(μ½λ°± ν¨μ)μ¬μΌ ν¨

<hr>

## π? κΈ°μ΅νμ!

![image-20220428084326955](JavaScript%20-%20Event.assets/image-20220428084326955.png)



<br>

### π Event μ·¨μ - preventDefault()

- `event.preventDefault()`
- νμ¬ μ΄λ²€νΈμ κΈ°λ³Έ λμμ μ€λ¨
- HTML μμμ κΈ°λ³Έ λμμ μλνμ§ μκ² λ§μ
  - ex) aνκ·Έμ κΈ°λ³Έ λμμ ν΄λ¦­ μ λ§ν¬λ‘ μ΄λ / form νκ·Έμ κΈ°λ³Έ λμμ form λ°μ΄ν° μ μ‘
- μ΄λ²€νΈλ₯Ό μ·¨μν  μ μλ κ²½μ°, μ΄λ²€νΈμ μ νλ₯Ό λ§μ§ μκ³  κ·Έ μ΄λ²€νΈλ₯Ό μ·¨μ

<br>

1. μ²΄ν¬λ°μ€λ₯Ό ν΄λ¦­ν΄λ μ²΄ν¬κ° λμ§ μμ!

```js
<!-- 1. checkbox -->
<input type="checkbox" id="my-checkbox">
 
const checkBox = document.querySelector('#my-checkbox')

checkBox.addEventListener('click', function (event) {
    event.preventDefault()
    console.log(event)
})
```

<br>

2. μ μΆν΄λ μ μΆλμ§ μμ.. λ¦¬μμμΌμ μ μΆλ λ―ν λλμ΄ λ  μ μμ!!

```js
<!-- 2. submit -->
<form action="/articles/" id="my-form">
    <input type="text">
    <input type="submit" value="μ μΆ!"
</form>

const formTag = document.querySelector('#my-form')            
            
formTag.addEventListener('submit', function (event) {
    console.log(event)
    event.preventDefault()	// μ΄λ²€νΈ μ·¨μ
    event.target.reset()	// λ΄κ° μΌλκ±° μ¬λΌμ§, λ¦¬μ
})
```

<br>

3. λ§ν¬λ₯Ό ν΄λ¦­ν΄λ μ΄λνμ§ μμ!

```js
<!-- 3. link -->
<a href="https://google.com/" target="_blank" id="my-link">GoToGoogle</a>

const aTag = document.querySelector('#my-link')

aTag.addEventListener('click', function (event) {
    console.log(event)
    event.preventDefault()
})
```



<br>

4. λ§μ°μ€ μ€ν¬λ‘€ν΄λ λμ§ μμ..γ·γ·

```js
<!-- 4. scroll -->

document.addEventListener('scroll', function (event) {
    console.log(event)
    event.preventDefault()
})
```

