# 📒 JavaScript - Ajax (Promise)

자바스크립트는 기본적으로 싱글스레드에서 동기식으로 작업이 이루어지지만 시간 관련 함수, 이벤트 등 즉시 처리하지 못하는 이벤트들을 Web API로 보내서 처리하도록 한다. 이때 Web API로 들어온느 순서는 중요하지 않고 먼저 처리된 이벤트를 Task Queue로 보내 대기시킨다. 이를 해결하기 위해 순차적인 비동기 처리를 위한 2가지 방식이 있고 그 중 한가지가 `Async callbacks` 방식으로 콜백 함수를 사용하는 것이다. 

순차적인 연쇄 비동기 작업을 처리하기 위해 콜백 함수를 호출하고, 그 다음 콜백 함수를 호출하고 또 그 함수의 콜백 함수를 호출하는 등의 패턴이 지속적으로 반복되는 `callback Hell` 에 빠지게 되며, 이와 같은 상황이 벌어질 경우 코드 가독성이 떨어지고 디버깅 또한 어렵게 된다. 따라서 우리가 사용할 것이 `Promise callbacks`이다. 

<br>

## 🌈 Promise object

- 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
  - 미래의 완료 또는 실패와 그 결과 값을 나타냄
  - 미래의 어떤 상황에 대한 약속
- `.then(callback)`
  - 이전 작업(promise)이 성공했을 때(이행했을 때) 수행할 작업을 나타내는 callback 함수
  - 그리고 각 callback 함수는 이전 작업의 성공 결과를 인자로 전달받음
  - 따라서 성공했을 때의 코드를 callback 함수 안에 작성!!
  - 각각의 `.then()` 블록은 서로 다른 promise를 반환 하기 때문에 `.then()`을 여러개 사용(chaining)하여 연쇄적인 작업을 수행할 수 있음
    - 결국 여러 비동기 작업을 차례대로 수행할 수 있음
- `.catch(callback)`
  - `.then`이 하나라도 실패하면(거부 되면) 동작 (동기식의 'try - except' 구문과 유사)
  - 이전 작업의 실패로 인해 생성된 error 객체는 catch 블록 안에서 사용할 수 있음

> `.then`과 `.catch` 메서드 모두 promise를 반환하기 때문에 chaining 가능, 주의할 점은 반환 값이 반드시 있어야 한다!!

- `.finally(callback)`
  - promise 객체를 반환
  - 결과와 상관없이 무조건 지정된 callback 함수가 실행
  - 어떠한 인자도 전달받지 않음
    - Promise가 성공되었는지 거절되었는지 판단할 수 없기 때문
  - 무조건 실행되어야 하는 절에서 활용
    - `.then()`과 `.catch()` 블록에서의 코드 중복을 방지

<br>

## 💡 Promise가 보장하는 것!

1. 콜백 함수는 JavaScript의 Event Loop가 현재 실행 중인 Call Stack을 완료하기 이전에는 절대 호출되지 않음
   - Promise callback 함수는 Event Queue에 배치되는 엄격한 순서로 호출됨
2. 비동기 작업이 성공하거나 실패한 뒤에 `.then()` 메서드를 이용하여 추가한 경우에도 1번과 똑같이 동작
3. `.then()`을 여러 번 사용하여 어러 개의 콜백 함수를 추가할 수 있음 (chaining)
   - 각각의 콜백은 주어진 순서대로 하나하나 실행하게 됨
   - Chaining은 Promise의 가장 뛰어난 장점

<br>



## 🌈 Axios

```js
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
axios.get('https://jsonplaceholder.typicode.com/todos/1')
	.then(res => console.log(res.data.title))
	.catch(err => console.log('Error!'))
</script>
```

- 브라우저를 위한 Promise 기반의 클라이언트
- 원래는 "XHR" 이라는 브라우저 내장 객체를 활용해 AJAX 요청을 처리하는데, 이보다 편리한 AJAX 요청이 가능하도록 도움을 줌
- 확장 가능한 인터페이스와 함께 패키지로 사용이 간편한 라이브러리를 제공

<br>



## 🌈 async & await

- 비동기 코드를 작성하는 새로운 방법으로 ES8에서 등장함
- 기존 Promise 시스템 위에 구축된 syntactic sugar
  - Promise 구조의 then chaining을 제거
  - 비동기 코드를 조금 더 동기 코드처럼 표현
  - `Syntactic sugar`
    - 더 쉽게 읽고 표현할 수 있도록 설계된 프로그래밍 언어 내의 구문
    - 즉, 문법적 기능은 그대로 유지하되 사용자가 직관적으로 코드를 읽을 수 있게 만듦

- Promise -> async & await 적용

  ![image-20220503231659368](JavaScript%20-%20Ajax%20(Promise).assets/image-20220503231659368.png)