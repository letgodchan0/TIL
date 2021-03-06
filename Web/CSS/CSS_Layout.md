# ๐ฑ CSS - Layout

## Float

- ๋ฐ์ค๋ฅผ ์ผ์ชฝ ํน์ ์ค๋ฅธ์ชฝ์ผ๋ก ์ด๋์์ผ ํ์คํธ๋ฅผ ํฌํจ ์ธ๋ผ์ธ ์์๋ค์ด ์ฃผ๋ณ์ wrapping ํ๋๋ก ํจ
- ์์๊ฐ Normal flow๋ฅผ ๋ฒ์ด๋๋๋ก ํจ

### Float ์์ฑ

- none : ๊ธฐ๋ณธ๊ฐ
- left : ์์๋ฅผ ์ผ์ชฝ์ผ๋ก ๋์
- right : ์์๋ฅผ ์ค๋ฅธ์ชฝ์ผ๋ก ๋์

#### ์์ 1

```html
# html
<body>
    <div class="box left">float left</div>
    <p>lorem300 ์๋ ์์ฑ์ผ๋ก ๊ธธ~๊ฒ</p>    
</body>
```

```css
# css
.box{
    width: 150px;
    height: 150px;
    border: 1px solid black;
    background-color: crimson;
    color: white;
    margin-right: 30px;
}

.left{
    float: left;
}
```

![image-20220207130256081](CSS_Layout.assets/image-20220207130256081.png)

#### ์์ 2

``` html
# html
<body>
    <header>
        <div class="box1">div</div>
    </header>
    <div class="box2">div</div>
</body>
```

```css
# css
.box1{
    width: 150px;
    height: 150px;
    border: 1px solid black;
    background-color: crimson;
    color: white;
    text-aligin: center;
    line-height: 150px;
}

.box2{
    width: 300px;
    height: 150px;
    border: 1px solid black;
    background-color: blue;
    color: white;
    text-align: center;
    line-height: 150px;
}

```

![image-20220207130811768](CSS_Layout.assets/image-20220207130811768.png)

- ์ฌ๊ธฐ์  ์ฒซ๋ฒ์งธ ๋นจ๊ฐ ๋ฐ์ค์ float๋ฅผ ์ฃผ๋ฉด ๋นจ๊ฐ ๋ฐ์ค๊ฐ ์๋ก ๋ ๋ฒ๋ ค์ ํ๋๋ฐ์ค๋ ๊ฒน์ณ์ง ํํ๋ก ๋ณด์ด๊ฒ ๋๋ค. 

![image-20220207131105362](CSS_Layout.assets/image-20220207131105362.png)

- ๋นจ๊ฐ์ ๋ฐ์ค์ ๋ด๋ถ ์์๊ฐ Float ์ํ๋ก ๋์ด๊ฐ ์ง์ ๋์ง ์์์ ์๊ธฐ๋ ํ์์ด๋ค.
- ์ด๋ฐ ์ํฉ์์, Clearing Float๋ฅผ ํด์ผํ๋ค. 

### Clearing Float

- Float๋ Normal Flow์์  ๋ฒ์ด๋ ๋ถ๋ ์ํ(๋  ์์)
- ๋ฐ๋ผ์, ์ดํ ์์์ ๋ํ์ฌ Float ์์ฑ์ด ์ ์ฉ๋์ง ์๋๋ก Clearing์ด ํ์์ ์
  - ::after : ์ ํํ ์์์ ๋งจ ๋ง์ง๋ง ์์์ผ๋ก ๊ฐ์ ์์๋ฅผ ํ๋ ์์ฑ
    - ๋ณดํต content ์์ฑ๊ณผ ํจ๊ป ์ง์ง์ด, ์์์ ์ฅ์์ฉ ์ฝํ์ธ ๋ฅผ ์ถ๊ฐํ  ๋ ์ฌ์ฉ
  - clear ์์ฑ ๋ถ์ฌ

```css
.clearfix::after{
    content: "";
    display: block;
    clear: both;
}

ํน์
.box2{
    width: 300px;
    height: 150px;
    border: 1px solid black;
    background-color: blue;
    color: white;
    text-align: center;
    line-height: 150px;
    # ์ด๊ฑฐ ์ถ๊ฐ
    clear: left;
}
```

![image-20220207130811768](CSS_Layout.assets/image-20220207130811768.png)

- Float๋ ๋ ์ด์์์ ๊ตฌ์ฑํ๊ธฐ ์ํด ํ์์ ์ผ๋ก ํ์ฉ ๋์์ผ๋ฉฐ, ์ต๊ทผ์ Flexbox, Grid ๋ฑ์ฅ๊ณผ ํจ๊ป ์ฌ์ฉ๋๊ฐ ๋ฎ์์ง
- Float ํ์ฉ ์ ๋ต - Normal Flow์์ ๋ฒ์ด๋ ๋ ์ด์์ ๊ตฌ์ฑ
  - ์ํ๋ ์์๋ค์ Float๋ก ์ง์ ํ์ฌ ๋ฐฐ์น
  - ๋ถ๋ชจ ์์์ ๋ฐ๋์ Clearing Float๋ฅผ ํ์ฌ ์ดํ ์์๋ถํฐ Normal Flow๋ฅผ ๊ฐ์ง๋๋ก ์ง์  

## Flexbox

- ํ๊ณผ ์ด ํํ๋ก ์์ดํ๋ค์ ๋ฐฐ์นํ๋ 1์ฐจ์ ๋ ์ด์์ ๋ชจ๋ธ
- ์ถ
  - main axis (๋ฉ์ธ ์ถ)
  - cross axis (๊ต์ฐจ ์ถ)
- ๊ตฌ์ฑ ์์
  - Flex Container (๋ถ๋ชจ ์์)
  - Flex Item (์์ ์์)

![image-20220207132613827](CSS_Layout.assets/image-20220207132613827.png)

- Flexbox ์ถ
  - flex-direction : row

![image-20220207132828614](CSS_Layout.assets/image-20220207132828614.png)

### Flexbox ๊ตฌ์ฑ ์์

- Flex Container (๋ถ๋ชจ ์์)
  - flexbox ๋ ์ด์์์ ํ์ฑํ๋ ๊ฐ์ฅ ๊ธฐ๋ณธ์ ์ธ ๋ชจ๋ธ
  - Flex Item๋ค์ด ๋์ฌ์๋ ์์ญ
  - display ์์ฑ์ flex ํน์ inline-flex๋ก ์ง์ 
- Flex Item (์์ ์์)
  - ์ปจํ์ด๋์ ์ํด ์๋ ์ปจํ์ธ (๋ฐ์ค)



<span style="color:indianred"><span style="color: **IndianRed**">Flexbox๋ ์๋ ๊ฐ ๋ถ์ฌ ์์ด (1)  ์์ง ์ ๋ ฌ (2) ์์ดํ์ ๋๋น์ ๋์ด ํน์ ๊ฐ๊ฒฉ์ ๋์ผํ๊ฒ ๋ฐฐ์นํ  ์ ์๋ค</span></span>

```css
.flex-container {
    display: flex;
}

# ๋ถ๋ชจ ์์์ display: flex ํน์ inline-flex
```



### Flex ์์ฑ

- ๋ฐฐ์น ์ค์ 
  - flex-direction
  - flex-wrap
- ๊ณต๊ฐ ๋๋๊ธฐ
  - justify-content (main axis)
  - align-content (cross axis)
- ์ ๋ ฌ
  - align-items (๋ชจ๋  ์์ดํ์ cross axis ๊ธฐ์ค์ผ๋ก)
  - align-self (๊ฐ๋ณ ์์ดํ)

#### flex-direction

- Main axis ๊ธฐ์ค ๋ฐฉํฅ ์ค์ 
- ์ญ๋ฐฉํฅ์ ๊ฒฝ์ฐ HTML ํ๊ทธ ์ ์ธ ์์์ ์๊ฐ์ ์ผ๋ก ๋ค๋ฅด๋ ์ ์ํด์ผ ํ๋ค (์น ์ ๊ทผ์ฑ์ ์ํฅ)

![image-20220207134026955](CSS_Layout.assets/image-20220207134026955.png)

#### flex-wrap

- ์์ดํ์ด ์ปจํ์ด๋๋ฅผ ๋ฒ์ด๋๋ ๊ฒฝ์ฐ ํด๋น ์์ญ ๋ด์ ๋ฐฐ์น๋๋๋ก ์ค์ 
- ์ฆ, ๊ธฐ๋ณธ์ ์ผ๋ก ์ปจํ์ด๋ ์์ญ์ ๋ฒ์ด๋์ง ์๋๋ก ํจ
- ์์๋ค์ด ๊ฐ์ ๋ก ํ ์ค์ ๋ฐฐ์น๋๊ฒ ํ  ๊ฒ์ธ์ง ์ฌ๋ถ ์ค์ 
  - nowrap (๊ธฐ๋ณธ๊ฐ) : ํ ์ค์ ๋ฐฐ์น
  - wrap : ๋์น๋ฉด ๊ทธ ๋ค์ ์ค๋ก ๋ฐฐ์น

![image-20220207134154819](CSS_Layout.assets/image-20220207134154819.png)

#### flex-flow

- flex-direction ๊ณผ flex-wrap์ shorthand
- flex-direction ๊ณผ flex-wrap์ ๋ํ ์ค์  ๊ฐ์ ์ฐจ๋ก๋ก ์์ฑ
- ์์ ) flex-flow : row nowrap;

#### justify-content

- Main axis๋ฅผ ๊ธฐ์ค์ผ๋ก ๊ณต๊ฐ ๋ฐฐ๋ถ

![image-20220207134624030](CSS_Layout.assets/image-20220207134624030.png)

#### align-content

- Cross axis๋ฅผ ๊ธฐ์ค์ผ๋ก ๊ณต๊ฐ ๋ฐฐ๋ถ (์์ดํ์ด ํ ์ค๋ก ๋ฐฐ์น๋๋ ๊ฒฝ์ฐ ํ์ธํ  ์ ์์)

![image-20220207220652588](CSS_Layout.assets/image-20220207220652588.png)

#### justify-content & align-content

- ๊ณต๊ฐ ๋ฐฐ๋ถ
  - flex-start (๊ธฐ๋ณธ ๊ฐ) : ์์ดํ๋ค์ axis ์์์ ์ผ๋ก 
  - flex-end : ์์ดํ๋ค์ axis ๋ ์ชฝ์ผ๋ก
  - center : ์์ดํ๋ค์ axis ์ค์์ผ๋ก
  - space-between : ์์ดํ ์ฌ์ด์ ๊ฐ๊ฒฉ์ ๊ท ์ผํ๊ฒ ๋ถ๋ฐฐ
  - space-around : ์์ดํ์ ๋๋ฌ์ผ ์์ญ์ ๊ท ์ผํ๊ฒ ๋ถ๋ฐฐ (๊ฐ์ง ์ ์๋ ์์ญ์ ๋ฐ์ผ๋ก ๋๋ ์ ์์ชฝ์)
  - space-evenly : ์ ์ฒด ์์ญ์์ ์์ดํ ๊ฐ ๊ฐ๊ฒฉ์ ๊ท ์ผํ๊ฒ ๋ถ๋ฐฐ

#### align-items

- ๋ชจ๋  ์์ดํ์ Cross axis๋ฅผ ๊ธฐ์ค์ผ๋ก ์ ๋ ฌ

![image-20220207221029747](CSS_Layout.assets/image-20220207221029747.png)

#### align-self

- ๊ฐ๋ณ ์์ดํ์ Cross axis ๊ธฐ์ค์ผ๋ก ์ ๋ ฌ
  - <span style="color:indianred"><span style="color: **IndianRed**">์ฃผ์! ํด๋น ์์ฑ์ ์ปจํ์ด๋์ ์ ์ฉํ๋ ๊ฒ์ด ์๋๋ผ ๊ฐ๋ณ ์์ดํ์ ์ ์ฉ!!!</span></span>

![image-20220207221201443](CSS_Layout.assets/image-20220207221201443.png)

#### align-items & align-self

- Cross axis๋ฅผ ์ค์ฌ์ผ๋ก
  - stretch (๊ธฐ๋ณธ ๊ฐ) : ์ปจํ์ด๋๋ฅผ ๊ฐ๋ ์ฑ์
  - flex-start : ์
  - flex-end : ์๋
  - center : ๊ฐ์ด๋ฐ
  - baseline : ํ์คํธ baseline์ ๊ธฐ์ค์ ์ ๋ง์ถค

#### ๊ธฐํ ์์ฑ

- flex-grow : ๋จ์  ์์ญ์ ์์ดํ์ ๋ถ๋ฐฐ
- order : ๋ฐฐ์น ์์

>![image-20220207222039826](CSS_Layout.assets/image-20220207222039826.png)

## ๋ ์ด์์ ํ์ฉ

### ์์ง ์ํ ๊ฐ์ด๋ฐ ์ ๋ ฌ

```css
# ๋ฐฉ๋ฒ 1
์ปจํ์ด๋ ์ค์ 

.container {
    display: flex;
    justify-content: center;
    align-items: center;
}

# ๋ฐฉ๋ฒ 2
์์ดํ ์ค์ 

.container {
    display: flex;
}

.item{
    margin: auto;
}
```

> ![image-20220207222247350](CSS_Layout.assets/image-20220207222247350.png)

### ์นด๋ ๋ฐฐ์น

```css
#layout_03 {
    display: flex;
    flex-direction: colum;
    flex-wrap: wrap;
    justify-content: space-around;
    align-content: space-around;
}
```

> ![image-20220207222522271](CSS_Layout.assets/image-20220207222522271.png)

```css
#layout_04{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
    align-content: space-around;
}
```

> ![image-20220207222600556](CSS_Layout.assets/image-20220207222600556.png)