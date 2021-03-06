# ๐ฐVuex Todo App ๊ตฌํ

vuex๋ฅผ ํ์ฉํด์ ๊ฐ๋จํ Todo App์ ๊ตฌํํด๋ณด๋ ค๊ณ  ํ๋ค. ์ปดํฌ๋ํธ๋ ์๋์ ๊ฐ์ด ๊ตฌ์ฑ!

![image-20220511235546321](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220511235546321.png)

<br>



## ๐ก ํ๋ก์ ํธ ์ธํ

#### 1. ํ๋ก์ ํธ ์์ฑ

```bash
$ vue create todo-vuex-app
$ cd todo-vuex-app
```

<br>

#### 2. Vuex ์ถ๊ฐ

```bash
$ vue add vuex
```

<br>

#### 3. commit ์ฌ๋ถ (Yes)

![image-20220512000806072](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512000806072.png)

<br>

#### 4. ์๋ฒ ์คํ

```bash
$ npm run serve
```

![image-20220512001232557](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512001232557.png)

<br>

## ๐ก ์ปดํฌ๋ํธ ์์ฑ

<hr>

### 1. TodoListItem.vue

![image-20220512002329813](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512002329813.png)

- ๊ฐ๋ณ todo๋ค์ ์ปดํฌ๋ํธ
- `TodoList` ์ปดํฌ๋ํธ์ ์์ ์ปดํฌ๋ํธ

<br>

### 2. TodoList.vue

![image-20220512002620820](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512002620820.png)

- todo ๋ชฉ๋ก ์ปดํฌ๋ํธ
- TodoListItem ์ปดํฌ๋ํธ์ ๋ถ๋ชจ ์ปดํฌ๋ํธ
- `import TodoListItem from '@/components/TodoListItem'` 
  - ์์ ์ปดํฌ๋ํธ๋ฅผ ๋ฑ๋กํ๊ธฐ ์ํด ํธ์ถ 
  - @๋ src ํด๋๋ฅผ ์๋ฏธ

<br>

### 3. TodoForm.vue

![image-20220512002805349](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512002805349.png)

- todo ๋ฐ์ดํฐ๋ฅผ ์๋ ฅ๋ฐ๋ ์ปดํฌ๋ํธ

<br>

### 4. App.vue

![image-20220512002933862](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512002933862.png)

- ์ต์์ ์ปดํฌ๋ํธ
- TodoList, TodoForm ์ปดํฌ๋ํธ์ ๋ถ๋ชจ ์ปดํฌ๋ํธ

- ์ ๋๊ฒฝ๋ก, ์๋๊ฒฝ๋ก ๊ฐ๊ฐ ํธ์ถํ๋ ๋ฒ์ ๋ณด์ฌ์ฃผ๊ธฐ ์ํด ๊ฐ๊ฐ ์์ฑํ์ง๋ง ๋ณดํต์ ํ๋์ ๋ฐฉ์์ผ๋ก ์ ํจ!

<br>

# ๐ Create Todo

<hr>

##  ๐ก State ์์ฑ

![image-20220512003849160](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512003849160.png)

- state๋ ๋ฐ์ดํฐ๊ฐ ๋ค์ด๊ฐ๋ ๊ณณ์ด๋ค. ์ฒดํฌํ๋ฉด์ ํ๊ธฐ ์ํด `todos`๋ผ๋ ๋ฐฐ์ด์ ๋ง๋ค๊ณ  ๋ฐ์ดํฐ๋ฅผ ์ง์  ์๋ ฅํด์ค๋ค!
- [new Date().getTime() -> ๊ณต์๋ฌธ์ ์ฐธ๊ณ ](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)
  - new Date() : 1970๋ 1์1์ผ 0์ 0๋ถ 0์ด๋ก๋ถํฐ ์์ํด์ ์ง๊ธ๊น์ง ๋ช ms๋ก ์ง๋ฌ๋์ง ํ์ฌ ์๊ฐ์ ์๋ ค์ค๋ค.
  - getTime() : Date ํ์์ ์๊ฐ์ ๋ฐ๋ฆฌ์ด๋ก ํ์ฐํด์ ๊ฐ์ ธ์จ๋ค
- [์ฃผ์] Vuex๋ฅผ ์ฌ์ฉํ๋ค๊ณ  ํด์ Vuex Store์ ๋ชจ๋  ์ํ๋ฅผ ๋ฃ์ด์ผ ํ๋ ๊ฒ์ ์๋

<br>

## ๐ก TodoList ๋ฐ์ดํฐ ๊ฐ์ ธ์ค๊ธฐ

![image-20220512004730865](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512004730865.png)

- ์ปดํฌ๋ํธ์์ Vuex Store์ state์ ์ ๊ทผ
  - `$store.state` : store/index.js์ state์์ ์ ์ํ todos๋ฅผ ํ์ฌ ํ์ผ์ธ TodoList.vue์์ ์ฌ์ฉํ๊ธฐ ์ํ ๋ฐฉ๋ฒ์ด๋ค. 
  - [v-for="todo in ~ todos" :key="todo.date"](https://github.com/letgodchan0/TIL/blob/main/vue/Vue%20Template%20Syntax.md) : v-for ์ฌ์ฉ๋ฒ!!

<br>

## ๐ก Computed๋ก ๋ณ๊ฒฝ

![์ ๋ชฉ ์์](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/%EC%A0%9C%EB%AA%A9%20%EC%97%86%EC%9D%8C-16522848472591.jpg)

- ํ์ฌ state์ todo๋ ๊ฐ์ด ๋ณํํ๋ ๊ฒ์ด ์๋
- store์ ์ ์ฅ๋ todo ๋ชฉ๋ก์ ๊ฐ์ ธ์ค๋ ๊ฒ์ด๊ธฐ ๋๋ฌธ์ ๋งค๋ฒ ์๋ก ํธ์ถํ๋ ๊ฒ์ ๋นํจ์จ์ !!
- ๋ฐ๋ผ์ todo๊ฐ ์ถ๊ฐ ๋๋ ๋ฑ์ ๋ณ๊ฒฝ ์ฌํญ์ด ์์ ๋๋ง ์๋ก ๊ณ์ฐํ ๊ฐ์ ๋ฐํํ๋ ๋ฐฉํฅ์ผ๋ก ๋ณ๊ฒฝ -> [computed](https://kr.vuejs.org/v2/guide/computed.html)
- `this(Vue Instance)`๋ก ์ ๊ทผ

<br>

## ๐กProps (TodoList -> Todo)

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522853660252.jpg)

- TodoList์ ์๋ Todo๋ค์ ํ๋์ฉ ๋๊ฒจ์ฃผ๊ธฐ!

<br>

## ๐กActions & Mutations

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522867864883.jpg)

<hr>

์ฌ๊ธฐ์ ํ๋๊ฒ ๋ญ๋๋ฉด, `TodoForm.vue`์์ ์ฌ์ฉ์๊ฐ ์๋ ฅ ์นธ์ ์ค๋ ํ  ์ผ์ ์์ฑํด์ ์ํฐ๋ฅผ ๋๋ฅด๊ฑฐ๋, Add ๋ฒํผ์ ๋๋ฅด๋ฉด (1) `createTodo` ๋ฉ์๋๊ฐ ์คํ๋๋ค. (2) ์ด ๋ฉ์๋๋  `todoItem` ์ด๋ผ๋ ๊ฐ์ฒด ์์ ์ ๋ฌํ  ๋ด์ฉ (title, isCompleted, date)์ ์ ์ํ๊ณ  ํนํ, ์ฌ์ฉ์๊ฐ ์์ฑํ ๋ด์ฉ์ title์ ๋ด์์ค๋ค. ๊ทธ๋ฆฌ๊ณ  ์ฌ์ฉ์๊ฐ ์์ฑ์ ํ๋ค๋ฉด `this.$store.dispatch('createTodo', todoItem)`์ ํตํด `action`์ `createTodo`๊ฐ ์คํ๋๊ณ  `todoItem`์ ๊ฐ์ด ๋๊ฒจ์ค๋ค.

- [v-model๊ณผ data](https://kr.vuejs.org/v2/guide/components.html#%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%9D%98-v-model-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%A0%95%EC%9D%98) : ์ฌ๊ธฐ์ `trim`์ ๋ฃ์ด์ฃผ๋ ๊ฑด ์ฌ์ฉ์๊ฐ ์๋ ฅํ  ๋ ์ ๋ค์ ๊ณต๋ฐฑ์ด ์๋ค๋ฉด ๋ฏธ๋ฆฌ ์ ๊ฑฐํด์ฃผ๊ธฐ ์ํจ! JS ๊ฐ์ ๊ธฐ!!
-   [this.$store.dispatch()](https://vuex.vuejs.org/guide/actions.html#dispatching-actions) : `actions`์ ๋ถํ์ ํ๋๊ฑฐ๋ผ๊ณ  ์๊ฐํ์! "์ฐ๋ฆฌ ์ง๊ธ ์ฌ์ฉ์๊ฐ ์๋ ฅํ๊ฑฐ ์๋๋ฐ ์ด๊ฑฐ mutations์ ์ ๋ฌ์ข ํด์ state ๋ณ๊ฒฝ์ข ๋ถํ๋๋ฆฝ๋๋ค ๋๋!!"

(3)`action`์ `createTodo`๋ commit์ ํตํด `mutations`์ `CREATE_TODO`์ ์คํ ์ํค๊ณ  `TodoForm`์์ ๋๊ฒจ์ค `todoItem`์ ๊ทธ๋๋ก ๊ฐ์ด ๋๊ฒจ์ค๋ค. (4) `CREATE_TODO`๋ state(์ด์  ๋ฐ์ดํฐ ์ ์ฅ์)์ `todos`์ ์ ๋ฌ ๋ฐ์ `todoItem`์ ์ถ๊ฐํด์ค๋ค.  

- Mutations ํจ์(ํธ๋ค๋ฌ ํจ์)์ ์ด๋ฆ์ ์์๋ก ์์ฑํ๋ ๊ฒ์ ๊ถ์ฅ, ํ ๋์ ํ์ํ๊ธฐ ์ํด!

<hr>

๐ Vuex์ ๋ฐฉ์์ ์ ๋ฆฌํ๋ฉด ๋ค์๊ณผ ๊ฐ๋ค. 

-  `TodoForm`์ createTodo ๋ฉ์๋๋ฅผ ํตํด `actions`์ createTodo ํจ์๋ฅผ ํธ์ถ
- `actions`์ createTodo ํจ์๋ฅผ ํตํด `mutation`์ CREATE_TODO ํจ์ ํธ์ถ
- `State`์ todo ๋ฐ์ดํฐ ์กฐ์

<br>

## ๐ก Actions์ "context" ๊ฐ์ฒด

![image-20220512015711760](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512015711760.png)

- Vuex store์ ์ ๋ฐ์ ์ธ ๋งฅ๋ฝ ์์ฑ์ ๋ชจ๋ ํฌํจํ๊ณ  ์์

- ๊ทธ๋์ context.commit์ ํธ์ถํ์ฌ mutation์ ํธ์ถํ๊ฑฐ๋, context.state์ context.getters๋ฅผ ํตํด state์ getters์ ์ ๊ทผํ  ์ ์์
  - .dispatch()๋ก ๋ค๋ฅธ actions๋ ํธ์ถ ๊ฐ๋ฅ

```js
actions: {
    createTodo: function(context, todoItem) {
        console.log(context)
        console.log(context.state)
        console.log(context.getters)
        console.dispatch(...)
        console.commit('CREATE_TODO', todoItem)
    }
}
```

- <span style="color: indianred">ํ  ์ ์์ง๋ง, actions์์ state๋ฅผ ์กฐ์ํ์ง ๋ง ๊ฒ"</span>

- [context.commit()](https://vuex.vuejs.org/guide/actions.html) : `mutations`์๊ฒ ๋ถํ! ์ฐธ๊ณ ๋ก action hanlders๋  `context`๋ฅผ ์ฒซ๋ฒ์งธ ์ธ์๋ก ๋ฐ์!!

<br>

## ๐ฏ JS ๊ตฌ์กฐ๋ถํด ํ ๋น ์ ์ฉ ํ ํ์ฌ ํ๋ฉด ์ฒดํฌ

[๊ตฌ์กฐ๋ถํดํ ๋น -> ๊ณต์๋ฌธ์](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522892362274.jpg)

<hr>

#### ๐ ํ์ฌ ํ๋ฉด!!!

![image-20220512022451238](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512022451238.png)



<br>



# โ๏ธ Delete Todo

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522902937255.jpg)

- ์ฌ์ฉ์๊ฐ ์์ฑํ Todo ์ญ์ ํ๊ธฐ
- [state.todos.splice(index, 1)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
  - ํด๋น index๋ฅผ 1๊ฐ๋ง ์ญ์ ํ๊ณ  ๋๋จธ์ง ์์๋ฅผ ํ ๋๋ก ์๋ก์ด ๋ฐฐ์ด ์์ฑ

<hr>

#### ๐ ํ์ฌ ํ๋ฉด!!!

![image-20220512023448222](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512023448222.png)

<br>

# โป๏ธ Update Todo

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522917046976.jpg)

- `Update Todo`๋ ์ฌ์ฉ์๊ฐ ํ  ์ผ์ ๋คํด์ ์ฒดํฌ๋ฅผ ํ๋์ง ์ฌ๋ถ๋ฅผ ๋ฐ์ํด์ฃผ๋ ๊ฒ์ด๋ค. ๋ฐ๋ผ์ 4๋ฒ์ `map`์ ํตํด ์๋ก์ด ๋ฐฐ์ด์ ๋ฆฌํดํ  ๋ ์๋์ ๊ฐ์ด ํด๋ ๋๋ค!

```js
UPDATE_TODO_STATUS(state, todoItem) {
    state.todos = state.todos.map(todo => {
        if (todo === todoItem) {
            todo.isCompleted = !todo.isCompleted
        } 
        return todo
    }
```

- [Js์์ map ์ฐ๋ ๋ฒ ํ์ธํ๊ธฐ](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

<br>

## ๐ฏ JS Spread Syntax

```js
const ob1 = { lunch: 'pizza', price: 10}
const ob2 = { lunch: 'chicken', price: 11}

const newObj = { ...obj1 }
// { lunch: 'pizza', price: 10}
```

- ๋ฐฐ์ด์ด๋ ๋ฌธ์์ด๊ณผ ๊ฐ์ด ๋ฐ๋ณต ๊ฐ๋ฅํ(iterable) ๋ฌธ์๋ฅผ ์์๋ก ํ์ฅํ์ฌ, 0๊ฐ ์ด์์ key-value์ ์์ผ๋ก ๋ ๊ฐ์ฒด๋ฅผ ํ์ฅ ์ํฌ ์ ์์
- '...' ์ ๋ถ์ฌ์ ์์ ๋๋ ํค๊ฐ 0๊ฐ ์ด์์ iterable object๋ฅผ ํ๋์ object๋ก ๊ฐ๋จํ๊ฒ ํํํ๋ ๋ฒ
- '....'์ ๋์์ ๋ฐ๋์ iterable ๊ฐ์ฒด์ฌ์ผ ํจ

- [๊ณต์๋ฌธ์](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax#%EB%B0%B0%EC%97%B4_%EB%A6%AC%ED%84%B0%EB%9F%B4%EC%97%90%EC%84%9C%EC%9D%98_%EC%A0%84%EA%B0%9C)

<hr>

## ๐ก Mutations spread๋ก ๋ณ๊ฒฝ

```js
// index.js

mutations: {
    UPDATE_TODO_STATUS(state, todoItem) {
        state.todos = state.todos.map(todo => {
            if (todo === todoItem) {
                return {
                    ...todo,
                    isCompleted: !todo.isCompleted
                }
            } else {
              return todo
            }
        })
    }
}
```

<br>

## ๐ก ์๋ฃํ ์ผ ์ทจ์์  ๊ธ๊ธฐ

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522931529688.jpg)

- v-bind๋ฅผ ์ฌ์ฉํ [class binding](https://kr.vuejs.org/v2/guide/class-and-style.html)

- `<style scoped>` ์ฌ๊ธฐ์ scoped๋ฅผ ์ค ๊ฒ์ ํด๋น ์ปดํฌ๋ํธ ํ์ผ์๋ง ์ด ์คํ์ผ์ ์ ์ฉํ๊ฒ ๋ค๋ ๊ฒ์ด๋ค. scoped๋ฅผ ์ ์ฉํ์ง ์์ผ๋ฉด, ๋ค๋ฅธ ์ปดํฌ๋ํธ์์ ํด๋น ์คํ์ผ์ ํด๋์ค๋ฅผ ์ฌ์ฉํ๋ฉด ์ ์ฉ์ด ๋์ด๋ฒ๋ฆผ!!

<br>

### ๐ ํ์ฌ ํ๋ฉด!!!

![image-20220512032122616](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512032122616.png)

<br>



## ๐ Getters

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522942257859.jpg)

- `index.js`์ getters๋ฅผ ์ฌ์ฉํ์ฌ `App.vue` ์ computed ๋ฐํ ๊ฐ์ผ๋ก ์ฌ์ฉํ  ์ ์๋ค.

- [this.$store.getters](https://vuex.vuejs.org/guide/getters.html)
  - ๋ฑ๋ก๋ getters๋ฅผ ๊ฐ ์ปดํฌ๋ํธ์์ ์ฌ์ฉํ๋ ค๋ฉด `this.$store`๋ฅผ ์ด์ฉํ์ฌ getters์ ์ ๊ทผํด์ผ ํ๋ค.
  - ์ฐธ๊ณ ๋ก `computed`์ ์ ์ฅ์ ํด์ฃผ๊ณ  ์๋๋ฐ, `computed`์ ์ฅ์ ์ธ Caching ํจ๊ณผ๋ ๋จ์ํ state ๊ฐ์ ๋ฐํํ๋ ๊ฒ์ด ์๋๋ผ, getters์ ์ ์ธ๋ ์์ฑ์์ `filter()`, `reverse()` ๋ฑ์ ์ถ๊ฐ์ ์ธ ๊ณ์ฐ ๋ก์ง์ด ๋ค์ด๊ฐ ๋ ๋ฐํ๋๋ค. ์ ๋ data๋ก ๋ฐ๋๊ฒ์ด ์๋!!

- [state.todos.filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
  - ๋ฐฐ์ด์ ๊ฐ ์์์ ๋ํด ์ฝ๋ฐฑ ํจ์๋ฅผ ํ ๋ฒ์ฉ ์คํ
  - ์ฝ๋ฐฑ ํจ์์ ๋ฐํ ๊ฐ์ด ์ฐธ์ธ ์์๋ค๋ง ๋ชจ์์ ์๋ก์ด ๋ฐฐ์ด์ ๋ฐํ
  - ๊ธฐ์กด ๋ฐฐ์ด์ ์์๋ค์ ํํฐ๋ง ํ  ๋ ์ ์ฉ

<br>

### ๐ ํ์ฌ ํ๋ฉด!!!

![image-20220512034543631](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512034543631.png)

<br>



## ๐ก ์ปดํฌ๋ํธ Binding Helper

- JS Array Helper Method๋ฅผ ํตํด ๋ฐฐ์ด ์กฐ์์ ํธํ๊ฒ ํ๋ ๊ฒ๊ณผ ์ ์ฌํ๊ฒ, ๋ผ๋ฆฌ์ ์ธ ์ฝ๋ ์์ฒด๊ฐ ๋ณํ๋ ๊ฒ์ด ์๋๋ผ "์ฝ๊ฒ" ์ฌ์ฉํ  ์ ์๋๋ก ๋์ด ์์์ ์ด์ !
- ์ข๋ฅ
  - [mapState](https://vuex.vuejs.org/guide/state.html#the-mapstate-helper)
  - [mapGetters](https://vuex.vuejs.org/guide/getters.html#the-mapgetters-helper)
  - [mapActions](https://vuex.vuejs.org/guide/actions.html#dispatching-actions)
  - [mapMutations](https://vuex.vuejs.org/guide/mutations.html#mutations-must-be-synchronous)

<hr>

## โ๏ธ "mapState"

```js
// before Todolist.vue

computed: {
    todos: function () {
        return this.$store.state.todos
    }
}

// after Todolist.vue

<script>
import { mapState } from 'vuex' // ์ถ๊ฐ

export default {
  ...
  computed: mapState([
      'todos'
  ]),

```

- computed์ Store์ state๋ฅผ ๋งคํ
- Vuex Store์ ํ์ ๊ตฌ์กฐ๋ฅผ ๋ฐํํ์ฌ component ์ต์์ ์์ฑํจ
- ๋งคํ๋ computed ์ด๋ฆ์ด state ์ด๋ฆ๊ณผ ๊ฐ์ ๋ ๋ฌธ์์ด ๋ฐฐ์ด์ ์ ๋ฌํ  ์ ์์ 

<hr>

```js
computed: {
    ...mapState([
        'todos'
    ])
}
```

- ํ์ง๋ง ๋ค๋ฅธ computed ๊ฐ์ ์์ฑํ๊ณ  ์ถ์ ๋๋ ํจ๊ป ์ฌ์ฉํ  ์ ์๊ธฐ ๋๋ฌธ์ ์ต์ข ๊ฐ์ฒด๋ฅผ computed์ ์ ๋ฌํ  ์ ์๋๋ก ๋ค์๊ณผ ๊ฐ์ด ๊ฐ์ฒด ์ ๊ฐ ์ฐ์ฐ์ (spread)๋ก ๊ฐ์ฒด๋ฅผ ๋ณต์ฌํ์ฌ ์์ฑ
  - `mapState()`๋ ๊ฐ์ฒด๋ฅผ ๋ฐํํจ

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-165229574138910.jpg)

<br>

## โ๏ธ "mapGetters"

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-165229591860411.jpg)

- Computed์ Getters๋ฅผ ๋งคํ
- getters๋ฅผ ๊ฐ์ฒด ์ ๊ฐ ์ฐ์ฐ์ (spread)๋ก ๊ณ์ฐํ์ฌ ์ถ๊ฐ
- ํด๋น ์ปดํฌ๋ํธ ๋ด์์ ๋งคํํ๊ณ ์ ํ๋ ์ด๋ฆ์ด `index.js`์ ์ ์ํด ๋์ getters์ ์ด๋ฆ๊ณผ ๋์ผํ๋ฉด ๋ฐฐ์ด์ ํํ๋ก ํด๋น ์ด๋ฆ๋ง ๋ฌธ์์ด๋ก ์ถ๊ฐ

<br>

## โ๏ธ "mapGetters"

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-165229630617612.jpg)

- action์ ์ ๋ฌํ๋ ์ปดํฌ๋ํธ method ์ต์์ ๋ง๋ฆ!
- actions๋ฅผ ๊ฐ์ฒด ์ ๊ฐ ์ฐ์ฐ์ (spread)๋ก ๊ณ์ฐํ์ฌ ์ถ๊ฐ
- [์ฃผ์] - mapActions๋ฅผ ์ฌ์ฉํ๋ฉด, ์ด์ ์ dispatch()๋ฅผ ์ฌ์ฉํ์ ๋  payload๋ก ๋๊ฒจ์คฌ๋ `this.todo`๋ฅผ pass prop์ผ๋ก ๋ณ๊ฒฝํด์ ์ ๋ฌํด์ผ ํจ..!! ํน์๋ฌธ๋ฒ..ใทใท

<br>

# ๐ LocalStorage

์ง๊ธ๊น์ง ์ ์์ง๋ง, ๋ฌธ์ ์ ์ด ์๋ก๊ณ ์นจ ํ  ๋๋ง๋ค ๋ฆฌ์ ๋์ด๋ฒ๋ฆฌ๋๊ฑฐ๋ค. ๋น์ฐํ ์๋ฒ๊ฐ ์๊ธฐ ๋๋ฌธ์ด์ง๋ง, ์ฐ๋ฆฌ๋ ๋ธ๋ผ์ฐ์ ์ LocalStorage๋ฅผ ํตํด ํ์ฌ Vuex state๋ฅผ ์ ์ฅํ์ฌ ํ์ด์ง๊ฐ ์๋ก๊ณ ์นจ์ด ๋์ด๋ Vuex state๋ฅผ ์ ์งํ  ์ ์๋ค.

- ์ค์น

```bash
$ npm i vuex-persistedstate
```

- `vuex-persistedstate` ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ฌ์ฉ

```js
// index.js
import createPersistedStae from 'vuex-presistedstate'

export default new Vuex.Store({
    plugins: [
        createPersistedState(),
    ],
    ...
})
```

