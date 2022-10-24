# 📒 JavaScript - AJAX

<br>

- Asynchronous JavaScript And XML (비동기식 JavaScript와 XML)
- 서버와 통신하기 위해 `XMLHttpRequest` 객체를 활용
- JSON, XML, HTML 그리고 일반 텍스트 형식 등을 포함한 다양한 포맷을 주고 받을 수 있음
  - [참고] AJAX의 X가 XML을 의미하긴 하지만, 요즘은 더 가벼운 용량과 JavaScript의 일부라는 장점 때문에 JSON을 더 많이 사용함!
- 페이지 전체를 reload(새로 고침)를 하지 않고서도 수행되는 "비동기성"
  - 서버의 응답에 따라 전체 페이지가 아닌 일부분만을 업데이트 할 수 있음

- AJAX의 주요 두가지 특징
  1. 페이지 새로 고침 없이 서버에 요청
  2. 서버로부터 데이터를 받고 작업을 수행

<br>

## 🌈 XMLHttpRequest 객체

> 서버와 상호작용하기 위해 사용되며 전체 페이지의 새로 고침 없이 데이터를 받아올 수 있음, 즉 사용자의 작업을 방해하지 않으면서 페이지 일부를 업데이트 할 수 있음

```js
const request = new XMLHttpRequest()
const URL = 'https://jsonplaceholder.typicode.com/todos/1'

request.open('GET', URL)
request.send()

const todo = request.response
console.log(`data: ${todo}`)
```

- 위 코드를 실행하면 console에 todo 데이터가 출력되지 않는데, 이는 브라우저의 동작 과정으로 인해 데이터 응답을 기다리지 않고 `console.log()`를 먼저 실행했기 때문!

<br>

## 🌈 Asynchronous JavaScript

<br>

### "왜 비동기(Asynchronous)"를 사용할까?

- 사용자 경험
  - 매우 큰 데이터를 동반하는 앱이 이다고 가정했을때, 동기식 코드라면 데이터를 모두 불러온 뒤 앱이 실행되기 때문에 데이터를 모두 불러올 때 까지 앱이 모두 멈춘 것처럼 보인다. 즉 코드 실행을 차단하여 화면이 멈추고 응답하지 않는 것 같은 사용자 경험을 제공한다.
  - 비동기식 코드라면 데이터를 요청하고 응답 받는 동안, 앱 실행을 함께 진행한다. 데이터를 불러오는 동안 지속적으로 응답하는 화면을 보여줌으로써 더욱 쾌적한 사용자 경험을 제공하기 때문에 많은 웹 API 기능은 현재 비동기 코드를 사용하여 실행되고 있다. 

<br>

#### [참고] Thread

- 프로그램이 작업을 완료하기 위해 사용할 수 있는 단일 프로세스
- 각 thread(스레드)는 한 번에 하나의 작업만 수행할 수 있음
- 예시) Task A -> Task B -> Task C
  - 다음 작업을 시작하려면 반드시 앞의 작업이 완료되어야 함
  - 컴퓨터 CPU는 여러 코어를 가지고 있기 때문에 한 번에 여러 가지 일을 처리할 수 있음

<br>

### "JavaScript는 single threaded"

- 컴퓨터가 여러 개의 CPU를 가지고 있어도 main thread라 불리는 단일 스레드에서만 작업 수행한다. 즉, 이벤트를 처리하는 `Call Stack`이 하나인 언어라는 의미
  1. 즉시 처리하지 못하는 이벤트들을 다른 곳(Web API)으로 보내서 처리하도록 하고,
  2. 처리된 이벤트들은 처리된 순서대로 대기실(Task queue)에 줄을 세워 놓고
  3. Call Stack이 비면 담당자(Event Loop)가 대기 줄에서 가장 오래된(제일 앞의) 이벤트를 Call Stack으로 보냄

<br>

### Concurrency model

1. Call Stack
   - 요청이 들어올 때마다 해당 요청을 순차적으로 처리하는 Stack(LIFO) 형태의 자료 구조
2. Web API (Browser API)
   - JavaScript 엔진이 아닌 브라우저 영역에서 제공하는 API
   - setTimeout(), DOM events 그리고 AJAX로 데이터를 가져오는 시간이 소요되는 일들을 처리
3. Task Queue (Event Queue, Message Queue)
   - 비동기 처리된 callback 함수가 대기하는 Queue(FIFO) 형태의 자료 구조
   - main thread가 끝난 후 실행되어 후속 JavaScript 코드가 차단되는 것을 방지
4. Event Loop
   - Call Stack이 비어 있는지 확인
   - 비어 있는 겨우 Task Queue에서 실행 대기 중인 callback 함수가 있는지 확인
   - Task Queue에 대기 중인 callback 함수가 있다면 가장 앞에 있는 callback 함수를 Call Stack으로 push

<br>

### Zero delays

```js
console.log('Hi')

setTimeout(function check () {
    console.log('나 언제 찍혀?')
}, 0)
console.log('Bye')

Hi
Bye
나 언제 찍혀?
```

- 기본적으로 setTimeout 함수에 특정 시간제한(0초)을 설정했더라도 대기 중인(Call Stack) 메시지의 모든 코드가 완료될 때까지 Task Queue에서 대기해야 함

<br>



### 💡 순차적인 비동기 처리하기

- Web API로 들어오는 순서는 중요하지 않고, 어떤 이벤트가 먼저 처리되느냐가 중요 (즉, 실행 순서 불명확!!!!!!!)
- 이를 해결하기 위해 순차적인 비동기 처리를 위한 2가지 작성 방식

#### 1. Async callbacks

- 백그라운드에서 실행을 시작할 함수를 호출할 때 인자로 지정된 함수
- 예시) addEventListener()의 두 번째 인자

#### 2. promise-style

- Modern Web APIs에서의 새로운 코드 스타일
- XMLHttpRequest 객체를 사용하는 구조보다 조금 더 현대적인 버전
