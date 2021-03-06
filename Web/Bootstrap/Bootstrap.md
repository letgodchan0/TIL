# ๐ฑ Bootstrap

- [ํญ์ ์ฐธ๊ณ  ํด์ผ ํ  ์ฌ์ดํธ](https://getbootstrap.com/docs/5.1/utilities/spacing/#margin-and-padding)
- ์ผ๋ฐ์ ์ธ ๋ถ์คํธํธ๋ฉ ์ฌ์ฉํ  ๋ ๋งํฌ์ ์คํฌ๋ฆฝํธ์ ๊ฐ๊ฐ `css` ํ์ผ๊ณผ `js` ํ์ผ์ ์ฝ์ํด์ผ ํ๋ค.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="bootstrap.css">
  <title>Document</title>
</head>
<body>
  <h1>๋ถ์คํธํธ๋ฉ!</h1>
    
    <script src="bootstrap.bundle.js"></script>
    # ์ผ๋ฐ์ ์ผ๋ก ์คํฌ๋ฆฝํธ๋ ๋ฐ๋ ํ๊ทธ ๋ซ๊ธฐ์ ์ ์๋ ฅํด์ค๋ค. 
</body>
</html>
```

- ์์ ์ฌ์ฉ๋ฒ์ ์ง์  `css` ํ์ผ๊ณผ `js` ํ์ผ์ ๋ค์ด๋ก๋ ๋ฐ๊ณ  ๊ฐ์ ๊ฒฝ๋ก ์์ ์์ด์ผ ํ๋ค.

- ๋ ๊ฐ๋จํ๊ฒ ์ฌ์ฉํ๋ ๋ฐฉ๋ฒ์ผ๋ก `CDN`์ด ์๋ค.
- Content Delivery(Distribution) Network
- ์ปจํ์ธ (CSS, JS, Image, Text ๋ฑ)์ ํจ์จ์ ์ผ๋ก ์ ๋ฌํ๊ธฐ ์ํด ์ฌ๋ฌ ๋ธ๋์ ๊ฐ์ง ๋คํธ์ํฌ์ ๋ฐ์ดํฐ๋ฅผ ์ ๊ณตํ๋ ์์คํ.
- ๊ฐ๋ณ end-uesr์ ๊ฐ๊น์ด ์๋ฒ๋ฅผ ํตํด ๋น ๋ฅด๊ฒ ์ ๋ฌ ๊ฐ๋ฅ(์ง๋ฆฌ์  ์ด์ )
- ์ธ๋ถ ์๋ฒ๋ฅผ ํ์ฉํจ์ผ๋ก์จ ๋ณธ์ธ ์๋ฒ์ ๋ถํ๊ฐ ์ ์ด์ง

```html
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
</head>
<body>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
</body>
```

## spacing

![image-20220207235739311](Bootstrap.assets/image-20220207235739311.png)

#### .mt-1

```css
๋ถํธ์คํธ๋ฉ์ mt-1์ ์๋์ ๊ฐ์ด ์ ์ธํ๋ค.
.mt-1 {
    margin-top: 0.25rem !important;
}

16 * 0.25 = 4px
๋ธ๋ผ์ฐ์  <html>์ root ๊ธ๊ผด ํฌ๊ธฐ๋ 16px
```

| class name | rem     | px   |
| ---------- | ------- | ---- |
| m-1        | 0.25rem | 4    |
| m-2        | 0.5rem  | 8    |
| m-3        | 1rem    | 16   |
| m-4        | 1.5rem  | 24   |
| m-5        | 3rem    | 48   |

#### .mx-0

```css
.mx-0 {
    margin-right: 0 !important;
    margin-left: 0 !important
}
```

#### .mx-auto

```css
.mx-auto {
    margin-right: auto !important;
    margin-left: auto !important;
}

# ์ํ ์ค์ ์ ๋ ฌ
```

#### .py-0

```css
.py-0 {
    padding-top : 0 !important;
    padding-bottom: 0 !important;
}
```



## color

![image-20220207235846072](Bootstrap.assets/image-20220207235846072.png)



## ์ค์ต ํด๋ณด๊ธฐ

### Spacing

```html
<h1>bootstrap ๊ธฐ์ด ํ์ฉ</h1>
<h2>spacing</h2>

# .mt - 5 {
	margin-top: 3rem !important;
} 
<div class="box mt-5">mt-5, 3rem</div>

# .mx-auto {
	margin-right: auto !important;
	margin-left: auto !important;
}
<div class="box mx-auto">mx-auto</div>

# .ms-auto {
	margin-left: auto !important;
}
<div class="box ms-auto">ms-auto</div>

# .py-3 {
	padding-top: 1rem !important;
	padding-bottom: 1rem !important;
} 
  .my-3 {
	margin-top: 1rem !important;
	margin-bottom: 1rem !important;
}
<div class="box py-3 my-3">py-3, my-3</div>
```

![image-20220208010333075](Bootstrap.assets/image-20220208010333075.png)

### Color

```html
<h2>color</h2>
<div class="bg-primary"></div>
<div class="bg-danger"></div>
<div class="bg-warning"></div>
<p class="text-secondary">Lorem ipsum dolor sit amet consectetur adipisicing elit. Blanditiis consequuntur doloremque distinctio odit incidunt soluta ut magni. Recusandae et laboriosam delectus cum, fuga perferendis neque mollitia tenetur minima ut dicta.</p>
<h3 class="text-info">์๋ํ์ธ์</h3>
```

![image-20220208010824373](Bootstrap.assets/image-20220208010824373.png)

### Display

```html
<h2>Display</h2>
<div class="d-inline text-white bg-primary">d-inline</div>
<div class="d-inline text-white bg-primary">d-inline</div>
<span class="d-block">d-block</span>
<span class="d-block">d-block</span>

<div class="d-none d-sm-block bg-warning">sm์ด์์์ ๋ณด์</div>
<div class="d-none d-md-block d-lg-none bg-warning">md์์๋ง ๋ณด์</div>
```

![image-20220208012854977](Bootstrap.assets/image-20220208012854977.png)

- ํน์  ๋๋ฐ์ด์ค์ ๊ฐ๋ก ํญ ํฌ๊ธฐ์ ๋ํด ํด๋น ํด๋์ค๊ฐ ํฌํจ๋ ํ๊ทธ๋ฅผ ๊ฐ๋ฆฌ๊ฑฐ๋ ๋ณด์ด๋๋ก ํ๋ ๋ฐฉ๋ฒ
- d-none, d-block๊ณผ ๊ฐ์ด ์ฌ์ฉ๋๋ฉฐ ํน์  ๋๋ฐ์ด์ค ํฌ๊ธฐ์ ๋ํด ๋ ํด๋์ค๋ฅผ ์กฐํฉํด์ ์ฌ์ฉํด์ผ ํ๋ค.

| Hidden on all                              | `.d-none`                         |
| ------------------------------------------ | --------------------------------- |
| Hidden only on all (๋ชจ๋  ํ๋ฉด์์ ์จ๊ธฐ๊ธฐ)  | `.d-none`                         |
| Hidden only on xs (xs ํฌ๊ธฐ์์๋ง ์จ๊ธฐ๊ธฐ)   | `.d-none .d-sm-block`             |
| Hidden only on sm (sm ํฌ๊ธฐ์์๋ง ์จ๊ธฐ๊ธฐ)   | `.d-sm-none .d-md-block`          |
| Hidden only on md (md ํฌ๊ธฐ์์๋ง ์จ๊ธฐ๊ธฐ)   | `.d-md-none .d-lg-block`          |
| Hidden only on lg (lg ํฌ๊ธฐ์์๋ง ์จ๊ธฐ๊ธฐ)   | `.d-lg-none .d-xl-block`          |
| Hidden only on xl (xl ํฌ๊ธฐ์์๋ง ์จ๊ธฐ๊ธฐ)   | `.d-xl-none .d-xxl-block`         |
| Hidden only on xxl (xxl ํฌ๊ธฐ์์๋ง ์จ๊ธฐ๊ธฐ) | `.d-xxl-none`                     |
| Visible on all (๋ชจ๋  ํ๋ฉด์์ ๋ณด์ด๊ธฐ)      | `.d-block`                        |
| Visible only on xs (xs ํฌ๊ธฐ์์๋ง ๋ณด์ด๊ธฐ)  | `.d-block .d-sm-none`             |
| Visible only on sm (sm ํฌ๊ธฐ์์๋ง ๋ณด์ด๊ธฐ)  | `.d-none .d-sm-block .d-md-none`  |
| Visible only on md (md ํฌ๊ธฐ์์๋ง ๋ณด์ด๊ธฐ)  | `.d-none .d-md-block .d-lg-none`  |
| Visible only on lg (lg ํฌ๊ธฐ์์๋ง ๋ณด์ด๊ธฐ)  | `.d-none .d-lg-block .d-xl-none`  |
| Visible only on xl (xl ํฌ๊ธฐ์์๋ง ๋ณด์ด๊ธฐ)  | `.d-none .d-lx-block .d-xxl-none` |

### Position

```html
<h2>Position</h2>
<div class="position-container position-relative bg-warning">
    <div class="position-absolute top-0 start-0 bg-primary"></div>
    <div class="position-absolute bottom-0 end-0 bg-info"></div>
</div>
```

![image-20220208013730132](Bootstrap.assets/image-20220208013730132.png)

```html
<h2>
    <div class="box fixed-top">fixed-top</div>
    <div class="box fixed-bottom">fixed-bottom</div>
</h2>
```

![image-20220208014043508](Bootstrap.assets/image-20220208014043508.png)
