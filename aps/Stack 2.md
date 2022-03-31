# 🌱 스택 2

## 백트래킹

> 해를 찾는 도중에 막히면 (해가 아니면) 되돌아가서 다시 해를 찾아가는 기법으로 최적화 (Optimization) 문제와 결정 (Decision) 문제를 해결할 수 있다.

결정 문제 : 문제의 조건을 만족하는 해가 존재하는지의 여부를 'yes' 또는 'no'가 답하는 문제

백트래킹 기법

- 어떤 노드의 유망성을 점검한 후에 유망하지 않다고 결정되면 그 노드의 부모로 되돌아가 다음 자식 노드로 감
- 어떤 노드를 방문하였을 때 그 노드를 포함한 경로가 해답이 될 수 없으면 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 한다.
- 가지치기 : 유망하지 않는 노드가 포함되는 경로는 더 이상 고려하지 않는다.

## 부분집합 구하기

> {1, 2, 3}의 부분집합 표현 - 0000으로 할꺼임 (앞에0은 그냥 형식 갖추려고 있음)

![image-20220227164927366](Stack%202.assets/image-20220227164927366.png)



```python
def f(i, N):  # i: 부분집합에 포함될지 결정할 원소의 시작 인덱스, N: 전체 원소 개수
    if i == N:   # 한 개의 부분집합 완성된 경우
    	print(bit, end = ' ')
        for j in range(N):
            if bit[j] == 1: # bit[j] == 1 해당 원소가 부분집합에 포함된다.
                print(a[j], end = ' ')
        print()
    # bit를 모두 채울 때까지 두 갈래로 나눠지는 부분
    else:
        bit[i] = 1
        f(i+1, N)
        bit[i] = 0
        f(i+1, N)
    return # 의미상 있는 부분
    
a = [1, 2, 3]
# bit의 모든 부분집합을 찾을 때 이런식으로 표현, 처음 비트를 무조건 포함 할꺼면 bit[0] = 1
bit = [0, 0, 0] 
f(0, 3)   # i 0번 부터 결정해봐

# 이렇게 하면 모든 부분 집합 완성
[1, 1, 1] 1 2 3 
[1, 1, 0] 1 2 
[1, 0, 1] 1 3 
[1, 0, 0] 1 
[0, 1, 1] 2 3 
[0, 1, 0] 2 
[0, 0, 1] 3 
[0, 0, 0] 

```

### 연습문제

>{1, 2, 3, 4, 5, 6, 7, 8, 9, 10} 의 부분집합 만들기

```python
def make(lst, n):	# lst: 리스트, n: 부분집합에 포함될지 결정할 원소의 시작 인덱스
    if n == len(lst): # 하나의 부분집합이 완성된 곳
        cnt += 1
        for i in range(n):
            if check[i] == 1:
                print(lst[i], end = ' ')
        print()
    else:
        check[n] = 1
        make(lst, n+1)
        check[n] = 0
        make(lst, n+1)

lst = list(range(1, 11))
check = [0] * len(lst)
make(lst, 0)
```

기본적으로 비트(?)를 이용해서 부분집합을 만드는 방법이다. 

1. 여기서는 `check`라는 0으로 채워져 있고 길이가 `lst`와 같은 리스트를 하나 만들어 준다.   

2. ```python
   lst   =  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
   check =  [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]  = 공집합
   check =  [1, 0, 0, 0, 0, 0, 0, 0, 0, 0]  = [1]
   check =  [1, 0, 0, 0, 0, 1, 0, 0, 0, 0]  = [1, 6]
   check =  [1, 0, 0, 1, 0, 0, 0, 0, 0, 1]  = [1, 4, 10]
   ```

   - `check`의 특정 인덱스가 `1`이라면 `lst`의 인덱스로 매핑되는 값의 번호가 부분집합으로 사용되었다는 것을 의미한다.

#### 연습문제

> {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}의 powerset 중 합이 10인 부분집합을 구하시오
>
> 모든 부분집합을 만들 때와 백트래킹 기법으로 했을 때 차이 비교

**<u>모든 부분집합을 만들고 합이 10인 부분집합만 따로 출력</u>**

```python
def f(i, N, k):  # i: 부분집합에 포함될지 결정할 원소의 인덱스, N: 전체 원소 개수, k: 찾는 합
    global cnt
    cnt += 1
    if i == N:   # 한개의 부분집합 완성
        s = 0 # 부분집합 원소의 합
        for j in range(N):
            if bit[j] == 1:   # 1이라는 것은 j 를 인덱스로 하는 값이 부분집합으로 포함된다는 의미
                s += a[j]
        if s == k:  # 찾는 합이면
            for j in range(N):
                print(bit[j], end = '')
                if bit[j] == 1:
                    print(a[j], end = ' ')
            print()
    else:
        bit[i] = 1
        f(i+1, N, k)
        bit[i] = 0
        f(i+1, N, k)
        
    return 
n = 10
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
bit = [0] * len(a)
cnt = 0  # 재귀 체크
k = 10
f(0, n, k)   # 0번 부터 부분집합의 합이 10인 애 찾아봐

cnt => 2047
```

**<u>백트래킹(가지치기)으로 합이 10될 것 같은 부분집합만 생성</u>**

```python
def f (i, n, k, s):  # s : i-1 원소까지 고려한 합
    global cnt
    cnt += 1
    if i == n:
        print(bit)
        if s == k:
            for j in range(n):
                if bit[j] == 1:
                    print(a[j], end= ' ')
            print()
        return
    # 다 내려가지 않았는데 합이 k보다 크다면 더 안내려가고 멈춤
    elif s > k:
        return
    else:
        bit[i] = 1
        f(i+1, n, k, s+a[i])    # a[i] 포함한 경우여서 s에 a[i]를 더해준다!
        bit[i] = 0
        f(i+1, n, k, s)  # 포함 안시키니까 s를 그대로!

n = 10
a = [x for x in range(1, n+1)]
bit = [0] * n
k = 10
cnt = 0
f(0, n, k, 0)   # 제일 오른쪽 0은 현재까지 생성된 부분집합의 합

cnt => 415
```

**<u>백트래킹(가지치기) 조금 더 정교하게</u>**

```python
# 모든 원소 고려x , s == k라면 뒤에 있는 요소 고려할 필요 x
def f (i, n, k, s):  # s : i-1 원소까지 고려한 합
    global cnt
    cnt += 1
    if s == k: # 목표값 찾았을 때
        for j in range(n):
            if bit[j]:
                print(a[j], end= ' ')
        print()
    elif i == n:  # 한 개의 부분집합이 완성된 경우
        return  # 되돌아가서 다른 노드 찾아봐
    elif s > k:   # 합이 k를 넘으면 더 밑으로 안내려감
        return
    else:
        bit[i] = 1
        f(i+1, n, k, s+a[i])    # a[i] 포함한 경우여서 s에 a[i]를 더해준다!
        bit[i] = 0
        f(i+1, n, k, s)  # 포함 안시키니까 s를 그대로!
    return

n = 10
a = [x for x in range(1, n+1)]
bit = [0] * n
k = 10
cnt = 0
f(0, n, k, 0)   # 제일 오른쪽 0은 현재까지 생성된 부분집합의 합

cnt => 349

```

**<u>위 방식과 동일하지만 코드 변형</u>**

```python
def f1(i, n, k, s): # n: bit의 크기, k: 조건의 합, s: i-1 원소 까지의 합
    global cnt
    if i == n:
        a = 0 # 현재 부분집합 원소의 합
        b = 0
        for j in range(n):
            if bit[j]:
                a += arr[j]
                b += 1
        if b == k and a == s:
            cnt += 1
    else:
        bit[i] = 1
        f1(i, n, k, s)
        bit[i] = 0
        f1(i, n, k, s)

n = 10
arr = [x for x in range(1, n+1)]
bit = [0] * n
k = 10
cnt = 0
f(0, n, k, 0)   # 제일 오른쪽 0은 현재까지 생성된 부분집합의 합
```



**<u>rs(남은 구간의 합) + s(현재까지 합)을 더해도 찾는 값보다 작다면 중단</u>**

![image-20220227182453450](Stack%202.assets/image-20220227182453450.png)

```python
def f(i, n, s, t, rs): # s:이전까지 고려된 원소의 합, t: 목표값
    global cnt
    cnt += 1
    if s == t: #  목표값을 찾으면
        for j in range(n):
            if bit[j]  == 1:
                print(a[j], end = ' ')
        print()
    elif i == n: # 더이상 고려할 원소가 없으면
        return # 되돌아가서 위의 다른 노드를 선택해봐
    elif s > t: # 목표를 넘어서면 밑에 그만 찾고 다른거 찾아봐
        return
    elif s+rs < t:
        return
    else:
        bit[i] = 1
        f(i+1, n, s+a[i], t, rs-a[i])
        bit[i] = 0
        f(i+1, n, s, t, rs-a[i])

n = 10
cnt = 0
a = [x for x in range(1, n+1)]
bit = [0] * n
t = 10 # 합이 10인경우가 있는가?
f(0, n, 0, t, sum(a))
print(cnt)

cnt => 349
```

## 순열

> [1, 2, 3]의 모든 원소를 사용한 순열
>
> 123, 132, 213, 231, 312, 321의 총 6가지 경우

![image-20220331152830110](Stack%202.assets/image-20220331152830110-16487234026311.png)

#### 1. 자리 바꾸는 방법

```python
def f(i, n):
    if i == n:
        print(p)
   	else:
        for j in range(i, n):
            p[i], p[j] = p[j], p[i]  # 다른거 넣어봐
            f(i+1, n)
            p[i], p[j] = p[j], p[i]   # 원상 복귀
	# 그냥 허전해 보여서 return 없어도 잘 돌아감
    return

n = 3
p = [x for x in range(1, n+1)]
f(0, n)  # 0번 자리부터 결정, 총 n개짜리
```

#### 2. 체크하는 방법

```python
def pm(start, cnt):
    if start == cnt:
        print(answer)
    else:
        for i in range(cnt):
            if check[i] == 0: 
                answer[start] = lst[i]
                check[i] = 1
                pm(start+1, cnt)
                check[i] = 0
lst = [1, 2, 3]
answer = [0] * len(lst)
check = [0] * len(lst)
pm(0, len(lst))
```

#### 3. 순열 - 백트래킹

> 73 21 21
> 11 59 40
> 24 31 83
>
> 행, 열들의 합 중 최솟 값은?

```python
def pm(i, n, s):  # i번 부터 자리 결정, 총 n개짜리, 현재까지 합
    global answer
    if i == n:
        if answer > s:
            answer = s
    elif answer <= s:
        return
    else:
        for j in range(i, n):
            p[i], p[j] = p[j], p[i]
            pm(i+1, n, s + arr[i][p[i]])
            p[i], p[j] = p[j], p[i]

arr = [[73, 21, 21],[11, 59, 40],[24, 31, 83]]
i = 0
n = 3
p = list(range(n))
answer = 1000
pm(i, n, 0)
print(answer)
```



## 조합

> {1, 2, 3, 4, 5, 6, 7, 8, 9, 10} 중 3개를 고르는 조합 출력

```python
# 기본적인 방법
def f(i, j, k):
    print(lst[i], lst[j], lst[k])

n = 10
r = 3
lst = list(range(1, n+1))
for i in range(n-2):
    for j in range(i+1, n-1):
        for k in range(j+1, n):
            f(i, j, k)
```

```python
# 위의 코드 일반화
def nCr(n, r, s):   # n개에서 r개를 고르는 조합. s고를 수 있는 구간의 시작 인덱스
    if r == 0:
        print(answer)
    else:
        for i in range(s, n-r+1):
            answer[r-1] = A[i]
            nCr(n, r-1, i+1)
n = 5
r = 2
answer = [0] * r
A = [1, 2, 3, 4, 5]
nCr(n, r, 0)
```


