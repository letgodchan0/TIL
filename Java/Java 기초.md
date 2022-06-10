# 🧋 Java 기본 문법

> Java를 처음 접하면서 새로 알게 된 부분만 적었음!

<br>

### ✔️ 이클립스 단축키

- 자동 완성

```java
ctrl + space 
```

- 여러줄 주석

```java
ctrl + shift + c
```

- 정렬 및 포맷팅

```java
ctrl + shift + f
```

- 실행

```java
ctrl + f11
```

- 입력 받기

```java
// 사용자한테 입력받기
Sca + ctrl + space
Scanner sc = new Scanner(System.in);

// next()는 문자열을 입력받음, nextInt()는 숫자를 받음
String name = sc.next();
```

- 출력

```java
print
println
printf
    - %d : 정수
    - %f : 실수
    - %c : 문자
    - %s : 문자열
```



<br>

### ✔️ 자료형

- 기본 자료형

![image-20220608211312528](Java%20%EA%B8%B0%EC%B4%88.assets/image-20220608211312528.png)

- 참조 자료형 (위 8가지 외 모든 것)

<br>

### ✔️ 데이터 형변환

- 묵시적(암묵적)

```java
// 범위가 넓은 데이터 형에 좁은 데이터 형을 대입하는 것
byte b = 100; int i = b;
```

- 명시적

```java
// 범위가 좁은 데이터 형에 넓은 데이터 형을 대입하는 것, 형변환 연산자 사용
int i = 100; byte b = i;(X), byte b = (byte) i; (O)
```

<br>



### ✔️ 3항 연산자

> 조건식 ? 수식 1 : 수식 2
>
> 수식1 : 조건식의 결과가 ture일 때 수행
>
> 수식2 : 조건식의 결과가 false일 때 수행

```java
int x = 10;
int y = 5;
int max = (x>y)? x:y;

Scanner sc = new Scanner(System.in);
int number = sc.nextInt();
boolean check = (number % 4 == 0 && number % 100 != 0)? true:false;
```

<br>

### ✔️ 증감 연산자

| 연산자 | 사용법          | 설명              | 예시              |
| ------ | --------------- | ----------------- | ----------------- |
| ++     | ++op (선행처리) | 바로 1 증가       | int a = 5; a++;   |
| ++     | op++ (후행처리) | 한 턴 쉬고 1 증가 | int a = 10; ++a;  |
| --     | --op (선행처리) | 바로 1 감소       | int b =  10; --b; |
| --     | op-- (후행처리) | 한 턴 쉬고 1 감소 | int b = 10; b--;  |

<br>

### ✔️ if 문

```java
int a = 3;
if(a=3) => 에러
if(0) => 에러
//    
if (조건식) {
    System.out.println("1")
} else if (조건식) {
    System.out.println("1")
} else {
    System.out.println("2")
}
```

- 실행 문장이 복수일 때는 블록으로 처리
- 조건식 자리에는 반드시 참과 거짓으로 구분해야 한다. 

<br>

### ✔️ switch 문

```java
int number = 7;
switch(number){
    case 10:
        System.out.println("ㅎㅎ")
        break
    case 9:
        System.out.println("ㅎㅎ")
        break
    case 7:
        System.out.println("ㅎㅎ")
        break
    default:
        묵시적으로 처리해야하는 명령어
}
```

- break문이 없을 경우 break를 찾을 때까지 선택된 case문 아래의 모든 문장을 실행

<br>

### ✔️ for 문

```java
for(i=1; n < 10;; i++) {
    System.out.println(i + " ");
}
```

<br>

### ✔️ while 문

```java
while (조건절) {
    반복문장
}

int a = 10, b=20;
while (a > b)
    System.out.println("조건 만족 안함")
```

<br>

### ✔️ do ~ while 문

```java
int a=5, b=10;
do {
    System.out.println("무조건 실행됨");
}while(a>b) => 1번 실행된다!
```

- do ~ while문은 조건을 나중에 평가하기 때문에 while 블록이 적어도 한번은 수행한다!

<br>

### ✔️ break

1. 기본

```java
int i = 1;
while (i < 100) {
    if(i == 5) break;
    System.out.println(i + "this is java");
    i++;
}

1 this is java
2 this is java
3 this is java
4 this is java
```

2. 반복문이 중첩되었을 경우 가장 가까운 반복문을 빠져 나온다.

```java
int i, j;
for(i=1; i<5; i++) {
    for(j=1; j<=i; j++){
        if(j>3) break;
        System.out.print("* ");
    }
    System.out.println();
}
```

3. 중첩된 반복문을 한번에 빠져나오기

```java
int i, j;
first: for(i=1; i<=5; i++) {
    for(j=1; j<=i; j++) {
        if(j>3) break first;	// first라는 이름의 블록을 벗어난다.
    	System.out.print("* ");
    }
    System.out.println();
}
```

