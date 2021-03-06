# π± SQL

## κ΄κ³ν λ°μ΄ν°λ² μ΄μ€ (RDB)

> ν€(key)μ κ°(value)λ€μ κ°λ¨ν κ΄κ³(relation)λ₯Ό ν(table) ννλ‘ μ λ¦¬ν λ°μ΄ν° λ² μ΄μ€

- μ€ν€λ§ : λ°μ΄ν°λ² μ΄μ€μμ μλ£μ κ΅¬μ‘°, ννλ°©λ², κ΄κ³, type λ± μ λ°μ μΈ λͺμΈλ₯Ό κΈ°μ ν κ²
- νμ΄λΈ : μ΄(μ»¬λΌ/νλ)κ³Ό ν(λ μ½λ/κ°)μ λͺ¨λΈμ μ¬μ©ν΄ μ‘°μ§λ λ°μ΄ν° μμλ€μ μ§ν©
- κΈ°λ³Έν€ (Primary Key) : κ° ν(λ μ½λ)μ κ³ μ  κ°μΌλ‘, λ°λμ μ€μ ν΄μΌ νλ©°, DB κ΄λ¦¬ λ° κ΄κ³ μ€μ μ μ£Όμνκ² νμ© λ¨

## κ΄κ³ν λ°μ΄ν°λ² μ΄μ€ κ΄λ¦¬ μμ€ν (RDBMS)

> MySQL, SQLite, PostgresSQL, ORACLE, MS SQL, λ± κ΄κ³ν λͺ¨λΈμ κΈ°λ°μΌλ‘ νλ λ°μ΄ν° λ² μ΄μ€ κ΄λ¦¬ μμ€νμ μλ―Έ

## SQLite

> μλ² ννκ° μλ νμΌ νμμΌλ‘ μμ© νλ‘κ·Έλ¨μ λ£μ΄μ μ¬μ©νλ λΉκ΅μ  κ°λ²Όμ΄ λ°μ΄ν°λ² μ΄μ€, κ΅¬κΈ μλλ‘μ΄λ μ΄μμ²΄μ μ κΈ°λ³Έμ μΌλ‘ νλλ λ°μ΄ν°λ² μ΄μ€μ΄λ©°, μλ² λλ μννΈμ¨μ΄μλ λ§μ΄ νμ©λ¨, λ‘μ»¬μμ κ°λ¨ν DB κ΅¬μ±μ ν  μ μμΌλ©°, μ€νμμ€ νλ‘μ νΈμ΄κΈ° λλ¬Έμ μμ λ‘­κ² μ¬μ© κ°λ₯!

### Data Type

1. NULL
2. INTEGER
   - ν¬κΈ°μ λ°λΌ 0, 1, 2, 3, 4, 6 λλ 8λ°μ΄νΈμ μ μ₯λ λΆνΈ μλ μ μ
3. REAL
   - 8λ°μ΄νΈ λΆλ μμμ  μ«μλ‘ μ μ₯λ λΆλ μμμ  κ°
4. TEXT
5. BLOB
   - μλ ₯λ κ·Έλλ‘ μ νν μ μ₯λ λ°μ΄ν° (λ³λ€λ₯Έ νμ μμ΄ κ·Έλλ‘ μ μ₯)

## SQLite μ€μΉ

1. [μ¬κΈ° λ€μ΄κ°μ 2κ° μ€μΉ!!](https://www.sqlite.org/download.html)

![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F85d304cf-7928-492c-833d-f69be6a30f4f%252Fsqlite%EC%88%981%EC%99%84.png)

2. `cλλΌμ΄λΈ` - sqlite ν΄λ μμ± ν λͺ¨λ μμΆ νμ΄μ νμΌμ `sqlite` ν΄λλ‘ μ΄λμν€κΈ°

![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F3dc21cca-b18c-489b-9497-235404f6ec71%252Fsqlite2.png)



3. νκ²½λ³μ νΈμ§

![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F4b36bdac-9c48-4fad-a94c-cea75980933e%252Fsqlite3.png)

![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F23936382-94cc-4fb8-bc68-3c84ee843dae%252Fsqlite4.png)





![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F401699a7-3ccc-4293-95bb-6c73f2bce699%252Fsqlite5.png)



![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F5dc263e5-a221-4745-ac33-b0f3db3f94c7%252Fsqlite6.png)



![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F0d83ba6d-cfa5-4b6f-b264-32c8dc390bb6%252Fsqlite%EC%88%982%EC%99%84.png)

## alias λ±λ‘

> `winpty sqlite3` μ»€λ§¨λλ₯Ό `sqlite3`λ‘ λ³κ²½νκΈ°

![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252Fbef45797-b786-42ee-a91c-5606bdc21e47%252F11.png)



![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252Fb028599c-0475-44d0-8213-72a3b8bf1ffa%252Falias2.png)

![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F47efac86-2043-43c4-9649-3b724b51e18f%252F22.png)

![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F7920d06b-7815-4203-a438-225bd71da5da%252Fsqlite%EC%88%983%EC%99%84.png)

![img](sql%20%EA%B8%B0%EC%B4%88.assets/https%253A%252F%252Fs3-us-west-2.amazonaws.com%252Fsecure.notion-static.com%252F508317b1-8bbd-4d2f-b721-24db948715a1%252Fsqlite%EC%88%984%EC%99%84.png)



## SQL

> κ΄κ³ν λ°μ΄ν°λ² μ΄μ€ κ΄λ¦¬μμ€νμ λ°μ΄ν° κ΄λ¦¬λ₯Ό μν΄ μ€κ³λ νΉμ λͺ©μ μ νλ‘κ·Έλλ° μΈμ΄
>
> - λ°μ΄ν°λ² μ΄μ€ μ€ν€λ§ μμ± λ° μμ 
> - μλ£μ κ²μ λ° κ΄λ¦¬
> - λ°μ΄ν°λ² μ΄μ€ κ°μ²΄ μ κ·Ό μ‘°μ  κ΄λ¦¬

|          λΆλ₯          |                             κ°λ                             |              μμ               |
| :--------------------: | :----------------------------------------------------------: | :-----------------------------: |
| DDL - λ°μ΄ν° μ μ μΈμ΄ | κ΄κ³ν λ°μ΄ν° λ² μ΄μ€ κ΅¬μ‘°(νμ΄λΈ, μ€ν€λ§)λ₯Ό μ μνκΈ° μν λͺλ Ήμ΄ |       CREATE, DROP, ALTER       |
| DML - λ°μ΄ν° μ‘°μ μΈμ΄ |    λ°μ΄ν°λ₯Ό μ μ₯, μ‘°ν, μμ , μ­μ  λ±μ νκΈ° μν λͺλ Ήμ΄     | INSERT, SELECT, UPDATE, DELETE  |
| DCL - λ°μ΄ν° μ μ΄ μΈμ΄ |    λ°μ΄ν°λ² μ΄μ€ μ¬μ©μμ κΆν μ μ΄λ₯Ό μν΄ μ¬μ©νλ λͺλ Ήμ΄    | GRANT, REVOKE, COMMIT, ROLLBACK |

### λ°μ΄ν°λ² μ΄μ€ μμ±νκΈ°

```BASH
$ sqlite3 tutorial.sqlite 3
sqlite> .database
```

### csv νμΌμ tableλ‘ λ§λ€κΈ°

```bahse
sqlite> .mode csv
sqlite> .import hellodb.csv examples
sqlite> .tables
examples
```

### SELECT - νΉμ  νμ΄λΈμ νμ λ°ν

```BASH
SELECT * FROM examples;
```

![image-20220314132608055](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314132608055.png)

### μ»¬λΌ κ°μ΄ λ³΄κΈ°

```bash
sqlite> .headers on
sqlite> SELECT * FROM examples;
```

![image-20220314133352960](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133352960.png)

### μ»¬λΌμ΄λ ν κ΅¬λΆν΄μ κ°μ΄ λ³΄μ¬μ£ΌκΈ°

```bash
sqlite> .mode column
sqlite> SELECT * FROM examples;
```

![image-20220314133316043](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133316043.png)



> μμμ μ¬μ©νλ©΄ λΆνΈν μ μ΄ λ§κΈ° λλ¬Έμ sqlite νμ₯ νλ‘κ·Έλ¨μ μ¬μ©ν  μ μλ€!

### 1. sqlite νμΌ μ°μΈ‘ ν΄λ¦­ - Open Database

![image-20220314133711714](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133711714.png)

### 2. New Query ν΄λ¦­

![image-20220314133744139](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133744139.png)

### 3. μ°μΈ‘ νλ©΄μ SQL λͺλ Ήμ΄λ₯Ό μμ±νλ νμ΄μ§κ° μΆλ ₯λ¨

![image-20220314133822044](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133822044.png)

### 4. μ½λ μμ± ν μ°μΈ‘ ν΄λ¦­ - Run Query(μ μ²΄ μ½λ μ€ν) or Run Selected Query(μ ν μ½λλ§ μ€ν)

![image-20220314133920794](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133920794.png)

### 5. μλ‘κ³ μΉ¨ ν΄λ¦­ ν λ°μ΄ν°λ² μ΄μ€ λ³ν νμΈ

![image-20220314133950943](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133950943.png)

### 6. νΉμ  μ½λλ§ μ€ν ν κ°μ₯ μ°μΈ‘ νλ©΄μμ κ²°κ³Ό νμΈν΄λ³΄κΈ°

![image-20220314134041171](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314134041171.png)