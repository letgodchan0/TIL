# 🌱 스택 1

## 스택(stack)의 특성

- 물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료구조이다.
- 스택에 저장된 자료는 선형 구조를 갖는다.
  - 선형 구조: 자료 간의 관계가 1대1의 관계를 갖는다.
  - 비선형 구조: 자료 간의 관계가 1대N의 관계를 갖는다. ex) 트리
- 스택에 자료를 삽입하거나 스택에서 자료를 꺼낼 수 있다.
- <span style="color:indianred">마지막에 삽입한 자료를 가장 먼저 껀내다. 후입선출(LIFO, Last-In-First-Out)이라고 부른다.</span>
  - 예를 들어, 스택에 1, 2, 3 순으로 자료를 삽입한 후 꺼내면 역순으로(3, 2, 1) 꺼낼 수 있다.

## 스택의 구현

> 스택을 프로그램에서 구현하기 위해서 필요한 자료구조와 연산

### 자료구조: 자료를 선형으로 저장할 저장소

- 배열을 사용할 수 있다.
- 저장소 자체를 스택이라 부르기도 한다.
- 스택에서 마지막 삽입된 원소의 위치를 `top`이라 부른다.

### 연산

- 삽입 : 저장소에 자료를 저장한다. 보통 push라고 부른다.
- 삭제: 저장소에서 자료를 꺼낸다. 꺼낸 자료는 삽입한 자료의 역순으로 꺼내고 보통 pop이라고 부른다.
- 스택이 공백인지 아닌지를 확인하는 연산 : isEmpty
- 스택의 top에 있는 item(원소)을 반환하는 연산 : peek

### 스택의 삽입/삭제 과정

> 빈 스택에 원소 A, B, C를 차례로 삽입 후 한번 삭제하는 연산과정

![image-20220221134805717](Stack%201.assets/image-20220221134805717.png)

### 스택의 구현

```python
# append, pop 메소드를 통해 리스트의 마지막에 데이터를 삽입
def push(item):
    s.append(item)
    
def pop():
    if len(s) == 0:
        # underflow 스택이 비어 있으므로 none 반환
        return None
    else:
        return s.pop(-1)
```

> but, 리스트 메서드를 통한 연산과정은 시간이 오래 걸리기 때문에 다른 방법을 통해 구현 가능하다.



```python
def push(item, size):
    global top
    top += 1
    if top == size:
        print('overflow!')
    else:
        stack[top] = item

size = 10
stack = [0]*size # 스택을 size에 맞춰 만들어 둔다.
top = -1 # 처음에는 스택에 아무것도 쌓여있지 않기 때문에 top을 -1로 준다.

push(10, size)
top += 1        # push(20)
stack[top] = 20 #


def pop():
    global top
    if top == -1:
        print('underflow')
        return 0
    else:
        top -= 1
        return stack[top+1]
print(pop())

if top > -1:   # pop()
    top -= 1
    print(stack[top+1])
```

1차원 배열을 사용하여 구현할 경우 구현이 용이하지만, 스택의 크기를 변경하기 어려운 단점이 있다. 이를 해결하기 위한 방법으로 저장소를 동적으로 할당하여 스택을 구현하는 방법이 있다. <span style="color:indianred">동적 연결리스트</span>를 이용하여 구현하는 방법을 의미한다. 구현이 복잡한 단점이 있지만, 메모리를 효율적으로 사용한다는 장점을 가진다.

### 스택 응용1 : 괄호 검사

> 스택을 이용하여 괄호 검사하기

```python
T = int(input())
for tc in range(1, T+1):
    s = input()

    stack = [0]*1000
    top = -1
    ans = 1
    for x in s:
        if x == '(':
            top += 1 # push
            stack[top] = x
        elif x == ')':
            if top == -1: # 닫는 괄호가 남은 경우
                ans = 0
                break
            else:
                top -= 1
    else:
        if top != -1: # 여는 괄호가 남은 경우
            ans = 0
    print(ans)
```



```python
def check(string):
    stack = []
    for i in range(len(string)):
        print(stack)
        if string[i] == '(':
            stack.append(string[i])
        elif string[i] ==')':
            if stack:
                if stack[-1] == '(':
                    stack.pop(-1)
                else:
                    return -1
            else:
                return -1
    # 스택이 비어 있어야 짝이 맞는 괄호
    if not stack:
        return 1
    else: # 스택에 '('가 남아있음
        return -1

```

### 스택의 응용2 : function call

- 프로그램에서의 함수 호출과 복귀에 따른 수행 순서를 관리
  - 가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 후입선출 구조이므로, 후입선출 구조의 스택을 이용하여 수행순서 관리
  - 함수 호출이 발생하면 호출한 함수 수행에 필요한 지역변수, 매개변수 및 수행 후 복귀할 주소 등의 정보를  스택 프레임에 저장하여 시스템 스택에 삽입
  - 함수의 실행이 끝나면 시스템 스택의 top 원소(스택 프레임)를 삭제(pop) 하면서 프레임에 저장되어 있던 복귀주소를 확인하고 복귀
  - 함수 호출과 복귀에 따라 이 과정을 반복하여 전체 프로그램 수행이 종료되면 시스템 스택은 공백 스택이 된다.
  - ![image-20220221211236192](Stack%201.assets/image-20220221211236192.png)



## 재귀 호출

> 자기 자신을 호출하여 순환 수행되는 것으로 함수에서 실행해야 하는 작업의 특성에 따라 일만적인 호출방식보다 재귀호출방식을 사용하여 함수를 만들면 프로그램의 크기를 줄이고 간단하게 작성

```python
# 재귀함수 바이블 factorial
def factorial(n):
    if n == 1:      # n이 1일 때
        return 1    # 1을 반환하고 재귀호출을 끝냄
    return n * factorial(n - 1)    # n과 factorial 함수에 n - 1을 넣어서 반환된 값을 곱함

# 피보나치 수를 구하는 재귀함수
def fibo(n):
    if n < 2:
        return n
    else:
        return fibo(n-1) + fibo(n-2)
```



## Memoization

> 메모이제이션(memoization)은 컴퓨터 프로그램을 실행할 때 이전에 계산한 값을 메모리에 저장해서 매번 다시 계산하지 않도록 하여 전체적인 실행속도를 빠르게 하는 기술로 동적 계획법의 핵심이 되는 기술이다.

일반적인 재귀함수를 통해 피보나치 수를 구하면 중복 호출이 발생한다.

![image-20220221211845723](Stack%201.assets/image-20220221211845723.png)

따라서 피보나치 수를 구하는 알고리즘에서 `fibo(n)`의 값을 계산하자마자 저장하면(memoize), 실행시간을 O(n)으로 줄일 수 있다.

```python
# memo를 위한 배열을 할당하고, 모두 0으로 초기화
# memo[0]을 0으로 memmo[1]는 1로 초기화 한다.

def fibo(n):
    global memo
    if n >= 2 and len(memo) <= n:
        memo.append(fibo(n-1) + fibo(n-2))
    return memo[n]
memo = [0, 1]
```

### DP (dynamic programming)

> 동적 계획 알고리즘은 그리디 알고리즘과 같이 최적화 문제를 해결하는 알고리즘으로, (1) 입력 크기가 작은 부분 문제들을 모두 해결한 후에 (2) 그 해들을 이용하여 보다 큰 크기의 부분 문제들을 해결하고, (3) 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘이다. 

#### DP로 피보나치수 찾기

1. 문제를 부분 문제로 분할한다.

   - fibo(n) 함수는 fibo(n-1) + fiob(n+1)
   - 즉, fibo(n) 은 fibo(n-1),  fibo(n-2), ............................., fibo(2), fibo(1), fibo(0)의 부분집합으로 나뉜다.

2. 부분 문제로 분할을 하면 가장 작은 부분의 문제부터 해를 구하여 테이블에 저장하고, 테이블에 저장된 부분 문제의 해를 이용하여 상위 문제의 해를 구한다.

   - | 테이블 인덱스 | 저장되어 있는 값 |
     | ------------- | ---------------- |
     | [0]           | 0                |
     | [1]           | 1                |
     | [2]           | 1                |
     | [3]           | 2                |
     | [4]           | 3                |
     | ...           | ...              |
     | [n]           | fibo(n)          |

```python
# 피보나치 수 DP 적용 알고리즘

def fibo(n):
    f = [0, 1]
    
    for i in range(2, n + 1):
        f.append(f[i-1] + f[i-2])
        
    return f[n]

# memoization을 재귀적 구조에 사용하는 것보다 반복적 구조로 DP를 구현하는 것이 성능 면에서 보다 효율적
# 재귀적 구조는 내부에 시스템 호출 스택을 사용하는 오버헤드가 발생하기 때문
```

