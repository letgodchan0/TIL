# ๐ฑ ๋ฐฑํธ๋ํน

> ํด๋ฅผ ์ฐพ๋ ๋์ค์ ๋งํ๋ฉด (ํด๊ฐ ์๋๋ฉด) ๋๋์๊ฐ์ ๋ค์ ํด๋ฅผ ์ฐพ์๊ฐ๋ ๊ธฐ๋ฒ์ผ๋ก ์ต์ ํ (Optimization) ๋ฌธ์ ์ ๊ฒฐ์  (Decision) ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ  ์ ์๋ค.

๊ฒฐ์  ๋ฌธ์  : ๋ฌธ์ ์ ์กฐ๊ฑด์ ๋ง์กฑํ๋ ํด๊ฐ ์กด์ฌํ๋์ง์ ์ฌ๋ถ๋ฅผ 'yes' ๋๋ 'no'๊ฐ ๋ตํ๋ ๋ฌธ์ 

๋ฐฑํธ๋ํน ๊ธฐ๋ฒ

- ์ด๋ค ๋ธ๋์ ์ ๋ง์ฑ์ ์ ๊ฒํ ํ์ ์ ๋งํ์ง ์๋ค๊ณ  ๊ฒฐ์ ๋๋ฉด ๊ทธ ๋ธ๋์ ๋ถ๋ชจ๋ก ๋๋์๊ฐ ๋ค์ ์์ ๋ธ๋๋ก ๊ฐ
- ์ด๋ค ๋ธ๋๋ฅผ ๋ฐฉ๋ฌธํ์์ ๋ ๊ทธ ๋ธ๋๋ฅผ ํฌํจํ ๊ฒฝ๋ก๊ฐ ํด๋ต์ด ๋  ์ ์์ผ๋ฉด ๊ทธ ๋ธ๋๋ ์ ๋งํ์ง ์๋ค๊ณ  ํ๋ฉฐ, ๋ฐ๋๋ก ํด๋ต์ ๊ฐ๋ฅ์ฑ์ด ์์ผ๋ฉด ์ ๋งํ๋ค๊ณ  ํ๋ค.
- ๊ฐ์ง์น๊ธฐ : ์ ๋งํ์ง ์๋ ๋ธ๋๊ฐ ํฌํจ๋๋ ๊ฒฝ๋ก๋ ๋ ์ด์ ๊ณ ๋ คํ์ง ์๋๋ค.

## ๋ถ๋ถ์งํฉ ๊ตฌํ๊ธฐ

> {1, 2, 3}์ ๋ถ๋ถ์งํฉ ํํ - 0000์ผ๋ก ํ ๊บผ์ (์์0์ ๊ทธ๋ฅ ํ์ ๊ฐ์ถ๋ ค๊ณ  ์์)

![image-20220227164927366](Stack%202.assets/image-20220227164927366.png)



```python
def f(i, N):  # i: ๋ถ๋ถ์งํฉ์ ํฌํจ๋ ์ง ๊ฒฐ์ ํ  ์์์ ์์ ์ธ๋ฑ์ค, N: ์ ์ฒด ์์ ๊ฐ์
    if i == N:   # ํ ๊ฐ์ ๋ถ๋ถ์งํฉ ์์ฑ๋ ๊ฒฝ์ฐ
    	print(bit, end = ' ')
        for j in range(N):
            if bit[j] == 1: # bit[j] == 1 ํด๋น ์์๊ฐ ๋ถ๋ถ์งํฉ์ ํฌํจ๋๋ค.
                print(a[j], end = ' ')
        print()
    # bit๋ฅผ ๋ชจ๋ ์ฑ์ธ ๋๊น์ง ๋ ๊ฐ๋๋ก ๋๋ ์ง๋ ๋ถ๋ถ
    else:
        bit[i] = 1
        f(i+1, N)
        bit[i] = 0
        f(i+1, N)
    return # ์๋ฏธ์ ์๋ ๋ถ๋ถ
    
a = [1, 2, 3]
# bit์ ๋ชจ๋  ๋ถ๋ถ์งํฉ์ ์ฐพ์ ๋ ์ด๋ฐ์์ผ๋ก ํํ, ์ฒ์ ๋นํธ๋ฅผ ๋ฌด์กฐ๊ฑด ํฌํจ ํ ๊บผ๋ฉด bit[0] = 1
bit = [0, 0, 0] 
f(0, 3)   # i 0๋ฒ ๋ถํฐ ๊ฒฐ์ ํด๋ด

# ์ด๋ ๊ฒ ํ๋ฉด ๋ชจ๋  ๋ถ๋ถ ์งํฉ ์์ฑ
[1, 1, 1] 1 2 3 
[1, 1, 0] 1 2 
[1, 0, 1] 1 3 
[1, 0, 0] 1 
[0, 1, 1] 2 3 
[0, 1, 0] 2 
[0, 0, 1] 3 
[0, 0, 0] 

```

### ์ฐ์ต๋ฌธ์ 

>{1, 2, 3, 4, 5, 6, 7, 8, 9, 10} ์ ๋ถ๋ถ์งํฉ ๋ง๋ค๊ธฐ

```python
def make(lst, n):	# lst: ๋ฆฌ์คํธ, n: ๋ถ๋ถ์งํฉ์ ํฌํจ๋ ์ง ๊ฒฐ์ ํ  ์์์ ์์ ์ธ๋ฑ์ค
    if n == len(lst): # ํ๋์ ๋ถ๋ถ์งํฉ์ด ์์ฑ๋ ๊ณณ
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

๊ธฐ๋ณธ์ ์ผ๋ก ๋นํธ(?)๋ฅผ ์ด์ฉํด์ ๋ถ๋ถ์งํฉ์ ๋ง๋๋ ๋ฐฉ๋ฒ์ด๋ค. 

1. ์ฌ๊ธฐ์๋ `check`๋ผ๋ 0์ผ๋ก ์ฑ์์ ธ ์๊ณ  ๊ธธ์ด๊ฐ `lst`์ ๊ฐ์ ๋ฆฌ์คํธ๋ฅผ ํ๋ ๋ง๋ค์ด ์ค๋ค.   

2. ```python
   lst   =  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
   check =  [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]  = ๊ณต์งํฉ
   check =  [1, 0, 0, 0, 0, 0, 0, 0, 0, 0]  = [1]
   check =  [1, 0, 0, 0, 0, 1, 0, 0, 0, 0]  = [1, 6]
   check =  [1, 0, 0, 1, 0, 0, 0, 0, 0, 1]  = [1, 4, 10]
   ```

   - `check`์ ํน์  ์ธ๋ฑ์ค๊ฐ `1`์ด๋ผ๋ฉด `lst`์ ์ธ๋ฑ์ค๋ก ๋งคํ๋๋ ๊ฐ์ ๋ฒํธ๊ฐ ๋ถ๋ถ์งํฉ์ผ๋ก ์ฌ์ฉ๋์๋ค๋ ๊ฒ์ ์๋ฏธํ๋ค.

#### ์ฐ์ต๋ฌธ์ 

> {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}์ powerset ์ค ํฉ์ด 10์ธ ๋ถ๋ถ์งํฉ์ ๊ตฌํ์์ค
>
> ๋ชจ๋  ๋ถ๋ถ์งํฉ์ ๋ง๋ค ๋์ ๋ฐฑํธ๋ํน ๊ธฐ๋ฒ์ผ๋ก ํ์ ๋ ์ฐจ์ด ๋น๊ต

**<u>๋ชจ๋  ๋ถ๋ถ์งํฉ์ ๋ง๋ค๊ณ  ํฉ์ด 10์ธ ๋ถ๋ถ์งํฉ๋ง ๋ฐ๋ก ์ถ๋ ฅ</u>**

```python
def f(i, N, k):  # i: ๋ถ๋ถ์งํฉ์ ํฌํจ๋ ์ง ๊ฒฐ์ ํ  ์์์ ์ธ๋ฑ์ค, N: ์ ์ฒด ์์ ๊ฐ์, k: ์ฐพ๋ ํฉ
    global cnt
    cnt += 1
    if i == N:   # ํ๊ฐ์ ๋ถ๋ถ์งํฉ ์์ฑ
        s = 0 # ๋ถ๋ถ์งํฉ ์์์ ํฉ
        for j in range(N):
            if bit[j] == 1:   # 1์ด๋ผ๋ ๊ฒ์ j ๋ฅผ ์ธ๋ฑ์ค๋ก ํ๋ ๊ฐ์ด ๋ถ๋ถ์งํฉ์ผ๋ก ํฌํจ๋๋ค๋ ์๋ฏธ
                s += a[j]
        if s == k:  # ์ฐพ๋ ํฉ์ด๋ฉด
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
cnt = 0  # ์ฌ๊ท ์ฒดํฌ
k = 10
f(0, n, k)   # 0๋ฒ ๋ถํฐ ๋ถ๋ถ์งํฉ์ ํฉ์ด 10์ธ ์  ์ฐพ์๋ด

cnt => 2047
```

**<u>๋ฐฑํธ๋ํน(๊ฐ์ง์น๊ธฐ)์ผ๋ก ํฉ์ด 10๋  ๊ฒ ๊ฐ์ ๋ถ๋ถ์งํฉ๋ง ์์ฑ</u>**

```python
def f (i, n, k, s):  # s : i-1 ์์๊น์ง ๊ณ ๋ คํ ํฉ
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
    # ๋ค ๋ด๋ ค๊ฐ์ง ์์๋๋ฐ ํฉ์ด k๋ณด๋ค ํฌ๋ค๋ฉด ๋ ์๋ด๋ ค๊ฐ๊ณ  ๋ฉ์ถค
    elif s > k:
        return
    else:
        bit[i] = 1
        f(i+1, n, k, s+a[i])    # a[i] ํฌํจํ ๊ฒฝ์ฐ์ฌ์ s์ a[i]๋ฅผ ๋ํด์ค๋ค!
        bit[i] = 0
        f(i+1, n, k, s)  # ํฌํจ ์์ํค๋๊น s๋ฅผ ๊ทธ๋๋ก!

n = 10
a = [x for x in range(1, n+1)]
bit = [0] * n
k = 10
cnt = 0
f(0, n, k, 0)   # ์ ์ผ ์ค๋ฅธ์ชฝ 0์ ํ์ฌ๊น์ง ์์ฑ๋ ๋ถ๋ถ์งํฉ์ ํฉ

cnt => 415
```

**<u>๋ฐฑํธ๋ํน(๊ฐ์ง์น๊ธฐ) ์กฐ๊ธ ๋ ์ ๊ตํ๊ฒ</u>**

```python
# ๋ชจ๋  ์์ ๊ณ ๋ คx , s == k๋ผ๋ฉด ๋ค์ ์๋ ์์ ๊ณ ๋ คํ  ํ์ x
def f (i, n, k, s):  # s : i-1 ์์๊น์ง ๊ณ ๋ คํ ํฉ
    global cnt
    cnt += 1
    if s == k: # ๋ชฉํ๊ฐ ์ฐพ์์ ๋
        for j in range(n):
            if bit[j]:
                print(a[j], end= ' ')
        print()
    elif i == n:  # ํ ๊ฐ์ ๋ถ๋ถ์งํฉ์ด ์์ฑ๋ ๊ฒฝ์ฐ
        return  # ๋๋์๊ฐ์ ๋ค๋ฅธ ๋ธ๋ ์ฐพ์๋ด
    elif s > k:   # ํฉ์ด k๋ฅผ ๋์ผ๋ฉด ๋ ๋ฐ์ผ๋ก ์๋ด๋ ค๊ฐ
        return
    else:
        bit[i] = 1
        f(i+1, n, k, s+a[i])    # a[i] ํฌํจํ ๊ฒฝ์ฐ์ฌ์ s์ a[i]๋ฅผ ๋ํด์ค๋ค!
        bit[i] = 0
        f(i+1, n, k, s)  # ํฌํจ ์์ํค๋๊น s๋ฅผ ๊ทธ๋๋ก!
    return

n = 10
a = [x for x in range(1, n+1)]
bit = [0] * n
k = 10
cnt = 0
f(0, n, k, 0)   # ์ ์ผ ์ค๋ฅธ์ชฝ 0์ ํ์ฌ๊น์ง ์์ฑ๋ ๋ถ๋ถ์งํฉ์ ํฉ

cnt => 349

```

**<u>์ ๋ฐฉ์๊ณผ ๋์ผํ์ง๋ง ์ฝ๋ ๋ณํ</u>**

```python
def f1(i, n, k, s): # n: bit์ ํฌ๊ธฐ, k: ์กฐ๊ฑด์ ํฉ, s: i-1 ์์ ๊น์ง์ ํฉ
    global cnt
    if i == n:
        a = 0 # ํ์ฌ ๋ถ๋ถ์งํฉ ์์์ ํฉ
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
f(0, n, k, 0)   # ์ ์ผ ์ค๋ฅธ์ชฝ 0์ ํ์ฌ๊น์ง ์์ฑ๋ ๋ถ๋ถ์งํฉ์ ํฉ
```



**<u>rs(๋จ์ ๊ตฌ๊ฐ์ ํฉ) + s(ํ์ฌ๊น์ง ํฉ)์ ๋ํด๋ ์ฐพ๋ ๊ฐ๋ณด๋ค ์๋ค๋ฉด ์ค๋จ</u>**

![image-20220227182453450](Stack%202.assets/image-20220227182453450.png)

```python
def f(i, n, s, t, rs): # s:์ด์ ๊น์ง ๊ณ ๋ ค๋ ์์์ ํฉ, t: ๋ชฉํ๊ฐ
    global cnt
    cnt += 1
    if s == t: #  ๋ชฉํ๊ฐ์ ์ฐพ์ผ๋ฉด
        for j in range(n):
            if bit[j]  == 1:
                print(a[j], end = ' ')
        print()
    elif i == n: # ๋์ด์ ๊ณ ๋ คํ  ์์๊ฐ ์์ผ๋ฉด
        return # ๋๋์๊ฐ์ ์์ ๋ค๋ฅธ ๋ธ๋๋ฅผ ์ ํํด๋ด
    elif s > t: # ๋ชฉํ๋ฅผ ๋์ด์๋ฉด ๋ฐ์ ๊ทธ๋ง ์ฐพ๊ณ  ๋ค๋ฅธ๊ฑฐ ์ฐพ์๋ด
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
t = 10 # ํฉ์ด 10์ธ๊ฒฝ์ฐ๊ฐ ์๋๊ฐ?
f(0, n, 0, t, sum(a))
print(cnt)

cnt => 349
```

## ์์ด

> [1, 2, 3]์ ๋ชจ๋  ์์๋ฅผ ์ฌ์ฉํ ์์ด
>
> 123, 132, 213, 231, 312, 321์ ์ด 6๊ฐ์ง ๊ฒฝ์ฐ

![image-20220331152830110](Stack%202.assets/image-20220331152830110-16487234026311.png)

#### 1. ์๋ฆฌ ๋ฐ๊พธ๋ ๋ฐฉ๋ฒ

```python
def f(i, n):
    if i == n:
        print(p)
    else:
        for j in range(i, n):
            p[i], p[j] = p[j], p[i]  # ๋ค๋ฅธ๊ฑฐ ๋ฃ์ด๋ด
            f(i+1, n)
            p[i], p[j] = p[j], p[i]   # ์์ ๋ณต๊ท
    # ๊ทธ๋ฅ ํ์ ํด ๋ณด์ฌ์ return ์์ด๋ ์ ๋์๊ฐ
    return

n = 3
p = [x for x in range(1, n+1)]
f(0, n)  # 0๋ฒ ์๋ฆฌ๋ถํฐ ๊ฒฐ์ , ์ด n๊ฐ์ง๋ฆฌ
```

#### 2. ์ฒดํฌํ๋ ๋ฐฉ๋ฒ

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

#### 3. ์์ด - ๋ฐฑํธ๋ํน

> 73 21 21
> 11 59 40
> 24 31 83
>
> ํ, ์ด๋ค์ ํฉ ์ค ์ต์ ๊ฐ์?

```python
def pm(i, n, s):  # i๋ฒ ๋ถํฐ ์๋ฆฌ ๊ฒฐ์ , ์ด n๊ฐ์ง๋ฆฌ, ํ์ฌ๊น์ง ํฉ
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



## ์กฐํฉ

> {1, 2, 3, 4, 5, 6, 7, 8, 9, 10} ์ค 3๊ฐ๋ฅผ ๊ณ ๋ฅด๋ ์กฐํฉ ์ถ๋ ฅ

```python
# ๊ธฐ๋ณธ์ ์ธ ๋ฐฉ๋ฒ
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
# ์์ ์ฝ๋ ์ผ๋ฐํ
def nCr(n, r, s):   # n๊ฐ์์ r๊ฐ๋ฅผ ๊ณ ๋ฅด๋ ์กฐํฉ. s๊ณ ๋ฅผ ์ ์๋ ๊ตฌ๊ฐ์ ์์ ์ธ๋ฑ์ค
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

