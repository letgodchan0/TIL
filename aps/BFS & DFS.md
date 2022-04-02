# 🌱 그래프 탐색

- 그래프는 아이템(사물 또는 추상적 개념)들과 이들 사이의 연결 관계를 표현한다.
- 정점(Vertex)들의 집합과 이들을 연결하는 간선(Edge)들의 집합으로 구성된 자료 구조
  - V - 정점의 개수
  - E - 그래프에 포함된 간선의 개수
  - V개의 정점을 가지는 그래프는 최대 `V * (V+1) / 2` 개의 간선을 가질 수 있음
- 선형 자료구조나 트리 자료구조로 표현하기 어려운 N : N 관계를 가지는 원소들을 표현하기에 용이



## 인접 행렬

> 행 번호와 열 번호를 그래프의 정점에 대응 하여 두 정점이 인접되어 있으면 1, 그렇지 않으면 0으로 표현

```python
'''
7 (정점의 개수), 8 (간선의 개수)
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''
v, e = map(int, input().split())
arr = list(map(int, input().split()))
# 인접 행렬
adj = [[0] * (v+1) for _ in range(v+1)]

for i in range(e):
    n1, n2 = arr[i*2], arr[i*2+1]
    adj[n1][n2] = 1	 # n1과 n2는 서로 인접
    adj[n2][n1] = 1  # 무방향 그래프일 경우

>>> adj
[[0, 0, 0, 0, 0, 0, 0, 0], # 인접한 친구들
 [0, 0, 1, 1, 0, 0, 0, 0], # (1, 2) (1, 3)
 [0, 1, 0, 0, 1, 1, 0, 0], # (2, 1), (2, 4), (4, 2)
 [0, 1, 0, 0, 0, 0, 0, 1], # (3, 1), (3, 7)
 [0, 0, 1, 0, 0, 0, 1, 0], # (4, 2), (4, 6)
 [0, 0, 1, 0, 0, 0, 1, 0], # (5, 2), (5, 6)
 [0, 0, 0, 0, 1, 1, 0, 1], # (6, 4), (6, 5), (6, 7)
 [0, 0, 0, 1, 0, 0, 1, 0]] # (7, 3), (7, 6)
```

## 인접 리스트

> 각 정점에 대한 인접 정점들을 순차적으로 표현, 인접리스트의 인덱스(정점)와 요소(인접한 정점)를 이용

```python
'''
7 (정점의 개수), 8 (간선의 개수)
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''
v, e = map(int, input().split())
arr = list(map(int, input().split()))
# 인접 리스트
adjList = [[] for _ in range(v+1)]

for i in range(e):
    n1, n2 = arr[i*2], arr[i*2+1]  
    adjList[n1].append(n2)  # n1과 n2는 서로 인접
    adjList[n2].append(n1)  # 무방향 그래프일 경우
```



## DFS(깊이 우선 탐색)

- 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회 방법
- 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로 후입선출 구조의 스택 사용

### 재귀

```python
'''
7 8
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''
def dfs(v, V):   # 인접 행렬의 경우
    visited[v] = 1
    print(v, end = ' ')
    for w in range(1, V+1):
        if adjM[v][w]==1 and visited[w]==0:
            dfs(w, V)
            
V, E = map(int, input().split())
arr = list(map(int, input().split()))
adjM = [[0]*(V+1) for _ in range(V+1)]


for i in range(E):
    n1, n2 = arr[i*2], arr[i*2+1]
    adjM[n1][n2] = 1
    adjM[n2][n1] = 1
```

### Stack

```python
'''
7 8
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''
def dfs(v, V):    # 인접 리스트인 경우
    stack = [v]             # 스택생성 + 시작정점 push
    visited = [0]*(V+1)
    visited[v] = 1          # push됨 표시
    while stack:
        v = stack.pop()
        print(v)            # visit()
        for w in adjL[v]:   # v에 인접한 정점 w
            if visited[w]==0: # 인접하고 미방문 w
                stack.append(w)         # 갈림길 목록
                visited[w] = 1
                
V, E = map(int, input().split())
arr = list(map(int, input().split()))
adjL = [[] for _ in range(V+1)]

for i in range(E):
    n1, n2 = arr[i*2], arr[i*2+1]  
    adjL[n1].append(n2)
    adjL[n2].append(n1)
```



## BFS(너비 우선 탐색)

- 너비 우선 탐색은 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후에, 방문했던 정점을 시작점으로 하여 다시 인접한 정점들을 차례로 방문하는 방식
- 인접한 정점들에 대해 탐색을 한 후, 차례로 다시 너비 우선 탐색을 진행해야 하므로, 선입선출 형태의 자료구조인 큐를 활용한다.

### 인접 행렬 사용

```python
'''
7 8
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''

def bfs(s, V):
    q = []                  # 큐샐성
    visited = [0]*(V+1)     # vistied생성
    q.append(s)             # 시작점 인큐
    visited[s] = 1          # 시작점 인큐표시
    while q:
        v = q.pop(0)        # 디큐
        print(v, end = ' ')
        for w in range(1, V+1):
            if adjM[v][w]==1 and visited[w]==0:
                q.append(w)
                visited[w] = visited[v] + 1
    return

V, E = map(int, input().split())
arr = list(map(int, input().split()))
adjM = [[0]*(V+1) for _ in range(V+1)]

for i in range(E):
    n1, n2 = arr[i*2], arr[i*2+1]
    adjM[n1][n2] = 1
    adjM[n2][n1] = 1
    
bfs(1, V)
```

### 인접 리스트 사용

```PYTHON
'''
7 8
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
'''
def bfs(s, V):
    q = []                  # 큐샐성
    visited = [0]*(V+1)     # vistied생성
    q.append(s)             # 시작점 인큐
    visited[s] = 1          # 시작점 인큐표시
    while q:
        v = q.pop(0)        # 디큐
        print(v, end = ' ')
        for w in adjL[v]: # 인접리스트
            if visited[w]==0:
                q.append(w)
                visited[w] = visited[v] + 1

V, E = map(int, input().split())
arr = list(map(int, input().split()))
adjL = [[] for _ in range(V+1)]

for i in range(E):
    n1, n2 = arr[i*2], arr[i*2+1]
    adjL[n1].append(n2)
    adjL[n2].append(n1)

bfs(1, V)
```

