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

# 🪴 Vue 기본 문법

[Vue 인스턴스](https://kr.vuejs.org/v2/guide/instance.html)

<hr>



