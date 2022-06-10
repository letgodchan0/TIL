# 🧋 Java 배열

> Java 배열 기본!

<br>

## ✔️ 배열 선언

- 1차원 배열 선언

```java
int [] prime;
prime = new int [10];		// new는 크기가 정해지지 않은 데이터를 생성할 때 사용하는 키워드

int [] prime = new int [10]; // 10개의 정수를 담을 수 있는 메모리의 주소가 prime에 담김
int prime [] = new int [10]; // 같은거
```

<br>

- 다차원 배열 선언

```java
int[][] prime = new int[][];
int prime[][] = new int[][];

int [][] prime = new int[3][3];
int [][] priem = new int[3][];
```

<br>

- 타입

| 타입    | 배열 이름 | 선언             |
| ------- | --------- | ---------------- |
| int     | iArr      | int [] iArr;     |
| char    | cArr      | char [] cArr     |
| boolean | bArr      | boolean [] bArr  |
| String  | strArr    | String [] strArr |
| Date    | dateArr   | Date [] dateArr  |

<br>

- 자동 초기화

> 배열이 생성되면 자동적으로 배열 요소는 기본값으로 초기화횐다.
>
> 멤버변수와 로컬변수 모두 배열이 생성되면 자동 초기화횐다.

| 타입    | 초기값   |
| ------- | -------- |
| int     | 0        |
| boolean | false    |
| char    | '\u0000' |
| 참조형  | null     |

<br>

##  ✔️ 초기화

```java
int [] intList = new int[3];
intList[0] = 10;
intList[1] = 20;
intList[2] = 30;

int [][] arrayList = new int[3][2];
arrayList[0][0] = 10;
arrayList[2][1] = 30;
```

- 배열의 인덱스는 0부터 시작
- 배열의 크기 : 배열이름.length
- 마지막 요소 인덱스 : 배열 크기 - 1

<br>

### {} 활용하는 초기화

```java
int [] prime = {1, 2, 3};
int [] prime = new int[] {1, 2};


int [][] twoArr = {{1, 2}, {3, 4}, {5, 6}};
```

<br>

## ✔️ 배열 관련 API

### - System.arraycopy

```java
/* System.arraycopy(src, srcPos, dest, destPos, length)
   src: 원본 배열, srcPos: 원본 배열의 복사 시작 위치 (0부터 시작)
   dest: 복사할 배열, destPos: 복사 받을 시작 위치
   length: 복사할 크기
*/

String [] oriArr = {"봄", "여름", "가을"};
String [] destArr = new String[oriArr.length+1];

System.arraycopy(oriArr, 0, destArr, 0, oriArr.length);
destArr[3] = "겨울";

for(int i =0; i < destArr.length; i++)
    System.out.println(destArr[i])
```

<br>

### - Arrays.toString

```java
/* Arrays.toString(배열객체) : 배열 안의 요소를 [요소, 요소,...] 의 형태로 출력 */

for (int i=0; i < destArr.length; i++)
    System.out.println(destArr[i]);
// 위와 같으면 배열의 요소 출력, but 단순 배열값 확인하고 싶은 경우
System.out.println(Arrays.toString(destArr));
```

<br>

## ✔️ 배열 응용

### - for-each

```java
int intArray [] = {1, 3, 5, 7, 9};

// 배열의 요소를 출력하고 싶다면 아래와 같이 해야 했는데
for (int i=0; i < intArray.length; i++){
    System.out.println(intArray[i])
}

// for-each 굳
for( int x : intArray){		// intArray의 요소를 차례대로 x에 담는다.
    System.out.println(x)
}
```

- 가독성이 개선된 반복문으로, 배열 및 Collections에서 사용
- index 대신 직접 요소에 접근하는 변수를 제공

<br>

### - 최대값 최소값 찾기

```java
int[] intArray = {3, 27, 13, 8, 235, 7, 22, 9}

int min = 1000;
int max = 0;

for(int num : intArray){
    if (num > max){
        max = num;
    }
    if (num < min){
        min = num;
    }
}
System.out.printf("min: %d, max: %d", min, max);
```

```java
// 다른 방법
int[] intArray = {3, 27, 13, 8, 235, 7, 22, 9}
int min = Integer.MAX_VALUE;
int max = Integer.MIN_VALUE;

for(int num : intArray){
    min = Math.min(min, num);
    max = Math.max(max, num);
}
System.out.printf("min: %d, max: %d", min, max);
```

<br>

### - 요소의 빈도 카운팅

```java
int[] intArray = {3, 7, 2, 5, 7, 7, 9, 2, 8, 1, 1, 5, 3};
int[] used = new int[10];

for(int num : intArray){
    used[num]++;
}
System.out.println(Arrays.toString(used));
```

<br>

### - 2차원 배열의 원소 중 3의 배수의 개수와 합을 출력

```java
int [][] grid = {
    {2, 3, 1, 4, 7}, {8, 13, 3, 33, 1},
    {7, 4, 5, 80, 12}, {17, 9, 11, 5, 5},
    {4, 5, 91, 27, 7}
}
int count = 0;
int sum = 0;
for (int i = 0; i < grid.length; i++){
    for (int j=0; j < i.length; j++) {
        if (grid[i][j] % 3 == 0){
            count++;
            sum += grid[i][j];
        }
    }
}
// 또는
for (int[] row: grid) {
    for (int num : row) {
        if (num%3==0) {
            cnt++;
            sumt += num;
        }
    }
}
```







