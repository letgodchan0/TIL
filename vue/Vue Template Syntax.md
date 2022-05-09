# 🔰Vue Template Syntax

> 렌더링 된 DOM을 기본 Vue 인스턴스의 데이터에 선언적으로 바인딩할 수 있는 HTML 기반 템플릿 구문을 사용

1. [Text Interpolation (보간법)은 공식문서 확인하자!](https://kr.vuejs.org/v2/guide/syntax.html#%EB%AC%B8%EC%9E%90%EC%97%B4)
2. [Directive 참고하기](https://kr.vuejs.org/v2/api/#%EB%94%94%EB%A0%89%ED%8B%B0%EB%B8%8C)



<br>

## 🪴Directive (디렉티브)

- `v-` 접두사가 있는 특수 속성
- 속성 값은 단일 JS 표현식이 됨 (v-for는 예외)
- 표현식의 값이 변경될 때 반응적으로 DOM에 적응하는 역할을 함

<hr>

1. 전달인자 (Arguments)

```html
<a v-bind:href="url">...</a>
<a v-on:click="doSomething">...</a>
```

- `:` (콜론)을 통해 전달인자를 받을 수 도 있음

2. 수식어

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

- `.` (점)으로 표시되는 특수 접미사
- 디렉티브를 특별한 방법으로 바인딩 해야 함을 나타냄!!

<br>



## 🪴 v-text

```html
<div id="app">
    <span v-text="msg"></span>
	<!-- 같습니다 -->
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

- 내부적으로 보간법 `{{ msg }}` 이 `v-text`로 컴파일 됨 

<br>

## 🪴 v-html

```html
<div id="app">
    <div v-html="myHtml"></div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            myHtml: '<strong>강하닷</strong>'
        }
    })
</script>
```

- 기본적으로 엘리먼트의 innerHTML을 업데이틑 하는데, 웹 사이트에서 임의의 HTML을 동적으로 렌더링하면 XSS 공격으로 이어질 수 있으므로 매우 위험할 수 있다. 따라서 신뢰할 수 있는 컨텐츠에만 `v-html`을 사용하고 사용자가 제공한 컨텐츠에는 `절대로` 사용하지 말자!

<br>

## 🪴 v-show

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

- 조건부 렌더링 중 하나
- 요소는 항상 렌더링 되고 DOM에 남아 있음
- 단순히 엘리먼트에 display CSS 속성을 토글하는 것

<br>

## 🪴 v-if, v-else-if, v-else

```html
<div id="app">
    <!-- 1 -->
	<div v-if="seen">seen이 true일때만 렌더링.</div>
    
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

- 조건부 렌더링 중 하나로 조건에 따라 요소를 렌더링 한다.
- 디렉티브의 표현식이 true일 때만 렌더링
- 엘리먼트 및 포함된 디렉티브는 토글하는 동안 삭제되고 다시 작서됨

<br>

## 🌳 v-show 와 v-if

- v-show (Expensive initial load, cheap toggle)
  - CSS display 속성을 hidden으로 만들어 토글
  - 실제로 렌더링은 되지만 눈에서 보이지 않는 것이기 때문에 딱 한 번만 렌더링이 되는 경우라면 `v-if`에 비해 상대적으로 렌더링 비용이 높음
  - 하지만, 자주 변경되는 요소라면 한 번 렌더링 된 이후부터는 보여주는지에 대한 여부만 판단하면 되기 때문에 토글 비용이 적음
- v-if (Cheap initial load, expensive toggle)
  - 전달인자가 false인 경우 렌더링 되지 않음
  - 화면에서 보이지 않을 뿐만 아니라 렌더링 자체가 되지 않기 때문에 렌더링 비용이 낮음
  - 하지만, 자주 변경되는 요소의 경우 다시 렌더링 해야 하므로 비용이 증가할 수 있음 

<br>

## 🪴 v-for

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

- 원본 데이터를 기반으로 엘리먼트 또는 템플릿 블록을 여러번 렌더링한다. 디렉티브의 값은 반복되는 현재 엘리먼트에 대한 별칭을 제공하기 위해 특수 구문인 `alias in expression`을 사용해야 한다.
  - item in items 구문을 사용해야 함
- item 위치의 변수를 각 요소에서 사용할 수 있음
  - 객체의 경우는 key
- `v-for` 사용 시 반드시 key 속성을 각 요소에 작성하자!!!
- `v-if` 와 함께 사용하는 경우 `v-for`가 우선순위가 더 높지만, 가능하면 동시에 사용하지 말 것!!

<br>

## 🪴 v-on

```html
<div id="app">
  <!-- 메서드 핸들러 -->
  <button v-on:click="doAlert">Button</button>
  <button @click="doAlert">Button</button>
  <button @click="onInputEnter">Button</button>
    
  <!-- 기본 동작 방지 -->
  <form action="/articles/" @submit.prevent>
      <button>Submit</button>
  </form>
    
  <!-- 키 별칭을 이용한 키 입력 수식어 -->
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

- 엘리먼트에 이벤트 리스너를 연결
- 이벤트 유형은 전달인자로 표시함
- 특정 이벤트가 발생했을 때, 주어진 코드가 실행 됨
- `v-on:click` === `@click`

<br>

## 🪴 v-bind

```html
<div id="app">
  <!-- 속성 바인딩 -->
  <img v-bind:src="imageSrc" :alt="altMsg">
  <img :src="imageSrc" alt="altMsg">
  <hr>

  <!-- 클래스 바인딩 -->
  <div :class="{ active: isRed }">
      클래스 바인딩
  </div>

  <h3 :class="[activeRed, myBackground]">
      hello vue
  </h3>
  <hr>

  <!-- 스타일 바인딩 -->
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

- HTML 요소의 속성에 Vue의 상태 데이터를 값으로 할당
- Object 형태로 사용하면 value가 true인 key가 class 바인딩 값으로 할당
- `:`
  - `v-bind:href` === `:href`

<br>

## 🪴 v-model

```html
<div id="app">
    <h2>1. Input -> Data 단방향</h2>
    <h3>{{ myMessage }}</h3>
    <input @input="onInputChange" type="text">
    <hr>
    
    <h2>2. Input <-> Data 양방향</h2>
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

- 사용자의 입력을 받는 UI 요소들에 `v-model` 속성을 사용하면 입력 값이 자동으로 뷰 데이터 속성에 연결된다.
- HTML form 요소의 값과 data를 양방향 바인딩
- 수식어
  - `.lazy` - input 대신 change 이벤트를 듣는다!
  - `.number` - 문자열을 숫자로 변경
  - `.trim` -  입력에 대한 trim을 진행

<br>



## 🪴 Options/Data - 'computed'

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

- 데이터를 기반으로 하는 계산된 속성
- 함수의 형태로 정의하지만 함수가 아닌 함수의 반환 값이 바인딩 됨
- 종속된 데이터에 따라 저장(캐싱)됨
- 종속된 데이터가 변경될 때만 함수를 실행
- 즉, 어떤 데이터에도 의존하지 않는 computed 속성의 경우 절대로 업데이트 되지 않음
- 반드시 반환 값이 있어야 함

<br>

## 🌳 computed & methods

- computed 속성 대신 methods에 함수를 정의할 수 있고 최종 결과에 대해 두 가지 접근 방식은 서로 동일하다.
- 차이점은 computed 속성은 종속 대상을 따라 저장(캐싱)된다. 즉, computed는 종속된 대상이 변경되지 않는 한 computed에 작성된 함수를 여러 번 호출해도 계산을 다시 하지 않고 계산되어 있던 결과를 반환한다.
- 이에 비해 methods를 호출하면 렌더링을 다시 할 때마다 항상 함수를 실행한다.

<br>

## 🪴 Options/Data - 'watch'

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
                console.log(`${this.num}이 변경되었습니다.`)
            }
        },
    })
</script>
```

- 데이터를 감시
- 데이터에 변화가 일어났을 때 실행되는 함수

<br>

## 🌳 computed & watch

- computed
  - 특정 데이터를 직접적으로 사용/가공하여 다른 값으로 만들 때 사용
  - 속성은 계산해야 하는 목표 데이터를 정의하는 방식으로 소프트웨어 공학에서 이야기하는 '선언형 프로그래밍 방식'
  - "특정 값이 변동하면 해당 값을 다시 계산해서 보여준다."
- watch
  - 특정 데이터의 변화 상황에 맞춰 다른 data 등이 바뀌어야 할 때 주로 사용
  - 감시할 데이터를 지정하고 그 데이터가 바뀌면 특정 함수를 실행하는 방식
  - 소프트웨어 공학에서 이야기하는 '명령형 프로그래밍' 방식
  - 특정 대상이 변경되었을 때 콜백 함수를 실행시키기 위한 트리거
  - "특정 값이 변동하면 다른 작업을 한다."
- [공식문서에서 찾아보기](https://kr.vuejs.org/v2/guide/computed.html#computed-%EC%86%8D%EC%84%B1-vs-watch-%EC%86%8D%EC%84%B1)
  - 어떤 것이 더 우수한 것이 아닌 사용하는 목적과 상황이 다름 

<br>

## 🪴 Options/Assets - 'filter'

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

- 텍스트 형식화를 적용할 수 있는 필터
- 보간법 혹은 `v-bind`를 이용할 때 사용 가능
- 필터는 자바스크립트 표현식 마지막에 "|" (파이프)와 함께 추가되어야 함
- 이어서 사용(chaining) 가능

<br>

## 🪴Lifecycle Hooks

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
      // 외부 API에서 초기 데이터 받아오기
      created: function () {
          this.getImg()
      }
  })
</script>
```

- 각 Vue 인스턴스는 생성될 때 일련의 초기화 단계를 거침
  - 데이터 관찰 설정이 필요한 경우, 인스턴스를 DOM에 마운트 하는 경우, 데이터가 변경되어 DOM를 업데이트 하는 경우 등
- 그 과정에서 사용자 정의 로직을 실행할 수 있는 `Lifecycle Hooks`도 호출됨
- 원래는 클릭을 해야 이미지를 가져오지만, created를 사용해 애플리케이션의 초기 데이터를 API 요청을 통해 불러올 수 있음
- [공식문서를 통해 각 라이프사이클 훅의 상세 동작을 참고하자!](https://kr.vuejs.org/v2/guide/instance.html#%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4-%ED%9B%85)