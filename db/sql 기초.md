# 🌱 SQL

## 관계형 데이터베이스 (RDB)

> 키(key)와 값(value)들의 간단한 관계(relation)를 표(table) 형태로 정리한 데이터 베이스

- 스키마 : 데이터베이스에서 자료의 구조, 표현방법, 관계, type 등 전반적인 명세를 기술한 것
- 테이블 : 열(컬럼/필드)과 행(레코드/값)의 모델을 사용해 조직된 데이터 요소들의 집합
- 기본키 (Primary Key) : 각 행(레코드)의 고유 값으로, 반드시 설정해야 하며, DB 관리 및 관계 설정시 주요하게 활용 됨

## 관계형 데이터베이스 관리 시스템 (RDBMS)

> MySQL, SQLite, PostgresSQL, ORACLE, MS SQL, 등 관계형 모델을 기반으로 하는 데이터 베이스 관리 시스템을 의미

## SQLite

> 서버 형태가 아닌 파일 형식으로 응용 프로그램에 넣어서 사용하는 비교적 가벼운 데이터베이스, 구글 안드로이드 운영체제에 기본적으로 탑대된 데이터베이스이며, 임베디드 소프트웨어에도 많이 활용됨, 로컬에서 간단한 DB 구성을 할 수 있으며, 오픈소스 프로젝트이기 때문에 자유롭게 사용 가능!

### Data Type

1. NULL
2. INTEGER
   - 크기에 따라 0, 1, 2, 3, 4, 6 또는 8바이트에 저장된 부호 있는 정수
3. REAL
   - 8바이트 부동 소수점 숫자로 저장된 부동 소수점 값
4. TEXT
5. BLOB
   - 입력된 그대로 정확히 저장된 데이터 (별다른 타입 없이 그대로 저장)



## SQL

> 관계형 데이터베이스 관리시스템의 데이터 관리를 위해 설계된 특수 목적의 프로그래밍 언어
>
> - 데이터베이스 스키마 생성 및 수정
> - 자료의 검색 및 관리
> - 데이터베이스 객체 접근 조정 관리

|          분류          |                             개념                             |              예시               |
| :--------------------: | :----------------------------------------------------------: | :-----------------------------: |
| DDL - 데이터 정의 언어 | 관계형 데이터 베이스 구조(테이블, 스키마)를 정의하기 위한 명령어 |       CREATE, DROP, ALTER       |
| DML - 데이터 조작 언어 |    데이터를 저장, 조회, 수정, 삭제 등을 하기 위한 명령어     | INSERT, SELECT, UPDATE, DELETE  |
| DCL - 데이터 제어 언어 |    데이터베이스 사용자의 권한 제어를 위해 사용하는 명령어    | GRANT, REVOKE, COMMIT, ROLLBACK |

### 데이터베이스 생성하기

```BASH
$ sqlite3 tutorial.sqlite 3
sqlite> .database
```

### csv 파일을 table로 만들기

```bahse
sqlite> .mode csv
sqlite> .import hellodb.csv examples
sqlite> .tables
examples
```

### SELECT - 특정 테이블의 행을 반환

```BASH
SELECT * FROM examples;
```

![image-20220314132608055](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314132608055.png)

### 컬럼 같이 보기

```bash
sqlite> .headers on
sqlite> SELECT * FROM examples;
```

![image-20220314133352960](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133352960.png)

### 컬럼이랑 행 구분해서 같이 보여주기

```bash
sqlite> .mode column
sqlite> SELECT * FROM examples;
```

![image-20220314133316043](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133316043.png)



> 쉘에서 사용하면 불편한 점이 많기 때문에 sqlite 확장 프로그램을 사용할 수 있다!

### 1. sqlite 파일 우측 클릭 - Open Database

![image-20220314133711714](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133711714.png)

### 2. New Query 클릭

![image-20220314133744139](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133744139.png)

### 3. 우측 화면에 SQL 명령어를 작성하는 페이지가 출력됨

![image-20220314133822044](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133822044.png)

### 4. 코드 작성 후 우측 클릭 - Run Query(전체 코드 실행) or Run Selected Query(선택 코드만 실행)

![image-20220314133920794](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133920794.png)

### 5. 새로고침 클릭 후 데이터베이스 변화 확인

![image-20220314133950943](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314133950943.png)

### 6. 특정 코드만 실행 후 가장 우측 화면에서 결과 확인해보기

![image-20220314134041171](sql%20%EA%B8%B0%EC%B4%88.assets/image-20220314134041171.png)