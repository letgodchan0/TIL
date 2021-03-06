# ๐ฐVue Props & Emit

<br>

## ๐ชด ์ปดํฌ๋ํธ

- ๋ถ๋ชจ๋ ์์์๊ฒ ๋ฐ์ดํฐ๋ฅผ ์ ๋ฌ (Pass props)ํ๋ฉฐ, ์์์ ์์ ์๊ฒ ์ผ์ด๋ ์ผ์ ๋ถ๋ชจ์๊ฒ ์๋ฆผ (Emit event)
  - ๋ถ๋ชจ์ ์์์ด ๋ชํํ๊ฒ ์ ์๋ ์ธํฐํ์ด์ค๋ฅผ ํตํด ๊ฒฉ๋ฆฌ๋ ์ํ๋ฅผ ์ ์งํ  ์ ์์
- "props๋ ์๋๋ก, events๋ ์๋ก"
- ๋ถ๋ชจ๋ props๋ฅผ ํตํด ์์์๊ฒ '๋ฐ์ดํฐ'๋ฅผ ์ ๋ฌํ๊ณ , ์์์ events๋ฅผ ํตํด ๋ถ๋ชจ์๊ฒ '๋ฉ์์ง'๋ฅผ ๋ณด๋

<br>

## ๐ชด ์ปดํฌ๋ํธ ๋ฑ๋ก 3๋จ๊ณ

1. import๋ฅผ ํ๋ค.

   ```javascript
   // 1. import
   import FirstVue from './components/FirstVue.vue'
   ```

2. ์ปดํฌ๋ํธ์ ๋ฑ๋กํ๋ค.

   ```javascript
   export default {
     name: 'App',
     // 2. ๋ฑ๋ก
     components: {
       FirstVue
     }
   }
   ```

3. ํ์ฉํ๋ค.

   ```vue
   <template>
     <div id="app">
       <h1>์๋, Vue</h1>
       <!-- 3. ์ปดํฌ๋ํธ ํ์ฉ -->
       <FirstVue/>
     </div>
   </template>
   ```



<br>

## ๐ชด Props

![image-20220509173322322](Vue%20Props%20&%20Emit.assets/image-20220509173322322-16521969827801.png)

<hr>

1. ์์ ์ปดํฌ๋ํธ(About.vue)์ ๋ณด๋ผ prop ๋ฐ์ดํฐ ์ ์ธ
   - ์์ฑ๋ฒ
     - prop-data-name = "value"

2. ์์ ํ  prop ๋ฐ์ดํฐ๋ฅผ ๋ช์์ ์ผ๋ก ์ ์ธ ํ ์ฌ์ฉ
3. [๊ณต์๋ฌธ์ ์ฐธ๊ณ ](https://kr.vuejs.org/v2/api/?#props)

<br>



## ๐ชด Props ์ด๋ฆ ์ปจ๋ฒค์

- ์ ์ธ ์
  - camelCase
- in template (HTML)
  - kebab-case

<br>

## ๐ณ ์ปดํฌ๋ํธ์ 'data'๋ ๋ฐ๋์ ํจ์!

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

- ๊ธฐ๋ณธ์ ์ผ๋ก ๊ฐ ์ธ์คํด์ค๋ ๋ชจ๋ ๊ฐ์ data ๊ฐ์ฒด๋ฅผ ๊ณต์ ํ๋ฏ๋ก ์๋ก์ด data ๊ฐ์ฒด๋ฅผ ๋ฐํํ์ฌ์ผ ํจ

<br>

## ๐ณ ๋จ๋ฐฉํฅ ๋ฐ์ดํฐ ํ๋ฆ

- ๋ชจ๋  props๋ ํ์ ์์ฑ๊ณผ ์์ ์์ฑ ์ฌ์ด์ ๋จ๋ฐฉํฅ ๋ฐ์ธ๋ฉ์ ํ์ฑํ๋ค. 

- ๋ถ๋ชจ์ ์์ฑ์ด ๋ณ๊ฒฝ๋๋ฉด ์์ ์์ฑ์๊ฒ ์ ๋ฌ๋์ง๋ง, ๋ฐ๋ ๋ฐฉํฅ์ผ๋ก๋ ์๋จ
  - ์์ ์์๊ฐ ์๋์น ์๊ฒ ๋ถ๋ชจ ์์์ ์ํ๋ฅผ ๋ณ๊ฒฝํ์ฌ ์ฑ์ ๋ฐ์ดํฐ ํ๋ฆ์ ์ดํดํ๊ธฐ ์ด๋ ต๊ฒ ๋ง๋๋ ์ผ์ ๋ฐฉ์ง
- ๋ถ๋ชจ ์ปดํฌ๋ํธ๊ฐ ์๋ฐ์ดํธ๋  ๋๋ง๋ค ์์ ์์์ ๋ชจ๋  prop๋ค์ด ์ต์  ๊ฐ์ผ๋ก ์๋ฐ์ดํธ ๋จ

<br>



## ๐ชด Emit event

![image-20220509175323959](Vue%20Props%20&%20Emit.assets/image-20220509175323959.png)

1. ์์ ์ปดํฌ๋ํธ์์ `keyup.enter` ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ๋ฉด `changeChildData` ๋ฉ์๋ ์คํ
2. ์ด ๋ฉ์๋ ๋ด๋ถ์์ `my-event` ์ด๋ฒคํธ๋ฅผ `$emit`ํ๊ฒ ๋๊ณ  `this.chilData` ํ๋ผ๋ฏธํฐ๋ก ์ ๋ฌ ๋จ
3. ๋ถ๋ชจ ์ปดํฌ๋ํธ์์ ์์ ์ปดํฌ๋ํธ์ ์ด๋ฒคํธ๋ฅผ `@my-event`ํ๊ณ  ์์๊ธฐ ๋๋ฌธ์ `getChildData` ๋ฉ์๋๊ฐ ์คํ๋๊ณ  ๋ถ๋ชจ ์ปดํฌ๋ํธ์์ ์์์ผ๋ก๋ถํฐ ๋ฐ์ดํฐ๋ฅผ ๋ฐ์ ํ ์ผ์ ํ๊ฒ ๋๋ค.

<br>

## ๐ชด event ์ด๋ฆ ์ปจ๋ฒค์

- ์ปดํฌ๋ํธ ๋ฐ props์ ๋ฌ๋ฆฌ, ์ด๋ฒคํธ๋ ์๋ ๋์๋ฌธ์ ๋ณํ์ ์ ๊ณตํ์ง ์์
- HTML์ ๋์๋ฌธ์ ๊ตฌ๋ถ์ ์ํด DOM ํํ๋ฆฟ์ `v-on` ์ด๋ฒคํธ ๋ฆฌ์คํฐ๋ ํญ์ ์๋์ผ๋ก ์๋ฌธ์ ๋ณํ๋๊ธฐ ๋๋ฌธ์ `v-on:myEvent`๋ ์๋์ผ๋ก `v-on:myevent`๋ก ๋ณํ
- ์ด๋ฌํ ์ด์ ๋ก ์ด๋ฒคํธ ์ด๋ฆ์๋ ํญ์ "kebab-case"๋ฅผ ์ฌ์ฉํ๋ ๊ฒ์ ๊ถ์ฅ