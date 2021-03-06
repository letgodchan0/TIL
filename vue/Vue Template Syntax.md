# ๐ฐVue Template Syntax

> ๋ ๋๋ง ๋ DOM์ ๊ธฐ๋ณธ Vue ์ธ์คํด์ค์ ๋ฐ์ดํฐ์ ์ ์ธ์ ์ผ๋ก ๋ฐ์ธ๋ฉํ  ์ ์๋ HTML ๊ธฐ๋ฐ ํํ๋ฆฟ ๊ตฌ๋ฌธ์ ์ฌ์ฉ

1. [Text Interpolation (๋ณด๊ฐ๋ฒ)์ ๊ณต์๋ฌธ์ ํ์ธํ์!](https://kr.vuejs.org/v2/guide/syntax.html#%EB%AC%B8%EC%9E%90%EC%97%B4)
2. [Directive ์ฐธ๊ณ ํ๊ธฐ](https://kr.vuejs.org/v2/api/#%EB%94%94%EB%A0%89%ED%8B%B0%EB%B8%8C)



<br>

## ๐ชดDirective (๋๋ ํฐ๋ธ)

- `v-` ์ ๋์ฌ๊ฐ ์๋ ํน์ ์์ฑ
- ์์ฑ ๊ฐ์ ๋จ์ผ JS ํํ์์ด ๋จ (v-for๋ ์์ธ)
- ํํ์์ ๊ฐ์ด ๋ณ๊ฒฝ๋  ๋ ๋ฐ์์ ์ผ๋ก DOM์ ์ ์ํ๋ ์ญํ ์ ํจ

<hr>

1. ์ ๋ฌ์ธ์ (Arguments)

```html
<a v-bind:href="url">...</a>
<a v-on:click="doSomething">...</a>
```

- `:` (์ฝ๋ก )์ ํตํด ์ ๋ฌ์ธ์๋ฅผ ๋ฐ์ ์ ๋ ์์

2. ์์์ด

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

- `.` (์ )์ผ๋ก ํ์๋๋ ํน์ ์ ๋ฏธ์ฌ
- ๋๋ ํฐ๋ธ๋ฅผ ํน๋ณํ ๋ฐฉ๋ฒ์ผ๋ก ๋ฐ์ธ๋ฉ ํด์ผ ํจ์ ๋ํ๋!!

<br>



## ๐ชด v-text

```html
<div id="app">
    <span v-text="msg"></span>
	<!-- ๊ฐ์ต๋๋ค -->
	<span>{{msg}}</span>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello',
        }
    })
</script>
```

- ๋ด๋ถ์ ์ผ๋ก ๋ณด๊ฐ๋ฒ `{{ msg }}` ์ด `v-text`๋ก ์ปดํ์ผ ๋จ 

<br>

## ๐ชด v-html

```html
<div id="app">
    <div v-html="myHtml"></div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            myHtml: '<strong>๊ฐํ๋ท</strong>'
        }
    })
</script>
```

- ๊ธฐ๋ณธ์ ์ผ๋ก ์๋ฆฌ๋จผํธ์ innerHTML์ ์๋ฐ์ดํ ํ๋๋ฐ, ์น ์ฌ์ดํธ์์ ์์์ HTML์ ๋์ ์ผ๋ก ๋ ๋๋งํ๋ฉด XSS ๊ณต๊ฒฉ์ผ๋ก ์ด์ด์ง ์ ์์ผ๋ฏ๋ก ๋งค์ฐ ์ํํ  ์ ์๋ค. ๋ฐ๋ผ์ ์ ๋ขฐํ  ์ ์๋ ์ปจํ์ธ ์๋ง `v-html`์ ์ฌ์ฉํ๊ณ  ์ฌ์ฉ์๊ฐ ์ ๊ณตํ ์ปจํ์ธ ์๋ `์ ๋๋ก` ์ฌ์ฉํ์ง ๋ง์!

<br>

## ๐ชด v-show

```html
<div id="app">
    <p v-show="isTrue">
        true
    </p>
    <p v-show="isFalse">
        false
    </p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            isTrue: true,
            isFalse: false,
        }
    })
</script>
```

- ์กฐ๊ฑด๋ถ ๋ ๋๋ง ์ค ํ๋
- ์์๋ ํญ์ ๋ ๋๋ง ๋๊ณ  DOM์ ๋จ์ ์์
- ๋จ์ํ ์๋ฆฌ๋จผํธ์ display CSS ์์ฑ์ ํ ๊ธํ๋ ๊ฒ

<br>

## ๐ชด v-if, v-else-if, v-else

```html
<div id="app">
  <!-- 1 -->
  <div v-if="seen">seen์ด true์ผ๋๋ง ๋ ๋๋ง.</div>
  
  <!-- 2 -->
  <div v-if="myType === 'A'">
    A
  </div>
  <div v-else-if="myType === 'B'">
    B
  </div>
  <div v-else-if="myType === 'C'">
      C
  </div>
  <div v-else>
      Not A/B/C
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  const app = new Vue({
      el: '#app',
      data: {
          seen: false,
          myType: 'A',
      }
  })
</script>
```

- ์กฐ๊ฑด๋ถ ๋ ๋๋ง ์ค ํ๋๋ก ์กฐ๊ฑด์ ๋ฐ๋ผ ์์๋ฅผ ๋ ๋๋ง ํ๋ค.
- ๋๋ ํฐ๋ธ์ ํํ์์ด true์ผ ๋๋ง ๋ ๋๋ง
- ์๋ฆฌ๋จผํธ ๋ฐ ํฌํจ๋ ๋๋ ํฐ๋ธ๋ ํ ๊ธํ๋ ๋์ ์ญ์ ๋๊ณ  ๋ค์ ์์๋จ

<br>

## ๐ณ v-show ์ v-if

- v-show (Expensive initial load, cheap toggle)
  - CSS display ์์ฑ์ hidden์ผ๋ก ๋ง๋ค์ด ํ ๊ธ
  - ์ค์ ๋ก ๋ ๋๋ง์ ๋์ง๋ง ๋์์ ๋ณด์ด์ง ์๋ ๊ฒ์ด๊ธฐ ๋๋ฌธ์ ๋ฑ ํ ๋ฒ๋ง ๋ ๋๋ง์ด ๋๋ ๊ฒฝ์ฐ๋ผ๋ฉด `v-if`์ ๋นํด ์๋์ ์ผ๋ก ๋ ๋๋ง ๋น์ฉ์ด ๋์
  - ํ์ง๋ง, ์์ฃผ ๋ณ๊ฒฝ๋๋ ์์๋ผ๋ฉด ํ ๋ฒ ๋ ๋๋ง ๋ ์ดํ๋ถํฐ๋ ๋ณด์ฌ์ฃผ๋์ง์ ๋ํ ์ฌ๋ถ๋ง ํ๋จํ๋ฉด ๋๊ธฐ ๋๋ฌธ์ ํ ๊ธ ๋น์ฉ์ด ์ ์
- v-if (Cheap initial load, expensive toggle)
  - ์ ๋ฌ์ธ์๊ฐ false์ธ ๊ฒฝ์ฐ ๋ ๋๋ง ๋์ง ์์
  - ํ๋ฉด์์ ๋ณด์ด์ง ์์ ๋ฟ๋ง ์๋๋ผ ๋ ๋๋ง ์์ฒด๊ฐ ๋์ง ์๊ธฐ ๋๋ฌธ์ ๋ ๋๋ง ๋น์ฉ์ด ๋ฎ์
  - ํ์ง๋ง, ์์ฃผ ๋ณ๊ฒฝ๋๋ ์์์ ๊ฒฝ์ฐ ๋ค์ ๋ ๋๋ง ํด์ผ ํ๋ฏ๋ก ๋น์ฉ์ด ์ฆ๊ฐํ  ์ ์์ 

<br>

## ๐ชด v-for

```html
<div id="app">
    <div v-for="fruit in fruits">
        {{ fruit }}
    </div>
    
    <div v-for="(fruit, index) in fruits" :key="'fruit-${index}'">
        {{ fruit }}
    </div>
    
    <div v-for="todo in todos" :key="todo.id">
        {{ todo.title }} : {{ todo.completed }}
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            fruits: ['apple', 'banana', 'coconut'],
            todos: [
                { id: 1, title: 'todo1', completed: true },
                { id: 2, title: 'todo2', completed: false },
                { id: 3, title: 'todo3', completed: true },
            ]
        }
    })
</script>
```

- ์๋ณธ ๋ฐ์ดํฐ๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ์๋ฆฌ๋จผํธ ๋๋ ํํ๋ฆฟ ๋ธ๋ก์ ์ฌ๋ฌ๋ฒ ๋ ๋๋งํ๋ค. ๋๋ ํฐ๋ธ์ ๊ฐ์ ๋ฐ๋ณต๋๋ ํ์ฌ ์๋ฆฌ๋จผํธ์ ๋ํ ๋ณ์นญ์ ์ ๊ณตํ๊ธฐ ์ํด ํน์ ๊ตฌ๋ฌธ์ธ `alias in expression`์ ์ฌ์ฉํด์ผ ํ๋ค.
  - item in items ๊ตฌ๋ฌธ์ ์ฌ์ฉํด์ผ ํจ
- item ์์น์ ๋ณ์๋ฅผ ๊ฐ ์์์์ ์ฌ์ฉํ  ์ ์์
  - ๊ฐ์ฒด์ ๊ฒฝ์ฐ๋ key
- `v-for` ์ฌ์ฉ ์ ๋ฐ๋์ key ์์ฑ์ ๊ฐ ์์์ ์์ฑํ์!!!
- `v-if` ์ ํจ๊ป ์ฌ์ฉํ๋ ๊ฒฝ์ฐ `v-for`๊ฐ ์ฐ์ ์์๊ฐ ๋ ๋์ง๋ง, ๊ฐ๋ฅํ๋ฉด ๋์์ ์ฌ์ฉํ์ง ๋ง ๊ฒ!!

<br>

## ๐ชด v-on

```html
<div id="app">
  <!-- ๋ฉ์๋ ํธ๋ค๋ฌ -->
  <button v-on:click="doAlert">Button</button>
  <button @click="doAlert">Button</button>
  <button @click="onInputEnter">Button</button>
    
  <!-- ๊ธฐ๋ณธ ๋์ ๋ฐฉ์ง -->
  <form action="/articles/" @submit.prevent>
      <button>Submit</button>
  </form>
    
  <!-- ํค ๋ณ์นญ์ ์ด์ฉํ ํค ์๋ ฅ ์์์ด -->
  <input @keyup.enter="onEnter">
  <input @keyup.enter="onEnter(something)">
    
  {{ message }}
  <button @click="changeMessage">Button</button>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const app = new Vue({
      el: '#app',
      data: {
          message: 'Hello Vue.js',
      },
      methods: {
          doAlert: function () {
              alert('hello!')
          },
          onEnter: function (something) {
              console.log(event)
              console.log(event.target.value)
              console.log(something)
          },
          onInputEnter: function (onInputValue) {
              console.log(onInputValue)
          },
          changeMessage: function () {
              this.message = "New message!"
          }
      }
  })
</script>
```

- ์๋ฆฌ๋จผํธ์ ์ด๋ฒคํธ ๋ฆฌ์ค๋๋ฅผ ์ฐ๊ฒฐ
- ์ด๋ฒคํธ ์ ํ์ ์ ๋ฌ์ธ์๋ก ํ์ํจ
- ํน์  ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ์ ๋, ์ฃผ์ด์ง ์ฝ๋๊ฐ ์คํ ๋จ
- `v-on:click` === `@click`

<br>

## ๐ชด v-bind

```html
<div id="app">
  <!-- ์์ฑ ๋ฐ์ธ๋ฉ -->
  <img v-bind:src="imageSrc" :alt="altMsg">
  <img :src="imageSrc" alt="altMsg">
  <hr>

  <!-- ํด๋์ค ๋ฐ์ธ๋ฉ -->
  <div :class="{ active: isRed }">
      ํด๋์ค ๋ฐ์ธ๋ฉ
  </div>

  <h3 :class="[activeRed, myBackground]">
      hello vue
  </h3>
  <hr>

  <!-- ์คํ์ผ ๋ฐ์ธ๋ฉ -->
  <ul>
      <li v-for="todo in todos" :class="{ active: todo.isActive }" : style="{ fontsize: fontSize + 'px' }">
          {{ todo }}
      </li>
  </ul>
</div>
  
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      fontSize: 16,
      altMsg: 'this is image',
      imageSrc: 'https://picsum.photos/200/300/',
      isRed: true,
      activeRed: 'active',
      myBackground: 'my-background-color',
      todos: [
        { id: 1, title: 'todo 1', isActive: true },
        { id: 2, title: 'todo 2', isActive: false },
      ]
    }
  })

</script>
```

- HTML ์์์ ์์ฑ์ Vue์ ์ํ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ผ๋ก ํ ๋น
- Object ํํ๋ก ์ฌ์ฉํ๋ฉด value๊ฐ true์ธ key๊ฐ class ๋ฐ์ธ๋ฉ ๊ฐ์ผ๋ก ํ ๋น
- `:`
  - `v-bind:href` === `:href`

<br>

## ๐ชด v-model

```html
<div id="app">
    <h2>1. Input -> Data ๋จ๋ฐฉํฅ</h2>
    <h3>{{ myMessage }}</h3>
    <input @input="onInputChange" type="text">
    <hr>
    
    <h2>2. Input <-> Data ์๋ฐฉํฅ</h2>
    <h3>{{ myMessage2 }}</h3>
    <input v-model="myMessage2" type="text">
    <hr>
    
    <h2>3. Checkbox</h2>
    <input type="checkbox" id="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>
        
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            myMessage: '',
            myMessage2: '',
            checkd: true,
        },
        methods: {
            onInputChange: function (event) {
                this.myMessage = event.target.value
            }
        }
    })
</script>
```

- ์ฌ์ฉ์์ ์๋ ฅ์ ๋ฐ๋ UI ์์๋ค์ `v-model` ์์ฑ์ ์ฌ์ฉํ๋ฉด ์๋ ฅ ๊ฐ์ด ์๋์ผ๋ก ๋ทฐ ๋ฐ์ดํฐ ์์ฑ์ ์ฐ๊ฒฐ๋๋ค.
- HTML form ์์์ ๊ฐ๊ณผ data๋ฅผ ์๋ฐฉํฅ ๋ฐ์ธ๋ฉ
- ์์์ด
  - `.lazy` - input ๋์  change ์ด๋ฒคํธ๋ฅผ ๋ฃ๋๋ค!
  - `.number` - ๋ฌธ์์ด์ ์ซ์๋ก ๋ณ๊ฒฝ
  - `.trim` -  ์๋ ฅ์ ๋ํ trim์ ์งํ

<br>



## ๐ชด Options/Data - 'computed'

```html
<div id="app">
    <p>{{ num }}</p>    
    <p>{{ doubleNum }}</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            num: 2,
        },
        computed: {
            doubleNum: function () {
                return this.num * 2
            }
        },
    })
</script>
```

- ๋ฐ์ดํฐ๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ํ๋ ๊ณ์ฐ๋ ์์ฑ
- ํจ์์ ํํ๋ก ์ ์ํ์ง๋ง ํจ์๊ฐ ์๋ ํจ์์ ๋ฐํ ๊ฐ์ด ๋ฐ์ธ๋ฉ ๋จ
- ์ข์๋ ๋ฐ์ดํฐ์ ๋ฐ๋ผ ์ ์ฅ(์บ์ฑ)๋จ
- ์ข์๋ ๋ฐ์ดํฐ๊ฐ ๋ณ๊ฒฝ๋  ๋๋ง ํจ์๋ฅผ ์คํ
- ์ฆ, ์ด๋ค ๋ฐ์ดํฐ์๋ ์์กดํ์ง ์๋ computed ์์ฑ์ ๊ฒฝ์ฐ ์ ๋๋ก ์๋ฐ์ดํธ ๋์ง ์์
- ๋ฐ๋์ ๋ฐํ ๊ฐ์ด ์์ด์ผ ํจ

<br>

## ๐ณ computed & methods

- computed ์์ฑ ๋์  methods์ ํจ์๋ฅผ ์ ์ํ  ์ ์๊ณ  ์ต์ข ๊ฒฐ๊ณผ์ ๋ํด ๋ ๊ฐ์ง ์ ๊ทผ ๋ฐฉ์์ ์๋ก ๋์ผํ๋ค.
- ์ฐจ์ด์ ์ computed ์์ฑ์ ์ข์ ๋์์ ๋ฐ๋ผ ์ ์ฅ(์บ์ฑ)๋๋ค. ์ฆ, computed๋ ์ข์๋ ๋์์ด ๋ณ๊ฒฝ๋์ง ์๋ ํ computed์ ์์ฑ๋ ํจ์๋ฅผ ์ฌ๋ฌ ๋ฒ ํธ์ถํด๋ ๊ณ์ฐ์ ๋ค์ ํ์ง ์๊ณ  ๊ณ์ฐ๋์ด ์๋ ๊ฒฐ๊ณผ๋ฅผ ๋ฐํํ๋ค.
- ์ด์ ๋นํด methods๋ฅผ ํธ์ถํ๋ฉด ๋ ๋๋ง์ ๋ค์ ํ  ๋๋ง๋ค ํญ์ ํจ์๋ฅผ ์คํํ๋ค.

<br>

## ๐ชด Options/Data - 'watch'

```html
<div id="app">
    <p>{{ num }}</p>    
    <button @click="num += 1">add 1</button>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            num: 2,
        },
        watch: {
            num: function () {
                console.log(`${this.num}์ด ๋ณ๊ฒฝ๋์์ต๋๋ค.`)
            }
        },
    })
</script>
```

- ๋ฐ์ดํฐ๋ฅผ ๊ฐ์
- ๋ฐ์ดํฐ์ ๋ณํ๊ฐ ์ผ์ด๋ฌ์ ๋ ์คํ๋๋ ํจ์

<br>

## ๐ณ computed & watch

- computed
  - ํน์  ๋ฐ์ดํฐ๋ฅผ ์ง์ ์ ์ผ๋ก ์ฌ์ฉ/๊ฐ๊ณตํ์ฌ ๋ค๋ฅธ ๊ฐ์ผ๋ก ๋ง๋ค ๋ ์ฌ์ฉ
  - ์์ฑ์ ๊ณ์ฐํด์ผ ํ๋ ๋ชฉํ ๋ฐ์ดํฐ๋ฅผ ์ ์ํ๋ ๋ฐฉ์์ผ๋ก ์ํํธ์จ์ด ๊ณตํ์์ ์ด์ผ๊ธฐํ๋ '์ ์ธํ ํ๋ก๊ทธ๋๋ฐ ๋ฐฉ์'
  - "ํน์  ๊ฐ์ด ๋ณ๋ํ๋ฉด ํด๋น ๊ฐ์ ๋ค์ ๊ณ์ฐํด์ ๋ณด์ฌ์ค๋ค."
- watch
  - ํน์  ๋ฐ์ดํฐ์ ๋ณํ ์ํฉ์ ๋ง์ถฐ ๋ค๋ฅธ data ๋ฑ์ด ๋ฐ๋์ด์ผ ํ  ๋ ์ฃผ๋ก ์ฌ์ฉ
  - ๊ฐ์ํ  ๋ฐ์ดํฐ๋ฅผ ์ง์ ํ๊ณ  ๊ทธ ๋ฐ์ดํฐ๊ฐ ๋ฐ๋๋ฉด ํน์  ํจ์๋ฅผ ์คํํ๋ ๋ฐฉ์
  - ์ํํธ์จ์ด ๊ณตํ์์ ์ด์ผ๊ธฐํ๋ '๋ช๋ นํ ํ๋ก๊ทธ๋๋ฐ' ๋ฐฉ์
  - ํน์  ๋์์ด ๋ณ๊ฒฝ๋์์ ๋ ์ฝ๋ฐฑ ํจ์๋ฅผ ์คํ์ํค๊ธฐ ์ํ ํธ๋ฆฌ๊ฑฐ
  - "ํน์  ๊ฐ์ด ๋ณ๋ํ๋ฉด ๋ค๋ฅธ ์์์ ํ๋ค."
- [๊ณต์๋ฌธ์์์ ์ฐพ์๋ณด๊ธฐ](https://kr.vuejs.org/v2/guide/computed.html#computed-%EC%86%8D%EC%84%B1-vs-watch-%EC%86%8D%EC%84%B1)
  - ์ด๋ค ๊ฒ์ด ๋ ์ฐ์ํ ๊ฒ์ด ์๋ ์ฌ์ฉํ๋ ๋ชฉ์ ๊ณผ ์ํฉ์ด ๋ค๋ฆ 

<br>

## ๐ชด Options/Assets - 'filter'

```html
<div id="app">
  {{ numbers | getOddNumbers | getUnderTen }}
  {{ getOddAndUnderTen }}
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  const app = new Vue({
      el: '#app',
      data: {
          numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
      },
      filters: {
          getOddNumbers(array) {
              return array.filter(num => num % 2)
          },
          getUnderTen(array) {
              return array.filter(num => num < 10)
          }
      },
      // w/ computed
      computed: {
          getOddAndUnderTen() {
              return this.numbers.filter(num => num % 2 && num < 10)
          }
      }
  })
</script>
```

- ํ์คํธ ํ์ํ๋ฅผ ์ ์ฉํ  ์ ์๋ ํํฐ
- ๋ณด๊ฐ๋ฒ ํน์ `v-bind`๋ฅผ ์ด์ฉํ  ๋ ์ฌ์ฉ ๊ฐ๋ฅ
- ํํฐ๋ ์๋ฐ์คํฌ๋ฆฝํธ ํํ์ ๋ง์ง๋ง์ "|" (ํ์ดํ)์ ํจ๊ป ์ถ๊ฐ๋์ด์ผ ํจ
- ์ด์ด์ ์ฌ์ฉ(chaining) ๊ฐ๋ฅ

<br>

## ๐ชดLifecycle Hooks

![image-20220510023322555](Vue%20Template%20Syntax.assets/image-20220510023322555.png)

```html
<div id="app">
  <img v-if="imgSrc" :src="imgSrc" alt="sample img">
  <button @click="getImg">GetDog</button>
</div>

<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  const API_URL = 'https://dog.ceo/api/breeds/image/random'
  const app = new Vue({
      el: '#app',
      data: {
          imgSrc: '',
      },
      methods: {
          getImg: function () {
              axios.get(API_URL)
                  .then(response => {
                  this.imgSrc = response.data.message
              })
          }
      },
      // ์ธ๋ถ API์์ ์ด๊ธฐ ๋ฐ์ดํฐ ๋ฐ์์ค๊ธฐ
      created: function () {
          this.getImg()
      }
  })
</script>
```

- ๊ฐ Vue ์ธ์คํด์ค๋ ์์ฑ๋  ๋ ์ผ๋ จ์ ์ด๊ธฐํ ๋จ๊ณ๋ฅผ ๊ฑฐ์นจ
  - ๋ฐ์ดํฐ ๊ด์ฐฐ ์ค์ ์ด ํ์ํ ๊ฒฝ์ฐ, ์ธ์คํด์ค๋ฅผ DOM์ ๋ง์ดํธ ํ๋ ๊ฒฝ์ฐ, ๋ฐ์ดํฐ๊ฐ ๋ณ๊ฒฝ๋์ด DOM๋ฅผ ์๋ฐ์ดํธ ํ๋ ๊ฒฝ์ฐ ๋ฑ
- ๊ทธ ๊ณผ์ ์์ ์ฌ์ฉ์ ์ ์ ๋ก์ง์ ์คํํ  ์ ์๋ `Lifecycle Hooks`๋ ํธ์ถ๋จ
- ์๋๋ ํด๋ฆญ์ ํด์ผ ์ด๋ฏธ์ง๋ฅผ ๊ฐ์ ธ์ค์ง๋ง, created๋ฅผ ์ฌ์ฉํด ์ ํ๋ฆฌ์ผ์ด์์ ์ด๊ธฐ ๋ฐ์ดํฐ๋ฅผ API ์์ฒญ์ ํตํด ๋ถ๋ฌ์ฌ ์ ์์
- [๊ณต์๋ฌธ์๋ฅผ ํตํด ๊ฐ ๋ผ์ดํ์ฌ์ดํด ํ์ ์์ธ ๋์์ ์ฐธ๊ณ ํ์!](https://kr.vuejs.org/v2/guide/instance.html#%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%ED%9B%85)