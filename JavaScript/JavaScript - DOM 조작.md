# ๐ JavaScript - DOM ์กฐ์

<br>







### ๐ DOM ์ ํ ๊ด๋ จ ๋ฉ์๋ (1)

```js
const nav = document.querySelector('nav')	// selector ๊ฐ์ ธ์ค๊ธฐ
const button = document.querySelector('#submitButton')	// id๋ ์ ์ผํ๊ธฐ ๋๋ฌธ์ id ๊ฐ์ ธ์ค๊ธฐ
const navImg = document.querySelector('nav > img')	// jspath ์๋ ฅ

const questionDivs = document.querySelectorAll('section div') // section์ ์์ ์ค div ๊ฐ์ ธ์ค๊ธฐ
```

- `document.querySelector(selector)`
  - ์ ๊ณตํ ์ ํ์์ ์ผ์นํ๋ element ํ๋ ์ ํ
  - ์ ๊ณตํ CSS selector๋ฅผ ๋ง์กฑํ๋ ์ฒซ ๋ฒ์งธ element `๊ฐ์ฒด`๋ฅผ ๋ฐํ (์๋ค๋ฉด null)
- `document.querySelectorAll(selector)`
  - ์ ๊ณตํ ์ ํ์์ ์ผ์นํ๋ ์ฌ๋ฌ element๋ฅผ ์ ํ
  - ๋งค์นญ ํ  ํ๋ ์ด์์ ์๋ ํฐ๋ฅผ ํฌํจํ๋ ์ ํจํ CSS selector๋ฅผ ์ธ์(๋ฌธ์์ด)๋ก ๋ฐ์
  - ์ง์ ๋ ์๋ ํฐ์ ์ผ์นํ๋ `NodeList`๋ฅผ ๋ฐํ

<br>

### ๐ DOM ์ ํ ๊ด๋ จ ๋ฉ์๋ (2)

- `getElementById(id)` - ๋จ์ผ element ๊ฐ์ฒด ๋ฐํ
- `getElementsByTagName(name)`  - HTML Collection ๋ฐํ
- `getElementsByClassName(names)` -  HTML Collection ๋ฐํ

>  ์ ํ ๊ด๋ จ ๋ฉ์๋ ์ค ์ด๋ฐ ์ ๋ค๋ ์์ง๋ง querySelector(), querySelectorAll()๋ง ์ฌ์ฉํด์ผ ํ๋ค!!
>
> - id, class ๊ทธ๋ฆฌ๊ณ  tag ์ ํ์ ๋ฑ์ ๋ชจ๋ ์ฌ์ฉ ๊ฐ๋ฅํ๋ฏ๋ก, ๋ ๊ตฌ์ฒด์ ์ด๊ณ  ์ ์ฐํ๊ฒ ์ ํ ๊ฐ๋ฅ

<br>

### ๐ก HTMLCollection & NodeList

> ๋ ๋ค ๋ฐฐ์ด๊ณผ ๊ฐ์ด ๊ฐ ํญ๋ชฉ์ ์ ๊ทผํ๊ธฐ ์ํ index๋ฅผ ์ ๊ณต (์ ์ฌ ๋ฐฐ์ด)

- HTMLCollection
  - name, id, index ์์ฑ์ผ๋ก ๊ฐ ํญ๋ชฉ์ ์ ๊ทผ ๊ฐ๋ฅ
- NodeList
  - index๋ก๋ง ๊ฐ ํญ๋ชฉ์ ์ ๊ทผ ๊ฐ๋ฅ
  - ๋จ, HTMLCollection๊ณผ ๋ฌ๋ฆฌ ๋ฐฐ์ด์์ ์ฌ์ฉํ๋ `forEach` ๋ฉ์๋ ๋ฐ ๋ค์ํ ๋ฉ์๋ ์ฌ์ฉ ๊ฐ๋ฅ

- "๋ ๋ค Live Collection์ผ๋ก DOM์ ๋ณ๊ฒฝ์ฌํญ์ ์ค์๊ฐ์ผ๋ก ๋ฐ์ํ์ง๋ง, querySelectorAll์ ์ํด์๋ง ๋ฐํ๋๋ `NodeList`๋ Static Collection์ผ๋ก ์ค์๊ฐ์ผ๋ก ๋ฐ์๋์ง ์์!!!!"

<br>

### ๐ Collection

- Live Collection
  - ๋ฌธ์๊ฐ ๋ฐ๋ ๋ ์ค์๊ฐ์ผ๋ก ์๋ฐ์ดํธ ๋จ
  - DOM์ ๋ณ๊ฒฝ์ฌํญ์ ์ค์๊ฐ์ผ๋ก collection์ ๋ฐ์
    - HTMLCollection, NodeList
- Static Collection (non-live)
  - DOM์ด ๋ณ๊ฒฝ๋์ด๋ collection ๋ด์ฉ์๋ ์ํฅ์ ์ฃผ์ง ์์
  - querySelectorALL()์ ๋ฐํ NodeList ๋ง static collection!!!!!!!!!!!!!!!!!!!!!!!!

<br>



### ๐ DOM ๋ณ๊ฒฝ ๊ด๋ จ ๋ฉ์๋ (Creation)

```js
document.createElement()
const footer = document.createElement('footer')
```

- ์์ฑํ ํ๊ทธ ๋ช์ HTML ์์๋ฅผ ์์ฑํ์ฌ ๋ฐํ!

<br>

### ๐ DOM ๋ณ๊ฒฝ ๊ด๋ จ ๋ฉ์๋ (append DOM)

#### Element.append()

```js
const ulTag = document.querySelector('ul')
const newLiTag = document.createElement('li')
newLiTag.innerText = '์๋ก์ด ๋ฆฌ์คํธ ํ๊ทธ'
ulTag.append(newLiTag)
ulTag.append('๊ทธ๋ฅ ์ด๋ฐ ๋ฌธ์์ด๋ ์ถ๊ฐ ๊ฐ๋ฅ!')

const new1 = document.createElement('li')
new1.innerText = '๋ฆฌ์คํธ 1'
const new2 = document.createElement('li')
new2.innerText = '๋ฆฌ์คํธ 2'
const new3 = document.createElement('li')
new3.innerText = '๋ฆฌ์คํธ 3'
ulTag.append(new1, new2, new3)
```

- ํน์  ๋ถ๋ชจ Node์ ์์ NodeList ์ค ๋ง์ง๋ง ์์ ๋ค์์ Node ๊ฐ์ฒด๋ DOMString์ ์ฝ์
- ์ฌ๋ฌ ๊ฐ์ Node ๊ฐ์ฒด, DOMString์ ์ถ๊ฐํ  ์ ์์
- ๋ฐํ ๊ฐ์ด ์์



<hr>

#### Node.appendChild()

```js
const ulTag = document.querySelector('ul')
const newLiTag = document.createElement('li')
newLiTag.innerText = '์๋ก์ด ๋ฆฌ์คํธ ํ๊ทธ'
ulTag.appendChild(newLiTag)
// ulTag.appendChild(newLiTag, newLiTag2) ์ฌ๋ฌ๊ฐ ๋ถ๊ฐ, but js ํน์ง ํจ์ ์์ ์ธ์ ๋ง์๋ ์๋ฌ x
ulTag.appendChild('๋ฌธ์์ด์ ์ถ๊ฐ ๋ถ๊ฐ')

const new1 = document.createElement('li')
new1.innerText = '๋ฆฌ์คํธ 1'
const new2 = document.createElement('li')
new2.innerText = '๋ฆฌ์คํธ 2'
ulTag.appendChild(new1, new2)	// new1๋ง ์ถ๊ฐ๋๋ค! 
```

- ํ Node๋ฅผ ํน์  ๋ถ๋ชจ Node ์์ NodeList ์ค ๋ง์ง๋ง ์ฌ์ง์ผ๋ก ์ฝ์ (Node๋ง ์ถ๊ฐ ๊ฐ๋ฅ)
- ํ๋ฒ์ ์ค์ง ํ๋์ Node๋ง ์ถ๊ฐํ  ์ ์์
- ๋ง์ฝ ์ฃผ์ด์ง Node๊ฐ ์ด๋ฏธ ๋ฌธ์์ ์กด์ฌํ๋ ๋ค๋ฅธ Node๋ฅผ ์ฐธ์กฐํ๋ค๋ฉด ์๋ก์ด ์์น๋ก ์ด๋

<hr>

#### ๐กParentNode.append() vs Node.appendChild()

- `append()`๋ฅผ ์ฌ์ฉํ๋ฉด DOMString ๊ฐ์ฒด๋ฅผ ์ถ๊ฐํ  ์๋ ์์ง๋ง, `.appendChild()`๋ Node ๊ฐ์ฒด๋ง ํ์ฉ
- `append()`๋ ๋ฐํ ๊ฐ์ด ์์ง๋ง, `appendChild()`๋ ์ถ๊ฐ๋ Node ๊ฐ์ฒด๋ฅผ ๋ฐํ
- `append()`๋ ์ฌ๋ฌ Node ๊ฐ์ฒด์ ๋ฌธ์์ด์ ์ถ๊ฐํ  ์ ์์ง๋ง `appendChild()`๋ ํ๋์ Node ๊ฐ์ฒด๋ง ์ถ๊ฐํ  ์ ์์ 

<br>

### ๐ DOM ๋ณ๊ฒฝ ๊ด๋ จ ์์ฑ (property)

```js
const ulTag = document.querySelector('ul')
const newLiTag = document.createElement('li')

liTag1.innerText = '<strong>ํผ์</strong>'	// <strong>ํผ์</strong> ๊ทธ๋๋ก ์๋ ฅ
liTag2.innerHTML = '<strong>ํผ์</strong>'	// ํผ์๊ฐ ๋ณผ๋์ฒด ๋์ด์ ์๋ ฅ๋๋ค!
```

- `Node.innerText`
  - Node ๊ฐ์ฒด์ ๊ทธ ์์์ ํ์คํธ ์ปจํ์ธ (DOMString)๋ฅผ ํํ (ํด๋น ์์ ๋ด๋ถ์ raw text) (์ฌ๋์ด ์ฝ์ ์ ์๋ ์์๋ง ๋จ๊น)
  - ์ฆ, ์ค ๋ฐ๊ฟ์ ์ธ์ํ๊ณ  ์จ๊ฒจ์ง ๋ด์ฉ์ ๋ฌด์ํ๋ ๋ฑ ์ต์ข์ ์ผ๋ก ์คํ์ผ๋ง์ด ์ ์ฉ๋ ๋ชจ์ต์ผ๋ก ํํ
- `Element.innerHTML`
  - ์์(element) ๋ด์ ํฌํจ๋ HTML ๋งํฌ์์ ๋ฐํ
  - [์ฐธ๊ณ ] XSS ๊ณต๊ฒฉ์ ์ทจ์ฝํ๋ฏ๋ก ์ฌ์ฉ์ ์ฃผ์!!!!

<br>

### ๐ก XSS (Cross-site Scripting)

- ๊ณต๊ฒฉ์๊ฐ ์๋ ฅ์์๋ฅผ ์ฌ์ฉํ์ฌ (`<input>`) ์น ์ฌ์ดํธ ํด๋ผ์ด์ธํธ ์ธก ์ฝ๋์ ์์ฑ ์คํฌ๋ฆฝํธ๋ฅผ ์ฝ์ํด ๊ณต๊ฒฉํ๋ ๋ฐฉ๋ฒ
- ํผํด์(์ฌ์ฉ์)์ ๋ธ๋ผ์ฐ์ ๊ฐ ์์ฑ ์คํฌ๋ฆฝํธ๋ฅผ ์คํํ๋ฉฐ ๊ณต๊ฒฉ์๊ฐ ์์ธ์ค ์ ์ด๋ฅผ ์ฐํํ๊ณ  ์ฌ์ฉ์๋ฅผ ๊ฐ์ฅ ํ  ์ ์๋๋ก ํจ

<br>



### ๐ DOM ์ญ์  ๊ด๋ จ ๋ฉ์๋

```js
const header = document.querySelector('#location-header')
header.remove()
```

- `ChildNode.remove()` - Node๊ฐ ์ํ ํธ๋ฆฌ์์ ํด๋น Node๋ฅผ ์ ๊ฑฐ

```js
const parent = document.querySelector('ul')
const child = document.querySelector('ul > li')

const removeChild = parent.removeChild(child)

console.log(removeChild)	// <li class="ssafy-location">์์ธ</li> ์ญ์ ๋ ๋์์ง๋ง ๊ฐ์ ๊ฐ์ง๊ณ  ์์, ์์น ๋ฐ๊ฟ๋ ์ด์ฉ ๊ฐ๋ฅ!!
```

- `Node.removeChild()` 

  - DOM์์ ์์ Node๋ฅผ ์ ๊ฑฐํ๊ณ  ์ ๊ฑฐ๋ Node๋ฅผ ๋ฐํ

  - Node๋ ์ธ์๋ก ๋ค์ด๊ฐ๋ ์์ Node์ ๋ถ๋ชจ Node

<br>

### ๐ DOM ์์ฑ ๊ด๋ จ ๋ฉ์๋

```js
const header = document.querySelector('#location-header')

header.setAttribute('class', 'ssafy-location')
```

- `Element.setAttribute(name, value)`
  - ์ง์ ๋ ์์์ ๊ฐ์ ์ค์ 
  - ์์ฑ์ด ์ด๋ฏธ ์กด์ฌํ๋ฉด ๊ฐ์ ๊ฐฑ์ , ์กด์ฌํ์ง ์์ผ๋ฉด ์ง์ ๋ ์ด๋ฆ๊ณผ ๊ฐ์ผ๋ก ์ ์์ฑ์ ์ถ๊ฐ
  - header class์ ssafy-location ์ถ๊ฐํ๋ค๋ ๋ป



```js
const getAttr = document.querySelector('.ssafy-location')

getAttr.getAttribute('class')	// 'ssafy-location'
```

- `Element.getAttribute(attributeName)`
  - ํด๋น ์์์ ์ง์ ๋ ๊ฐ(๋ฌธ์์ด)์ ๋ฐํ
  - ์ธ์(attributeName)๋ ๊ฐ์ ์ป๊ณ ์ ํ๋ ์์ฑ์ ์ด๋ฆ

<br>



## ๐ฎ ๊ฒฐ๋ก 

![image-20220427173042108](JavaScript%20-%20DOM%20%EC%A1%B0%EC%9E%91.assets/image-20220427173042108.png)