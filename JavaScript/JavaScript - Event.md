# 📒 JavaScript - Event

- 네트워크 활동이나 사용자와의 상호작용 같은 사건의 발생을 알리기 위한 객체
- 이벤트 발생
  - 마우스 클릭을 하거나 키보드를 누르는 등 사용자 행동으로 발생할 수도 있음
  - 특정 메서드를 호출(Element.click())하여 프로그래밍적으로도 만들어 낼 수 있음

<br>

### 🌈 addEventListener

```js
// 1번
<button id="my-button">나를 눌러봐!!</button>
<p id="my-paragraph"> </p>

// 나를 눌러봐 버튼을 눌렀을 때 aler 메세지 띄우기
const myButton = document.querySelector('#my-button')
myButton.addEventListener('click', alertMessage)
```

```js
// 2번
<form action="#">
    <label for="my-text-input">내용을 입력하세요.</label>
	<input id="my-text-input" type="text">
</form>

// from 태그로 내가 입력한 내용을 바로 위에 P태그에 넣기!
const alertMessage = function () {
    alert('하하하하')
}

const myTextInput = document.querySelector('#my-text-input')

myTextInput.addEventListener('input', function (event) {
    const myPtag = document.querySelector('#my-paragraph')
    myPtag.innerText = event.target.value
})
```

```js
// 3번
<h2>Change My Color</h2>
<label for="change-color-input">원하는 색상을 영어로 입력하세요.</label>
<input id="change-color-input"></input>

// 내가 입력한 대로 라벨 태그의 색상 변경하기
const h2Tag = document.querySelector('h2')

const onColorInput = function () {
    const userInput = event.target.value
    //console.log(userInput)

    // const h2Tag = document.querySelector('h2') red라고 하면 함수가 3번 실행하는건데 굳이 얘를 그래서 위에 정의

    h2Tag.style.color = userInput
}

const colorInput = document.querySelector('#change-color-input')
// 키보드 이벤트가 발생했을 때
colorInput.addEventListener('input', onColorInput)
```



- `EventTarget.addEventListener(type, listenr[, options])`
  - 지정한 이벤트가 대상에 전달될 때마다 호출할 함수를 설정
  - 이벤트를 지원하는 모든 객체(Element, Document, Window 등)를 대상으로 지정 가능

- `type`
  - 반응 할 이벤트 유형 (대소문자 구분 문자열)
- `listener`
  - 지정된 타입의 이벤트가 발생했을 때 알림을 받는 객체
  - EventListener 인터페이스 혹은 JS function 객체(콜백 함수)여야 함

<hr>

## 📮 기억하자!

![image-20220428084326955](JavaScript%20-%20Event.assets/image-20220428084326955.png)



<br>

### 🌈 Event 취소 - preventDefault()

- `event.preventDefault()`
- 현재 이벤트의 기본 동작을 중단
- HTML 요소의 기본 동작을 작동하지 않게 막음
  - ex) a태그의 기본 동작은 클릭 시 링크로 이동 / form 태그의 기본 동작은 form 데이터 전송
- 이벤트를 취소할 수 있는 경우, 이벤트의 전파를 막지 않고 그 이벤트를 취소

<br>

```js
<!-- 1. checkbox -->
<input type="checkbox" id="my-checkbox">
 
const checkBox = document.querySelector('#my-checkbox')

checkBox.addEventListener('click', function (event) {
    event.preventDefault()
    console.log(event)
})
```

- 체크박스를 클릭했도 체크가 되지 않음!

<br>

```js
<!-- 2. submit -->
<form action="/articles/" id="my-form">
    <input type="text">
    <input type="submit" value="제출!"
</form>

const formTag = document.querySelector('#my-form')            
            
formTag.addEventListener('submit', function (event) {
    console.log(event)
    event.preventDefault()	// 이벤트 취소
    event.target.reset()	// 내가 썼던거 사라짐, 리셋
})
```

- 제출해도 제출되지 않음.. 리셋되서 제출된 듯한 느낌이 날 수 있음!

<br>

```js
<!-- 3. link -->
<a href="https://google.com/" target="_blank" id="my-link">GoToGoogle</a>

const aTag = document.querySelector('#my-link')

aTag.addEventListener('click', function (event) {
    console.log(event)
    event.preventDefault()
})
```

- 링크를 클릭해도 이동하지 않음!

<br>

```js
<!-- 4. scroll -->

document.addEventListener('scroll', function (event) {
    console.log(event)
    event.preventDefault()
})
```

- 마우스 스크롤해도 되지 않음..ㄷㄷ