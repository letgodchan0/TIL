# 🌱 SQL 문법

## 테이블 생성 (CREATE)

```sqlite
CREATE TABLE classmates (
id INTEGER PRIMARY KEY,    				# PRIMARY KEY로 설정되는 부분은 무조건 INTEGER로
name TEXT
age INT									# 여기는 INT로도 가능!
);
```

## 테이블 조회

```sql
.tables
```

## 특정 테이블의 스키마 조회

```sql
.schema classmates
```

## 테이블 삭제

```sql
DROP TABLE classmates;
```

## INSERT - 특정 테이블에 레코드 삽입

**<u>Q. 컬럼이 name, age, address인 테이블 살포시 생성해봐</u>**

```sqlite
CREATE TABLE classmates (
name TEXT,
age TEXT,
address TEXT
);
```

> INSERT INTO 테이블 이름 (컬럼1, 컬럼2, ....) VALUES (값1, 값2, ....);

**<u>Q. 이름이 홍길동, 나이가 23인 데이터 삽입</u>**

```SQL
INSERT INTO classmates (name, age) VALUES ('홍길동', 23);
```

**<u>Q. 이름이 홍길동, 나이가 30, 주소가 서울인 데이터 삽입</u>**

```sql
INSERT INTO classmates VALUES ('홍길동', 30, '서울');
```

- 현재 classmates 의 컬럼이 `이름`, `나이`, `주소`, 3가지 일때, 모든 컬럼에 데이터가 있는 경우 따로 column을 명시하지 않아도 된다!!

**<u>Q. SQLite가 따로 관리하고 있는 rowid도 같이 확인하기</u>**

```SQLITE
SELECT rowid, * FROM classmates;
```

<u>**Q. 각각의 속성을 비워두지 않고 꼭 값을 입력하도록 설정하기 (NOT NULL)**</u>

```SQLITE
CREATE TABLE classmates (
id INTEGER PRIMARY KEY,    				
name TEXT NOT NULL,
age INT NOT NULL,
address TEXT NOT NULL,
);
```

<u>**Q. 스키마에 id도 직접 장성했을 때, (홍길동, 나이가 30, 주소가 서울)인 데이터 넣어보기**</u>

```sqlite
INSERT INTO classmates VALUES ('홍길동', 30, '서울');
```

![image-20220315211638139](sql%20%EB%AC%B8%EB%B2%95.assets/image-20220315211638139.png)

1. **첫번째 방법 - id를 포함한 모든 value를 작성**

```sqlite
INSERT INTO classmates VALUES (1, '홍길동', 30, '서울');
```

2. **각 value에 맞는 column들을 명시적으로 작성**

```sqlite
INSERT INTO classmates (name, age, address) VALUES ('홍길동', 30, '서울');
```

### 굳이 id를 추가 안해주면 된다!!!!

**<u>Q. INSERT 여러개 해보기</u>**

```sqlite
INSERT INTO classmates VALUES 
('홍길동', 30, '서울'),
('김철수', 24, '경기'),
('이태양', 53, '부산'),
('서희수', 17, '광주'),;
```

## Select - 테이블에서 데이터 조회

### **기본**

**<u>Q. 테이블에서 id, name 컬럼 값만 조회</u>**

```sqlite
SELECT rowid, name FROM classmates;
```

### **LIMIT**

- 쿼리에서 반환되는 행 수를 제한
- 특정 행부터 시작해서 조회하기 위해 offset 키워드와 함께 사용하기도 함

**<u>Q. 테이블에서 id, name 컬럼 값을 1개 조회</u>**

```sqlite
SELECT rowid, name FROM classmates LIMIT 1;
```

### **OFFSET**

- 동일 오브젝트 안에서 오브젝트 처음부터 주어진 요소나 지점까지의 변위치(위치 변화량)를 나타내는 정수형

- 문자열 'ABCDEF' 에서 문자 'C'는 시작점 'A' 에서 2의 OFFSET을 지님

- ```SQLITE
  SELECT * FROM MY_TABLE LIMIT 10 OFFSET 5
  ```

  - 6번째 행 부터 10개 행을 조회 (6번째 행부터 10개를 출력)
  - 0부터 시작함

<u>**Q. 테이블에서 id, name 컬럼 값을 세 번째에 있는 하나만 조회 (OFFSET)**</u>

```sqlite
SELECT rowid, name FROM classmates LIMIT 1 OFFSET 2;
```

### **WHERE**

- 쿼리에서 반환된 행에 대한 특정 검색 조건을 지정

**<u>Q. id, name 컬럼 값 중에 주소가 서울인 경우의 데이터를 조회</u>**

```sqlite
SELECT rowid, name FROM classmates WHERE address='서울';
```

### **DISTINCT**

- 조회 결과에서 중복 행을 제거
- DISTINCT 절은 SELECT 키워드 바로 뒤에 작성해야 함

**<u>Q. 테이블에서 age값 전체를 중복없이 조회</u>**

```sqlite
SELECT DISTINCT age FROM classmates;
```



## DELETE - 테이블에서 행 제거

**<u>Q. 테이블에 id가 5인 레코드 삭제</u>**

```sqlite
DELETE FROM classmates WHERE rowid=5;
```

> 만약 지금 다시 데이터를 추가하면 그 데이터는 id 5를 받는다, 즉 sqlite는 기본적으로 id를 재사용한다!!!

### AUTOINCREMENT

- SQLite가 사용되지 않은 값이나 이전에 삭제된 행의 값을 재사용하는 것을 방지
- 테이블을 생성하는 단계에서 AUTOINCREMENT를 설정한다. (장고에서는 기본적으로 ID 재사용 X)

```SQLITE
CREATE TABLE 테이블 이름 (
id INTEGER PRIMARY KEY AUTOINCREMENT,
...
)
```



## UPDATE - 기존 행의 데이터를 수정

<u>**Q. id가 5인 레코드를 수정, 이름을 홍길동, 주소를 제주도로!**</u>

```sqlite
UPDATE classmates SET name='홍길동', address='제주도' WHERE rowid=5;
```

## 요약

| CRUD |  구문  |                             예시                             |
| :--: | :----: | :----------------------------------------------------------: |
|  C   | INSERT | **INSERT INTO** 테이블 이름 (컬럼1, 컬럼2, ...) **VALUES** (값1, 값2); |
|  R   | SELECT |      **SELECT** * **FROM** 테이블 이름 **WHERE** 조건;       |
|  U   | UPDATE | **UPDATE** 테이블 이름 **SET** 컬럼1=값1, 컬럼2=값2 **WHERE** 조건; |
|  D   | DELETE |           **DELETE FROM** 테이블 이름 WHERE 조건;            |

