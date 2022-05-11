# 🔰Vuex Todo App 구현

vuex를 활용해서 간단한 Todo App을 구현해보려고 한다. 컴포넌트는 아래와 같이 구성!

![image-20220511235546321](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220511235546321.png)

<br>



## 💡 프로젝트 세팅

#### 1. 프로젝트 생성

```bash
$ vue create todo-vuex-app
$ cd todo-vuex-app
```

<br>

#### 2. Vuex 추가

```bash
$ vue add vuex
```

<br>

#### 3. commit 여부 (Yes)

![image-20220512000806072](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512000806072.png)

<br>

#### 4. 서버 실행

```bash
$ npm run serve
```

![image-20220512001232557](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512001232557.png)

<br>

## 💡 컴포넌트 생성

<hr>

### 1. TodoListItem.vue

![image-20220512002329813](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512002329813.png)

- 개별 todo들의 컴포넌트
- `TodoList` 컴포넌트의 자식 컴포넌트

<br>

### 2. TodoList.vue

![image-20220512002620820](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512002620820.png)

- todo 목록 컴포넌트
- TodoListItem 컴포넌트의 부모 컴포넌트
- `import TodoListItem from '@/components/TodoListItem'` 
  - 자식 컴포넌트를 등록하기 위해 호출 
  - @는 src 폴더를 의미

<br>

### 3. TodoForm.vue

![image-20220512002805349](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512002805349.png)

- todo 데이터를 입력받는 컴포넌트

<br>

### 4. App.vue

![image-20220512002933862](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512002933862.png)

- 최상위 컴포넌트
- TodoList, TodoForm 컴포넌트의 부모 컴포넌트

- 절대경로, 상대경로 각각 호출하는 법을 보여주기 위해 각각 작성했지만 보통은 하나의 방식으로 정함!

<br>

# 📝 Create Todo

<hr>

##  💡 State 작성

![image-20220512003849160](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512003849160.png)

- state는 데이터가 들어가는 곳이다. 체크하면서 하기 위해 `todos`라는 배열을 만들고 데이터를 직접 입력해준다!
- [new Date().getTime() -> 공식문서 참고](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)
  - new Date() : 1970년 1월1일 0시 0분 0초로부터 시작해서 지금까지 몇 ms로 지났는지 현재 시간을 알려준다.
  - getTime() : Date 타입의 시간을 밀리초로 환산해서 가져온다
- [주의] Vuex를 사용한다고 해서 Vuex Store에 모든 상태를 넣어야 하는 것은 아님

<br>

## 💡 TodoList 데이터 가져오기

![image-20220512004730865](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512004730865.png)

- 컴포넌트에서 Vuex Store의 state에 접근
  - `$store.state` : store/index.js의 state에서 정의한 todos를 현재 파일인 TodoList.vue에서 사용하기 위한 방법이다. 
  - [v-for="todo in ~ todos" :key="todo.date"](https://github.com/letgodchan0/TIL/blob/main/vue/Vue%20Template%20Syntax.md) : v-for 사용법!!

<br>

## 💡 Computed로 변경

![제목 없음](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/%EC%A0%9C%EB%AA%A9%20%EC%97%86%EC%9D%8C-16522848472591.jpg)

- 현재 state의 todo는 값이 변화하는 것이 아님
- store에 저장된 todo 목록을 가져오는 것이기 때문에 매번 새로 호출하는 것은 비효율적!!
- 따라서 todo가 추가 되는 등의 변경 사항이 있을 때만 새로 계산한 값을 반환하는 방향으로 변경 -> [computed](https://kr.vuejs.org/v2/guide/computed.html)
- `this(Vue Instance)`로 접근

<br>

## 💡Props (TodoList -> Todo)

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522853660252.jpg)

- TodoList에 있는 Todo들을 하나씩 넘겨주기!

<br>

## 💡Actions & Mutations

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522867864883.jpg)

<hr>

여기서 하는게 뭐냐면, `TodoForm.vue`에서 사용자가 입력 칸에 오늘 할 일을 작성해서 엔터를 누르거나, Add 버튼을 누르면 (1) `createTodo` 메서드가 실행된다. (2) 이 메서드는  `todoItem` 이라는 객체 안에 전달할 내용 (title, isCompleted, date)을 정의하고 특히, 사용자가 작성한 내용을 title에 담아준다. 그리고 사용자가 작성을 했다면 `this.$store.dispatch('createTodo', todoItem)`을 통해 `action`의 `createTodo`가 실행되고 `todoItem`을 같이 넘겨준다.

- [v-model과 data](https://kr.vuejs.org/v2/guide/components.html#%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%9D%98-v-model-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%A0%95%EC%9D%98) : 여기서 `trim`을 넣어주는 건 사용자가 입력할 때 앞 뒤에 공백이 있다면 미리 제거해주기 위함! JS 개신기!!
-   [this.$store.dispatch()](https://vuex.vuejs.org/guide/actions.html#dispatching-actions) : `actions`에 부탁을 하는거라고 생각하자! "우리 지금 사용자가 입력한거 있는데 이거 mutations에 전달좀 해서 state 변경좀 부탁드립니다 느낌!!"

(3)`action`의 `createTodo`는 commit을 통해 `mutations`의 `CREATE_TODO`을 실행 시키고 `TodoForm`에서 넘겨준 `todoItem`을 그대로 같이 넘겨준다. (4) `CREATE_TODO`는 state(이제 데이터 저장소)의 `todos`에 전달 받은 `todoItem`을 추가해준다.  

- Mutations 함수(핸들러 함수)의 이름은 상수로 작성하는 것을 권장, 한 눈에 파악하기 위해!

<hr>

📌 Vuex의 방식을 정리하면 다음과 같다. 

-  `TodoForm`의 createTodo 메서드를 통해 `actions`의 createTodo 함수를 호출
- `actions`의 createTodo 함수를 통해 `mutation`의 CREATE_TODO 함수 호출
- `State`의 todo 데이터 조작

<br>

## 💡 Actions의 "context" 객체

![image-20220512015711760](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512015711760.png)

- Vuex store의 전반적인 맥락 속성을 모두 포함하고 있음

- 그래서 context.commit을 호출하여 mutation을 호출하거나, context.state와 context.getters를 통해 state와 getters에 접근할 수 있음
  - .dispatch()로 다른 actions도 호출 가능

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

- <span style="color: indianred">할 수 있지만, actions에서 state를 조작하지 말 것"</span>

- [context.commit()](https://vuex.vuejs.org/guide/actions.html) : `mutations`에게 부탁! 참고로 action hanlders는  `context`를 첫번째 인자로 받음!!

<br>

## 💯 JS 구조분해 할당 적용 후 현재 화면 체크

[구조분해할당 -> 공식문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522892362274.jpg)

<hr>

#### 📌 현재 화면!!!

![image-20220512022451238](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512022451238.png)



<br>



# ✂️ Delete Todo

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522902937255.jpg)

- 사용자가 작성한 Todo 삭제하기
- [state.todos.splice(index, 1)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
  - 해당 index를 1개만 삭제하고 나머지 요소를 토대로 새로운 배열 생성

<hr>

#### 📌 현재 화면!!!

![image-20220512023448222](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512023448222.png)

<br>

# ♻️ Update Todo

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522917046976.jpg)

- `Update Todo`는 사용자가 할 일을 다해서 체크를 하는지 여부를 반영해주는 것이다. 따라서 4번의 `map`을 통해 새로운 배열을 리턴할 때 아래와 같이 해도 된다!

```js
UPDATE_TODO_STATUS(state, todoItem) {
    state.todos = state.todos.map(todo => {
        if (todo === todoItem) {
            todo.isCompleted = !todo.isCompleted
        } 
        return todo
    }
```

- [Js에서 map 쓰는 법 확인하기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

<br>

## 💯 JS Spread Syntax

```js
const ob1 = { lunch: 'pizza', price: 10}
const ob2 = { lunch: 'chicken', price: 11}

const newObj = { ...obj1 }
// { lunch: 'pizza', price: 10}
```

- 배열이나 문자열과 같이 반복 가능한(iterable) 문자를 요소로 확장하여, 0개 이상의 key-value의 쌍으로 된 객체를 확장 시킬 수 있음
- '...' 을 붙여서 요소 또는 키가 0개 이상의 iterable object를 하나의 object로 간단하게 표현하는 법
- '....'의 대상은 반드시 iterable 객체여야 함

- [공식문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax#%EB%B0%B0%EC%97%B4_%EB%A6%AC%ED%84%B0%EB%9F%B4%EC%97%90%EC%84%9C%EC%9D%98_%EC%A0%84%EA%B0%9C)

<hr>

## 💡 Mutations spread로 변경

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

## 💡 완료한 일 취소선 긋기

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522931529688.jpg)

- v-bind를 사용한 [class binding](https://kr.vuejs.org/v2/guide/class-and-style.html)

- `<style scoped>` 여기서 scoped를 준 것은 해당 컴포넌트 파일에만 이 스타일을 적용하겠다는 것이다. scoped를 적용하지 않으면, 다른 컴포넌트에서 해당 스타일의 클래스를 사용하면 적용이 되어버림!!

<br>

### 📌 현재 화면!!!

![image-20220512032122616](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512032122616.png)

<br>



## 🗂 Getters

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-16522942257859.jpg)

- `index.js`의 getters를 사용하여 `App.vue` 의 computed 반환 값으로 사용할 수 있다.

- [this.$store.getters](https://vuex.vuejs.org/guide/getters.html)
  - 등록된 getters를 각 컴포넌트에서 사용하려면 `this.$store`를 이용하여 getters에 접근해야 한다.
  - 참고로 `computed`에 저장을 해주고 있는데, `computed`의 장점인 Caching 효과는 단순히 state 값을 반환하는 것이 아니라, getters에 선언된 속성에서 `filter()`, `reverse()` 등의 추가적인 계산 로직이 들어갈 때 발휘된다. 절때 data로 받는것이 아님!!

- [state.todos.filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
  - 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행
  - 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환
  - 기존 배열의 요소들을 필터링 할 때 유용

<br>

### 📌 현재 화면!!!

![image-20220512034543631](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/image-20220512034543631.png)

<br>



## 💡 컴포넌트 Binding Helper

- JS Array Helper Method를 통해 배열 조작을 편하게 하는 것과 유사하게, 논리적인 코드 자체가 변하는 것이 아니라 "쉽게" 사용할 수 있도록 되어 있음에 초점!
- 종류
  - [mapState](https://vuex.vuejs.org/guide/state.html#the-mapstate-helper)
  - [mapGetters](https://vuex.vuejs.org/guide/getters.html#the-mapgetters-helper)
  - [mapActions](https://vuex.vuejs.org/guide/actions.html#dispatching-actions)
  - [mapMutations](https://vuex.vuejs.org/guide/mutations.html#mutations-must-be-synchronous)

<hr>

## ✏️ "mapState"

```js
// before Todolist.vue

computed: {
    todos: function () {
        return this.$store.state.todos
    }
}

// after Todolist.vue

<script>
import { mapState } from 'vuex' // 추가

export default {
  ...
  computed: mapState([
      'todos'
  ]),

```

- computed와 Store의 state를 매핑
- Vuex Store의 하위 구조를 반환하여 component 옵션을 생성함
- 매핑된 computed 이름이 state 이름과 같을 때 문자열 배열을 전달할 수 있음 

<hr>

```js
computed: {
    ...mapState([
        'todos'
    ])
}
```

- 하지만 다른 computed 값을 작성하고 싶을 때는 함께 사용할 수 없기 때문에 최종 객체를 computed에 전달할 수 있도록 다음과 같이 객체 전개 연산자 (spread)로 객체를 복사하여 작성
  - `mapState()`는 객체를 반환함

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-165229574138910.jpg)

<br>

## ✏️ "mapGetters"

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-165229591860411.jpg)

- Computed와 Getters를 매핑
- getters를 객체 전개 연산자 (spread)로 계산하여 추가
- 해당 컴포넌트 내에서 매핑하고자 하는 이름이 `index.js`에 정의해 놓은 getters의 이름과 동일하면 배열의 형태로 해당 이름만 문자열로 추가

<br>

## ✏️ "mapGetters"

![1](Vuex%20Todo%20App%20%EA%B5%AC%ED%98%84.assets/1-165229630617612.jpg)

- action을 전달하는 컴포넌트 method 옵션을 만듦!
- actions를 객체 전개 연산자 (spread)로 계산하여 추가
- [주의] - mapActions를 사용하면, 이전에 dispatch()를 사용했을 때  payload로 넘겨줬던 `this.todo`를 pass prop으로 변경해서 전달해야 함..!! 특수문법..ㄷㄷ

<br>

# 📂 LocalStorage

지금까지 잘 왔지만, 문제점이 새로고침 할 때마다 리셋 되어버리는거다. 당연히 서버가 없기 때문이지만, 우리는 브라우저의 LocalStorage를 통해 현재 Vuex state를 저장하여 페이지가 새로고침이 되어도 Vuex state를 유지할 수 있다.

- 설치

```bash
$ npm i vuex-persistedstate
```

- `vuex-persistedstate` 라이브러리 사용

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

