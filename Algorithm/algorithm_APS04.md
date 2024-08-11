# APS
## MST(최소 비용 신장 트리)
- 그래프에서 최소 비용 문제
    1. 모든 정점을 연결하는 간선들의 가중치의 합이 최소가 되는 트리
    2. 두 정점 사이의 최소 비용의 경로 찾기
- 신장 트리
    - n개의 정점으로 이루어진 무방향 그래프에서 n개의 정점과 n - 1개의 간선으로 이루어진 트리
- 최소 신장 트리 (Minimum Spanning Tree)
    - 무방향 가중치 그래프에서 신장 트리를 구성하는 간선들의 가중치의 합이 최소인 신장 트리

### MST 구현
![APS69](./asset/APS69.PNG)
![APS70](./asset/APS70.PNG)

### MST 구현 방법
- Prim 알고리즘
- Kruskal 알고리즘

## Prim 알고리즘
- 하나의 정점에서 연결된 간선들 중에 하나씩 선택하면서 MST를 만들어 가는 방식
    1. 임의 정점을 하나 선택해서 시작
    2. 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택
    3. 모든 정점이 선택될 때까지 위 두 과정을 반복
- 서로소인 2개의 집합 (2 disjoint-sets)정보를 유지
    - 트리 정점들(tree verticles) - MST를 만들기 위해 선택된 정점들
    - 비트리 정점들(nontree verticles) - 선택되지 않은 정점들

### 알고리즘 적용 예
![APS71](./asset/APS71.PNG)
![APS72](./asset/APS72.PNG)

### 알고리즘
```python
def MST_PRIM(G, r):     # G: 그래프, r: 시작 정점
    for u in G.V:
        u.key <- inf    # u.key: u에 연결된 간선 중 최소 가중치
        u.pi <- Null    # u.pi: 트리에서 u의 부모
    r.key <- 0
    Q <- G.V
    while Q != 0:
        u <- Extract_Min(Q)
        for v in G.Adj[u]:
            if v in Q and w(u, v) < v.key:
                v.pi <- u
                v.key <- w(u, v)
```

### 연습 문제
```python
'''
7 11
0 1 32
0 2 31
0 5 60
0 6 51
1 2 21
2 4 46
2 6 25
3 4 34
3 5 18
4 5 40
4 6 51
'''
from heapq import heappush, heappop

def prim(start):
    pq = []
    MST = [0] * V
    # 최소 비용
    sum_weight = 0
    # 시작점 추가
    # prim: 가중치 낮으면 먼저 나와야 함
        # 관리해야할 데이터: 가중치, 노드 번호 2가지
        # 동시에 두가지 데이터 다루기
            # 1. class로 만들기
            # 2. 튜플로 관리
    heappush(pq, (0, start))
    while pq:
        weight, now = heappop(pq)

        print(now, '/', MST)
        # 방문했다면 continue
        if MST[now]:
            continue
        # 방문 처리
        MST[now] = 1
        # 누적합 추가
        sum_weight += weight
        # 갈수 있는 노드들
        for to in range(V):
            # 갈수 없거나 이미 방문했다면 pass
            if graph[now][to] == 0 or MST[to]:
                continue
            hepapush(pq, (graph[now][to], to))
    print(f'최소 비용: {sum_weight}')

V, E = map(int, input().split())
# 인접 행렬
graph = [[0] * V for _ in range(V)]
for _ in range(E):
    s, e, w = map(int, input().split())
    graph[s][e] = w
    graph[e][s] = w
prim(0)
```

## Kruskal 알고리즘
- 간선을 하나씩 선택해서 MST를 찾는 알고리즘
    1. 최초, 모든 간선을 가중치에 따라 오름차순으로 정렬
    2. 가중치가 가장 낮은 간선부터 선택하면서 트리를 증가시킴
        - 사이클이 존재하면 다음으로 가중치가 낮은 간선 선택
    3. n - 1개의 간선이 선택될 때까지 2.를 반복

### 알고리즘 적용 예시
![APS73](./asset/APS73.PNG)
![APS74](./asset/APS74.PNG)
![APS75](./asset/APS75.PNG)

### 알고리즘
```python
def MST_KRUSKAL(G, w):
    A <- 0                  # 0: 공집합
    for vertex v in G.V:    # G.V: 그래프의 정점 집합
        Make_Set(v)         # G.E: 그래프의 간선 집합
    
    G.E에 포함된 간선들을 가중치 w에 의해 정렬

    for 가중치가 가장 낮은 간선 (u, v) in G.E 선택:
        if Find_Set(u) != Find_Set(v):
            A <- A + {(u, v)}
            Union(u, v)
    return A
```

### 연습 문제
```python
'''
7 11
0 1 32
0 2 31
0 5 60
0 6 51
1 2 21
2 4 46
2 6 25
3 4 34
3 5 18
4 5 40
4 6 51
'''
def find_set(x):
    if parents[x] == x:
        return x
    # 경로 압축
    parents[x] = find_set(parents[x])
    return parents[x]

def union(x, y):
    x = find_set(x)
    y = find_set(y)
    # 같은 집합이면 pass
    if x == y:
        return
    if x < y:
        parents[y] = x
    else:
        parents[x] = y

V, E = map(int, input().split())
# 간선 정보들을 모두 저장
edges = []
for _ in range(V):
    s, e, w = map(int, input().split())
    edges.append([s, e, w])
# 가중치를 기준으로 정렬
edges.sort(key=lambda x: x[2])
# 대표자 배열
parents = [i for i in range(V)]

# MST 완성 = 간선의 개수가 V - 1개 일때
cnt = 0
sum_weight = 0
# 간선 확인
for s, e, w in edges:
    # 싸이클 발생시 pass
    # 이미 같은 집합에 속해있다면 pass
    if find_set(s) == find_set(e):
        continue
    cnt += 1
    # 싸이클 없으면 방문 처리
    union(s, e)
    sum_weight += w

    # MST 완성: 간선의 개수 = V - 1
    if cnt == V:
        break

print(f'최소 비용: {sum_weight}')
```

## 최단 경로(Dijkstra)
- 정의: 간선의 가중치가 있는 그래프에서 두 정점 사이의 경로들 중에 간선의 가중치의 합이 최소인 경로
- 하나의 시작 정점에서 끝 정점까지의 최단 경로
    - 다익스트라(dijkstra) 알고리즘
        - 음의 가중치를 허용하지 않음
    - 벨만-포드(Bellman-Ford) 알고리즘
        - 음의 가중치 허용
- 모든 정점들에 대한 최단 경로
    - 플로이드-워샬(Floyd-Warshall) 알고리즘

### Dijkstra 알고리즘
- 시작 정점에서 거리가 최소인 정점을 선택해 나가면서 최단 경로를 구하는 방식
- 시작 정점(s)에서 끝 정점(t) 까지의 최단 경로에 정점 x가 존재
- 이때, 최단경로는 s에서 x까지의 최단 뎡로와 x에서 t까지의 최단경로 구성됨
- 탐욕 기법을 사용한 알고리즘으로 MST의 프림 알고리즘과 유사함

![APS76](./asset/APS76.PNG)

#### 알고리즘
```python
# s: 시작 정점, A: 인접 행렬, D: 거리
# V: 정점 집합, U: 선택된 정점 집합
def Dijkstra(s, A, D):
    U = {s}
    for 모든 정점 v:
        D[v] <- A[s][v]
    while U != V:
        D[w]가 최소인 정점 w in V - U 를 선택
        U <- U + {w}
        for w에 인접한 모든 정점 v:
            D[v] <- min(D[v], D[w], A[w][v])
```

#### 알고리즘 적용 예시
![APS77](./asset/APS77.PNG)
![APS78](./asset/APS78.PNG)
![APS79](./asset/APS79.PNG)
![APS80](./asset/APS80.PNG)
![APS81](./asset/APS81.PNG)
![APS82](./asset/APS82.PNG)
![APS83](./asset/APS83.PNG)
![APS84](./asset/APS84.PNG)
![APS85](./asset/APS85.PNG)

#### 연습 문제
```python
from heapq import heappush, heappop
INF = int(1e9)

def dijkstra(start):
    pq = []
    # 시작점의 weight, node 번호를 한번에 저장
    heappush(pq, (0, start))
    # 시작 노드 초기화
    distance[start] = 0
    
    while pq:
        # 최단 거리 노드에 대한 정보
        dist, now = heappop(pq)
        # pq의 특성 때문에 더 긴거리가 미리 저장되어 있음
        # now가 이미 더 짧은 거리로 온 적이 있다면 pass
        if distance[now] < dist:
            continue
        # now에서 인접한 다른 노드 확인
        for to in graph[now]:
            next_dist = to[0]
            next_node = to[1]
            # 누적 거리 계산
            new_dist = dist + next_dist
            # 이미 더 짧은 거리로 간 경우 pass
            if new_dist >= distance[next_node]:
                continue
            #누적 거리를 최단 거리로 갱신
            distance[next_node] = new_dist
            # next_node의 인접 노드들을 pq에 추가
            heappush(pq, (new_dist, next_node))

V, E = map(int, input().split())
# 시작 노드 번호
start = 0
# 인접 리스트
graph = [[] for _ in range(V)]
# 누적 거리를 저장할 변수
distance = [INF] * V

# 간선 정보 저장
for _ in range(E):
    s, e, w = map(int, input().split())
    graph[s].append([w, e])

dijkstra(0)
print(distance)
```
