# 🌱 SQL - Join

> 두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법

테이블을 연결하려면, 적어도 하나의 컬럼을 서로 공유하고 있어야 하므로, 이를 이용하여 데이터 검색에 활용한다.

<br>

### INNER JOIN

![image-20220905233522540](sql JOIN.assets/image-20220905233522540.png)

```SQL
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
INNER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키...
```

```sql
SELECT A.NAME, B.AGE FROM EX_TABLE A
INNER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

- 교집합으로, 기준 테이블과 JOIN 테이블의 중복된 값을 보여준다. 

<BR>

### LEFT OUTER JOIN

![image-20220905233538357](sql JOIN.assets/image-20220905233538357.png)

```SQL
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
LEFT OUTER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키...
```

```sql
SELECT A.NAME, B.AGE FROM EX_TABLE A
LEFT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

- 기준 테이블값과 조인 테이블과 중복된 값을 보여준다.
- 왼쪽 테이블 기준으로 JOIN을 한다고 생각하자!!

<BR>

### RIGHT OUTER JOIN

![image-20220905233645252](sql JOIN.assets/image-20220905233645252.png)

```SQL
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
RIGHT OUTER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키...
```

```SQL
SELECT A.NAME, B.AGE FROM EX_TABLE A
RIGHT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

- 오른쪽 테이블을 기준으로 JOIN 하는 것!

<br>

### FULL OUTER JOIN

![image-20220905233736796](sql JOIN.assets/image-20220905233736796.png)

```SQL
SELECT
테이블별칭.조회할칼럼,
테이블별칭.조회할칼럼
FROM 기준테이블 별칭
FULL OUTER JOIN 조인테이블 별칭 ON 기준테이블별칭.기준키 = 조인테이블별칭.기준키...
```

```sql
SELECT A.NAME, B.AGE FROM EX_TABLE A
FULL OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
```

