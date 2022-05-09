# 🔰Vue 찍어먹기

<br>

[Vue 공식문서 시작하기](https://kr.vuejs.org/v2/guide/index.html) 부터 찍먹 해보자!

<hr>

1. CDN 작성

```html
<!-- 개발버전, 도움되는 콘솔 경고를 포함. -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

또는,

<!-- 상용버전, 속도와 용량이 최적화됨. -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

<br>

2. 선언적 렌더링

```html
<div id="app">
  {{ message }}
</div>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```

<br>

3. Element 속성 바인딩

```html
<h2>Element 속성 바인딩</h2>
<div id="app2">
    <span v-bind: title="message">
      내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
    </span>
</div>
```

```js
const app2 = new Vue({
    el: '#app2',
    data: {
        message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
    }
})
```

<br>

4. 조건문

```html
<h2>조건</h2>
<div id="app3">
  <p v-if="seen">이제 나를 볼 수 있어요</p>
</div>
```

```js
const app3 = new Vue({
    el: '#app3',
    data: {
        seen: true // false로 토글 가능
    }
})
```

<br>

5. 반복문

```html
<h2>반복</h2>
<div id="app4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
const app4 = new Vue({
    el: '#app4',
    data : {
        todos: [
          { text: 'JavaScript 배우기' },
          { text: 'Vue 배우기' },
          { text: '무언가 멋진 것을 만들기' }
        ]
    }
})
```

<br>

6. 사용자 입력 핸들링

```html
<h2>사용자 입력 핸들링</h2>
<div id="app5">
    <p>{{ message }}</p>
    <button v-on: click="reverseMessage">메시지 뒤집기</button>
</div>
```

```js
const app5 = new Vue({
    el: '#app5',
    data: {
        message: '안녕하세요! Vue.js!'
    },
    methods: {
        reverseMessage: function () {
            this.message = this.message.split('').reverse().join('')
        }
    }
})
```

<br>

7. 사용자 입력 핸들링

```html
<div id="app6">
    <p>{{ message }}</p>
    <input v-model="message">
</div>
```

```js
const app6 = new Vue({
    el: '#app6',
    data: {
        message: '안녕하세요 Vue!'
    }
})
```

<br>

# 🔰 Vue 기본 문법

[Vue 인스턴스](https://kr.vuejs.org/v2/guide/instance.html)

<hr>

## 🪴 Vue instance

```js
const app = new Vue({
    
})
```

- 모든 Vue 앱은 Vue 함수로 새 인스턴스를 만드는 것부터 시작!
- Vue 인스턴스를 생성할 때는 Options 객체를 전달해야 함
- 여러 Options들을 사용하여 원하는 동작을 구현
- [전체 옵션 목록](https://kr.vuejs.org/v2/api/#propsData)

<br>

## 🪴 Options/DOM - 'el'

```js
const app = new Vue({
    el: '#app'
})
```

- Vue 인스턴스에 연결(마운트)할 기존 DOM 요소가 필요!
- CSS 선택자 문자열 혹은 HTML Element로 작성
- new를 이용한 인스턴스 생성 때만 사용

<br>

## 🪴 Options/Data - 'data'

```js
const app = new Vue({
    el: '#app',
    data: {
        message: 'Hello',
    }
})
```

- Vue 인스턴스의 데이터 객체로 Vue 인스턴스의 상태 데이터를 정의하는 곳
- Vue template에서 interpolation을 통해 접근 가능
- `v-bind`, `v-on`과 같은 디렉티브 표현식에서도 사용 가능
- Vue 객체 내 다른 함수에서 this 키워드를 통해 접근 가능

<br>

## 🪴 Options/Data - 'methods'

```js
const app = vew Vue({
    el: '#app',
    data: {
        message: 'Hello',
    },
    methods: {
        greeting: function () {
            console.log('hello')
        }
    }
})
```

- Vue 인스턴스에 추가할 메서드
- Vue template에서 interpolation을 통해 접근 가능
- `v-on`과 같은 디렉티브 표현식에서도 사용 가능
- Vue 객체 내 다른 함수에서 this 키워드를 통해 접근 가능
- [주의!]
  - 화살표 함수를 메서드를 정의하는데 사용하면 안됩니다!!
  - 화살표 함수가 부모 컨텍스트를 바인딩하기 때문에 this는 Vue 인스턴스가 아님.

<br>

## 🪴 'this' in Vue.js

```js
const app = new Vue({
    el: '#app',
    data: {
        number: 10,
    },
    methods: {
        myFunc: function () {
            console.log(this)	// Vue instance
        },
        yourFunc: () => {
            console.log(this)	// window
        }
    }
})
```

- Vue 함수 객체 내에서 vue 인스턴스를 가리킴

<br>

