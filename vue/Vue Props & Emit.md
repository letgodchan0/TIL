# 🔰Vue Props & Emit

<br>

## 🪴 컴포넌트

- 부모는 자식에게 데이터를 전달 (Pass props)하며, 자식은 자신에게 일어난 일을 부모에게 알림 (Emit event)
  - 부모와 자식이 명확하게 정의된 인터페이스를 통해 격리된 상태를 유지할 수 있음
- "props는 아래로, events는 위로"
- 부모는 props를 통해 자식에게 '데이터'를 전달하고, 자식은 events를 통해 부모에게 '메시지'를 보냄

<br>

## 🪴 컴포넌트 등록 3단계

1. import를 한다.

   ```javascript
   // 1. import
   import FirstVue from './components/FirstVue.vue'
   ```

2. 컴포넌트에 등록한다.

   ```javascript
   export default {
     name: 'App',
     // 2. 등록
     components: {
       FirstVue
     }
   }
   ```

3. 활용한다.

   ```vue
   <template>
     <div id="app">
       <h1>안녕, Vue</h1>
       <!-- 3. 컴포넌트 활용 -->
       <FirstVue/>
     </div>
   </template>
   ```



<br>

## 🪴 Props

![image-20220509173322322](Vue%20Props%20&%20Emit.assets/image-20220509173322322-16521969827801.png)

<hr>

- props는 부모(상위) 컴포넌트의 정보를 전달하기 위한 사용자 지정 특성
- 자식(하위) 컴포넌트는 props 옵션을 사용하여 수신하는 props를 명시적으로 선언해야 함
- 즉, 데이터는 props 옵션을 사용하여 자식 컴포넌트로 전달됨

- [공식문서 참고](https://kr.vuejs.org/v2/api/?#props)

<hr>

```vue
// App.vue

<template>
  <div id="app">
  	<about my-message="This is prop data"></about>    
  </div>
</template>
```

- 자식 컴포넌트(About.vue)에 보낼 prop 데이터 선언
- 작성법
  - prop-data-name = "value"

```vue
// About.vue

<template>
  <div>
  	<h1>About</h1>    
    <h2>{{ myMessage }}</h2>
  </div>
</template>

<script>
export default {
    name: 'About',
    props: {
        myMessage: String,
    }
}
</script>
```

- 수신할 prop 데이터를 명시적으로 선언 후 사용

<br>



## 🪴 Props 이름 컨벤션

- 선언 시
  - camelCase
- in template (HTML)
  - kebab-case

<br>

## 🌳 컴포넌트의 'data'는 반드시 함수!

```vue
<script>
	export default {
        ...
        data: function () {
            return {
                myData: null,
            }
        }
    }
</script>
```

- 기본적으로 각 인스턴스는 모두 같은 data 객체를 공유하므로 새로운 data 객체를 반환하여야 함

<br>

## 🌳 단방향 데이터 흐름

- 모든 props는 하위 속성과 상위 속성 사이의 단방향 바인딩을 형성한다. 

- 부모의 속성이 변경되면 자식 속성에게 전달되지만, 반대 방향으로는 안됨
  - 자식 요소가 의도치 않게 부모 요소의 상태를 변경하여 앱의 데이터 흐름을 이해하기 어렵게 만드는 일을 방지
- 부모 컴포넌트가 업데이트될 때마다 자식 요소의 모든 prop들이 최신 값으로 업데이트 됨

<br>



## 🪴 Emit event

![image-20220509175323959](Vue%20Props%20&%20Emit.assets/image-20220509175323959.png)

1. 자식 컴포넌트에서 `keyup.enter` 이벤트가 발생하면 `changeChildData` 메서드 실행
2. 이 메서드 내부에서 `my-event` 이벤트를 `$emit`하게 되고 `this.chilData` 파라미터로 전달 됨
3. 부모 컴포넌트에서 자식 컴포넌트의 이벤트를 `@my-event`하고 있었기 때문에 `getChildData` 메서드가 실행되고 부모 컴포넌트에서 자식으로부터 데이터를 받아 할일을 하게 된다.

<br>

## 🪴 event 이름 컨벤션

- 컴포넌트 및 props와 달리, 이벤트는 자동 대소문자 변환을 제공하지 않음
- HTML의 대소문자 구분을 위해 DOM 템플릿의 `v-on` 이벤트 리스터는 항상 자동으로 소문자 변환되기 때문에 `v-on:myEvent`는 자동으로 `v-on:myevent`로 변환
- 이러한 이유로 이벤트 이름에는 항상 "kebab-case"를 사용하는 것을 권장