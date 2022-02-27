# 🌱 큐

```python
front = -1
rear = -1
Q = [0] * 10
rear += 1 # enqueue(1)
Q[rear] = 1

front += 1
Q[front] = 0
```







## BFS (Breadth First Search)

> 너비우선탐색은 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후에 방문했던 정점을 시작점으로 하여 다시 인첩한 정점들을 차례로 방문하는 방식, 인접한 정점들에 대해 탐색을 한 후, 차례로 다시 너비우선탐색을 진행해야 하므로, 선입선출 형태의 자료구조인 큐를 활용한다.

- 다음과 같은 순서로 탐색한다! (A - B - C - D - E - F - G -H - I)

![image-20220225102127400](C:/Users/livem/AppData/Roaming/Typora/typora-user-images/image-20220225102127400.png)

```python
# 그래프 G, 탐색 시작점 v
def bfs(G, v):
    visited = [0] * (n+1)
    queue = []
    queue.append(v)
    while queue:
        t = queue.pop(0)
        if not visited[t]:
            visited[t] = True
            visit(t)
        for i in G[t]:
            if not visited[i]:
                queue.append(i)
```



큐의 중복된 정보가 들어가 경우에 큐를 얼마나 크게 만들어야 할까?

=> visited를 처리된 애를 체크했다면, 줄서고 있는애들로 바꾸면 된다!

- visited 배열 초기화

- Q 생성

- 시적점 enqueue, 시작점 줄섬 표시

  위 과정은 while문 전에

  ```python
  # 그래프 G, 탐색 시작점 v
  def bfs(G, v, n):
      visited = [0] * (n+1)
      queue = []
      queue.append(v)
      visited[0] = 1  # 시작점 줄섬 표시
      while queue:
          t = queue.pop(0)
          visit(t) # print(t) 하면 방문한 순서 찍힘, visit(t) 부분에서 어떤 목적 코드  실행
          for i in G[t]:
              if not visited[i]:
                  # 그냥 1해줘도 갠츈, 
                  visited[i] = visited[t] + 1
                  queue.append(i)
  ```

  

|         | A    | B    | C    | D    | E    | F    | G    | H    | I    |
| ------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Visited | [0]  | [1]  | [2]  | [3]  | [4]  | [5]  | [6]  | [7]  | [8]  |
| Queue   | 1    | 2    | 2    | 2    | 3    | 3    | 3    | 3    | 3    |
|         | A    | B    | C    | D    | E    | F    | G    | H    | I    |

