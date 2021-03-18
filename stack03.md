### 선형 구조와 비선형 구조

- 선형 구조
    - 표현 : 배열에 값을 집어넣는 방식
    - 순회 : for문 이용
- 비선형 구조
    - 표현 : 메모리 저장
    - 순회 :
        - 탐색의 두 가지 방법:
            - 깊이 우선 탐색(Depth First Search, DFS)
            - 너비 우선  탐색(Breadth First Search, BFS)

# 깊이우선탐색 DFS

- 비선형구조인 그래프 구조는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 것이 중요하다.
- 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회 방법.
- 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로, 후입선출 구조의 스택을 사용한다.

## 구현

1. 시작 정점 v를 결정하여 방문한다.
2. 정점 v에 인접한 정점 중에서
    1. 방문하지 않은 정점 w가 있으면, 정점 v를 스택에 push하고 정점 w를 방문한다. 그리고 w를 v로 하여 다시 2.를 반복한다.
    2. 방문하지 않은 정점이 없으면, 탐색의 방향을 바꾸기 위해서 스택을 pop하여 받은 가장 마지막 방문 정점을 v로 하여 다시 2.를 반복한다.
3. 스택이 공백이 될 때까지 2.를 반복한다.

### 💡 STACK(반복) VS 재귀?

- DFS에는 stack을 만들어 구현하는 방법과 재귀로 구현하는 방법 두 가지가 있다.
    - stack을 이용하는 것이 좀더 빠르다.
    - 그러나 재귀를 이용하는 것이 코드가 더 간단해진다.

### 재귀를 이용한 DFS 구현

```python
DFS Recursive(G, v)
# G는 그래프, v는 시작 정점

    visited[v] = True       # v 방문설정

    FOR each all w in adjacency(G, v)
        IF visited[w] != True
            DFS_Recursive(G, v)
```

### 반복을 이용한 DFS 구현

```python
STACK s
visited []

DFS(v)
    push(s, v)
    WHILE NOT isEmpty(s)
        v <- pop(s)
        IF NOT visited[v]
            visit(v)
            FOR each w in adjacency(v)
                IF NOT visited[w]
                    push(s, w)
```

## DFS 예제 :

- 다음은 연결되어 있는 두 개의 정점 사이의 간선을 순서대로 나열 해 놓은 것이다. 모든 정점을 깊이 우선 탐색하여 화면에 깊이 우선 탐색 경로를 출력하시오. 시작 정점은 1로 시작하시오.

    ```python
    7 8
    1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
    ```

    ```python
    # 정점, 간선의 개수
    N, E = map(int, input().split())
    # 간선들의 정보
    temp = list(map(int, input().split()))

    # 인접행렬
    G = [[0] * (N + 1) for _ in range(N + 1)]

    # 방문체크
    visited = [0] * (N + 1)

    # 간선들을 인접행렬에 저장
    for i in range(E):
        s, e = temp[2 * i], temp[(2 * i) + 1]
        G[s][e] = 1
        G[e][s] = 1

    def dfs(v):
        # 방문체크
        visited[v] = 1
        print(v, end = " ")
        # v의 인접한 정점 중에서 방문 안 한 정점을 재귀호출
        for w in range(1, N+1):
            if G[v][w] == 1 and visited[w] == 0:
                dfs(w)

    dfs(1)
    ```
    
    ------
    ⓒ 2021. `chloeelog` all rights reserved.
