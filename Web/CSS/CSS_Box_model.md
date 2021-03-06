# ๐ฑ CSS - Box model

**๋ชจ๋  ์์๋ ๋ค๋ชจ(๋ฐ์ค ๋ชจ๋ธ)์ด๊ณ , ์์์๋ถํฐ ์๋๋ก, ์ผ์ชฝ์์ ์ค๋ฅธ์ชฝ์ผ๋ก ์์ธ๋ค. (์ข์ธก ์๋จ์ ๋ฐฐ์น)**

![image-20220205230613043](CSS_Box_model.assets/image-20220205230613043.png)

- ๋ชจ๋  HTML ์์๋ box ํํ๋ก ๋์ด ์์

- box model ๊ตฌ์ฑ์์ - ํ๋์ ๋ฐ์ค๋ 4๊ฐ์ง ์์ญ์ผ๋ก ์ด๋ฃจ์ด์ ธ ์์

  - contents
  - padding
  - border
  - margin

  ![image-20220205230757170](CSS_Box_model.assets/image-20220205230757170.png)

### margin

```css
.margin{
    margin-top: 10px;
    margin-right: 20px;
    margin-bottom: 30px;
    margin-left: 40px;
}
์ - ์ฐ - ํ -์ข
```

![image-20220205233133692](CSS_Box_model.assets/image-20220205233133692.png)



### padding

```css
.margin-padding{
    margin: 10px;
    padding: 30px;
}
```

![image-20220205233157839](CSS_Box_model.assets/image-20220205233157839.png)

### border

```css
.border{
    border-width: 2px;
    border-style: dashed;
    border-color: black;
}
```

![image-20220205233251459](CSS_Box_model.assets/image-20220205233251459.png)

### shorthand๋ฅผ ํตํด์ ํํ

- margin/padding

![image-20220205233359144](CSS_Box_model.assets/image-20220205233359144.png)

- border

```css
.border{
    border: 2px dashed black;
}
```

![image-20220205233528747](CSS_Box_model.assets/image-20220205233528747.png)

### box ๋ง๋ค์ด ๋ณด๊ธฐ(1)

```html
#html
<body>
    <div class="box1">div</div>
    <div class="box2">div</div>
</body>
```

```css
#css
<style>
.box1{
    width: 500px;
    border-width: 2px;
    border-style: dashed;
    border-color: black;
    padding-left: 50px;
    margin-bottom: 30px;
}

.box2{
    width: 500px;
    border: 2px solid black;
    padding: 20px 30px;
}
</style>
```

![image-20220205233939451](CSS_Box_model.assets/image-20220205233939451.png)

### box ๋ง๋ค์ด ๋ณด๊ธฐ(2)

```html
#html
<body>
    <div class="box">content-box</div>
    <div class="box box-sizing">border-box</div>
</body>
```

```css
#css
<style>
.box{
    width: 100px;
    margin: 10px auto;
    padding: 20px;
    border: 1px solid black;
    color: white;
    text-align: center;
    background-color: blueviolet;
}

.box-sizing{
    box-sizing: border-box;
    margin-top: 50px;
}
</style>
```

- box ํด๋์ค์ ์ ์ฉํ๋ css๋ฅผ ๋ณด๋ฉด margin ์์ `auto`๋ผ๋ ๊ฒ์ด ์๋๋ฐ, ์  ์ฝ๋๋ฅผ ํด์ ํ๋ฉด, **์ ์๋ ์ฌ๋ฐฑ์ 10px ์ ๋ ์ฃผ๊ณ  ์ข ์ฐ๋ `auto`, ์ฆ ์์ชฝ ๋์ผํ๊ฒ ์ฌ๋ฐฑ์ ๋ง์ถฐ์ค๋ค๋ ์๋ฏธ์ด๋ค.** ๊ฒฐ๊ณผ์ ์ผ๋ก ์ฌ์ดํธ ์ค์์ ์์นํ๋ ๊ฒ์ ๋ณผ ์ ์๋ค. 
- box-sizing์ ๋ฐ์ค์ ํฌ๊ธฐ๋ฅผ ์ด๋ค ๊ฒ์ ๊ธฐ์ค์ผ๋ก ๊ณ์ฐํ ์ง๋ฅผ ์ ํ๋ ์์ฑ์ผ๋ก ๊ธฐ๋ณธ๊ฐ์ `content-box`์ด๋ค.
  - content-box : ์ปจํ์ธ  ์์ญ์ ๊ธฐ์ค์ผ๋ก ํฌ๊ธฐ๋ฅผ ์ ํจ
  - border-box : ํ๋๋ฆฌ๋ฅผ ๊ธฐ์ค์ผ๋ก ํฌ๊ธฐ ์ ํจ
  - initial : ๊ธฐ๋ณธ๊ฐ์ผ๋ก ์ค์ 
  - inherit : ๋ถ๋ชจ ์์์ ์์ฑ ๊ฐ์ ์์ ๋ฐ์
- box์ `width`๋ฅผ 100์ผ๋ก ์ฃผ์์ง๋ง, `box-sizing` ์์ฑ์ ๊ฐ์ ๋ค๋ฅด๊ฒ ์ฃผ๋ฉด ๋ฐ์ค์ ํฌ๊ธฐ๊ฐ ๋ฌ๋ผ์ง๋ค.
  - content-box๊ฐ์ ์ฃผ๋ฉด ์ปจํ์ธ  ์์ญ์ ๋๋น๊ฐ 100px์ด๊ณ  ํ๋๋ฆฌ๋ฅผ ํฌํจํ ๋๋น๋ 142px์ด๋ค.
  - border-box ๊ฐ์ ์ฃผ๋ฉด, ํ๋๋ฆฌ๋ฅผ ํฌํจํ ๋๋น๊ฐ 100px์ด๊ณ  ์ผํ์ธ  ์์ญ์ ๋๋น๋ 58์ด๋ค. 

![image-20220206204220033](CSS_Box_model.assets/image-20220206204220033.png)

### content-box์ border-box ์ฐจ์ด

![image-20220206000739134](CSS_Box_model.assets/image-20220206000739134.png)

- ๊ธฐ๋ณธ์ ์ผ๋ก ๋ชจ๋  ์์์ box-sizing์ content-box
  - Padding์ ์ ์ธํ ์์ contents ์์ญ๋ง์ box๋ก ์ง์ 
- ๋ค๋ง, ์ฐ๋ฆฌ๊ฐ ์ผ๋ฐ์ ์ผ๋ก ์์ญ์ ๋ณผ ๋ border๊น์ง์ ๋๋น๋ฅผ 100px๋ก ๋ณด๋ ๊ฒ์ ์ํจ
  - ๊ทธ ๊ฒฝ์ฐ box-sizing์ border-box๋ก ์ค์  