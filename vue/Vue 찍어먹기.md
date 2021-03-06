# ๐ฐVue ์ฐ์ด๋จน๊ธฐ

<br>

[Vue ๊ณต์๋ฌธ์ ์์ํ๊ธฐ](https://kr.vuejs.org/v2/guide/index.html) ๋ถํฐ ์ฐ๋จน ํด๋ณด์!

<hr>

1. CDN ์์ฑ

```html
<!-- ๊ฐ๋ฐ๋ฒ์ , ๋์๋๋ ์ฝ์ ๊ฒฝ๊ณ ๋ฅผ ํฌํจ. -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

๋๋,

<!-- ์์ฉ๋ฒ์ , ์๋์ ์ฉ๋์ด ์ต์ ํ๋จ. -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

<br>

2. ์ ์ธ์  ๋ ๋๋ง

```html
<div id="app">
  {{ message }}
</div>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: '์๋ํ์ธ์ Vue!'
  }
})
```

<br>

3. Element ์์ฑ ๋ฐ์ธ๋ฉ

```html
<h2>Element ์์ฑ ๋ฐ์ธ๋ฉ</h2>
<div id="app2">
    <span v-bind: title="message">
      ๋ด ์์ ์ ์ ๋ง์ฐ์ค๋ฅผ ์ฌ๋ฆฌ๋ฉด ๋์ ์ผ๋ก ๋ฐ์ธ๋ฉ ๋ title์ ๋ณผ ์ ์์ต๋๋ค!
    </span>
</div>
```

```js
const app2 = new Vue({
    el: '#app2',
    data: {
        message: '์ด ํ์ด์ง๋ ' + new Date() + ' ์ ๋ก๋ ๋์์ต๋๋ค'
    }
})
```

<br>

4. ์กฐ๊ฑด๋ฌธ

```html
<h2>์กฐ๊ฑด</h2>
<div id="app3">
  <p v-if="seen">์ด์  ๋๋ฅผ ๋ณผ ์ ์์ด์</p>
</div>
```

```js
const app3 = new Vue({
    el: '#app3',
    data: {
        seen: true // false๋ก ํ ๊ธ ๊ฐ๋ฅ
    }
})
```

<br>

5. ๋ฐ๋ณต๋ฌธ

```html
<h2>๋ฐ๋ณต</h2>
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
          { text: 'JavaScript ๋ฐฐ์ฐ๊ธฐ' },
          { text: 'Vue ๋ฐฐ์ฐ๊ธฐ' },
          { text: '๋ฌด์ธ๊ฐ ๋ฉ์ง ๊ฒ์ ๋ง๋ค๊ธฐ' }
        ]
    }
})
```

<br>

6. ์ฌ์ฉ์ ์๋ ฅ ํธ๋ค๋ง

```html
<h2>์ฌ์ฉ์ ์๋ ฅ ํธ๋ค๋ง</h2>
<div id="app5">
    <p>{{ message }}</p>
    <button v-on: click="reverseMessage">๋ฉ์์ง ๋ค์ง๊ธฐ</button>
</div>
```

```js
const app5 = new Vue({
    el: '#app5',
    data: {
        message: '์๋ํ์ธ์! Vue.js!'
    },
    methods: {
        reverseMessage: function () {
            this.message = this.message.split('').reverse().join('')
        }
    }
})
```

<br>

7. ์ฌ์ฉ์ ์๋ ฅ ํธ๋ค๋ง

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
        message: '์๋ํ์ธ์ Vue!'
    }
})
```

<br>

# ๐ฐ Vue ๊ธฐ๋ณธ ๋ฌธ๋ฒ

[Vue ์ธ์คํด์ค](https://kr.vuejs.org/v2/guide/instance.html)

<hr>

## ๐ชด Vue instance

```js
const app = new Vue({
    
})
```

- ๋ชจ๋  Vue ์ฑ์ Vue ํจ์๋ก ์ ์ธ์คํด์ค๋ฅผ ๋ง๋๋ ๊ฒ๋ถํฐ ์์!
- Vue ์ธ์คํด์ค๋ฅผ ์์ฑํ  ๋๋ Options ๊ฐ์ฒด๋ฅผ ์ ๋ฌํด์ผ ํจ
- ์ฌ๋ฌ Options๋ค์ ์ฌ์ฉํ์ฌ ์ํ๋ ๋์์ ๊ตฌํ
- [์ ์ฒด ์ต์ ๋ชฉ๋ก](https://kr.vuejs.org/v2/api/#propsData)

<br>

## ๐ชด Options/DOM - 'el'

```js
const app = new Vue({
    el: '#app'
})
```

- Vue ์ธ์คํด์ค์ ์ฐ๊ฒฐ(๋ง์ดํธ)ํ  ๊ธฐ์กด DOM ์์๊ฐ ํ์!
- CSS ์ ํ์ ๋ฌธ์์ด ํน์ HTML Element๋ก ์์ฑ
- new๋ฅผ ์ด์ฉํ ์ธ์คํด์ค ์์ฑ ๋๋ง ์ฌ์ฉ

<br>

## ๐ชด Options/Data - 'data'

```js
const app = new Vue({
    el: '#app',
    data: {
        message: 'Hello',
    }
})
```

- Vue ์ธ์คํด์ค์ ๋ฐ์ดํฐ ๊ฐ์ฒด๋ก Vue ์ธ์คํด์ค์ ์ํ ๋ฐ์ดํฐ๋ฅผ ์ ์ํ๋ ๊ณณ
- Vue template์์ interpolation์ ํตํด ์ ๊ทผ ๊ฐ๋ฅ
- `v-bind`, `v-on`๊ณผ ๊ฐ์ ๋๋ ํฐ๋ธ ํํ์์์๋ ์ฌ์ฉ ๊ฐ๋ฅ
- Vue ๊ฐ์ฒด ๋ด ๋ค๋ฅธ ํจ์์์ this ํค์๋๋ฅผ ํตํด ์ ๊ทผ ๊ฐ๋ฅ

<br>

## ๐ชด Options/Data - 'methods'

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

- Vue ์ธ์คํด์ค์ ์ถ๊ฐํ  ๋ฉ์๋
- Vue template์์ interpolation์ ํตํด ์ ๊ทผ ๊ฐ๋ฅ
- `v-on`๊ณผ ๊ฐ์ ๋๋ ํฐ๋ธ ํํ์์์๋ ์ฌ์ฉ ๊ฐ๋ฅ
- Vue ๊ฐ์ฒด ๋ด ๋ค๋ฅธ ํจ์์์ this ํค์๋๋ฅผ ํตํด ์ ๊ทผ ๊ฐ๋ฅ
- [์ฃผ์!]
  - ํ์ดํ ํจ์๋ฅผ ๋ฉ์๋๋ฅผ ์ ์ํ๋๋ฐ ์ฌ์ฉํ๋ฉด ์๋ฉ๋๋ค!!
  - ํ์ดํ ํจ์๊ฐ ๋ถ๋ชจ ์ปจํ์คํธ๋ฅผ ๋ฐ์ธ๋ฉํ๊ธฐ ๋๋ฌธ์ this๋ Vue ์ธ์คํด์ค๊ฐ ์๋.

<br>

## ๐ชด 'this' in Vue.js

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

- Vue ํจ์ ๊ฐ์ฒด ๋ด์์ vue ์ธ์คํด์ค๋ฅผ ๊ฐ๋ฆฌํด

<br>

