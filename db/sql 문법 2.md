# 🌱 SQL - 실습

## CSV 파일 정보를 테이블에 적용하기

1. 먼저 테이블을 생성한다

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

2. csv 파일을 테이블에 적용한다

```sqlite
.mode csv
.import users.csv users
.tables
```



## WHERE 활용

<u>**Q. age가 30 이상인 유저의 모든 컬럼 정보를 조회**</u>

```sqlite
SELECT * FROM users WHERE age >= 30;
```

**<u>Q. age가 30 이상인 유저의 이름만 조회</u>**

```sqlite
SELECT first_name FROM users WHERE age >= 30;
```

**<u>Q. age가 30 이상이고 성이 '김'인 사람의 나이와 성만 조회</u>**

```sqlite
SELECT age, last_name FROM users WHERE age >= 30 AND last_name='김';
```

## AGGREGATE function

- 집계함수, 값 집합에 대해 계산을 수행하고 단일 값을 반환
  - 여러 행으로부터 하나의 결과값을 반환하는 함수
- SELECT 구문에서만 사용됨

### COUNT - 그룹의 항목 수를 가져옴

**<u>Q. 테이블 레코드 총 개수를 조회</u>**

```SQLITE
SELECT COUNT(*) FROM users;
```

### AVG - 모든 값의 평균을 계산

**<u>Q. 30살 이상인 사람들의 평균 나이는?</u>**

```SQLITE
SELECT AVG(age) FROM users WHERE age>=30;
```

**<u>Q. 30살 이상인 사람의 계좌 평균 잔액을 조회</u>**

```sqlite
SELECT AVT(balance) FROM users WHERE age>=30;
```



### MAX - 그룹에 있는 모든 값의 최대값을 가져옴

**<u>Q. balance가 가장 높은 사람과 그 액수를 조회</u>**

```sqlite
SELECT first_name, MAX(balance) FROM users;
```

- MIN
  - 그룹에 있는 모든 값의 최소값을 가져옴
- SUM
  - 모든 값의 합을 계산

## LIKE operator

- 패턴 일치를 기반으로 데이터를 조회하는 방법
- `%` - 0개 이상의 문자, **이 자리에 문자열이 있을 수도, 없을 수도 있다.**
- `_` - 임의의 단일 문자, **반드시 이 자리에 한 개의 문자가 존재해야 한다.**

| 와일드카드 패턴 |                     의미                      |
| :-------------: | :-------------------------------------------: |
|       2%        |                2로 시작하는 값                |
|       %2        |                 2로 끝나는 값                 |
|       %2%       |                2가 들어가는 값                |
|       _2%       | 아무 값이 하나 있고 두 번째가 2로 시작하는 값 |
|      1___       |          1로 시작하고 총 4자리인 값           |
| 2_%_% or 2__3%  |        2로 시작하고 적어도 3자리인 값         |

**<u>Q. 나이가 20대인 사람만 조회</u>**

```SQLITE
SELECT * FROM users WHERE age LIKE '2_';
```

**<u>Q. 지역 번호가 02인 사람만 조회</u>**

```SQLITE
SELECT * FROM users WHERE phone LIKE '02-%';
```

**<u>Q. 이름이 '준'으로 끝나는 사람만 조회</u>**

```SQLITE
SELECT * FROM users WHERE first_name LIKE '%준';
```

**<u>Q. 중간 번호가 5114인 사람만 조회</u>**

```SQLITE
SELECT * FROM users WHERE phone LIKE '%-5114-%';
```

## ORDER BY

- 조회 결과 집합을 정렬
- SELECT 문에 추가하여 사용
  - ASC - 오름차순 (default)
  - DESC - 내림차순

**<u>Q. 나이 순으로 오름차순 정렬하여 상위 10개만 조회</u>**

```SQLITE
SELECT * FROM users ORDER BY age ASC LIMIT 10;
```

<u>**Q. 나이 순, 성 순으로 오름차순 정렬하여 상위 10개만 조회**</u>

```sqlite
SELECT * FROM users ORDER BY age, last_name ASC LIMIT 10;
```

<u>**Q. 계좌 잔액 순으로 내림차순 정렬하여 해당 유저의 성과 이름을 10개만 조회**</u>

```sqlite
SELECT last_name, first_name FROM users ORDER BY balance DESC LIMIT 10;
```

## GROUP BY

- 행 집합에서 요약 행 집합을 만듦
- SELECT 문의 optional 절
- 선택된 그룹을 하나 이상의 열 값으로 요약 행으로 만듦
- 문장에 WHERE 절이 포함된 경우 반드시 WHERE 절 뒤에 작성해야 함

<hr>

<u>**Q. 테이블에서 각 성(last_name)씨가 몇 명씩 있는지 조회한다면?**</u>

```SQLITE
SELECT last_name COUNT(*) FROM users GROUP BY last_name;
```

> AS를 활용해서 COUNT에 해당하는 컬럼 명을 바꿔서 조회할 수 있음

```sqlite
SELECT last_name COUNT(*) AS name_count FROM users GROUP BY last_name;
```

## ALTER TABLE

**<u>Q. title과 content라는 컬럼을 가진 articles라는 테이블 만들기 (두 털럼 모두 비어 있으면 x, rowid 사용 o)</u>**

```SQLITE
CREATE TABLE articles (
title TEXT NOT NULL,
content TEXT NOT NULL
)
```

**<u>Q. 값을 추가하기 (title은 '1번제목', content는 '1번 내용')</u>**

```sqlite
INSERT INTO articles VALUES ('1번제목', '1번 내용')
```

### ALTER의 기능

**<u>Q. 방금 만든 테이블의 이름 변경하기</u>** (테이블 이름 변경)

```sqlite
ALTER TABLE articles RENAME TO news;
```

**<u>Q. 새로운 컬럼(created_at, TEXT 타입, NULL설정) 추가 (테이블에 새로운 컬럼 추가)</u>**

```SQLITE
ALTER TABLE news ADD COLUMN created_at TEXT NOT NULL;

# 실패... Error를 읽어보자!
# Error: Cannot add a NOT NULL column with default value NULL
```

- 해결 방법

1. NOT NULL 설정 없이 추가하기

```sqlite
ALTER TABLE news ADD COLUMN created_at TEXT;
```

2. 기본 값(DEFAULT) 설정하기

```SQLITE
ALTER TABLE news ADD COLUMN subtitle TEXT NOT NULL DEFAULT '소제목';
```

> 장고에서는 테이블 열을 추가하려면, 새로운 표를 만들고 원래 있던 표를 지우고 새로운 표의 이름을 바꾸는 방식이다!

