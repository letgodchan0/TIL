# ๐ฑ SQL - ์ค์ต

## CSV ํ์ผ ์ ๋ณด๋ฅผ ํ์ด๋ธ์ ์ ์ฉํ๊ธฐ

1. ๋จผ์  ํ์ด๋ธ์ ์์ฑํ๋ค

```SQLITE
CREATE TABLE users (
first_name TEXT NOT NULL,
last_name TEXT NOT NULL,
age INTEGER NOT NULL,
country TEXT NOT NULL, 
phone TEXT NOT NULL,    
balance INTEGER NOT NULL,    
);
```

2. csv ํ์ผ์ ํ์ด๋ธ์ ์ ์ฉํ๋ค

```sqlite
.mode csv
.import users.csv users
.tables
```



## WHERE ํ์ฉ

<u>**Q. age๊ฐ 30 ์ด์์ธ ์ ์ ์ ๋ชจ๋  ์ปฌ๋ผ ์ ๋ณด๋ฅผ ์กฐํ**</u>

```sqlite
SELECT * FROM users WHERE age >= 30;
```

**<u>Q. age๊ฐ 30 ์ด์์ธ ์ ์ ์ ์ด๋ฆ๋ง ์กฐํ</u>**

```sqlite
SELECT first_name FROM users WHERE age >= 30;
```

**<u>Q. age๊ฐ 30 ์ด์์ด๊ณ  ์ฑ์ด '๊น'์ธ ์ฌ๋์ ๋์ด์ ์ฑ๋ง ์กฐํ</u>**

```sqlite
SELECT age, last_name FROM users WHERE age >= 30 AND last_name='๊น';
```

## AGGREGATE function

- ์ง๊ณํจ์, ๊ฐ ์งํฉ์ ๋ํด ๊ณ์ฐ์ ์ํํ๊ณ  ๋จ์ผ ๊ฐ์ ๋ฐํ
  - ์ฌ๋ฌ ํ์ผ๋ก๋ถํฐ ํ๋์ ๊ฒฐ๊ณผ๊ฐ์ ๋ฐํํ๋ ํจ์
- SELECT ๊ตฌ๋ฌธ์์๋ง ์ฌ์ฉ๋จ

### COUNT - ๊ทธ๋ฃน์ ํญ๋ชฉ ์๋ฅผ ๊ฐ์ ธ์ด

**<u>Q. ํ์ด๋ธ ๋ ์ฝ๋ ์ด ๊ฐ์๋ฅผ ์กฐํ</u>**

```SQLITE
SELECT COUNT(*) FROM users;
```

### AVG - ๋ชจ๋  ๊ฐ์ ํ๊ท ์ ๊ณ์ฐ

**<u>Q. 30์ด ์ด์์ธ ์ฌ๋๋ค์ ํ๊ท  ๋์ด๋?</u>**

```SQLITE
SELECT AVG(age) FROM users WHERE age>=30;
```

**<u>Q. 30์ด ์ด์์ธ ์ฌ๋์ ๊ณ์ข ํ๊ท  ์์ก์ ์กฐํ</u>**

```sqlite
SELECT AVT(balance) FROM users WHERE age>=30;
```



### MAX - ๊ทธ๋ฃน์ ์๋ ๋ชจ๋  ๊ฐ์ ์ต๋๊ฐ์ ๊ฐ์ ธ์ด

**<u>Q. balance๊ฐ ๊ฐ์ฅ ๋์ ์ฌ๋๊ณผ ๊ทธ ์ก์๋ฅผ ์กฐํ</u>**

```sqlite
SELECT first_name, MAX(balance) FROM users;
```

- MIN
  - ๊ทธ๋ฃน์ ์๋ ๋ชจ๋  ๊ฐ์ ์ต์๊ฐ์ ๊ฐ์ ธ์ด
- SUM
  - ๋ชจ๋  ๊ฐ์ ํฉ์ ๊ณ์ฐ

## LIKE operator

- ํจํด ์ผ์น๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ๋ฐ์ดํฐ๋ฅผ ์กฐํํ๋ ๋ฐฉ๋ฒ
- `%` - 0๊ฐ ์ด์์ ๋ฌธ์, **์ด ์๋ฆฌ์ ๋ฌธ์์ด์ด ์์ ์๋, ์์ ์๋ ์๋ค.**
- `_` - ์์์ ๋จ์ผ ๋ฌธ์, **๋ฐ๋์ ์ด ์๋ฆฌ์ ํ ๊ฐ์ ๋ฌธ์๊ฐ ์กด์ฌํด์ผ ํ๋ค.**

| ์์ผ๋์นด๋ ํจํด |                     ์๋ฏธ                      |
| :-------------: | :-------------------------------------------: |
|       2%        |                2๋ก ์์ํ๋ ๊ฐ                |
|       %2        |                 2๋ก ๋๋๋ ๊ฐ                 |
|       %2%       |                2๊ฐ ๋ค์ด๊ฐ๋ ๊ฐ                |
|       _2%       | ์๋ฌด ๊ฐ์ด ํ๋ ์๊ณ  ๋ ๋ฒ์งธ๊ฐ 2๋ก ์์ํ๋ ๊ฐ |
|      1___       |          1๋ก ์์ํ๊ณ  ์ด 4์๋ฆฌ์ธ ๊ฐ           |
| 2_%_% or 2__3%  |        2๋ก ์์ํ๊ณ  ์ ์ด๋ 3์๋ฆฌ์ธ ๊ฐ         |

**<u>Q. ๋์ด๊ฐ 20๋์ธ ์ฌ๋๋ง ์กฐํ</u>**

```SQLITE
SELECT * FROM users WHERE age LIKE '2_';
```

**<u>Q. ์ง์ญ ๋ฒํธ๊ฐ 02์ธ ์ฌ๋๋ง ์กฐํ</u>**

```SQLITE
SELECT * FROM users WHERE phone LIKE '02-%';
```

**<u>Q. ์ด๋ฆ์ด '์ค'์ผ๋ก ๋๋๋ ์ฌ๋๋ง ์กฐํ</u>**

```SQLITE
SELECT * FROM users WHERE first_name LIKE '%์ค';
```

**<u>Q. ์ค๊ฐ ๋ฒํธ๊ฐ 5114์ธ ์ฌ๋๋ง ์กฐํ</u>**

```SQLITE
SELECT * FROM users WHERE phone LIKE '%-5114-%';
```

## ORDER BY

- ์กฐํ ๊ฒฐ๊ณผ ์งํฉ์ ์ ๋ ฌ
- SELECT ๋ฌธ์ ์ถ๊ฐํ์ฌ ์ฌ์ฉ
  - ASC - ์ค๋ฆ์ฐจ์ (default)
  - DESC - ๋ด๋ฆผ์ฐจ์

**<u>Q. ๋์ด ์์ผ๋ก ์ค๋ฆ์ฐจ์ ์ ๋ ฌํ์ฌ ์์ 10๊ฐ๋ง ์กฐํ</u>**

```SQLITE
SELECT * FROM users ORDER BY age ASC LIMIT 10;
```

<u>**Q. ๋์ด ์, ์ฑ ์์ผ๋ก ์ค๋ฆ์ฐจ์ ์ ๋ ฌํ์ฌ ์์ 10๊ฐ๋ง ์กฐํ**</u>

```sqlite
SELECT * FROM users ORDER BY age, last_name ASC LIMIT 10;
```

<u>**Q. ๊ณ์ข ์์ก ์์ผ๋ก ๋ด๋ฆผ์ฐจ์ ์ ๋ ฌํ์ฌ ํด๋น ์ ์ ์ ์ฑ๊ณผ ์ด๋ฆ์ 10๊ฐ๋ง ์กฐํ**</u>

```sqlite
SELECT last_name, first_name FROM users ORDER BY balance DESC LIMIT 10;
```

## GROUP BY

- ํ ์งํฉ์์ ์์ฝ ํ ์งํฉ์ ๋ง๋ฆ
- SELECT ๋ฌธ์ optional ์ 
- ์ ํ๋ ๊ทธ๋ฃน์ ํ๋ ์ด์์ ์ด ๊ฐ์ผ๋ก ์์ฝ ํ์ผ๋ก ๋ง๋ฆ
- ๋ฌธ์ฅ์ WHERE ์ ์ด ํฌํจ๋ ๊ฒฝ์ฐ ๋ฐ๋์ WHERE ์  ๋ค์ ์์ฑํด์ผ ํจ

<hr>

<u>**Q. ํ์ด๋ธ์์ ๊ฐ ์ฑ(last_name)์จ๊ฐ ๋ช ๋ช์ฉ ์๋์ง ์กฐํํ๋ค๋ฉด?**</u>

```SQLITE
SELECT last_name COUNT(*) FROM users GROUP BY last_name;
```

> AS๋ฅผ ํ์ฉํด์ COUNT์ ํด๋นํ๋ ์ปฌ๋ผ ๋ช์ ๋ฐ๊ฟ์ ์กฐํํ  ์ ์์

```sqlite
SELECT last_name COUNT(*) AS name_count FROM users GROUP BY last_name;
```

## ALTER TABLE

**<u>Q. title๊ณผ content๋ผ๋ ์ปฌ๋ผ์ ๊ฐ์ง articles๋ผ๋ ํ์ด๋ธ ๋ง๋ค๊ธฐ (๋ ํธ๋ผ ๋ชจ๋ ๋น์ด ์์ผ๋ฉด x, rowid ์ฌ์ฉ o)</u>**

```SQLITE
CREATE TABLE articles (
title TEXT NOT NULL,
content TEXT NOT NULL
)
```

**<u>Q. ๊ฐ์ ์ถ๊ฐํ๊ธฐ (title์ '1๋ฒ์ ๋ชฉ', content๋ '1๋ฒ ๋ด์ฉ')</u>**

```sqlite
INSERT INTO articles VALUES ('1๋ฒ์ ๋ชฉ', '1๋ฒ ๋ด์ฉ')
```

### ALTER์ ๊ธฐ๋ฅ

**<u>Q. ๋ฐฉ๊ธ ๋ง๋  ํ์ด๋ธ์ ์ด๋ฆ ๋ณ๊ฒฝํ๊ธฐ</u>** (ํ์ด๋ธ ์ด๋ฆ ๋ณ๊ฒฝ)

```sqlite
ALTER TABLE articles RENAME TO news;
```

**<u>Q. ์๋ก์ด ์ปฌ๋ผ(created_at, TEXT ํ์, NULL์ค์ ) ์ถ๊ฐ (ํ์ด๋ธ์ ์๋ก์ด ์ปฌ๋ผ ์ถ๊ฐ)</u>**

```SQLITE
ALTER TABLE news ADD COLUMN created_at TEXT NOT NULL;

# ์คํจ... Error๋ฅผ ์ฝ์ด๋ณด์!
# Error: Cannot add a NOT NULL column with default value NULL
```

- ํด๊ฒฐ ๋ฐฉ๋ฒ

1. NOT NULL ์ค์  ์์ด ์ถ๊ฐํ๊ธฐ

```sqlite
ALTER TABLE news ADD COLUMN created_at TEXT;
```

2. ๊ธฐ๋ณธ ๊ฐ(DEFAULT) ์ค์ ํ๊ธฐ

```SQLITE
ALTER TABLE news ADD COLUMN subtitle TEXT NOT NULL DEFAULT '์์ ๋ชฉ';
```

> ์ฅ๊ณ ์์๋ ํ์ด๋ธ ์ด์ ์ถ๊ฐํ๋ ค๋ฉด, ์๋ก์ด ํ๋ฅผ ๋ง๋ค๊ณ  ์๋ ์๋ ํ๋ฅผ ์ง์ฐ๊ณ  ์๋ก์ด ํ์ ์ด๋ฆ์ ๋ฐ๊พธ๋ ๋ฐฉ์์ด๋ค!

