# 🌱 스택 2

## 계산기

**문자열로 된 계산식이 주어질 때 스택을 이용하여 이 계산식의 값을 계산할 수 있다.**

<u>step 1. 중위 표기법의 수식을 후위 표기법으로 변경한다 (스택 이용)</u>

	- 수식의 각 연산자에 대해서 우선 순위에 따라 괄호를 사용하여 다시 표현한다.
	- 각 연산자를 그에 대응하는 오른쪽 괄호의 뒤로 이동시킨다.
	- 괄호를 제거한다.
	
	ex) A*B-C/D - > AB*CD/-

> 중위 표기법에서 후위 표기법으로의 변환 알고리즘 (스택 이용)
>

```python
string = '(6+5*(2-8)/2)'
N = len(string)
isp = {'+': 1, '-' : 1, '*': 2, '/': 2, '(' : 0} # 스택 push 전 우선순위
icp = {'+': 1, '-' : 1, '*': 2, '/': 2, '(' : 3} # 스택 내부 우선순위
stack = [0] * N # 연산자를 담는 곳
top = -1
postfix = '' # 최종 후위표기식
for i in range(N):
    if '0' <= string[i] <= '9': # 피연산자면 postfix에 붙여주기
        postfix += string[i]
    # 연산자라면, 스택 내부
    elif string[i] == '+' or string[i] == '*' or string[i] == '-' or string[i] =='/':
        # 토큰이 스택의 top 우선순위보다 높을 때까지 스택에서 pop해서 postfix에 붙여준다. 
        while top > -1 and icp[string[i]] <= isp[stack[top]]:
            postfix += stack[top]
            stack[top] = 0
            top -= 1
        # 스택이 비어 있거나 토큰이 스택의 top보다 우선순위가 높다면 그대로 스택에 push
        top += 1
        stack[top] = string[i]
    elif string[i] == '(':
        top += 1
        stack[top] = string[i]
    else:
        # 왼쪽 괄호 나올 때까지 stack을 pop해서 postfix에 푸쉬하기
        while stack[top] != '(':
            postfix += stack[top]
            stack[top] = 0
            top -= 1
        # 왼쪽 괄호 만나면 pop 하기
        if stack[top] == '(':
            stack[top] = 0
            top -= 1
# 스택에 남아있는 연산자 모두 pop하여 postfix에 푸쉬하기
while top > -1:
    postfix += stack[top]
    top -= 1
    
postfix => '6528-*2/+'
```

1. 입력 받은 중위 표기식에서 토큰을 읽는다. *토큰: 문자열에서 최소 단위
2. 토큰이 피연산자이면 `postfix (최종으로 후위표기된 식)`에 붙여준다. 
3. 토큰이 연산자일 때 이 토큰이 스택의 top에 저장되어 있는 연산자보다 우선순위가 높으면 스택에 push하고 그렇지 않다면 top의 연산자 우선순위가 토큰의 우선순위보다 작을 때 까지 스택에서 pop한 후 토큰의 연산자를 push 한다. 만약 top에 연산자가 없으면 push 한다.

4. 토큰이 오른쪽 괄호 ')' 이면 스택 top에 왼쪽 괄호 '(' 가 올 때까지 스택에 pop 연산을 수행하고 pop한 연산자를 출력한다.  왼쪽 괄호를 만나면 pop만 하고 출력하지는 않는다.
5. 중위 표기식에 더 읽을 것이 없다면 중지하고, 더 읽을 것이 있다면 1부터 다시 반복한다.
6. 스택에 남아 있는 연산자를 모두 pop하여 출력한다.



<u>step 2. 후위 표기법의 수식을 스택을 이용하여 계산한다.</u>

```python
numbers = []
for x in range(len(postfix)):
    if postfix[x].isdigit():
        numbers.append(postfix[x])
    elif postfix[x] == '*':
        numbers.append(int(numbers.pop() * int(numbers.pop())))
    elif postfix[x] == '+':
        numbers.append(int(numbers.pop() + int(numbers.pop())))
    elif postfix[x] == '-':
        second = int(numbers.pop())
        first = int(numbers.pop())
        numbers.append(first - second)
    elif postfix[x] == '/':
        second = int(numbers.pop())
        first = int(numbers.pop())
        numbers.append(first / second)

numbers[0] => -9 # 최종 결과
```



## 백트래킹

> 해를 찾는 도중에 막히면 (해가 아니면) 되돌아가서 다시 해를 찾아가는 기법으로 최적화 (Optimization) 문제와 결정 (Decision) 문제를 해결할 수 있다.

결정 문제 : 문제의 조건을 만족하는 해가 존재하는지의 여부를 'yes' 또는 'no'가 답하는 문제

백트래킹 기법

- 어떤 노드의 유망성을 점검한 후에 유망하지 않다고 결정되면 그 노드의 부모로 되돌아가 다음 자식 노드로 감
- 어떤 노드를 방문하였을 때 그 노드를 포함한 경로가 해답이 될 수 없으면 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 한다.
- 가지치기 : 유망하지 않는 노드가 포함되는 경로는 더 이상 고려하지 않는다.



### 부분집합 구하기

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



### 순열

> [1, 2, 3]의 모든 원소를 사용한 순열
>
> 123, 132, 213, 231, 312, 321의 총 6가지 경우



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







## 분할 정복

- 분할 : 해결할 문제를 여러 개의 작은 부분으로 나눈다.
- 정복 : 나눈 작은 문제를 각각 해결한다.
- 통합 : (필요하다면) 해결된 해답을 모은다.

### 분할정복 예제 - 거듭 제곱

**Cn = C X C X C ... X C**

```python
def power(base, exponent):
    if base == 0:
        return 1
    result = 1
    for i in range(exponent):
        result *= base
    return result
```

![image-20220227183926841](Stack%202.assets/image-20220227183926841.png)

```python
def power(base, exponent):
    if exponent == 0 or base == 0:
        return 1
    
    if exponent % 2 == 0:
        newbase = power(base, exponent/2)
        return newbase * newbase
    else:
        newbase = power(base, (exponent-1)/2)
        return (newbase * newbase) * base
```


