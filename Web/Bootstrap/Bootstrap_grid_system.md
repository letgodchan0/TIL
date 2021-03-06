# ๐ฑ Bootstrap - Grid System

## Responsive Web Design

- ๋ค์ํ ํ๋ฉด ํฌ๊ธฐ๋ฅผ ๊ฐ์ง ๋๋ฐ์ด์ค๋ค์ด ๋ฑ์ฅํจ์ ๋ฐ๋ผ responsive web design ๊ฐ๋์ด ๋ฑ์ฅ
- ๋ฐ์ํ ์น์ ๋ณ๋์ ๊ธฐ์  ์ด๋ฆ์ด ์๋ ์น ๋์์ธ์ ๋ํ ์ ๊ทผ ๋ฐฉ์, ๋ฐ์ํ ๋ ์ด์์ ์์ฑ์ ๋์์ด ๋๋ ์ฌ๋ก๋ค์ ๋ชจ์ ๋ฑ์ ๊ธฐ์ ํ๋๋ฐ ์ฌ์ฉ๋๋ ์ฉ์ด
- Media Queries, Flexbox, Bootstrap Grid System, The viewport meta tag

![image-20220208210429775](Bootstrap_grid_system.assets/image-20220208210429775.png)

**<span style=color:indianred>๊ฐ์ ์ปจํ์ธ ๋ฅผ ๋ณด๋ ๊ฐ๊ธฐ ๋ค๋ฅธ ๋๋ฐ์ด์ค์ ํ๋ฉด์ ๋ง์ถ์ด ๋ณด์ด๋๋ก ํ๋ ๊ฒ!</span>**

## Grid system (web design)

- ์์๋ค์ ๋์์ธ๊ณผ ๋ฐฐ์น์ ๋์์ ์ฃผ๋ ์์คํ
- ๊ธฐ๋ณธ์์
  - Column : ์ค์  ์ปจํ์ธ ๋ฅผ ํฌํจํ๋ ๋ถ๋ถ (12๊ฐ)
  - Gutter : ์ปฌ๋ผ๊ณผ ์ปฌ๋ผ ์ฌ์ด์ ๊ณต๊ฐ (์ฌ์ด ๊ฐ๊ฒฉ)
  - Container : Column๋ค์ ๋ด๊ณ  ์๋ ๊ณต๊ฐ 

## Bootstrap grid system

- bootstrap grid system์ flexbox๋ก ์ ์๋จ
- container, rows, column์ผ๋ก ์ปจํ์ธ ๋ฅผ ๋ฐฐ์นํ๊ณ  ์ ๋ ฌ
- ๋ฐ๋์ ๊ธฐ์ตํด์ผํ  2๊ฐ์ง!
  1. **12๊ฐ์ column**
  2. **6๊ฐ์ grid breakpoints**

### ๊ธฐ๋ณธ์ ์ธ ์ฌ์ฉ๋ฒ

```html
<div class="container">
    <div class="row">
        <div class="col">col</div>
        <div class="col">col</div>
        <div class="col">col</div>
    </div>
</div>
```

### Grid system breakpoints

![image-20220208212823449](Bootstrap_grid_system.assets/image-20220208212823449.png)

### Grid system breakpoints ์ฐ์ต1

```html
<div class="container">
    <h2 class="text-center">column</h2>
    <div class="row">
        <div class="box col">1</div>
        <div class="box col">2</div>
        <div class="box col">3</div>
    </div>
    <hr>
  
    <div class="row">
      <div class="box col">1</div>
      <div class="box col">2</div>
      
      # ์๋ก์ด class "w-100"(width 100%)์ ํด์ฃผ๋ฉด์ ์ค๋ฐ๊ฟ์ ํ  ์ ์๋ค. 
      <div class="w-100"></div>
      
      <div class="box col">3</div>
      <div class="box col">4</div>
    </div>
  </div>
```

![image-20220208214223652](Bootstrap_grid_system.assets/image-20220208214223652.png)

### Grid system breakpoints ์ฐ์ต2

```html
<div class="container">
    <div class="row">
      <div class="box col-3">1</div>
      <div class="box col-6">2</div>
      <div class="box col-3">3</div>
  </div>
  <hr>
  
    <div class="row">
      <div class="box col-1">1</div>
      <div class="box col-1">2</div>
      <div class="box col-1">3</div>
      <div class="box col-1">4</div>
      <div class="box col-1">5</div>
      <div class="box col-1">6</div>
      <div class="box col-1">7</div>
      <div class="box col-1">8</div>
      <div class="box col-1">9</div>
      <div class="box col-1">10</div>
      <div class="box col-1">11</div>
      <div class="box col-1">12</div>
      <div class="box col-1">13</div>
    </div>
  </div>
```

![image-20220208214547767](Bootstrap_grid_system.assets/image-20220208214547767.png)

### Grid system breakpoints ์ฐ์ต3

```html
<div class="container">
	<div class="row">
    	<div class="box col-9">col-9</div>
        <div class="box col-4">col-4</div>
        <div class="box col-3">col-3</div>
</div>
```



![image-20220208214739615](Bootstrap_grid_system.assets/image-20220208214739615.png)

### ๋ฐ์ํ ์ฐ์ต

```html
# breakpoint์ ๋ง๊ฒ col์ ๋ฐ๊ฟ
<div class="container">
    <h2 class="text-center">๋ฐ์ํ ์ฐ์ต</h2>
    <div class="row">
        <div class="box col-2 col-sm-8 col-md-4 col-lg-5">576px ๋ฏธ๋ง 2, 576 ์ด์ 8, 768 ์ด์ 4, 992 ์ด์ 5</div>
        <div class="box col-8 col-sm-2 col-md-4 col-lg-2">576px ๋ฏธ๋ง 8, 576 ์ด์ 2, 768 ์ด์ 4, 992 ์ด์ 2</div>
        <div class="box col-2 col-sm-2 col-md-4 col-lg-5">576px ๋ฏธ๋ง 2, 576 ์ด์ 2, 768 ์ด์ 4, 992 ์ด์ 5</div>
    </div>
```

![image-20220208220737566](Bootstrap_grid_system.assets/image-20220208220737566.png)

### Nesting

```html
# col์ ํ ๋นํ ์์ ํ์์ row๋ฅผ ๋ง๋ค๊ณ  col์ ๊ฐ๊ฐ ํ ๋นํจ 
<div class="container">
    <h2 class="text-center">Nesting ์ฐ์ต</h2>
    <div class="row">
      <div class="box col-6">
        <div class="row">
          # 4๋ฑ๋ถ
          <div class="box col-3">1</div>
          <div class="box col-3">2</div>
          <div class="box col-3">3</div>
          <div class="box col-3">4</div>
        </div>
      </div>
      <div class="box col-6">1</div>
      <div class="box col-6">2</div>
      <div class="box col-6">3</div>
```

![image-20220208221825745](Bootstrap_grid_system.assets/image-20220208221825745.png)

### Offset

- offset ์ ํ์๋ฅผ ์ถ๊ฐํ๋ฉด ์ผ์ชฝ์ผ๋ก๋ถํฐ ํฌ๊ธฐ๋งํผ ๋จ์ด์ ธ์ ๋ฐฐ์น๋๋ค.
- ์ด๋ค ์์๋ฅผ ์ค์ ์ ๋ ฌํ๊ฑฐ๋ ์ค๋ฅธ์ชฝ ์ ๋ ฌํ  ๋ ์ ์ฉํ๋ค.

```html
<div class="container">
    <div class="row">
      <div class="box col-md-4 offset-4">md-4 offset-4</div>
      <div class="box col-md-4 offset-md-4 offset-lg-2">md-4 offset-md-4 offset-lg-2</div>
    </div>
  </div>
```

- md ๋ณด๋ค ์์ ๋

![image-20220209005042679](Bootstrap_grid_system.assets/image-20220209005042679.png)

- md ์ด์์ผ ๋

![image-20220209005101275](Bootstrap_grid_system.assets/image-20220209005101275.png)

- lg ์ด์์ผ ๋

![image-20220209005125309](Bootstrap_grid_system.assets/image-20220209005125309.png)

