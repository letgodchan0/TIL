# ๐ง Java ๋ฐฐ์ด

> Java ๋ฐฐ์ด ๊ธฐ๋ณธ!

<br>

## โ๏ธ ๋ฐฐ์ด ์ ์ธ

- 1์ฐจ์ ๋ฐฐ์ด ์ ์ธ

```java
int [] prime;
prime = new int [10];		// new๋ ํฌ๊ธฐ๊ฐ ์ ํด์ง์ง ์์ ๋ฐ์ดํฐ๋ฅผ ์์ฑํ  ๋ ์ฌ์ฉํ๋ ํค์๋

int [] prime = new int [10]; // 10๊ฐ์ ์ ์๋ฅผ ๋ด์ ์ ์๋ ๋ฉ๋ชจ๋ฆฌ์ ์ฃผ์๊ฐ prime์ ๋ด๊น
int prime [] = new int [10]; // ๊ฐ์๊ฑฐ
```

<br>

- ๋ค์ฐจ์ ๋ฐฐ์ด ์ ์ธ

```java
int[][] prime = new int[][];
int prime[][] = new int[][];

int [][] prime = new int[3][3];
int [][] priem = new int[3][];
```

<br>

- ํ์

| ํ์    | ๋ฐฐ์ด ์ด๋ฆ | ์ ์ธ             |
| ------- | --------- | ---------------- |
| int     | iArr      | int [] iArr;     |
| char    | cArr      | char [] cArr     |
| boolean | bArr      | boolean [] bArr  |
| String  | strArr    | String [] strArr |
| Date    | dateArr   | Date [] dateArr  |

<br>

- ์๋ ์ด๊ธฐํ

> ๋ฐฐ์ด์ด ์์ฑ๋๋ฉด ์๋์ ์ผ๋ก ๋ฐฐ์ด ์์๋ ๊ธฐ๋ณธ๊ฐ์ผ๋ก ์ด๊ธฐํํ๋ค.
>
> ๋ฉค๋ฒ๋ณ์์ ๋ก์ปฌ๋ณ์ ๋ชจ๋ ๋ฐฐ์ด์ด ์์ฑ๋๋ฉด ์๋ ์ด๊ธฐํํ๋ค.

| ํ์    | ์ด๊ธฐ๊ฐ   |
| ------- | -------- |
| int     | 0        |
| boolean | false    |
| char    | '\u0000' |
| ์ฐธ์กฐํ  | null     |

<br>

##  โ๏ธ ์ด๊ธฐํ

```java
int [] intList = new int[3];
intList[0] = 10;
intList[1] = 20;
intList[2] = 30;

int [][] arrayList = new int[3][2];
arrayList[0][0] = 10;
arrayList[2][1] = 30;
```

- ๋ฐฐ์ด์ ์ธ๋ฑ์ค๋ 0๋ถํฐ ์์
- ๋ฐฐ์ด์ ํฌ๊ธฐ : ๋ฐฐ์ด์ด๋ฆ.length
- ๋ง์ง๋ง ์์ ์ธ๋ฑ์ค : ๋ฐฐ์ด ํฌ๊ธฐ - 1

<br>

### - {} ํ์ฉํ๋ ์ด๊ธฐํ

```java
int [] prime = {1, 2, 3};
int [] prime = new int[] {1, 2};


int [][] twoArr = {{1, 2}, {3, 4}, {5, 6}};
```

<br>

## โ๏ธ ๋ฐฐ์ด ๊ด๋ จ API

### - System.arraycopy

```java
/* System.arraycopy(src, srcPos, dest, destPos, length)
   src: ์๋ณธ ๋ฐฐ์ด, srcPos: ์๋ณธ ๋ฐฐ์ด์ ๋ณต์ฌ ์์ ์์น (0๋ถํฐ ์์)
   dest: ๋ณต์ฌํ  ๋ฐฐ์ด, destPos: ๋ณต์ฌ ๋ฐ์ ์์ ์์น
   length: ๋ณต์ฌํ  ํฌ๊ธฐ
*/

String [] oriArr = {"๋ด", "์ฌ๋ฆ", "๊ฐ์"};
String [] destArr = new String[oriArr.length+1];

System.arraycopy(oriArr, 0, destArr, 0, oriArr.length);
destArr[3] = "๊ฒจ์ธ";

for(int i =0; i < destArr.length; i++)
    System.out.println(destArr[i])
```

<br>

### - Arrays.toString

```java
/* Arrays.toString(๋ฐฐ์ด๊ฐ์ฒด) : ๋ฐฐ์ด ์์ ์์๋ฅผ [์์, ์์,...] ์ ํํ๋ก ์ถ๋ ฅ */

for (int i=0; i < destArr.length; i++)
    System.out.println(destArr[i]);
// ์์ ๊ฐ์ผ๋ฉด ๋ฐฐ์ด์ ์์ ์ถ๋ ฅ, but ๋จ์ ๋ฐฐ์ด๊ฐ ํ์ธํ๊ณ  ์ถ์ ๊ฒฝ์ฐ
System.out.println(Arrays.toString(destArr));
```

<br>

## โ๏ธ ๋ฐฐ์ด ์์ฉ

### - for-each

```java
int intArray [] = {1, 3, 5, 7, 9};

// ๋ฐฐ์ด์ ์์๋ฅผ ์ถ๋ ฅํ๊ณ  ์ถ๋ค๋ฉด ์๋์ ๊ฐ์ด ํด์ผ ํ๋๋ฐ
for (int i=0; i < intArray.length; i++){
    System.out.println(intArray[i])
}

// for-each ๊ตณ
for( int x : intArray){		// intArray์ ์์๋ฅผ ์ฐจ๋ก๋๋ก x์ ๋ด๋๋ค.
    System.out.println(x)
}
```

- ๊ฐ๋์ฑ์ด ๊ฐ์ ๋ ๋ฐ๋ณต๋ฌธ์ผ๋ก, ๋ฐฐ์ด ๋ฐ Collections์์ ์ฌ์ฉ
- index ๋์  ์ง์  ์์์ ์ ๊ทผํ๋ ๋ณ์๋ฅผ ์ ๊ณต

<br>

### - ์ต๋๊ฐ ์ต์๊ฐ ์ฐพ๊ธฐ

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
// ๋ค๋ฅธ ๋ฐฉ๋ฒ
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

### - ์์์ ๋น๋ ์นด์ดํ

```java
int[] intArray = {3, 7, 2, 5, 7, 7, 9, 2, 8, 1, 1, 5, 3};
int[] used = new int[10];

for(int num : intArray){
    used[num]++;
}
System.out.println(Arrays.toString(used));
```

<br>

### - 2์ฐจ์ ๋ฐฐ์ด์ ์์ ์ค 3์ ๋ฐฐ์์ ๊ฐ์์ ํฉ์ ์ถ๋ ฅ

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
// ๋๋
for (int[] row: grid) {
    for (int num : row) {
        if (num%3==0) {
            cnt++;
            sumt += num;
        }
    }
}
```







