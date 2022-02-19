# 🌱 문자열 (string) 1

## 문자열 처리

### 문자의 표현

- 컴퓨터에서는 각 문자에 대해 대응되는 숫자를 정해 놓고 이것을 메모리에 저장하는 방법을 사용하고 있다.
- 영어가 대소문자 합쳐서 52이므로 6(64가지)비트면 모두 표현 할 수 있고 이를 코드체계라고 한다.
  - 000000 -> 'a', 000001 -> 'b'
- ASCII 코드
  - 7bit 인코딩으로 128 문자를 표현하며 33개의 출력 불가능한 제어 문자들과 공백을 비롯한 95개의 출력 가능한 문자들로 이루어져 있다.

### 문자열 뒤집기

```python
s = 'happy day'
s = s[::-1]
>>> 'yad yppah'
```

### 문자열 비교

```python
s1 = 'abc'
s2 = s1[:2] + 'c'
s1 == s2  => True  # 각각 참조하고 있는 값이 같음
s1 is s2 => False  # 참조하고 있는게 다름
```

### 문자열 정수로 변환

```python
#1
int('123') => 123

#2
# int() 와 같은 atoi() 함수 만들기
def atoi(s):
    i = 0
    for x in s:
        i = i*10 + ord(x) - ord('0')
    return i
```

## 패턴 매칭

- 고지식한 패턴 검색 알고리즘
- 카프-라빈 알고리즘
- KMP 알고리즘
- 보이어-무어 알고리즘

### 고지식한 알고리즘(Brute Force)

- 본문 문자열을 처음부터 끝까지 차례대로 순회하면서 패턴 내의 문자들을 일일이 비교하는 방식으로 동작

![image-20220217011617849](String%201.assets/image-20220217011617849.png)

- 다른 부분이 발생하면, t의 인덱스는 +1, p의 인덱스는 0으로 초기화

![image-20220217011734110](String%201.assets/image-20220217011734110.png)

####  코드로 구현

```python
P = 'is'
T = 'This is a book~!'
M = len(P)
N = len(T)
```

```python
# 1. 함수로 표현
def BruteForce(p, t):
    i = 0
    j = 0
    while j < M and i < N:
        if t[i] != p[j]:
            i = i - j
            j = -1
        i += 1
        j += 1
    if j == M:
        return i - M
    else:
        return -1
```

```python
# 2. 이중 for문
ans = -1
for i in range(N-M+1):  # 비교 시작 인덱스
    for j in range(M):
        if T[i+j] != P[j]:
            break
    else:
        ans = i
        break
```

```python
# 3. 2번 변형
for i in range(N-M+1):  # 비교 시작 인덱스
    j = 0
    while j < M:
        if t[i+j] != p[j]:
            break
        j += 1
    if j==M: # 패턴을 찾음
        ans = i
        break
```

#### 고지식한 패턴 검색 알고리즘의 시간 복잡도

- 최악의 경우 시간 복잡도는 텍스트의 모든 위치에서  패턴을 비교해야 하므로 O(MN)이 됨

  

### KMP 알고리즘

- 찾고자 하는 문자열(Pattern)을 주어진 문자열(Text)에서 빠르게 찾아내는 방법 
- 불일치가 발생한 텍스트 스트링의 앞 부분에 어떤 문자가 있는지를 미리 알고 있으므로, 불일치가 발생한 앞 부분에 대하여 다시 비교하지 않고 매칭을 수행
- 패턴을 전처리하여 배열 next[M]을 구해서 잘못된 시작을 최소화 함
  - next[M]: 불일치가 발생했을 경우 이동할 다음 위치
- 시간 복잡도
  - O(M+N)

#### 아이디어

- KMP 알고리즘을 이용하기 위해서 `Prefix (접두어)`, `Suffix (접미어)`개념에 대해 알아야 한다.

  ```bash
  ABCAB
  ```

  - Prefix는 `A, AB, ABC, ABCA `  , Suffix는 `B, AB, CAB,BCAB`

- LPS (Longest proper prefix which is suffix)는 Prefix와 Suffix가 같은 경우 중 가장 길이가 긴 경우를 뜻한다.

| substring | lps 값 |                                            |
| --------- | ------ | ------------------------------------------ |
| A         | 0      | Prefix와 Suffix가 없음                     |
| AB        | 0      | Prefix - (A)와 Suffix - (B) 불일치         |
| ABC       | 0      | Prefix - (A, AB)와 Suffix - (C, BC) 불일치 |
| ABCA      | 1      | Prefix - (A)와 Suffix  - (A) 일치          |
| ABCAB     | 2      | Prefix - (AB)와 Suffix  - (A)B 일치        |

> 결과적으로, lps = [0, 0, 0, 1, 2]



```python
def kmp(t, p):
    N = len(t)
    M = len(p)
    lps = [0] * (M+1)
    j = 0
    lps[0] = -1
    # lps 만드는 반복문
    for i in range(1, M):
        lps[i] = j 
        if p[i] == p[j]:
            j += 1
        else:
            j = 0
    lps[M] = j

    i = 0 # 비교할 텍스트 위치, t[i]
    j = 0 # 비교할 패턴 위치, p[j]
    while i < N and j <= M:
        if j == -1 or t[i] == p[j]:
            i += 1
            j += 1
        else:
            j = lps[j]
        if j == M:  # j가 M과 같다는 것은 p라는 패턴이 t에 존재
            print(i-M, end=' ')
            j = lps[j]

t = 'zzzabcdabcdabcefabcd'
p = 'abcdabcef'
kmp(t, p)
```

## 보이어 - 무어 알고리즘

- 대부분의 상용 소프트웨어에서 채택하고 있는 알고리즘
- 패턴의 오른쪽 끝에 있는 문자가 불일치 하고 이 문자가 패턴 내에 존재하지 않는 경우, 이동 거리를 패턴의 길이만큼 잡아주는 방법

![image-20220219164338784](String%201.assets/image-20220219164338784.png)

```python
Text = 'a pattern matching algorithm'
Pattern = 'rithm'
```

1. Pattern의 제일 오른 쪽 글자인 m과 Text의 제일 오른 쪽 문자인 t를 비교했을 때 불일치임을 확인하고 'rith' 중 t가 겹치기 때문에 2칸 이동
2. 다시 제일 오른쪽 글자인 e와 m이 불일치하고 rith 중에 e가 없기 때문에 5칸 이동
3. 제일 오른쪽 글자인 a와 m이 불일치하고 rith 중에 a가 없으므로 5칸 이동
4. 위 과정을 끝가지 진행한다. 

- 시간 복잡도
  - 최악의 경우 수행시간이 O(m*n)이지만, 텍스트 문자를 다 보지 않아도 되기 때문에 일반적으로 O(n)보다 시간이 덜 든다.