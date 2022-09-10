# 🌱 Index

[참고](https://gyoogle.dev/blog/computer-science/data-base/Index-.html)

> 추가적인 쓰기 작업과 저장 공간을 활용하여 DB 테이블의 검색 속도를 향상 시키기위한 자료구조.
>
> 해당 테이블의 컬럼을 색인화(따로 파일로 저장)하여 검색 시 해당 테이블의 레코드를 Full Scan 하는 것이 아니라, 색인화 되어 있는 INDEX 파일을 검색하여 검색 속도를 빠르게 한다!

<br>

## DB Index

#### 1. 목적 : RDBMS에서 검색 속도를 높이기 위한 기술

- 테이블의 Column을 색인화 함 (따로 파일로 저장)
- 해당 테이블의 레코드를 Full scan 하지 않음
- 색인화 된 (B + Tree 구조) Index 파일 검색으로 검색 속도 향상

<br>

#### 2. 과정 : Table을 생성하면, MYD, MYI, FRM 3개의 파일이 생성됨

> Index를 해당 컬럼에 주게 되면 초기 테이블 생성시 만들어진 MYD,MYI,FRM 3개의 파일중에서
> MYI에 해당 컬럼을 색인화 하여 저장한다. (Index를 사용 안할시에는 MYI파일은 비어 있음.) 그래서 Index를 해당 컬럼에 만들게 되면 해당컬럼을 따로 인덱싱하여 MYI 파일에 입력한다. 그래서 사용자가 SELECT쿼리로 Index가 사용하는 쿼리를 사용시 해당 테이블을 검색하는것이 아니라 빠른 TREE로 정리해둔 MYI파일의 내용을 검색한다.

- FRM : 테이블 구조가 저장되어 있는 파일
- MYD : 실제 데이터가 있는 파일
- MYI : Index 정보가 들어가 있는 파일

<br>

#### 3. 단점

- Index 생성시 `.mdb` 파일 크기가 증가함
- 한 페이지를 동시에 수정할 수 있는 병행성이 줄어듬
- 인덱스 된 Field에서 Data를 업데이트 하거나, 레코드를 추가 또는 삭제 시 성능이 떨어짐
- 데이터 변경 작업이 자주 일어나는 경우, Index를 재작성 해야 하므로, 성능에 영향을 미침

<br>

#### 4. 상황 분석

-  사용하면 좋은 경우

  - Where 절에서 자주 사용되는 Column

  - 외래키가 사용되는 Column
  - Join에 자주 사용되는 Column

- Index 사용을 피해야 하는 경우

  - Data 중복도가 높은 Column
  - DML이 자주 일어나는 Column

<br>

#### 5. DML이 일어났을 때의 상황

- INSERT

  - 기존 Block에 여유가 없을 때, 새로운 Data가 입력됨
  - 새로운 Block을 할당 받은 후, Key를 옮기는 작업을 수행 (많은 양의 Redo를 유발함)
  - Index split 작업 동안, 해당 Block의 Key 값에 대해서 DML이 블로킹 됨(대기 이벤트가 발생)

- DELETE

  - **[Table과 Index 상황 비교]**

  - Table에서 data가 delete 되는 경우 : Data가 지워지고, 다른 Data가 그 공간을 사용 가능

  - Index에서 Data가 delete 되는 경우 : Data가 지워지지 않고, 사용 안 됨 표시만 해둠.

    -> **Table의 Data 수와 Index의 Data 수가 다를 수 있음**.

- UPDATE

  - Table에서 update가 발생하면 -> Index는 Update 할 수 없음.

  - Index에서는 **Delete가 발생한 후**, **새로운 작업의 Insert 작업** / 2배의 작업이 소요되어, 힘듬