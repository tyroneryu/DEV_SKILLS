# Stack
## DP(Dynamic Programming)
- 동적 계획 (Dynamic Programming) 알고리즘은 그리디 알고리즘과 같이 **최적화 문제**를 해결하는 알고리즘임
- 동적 계획 알고리즘은 먼저 입력 크기가 작은 부분 문제들을 모두 해결한 후에 그 해들을 이용하여 보다 큰 크기의 부분 문제들을 해결하여, 최종적으로 원래 주어진 입력의 문제를 해결하는 알고리즘임

### DP in 피보나치 수
- 피보나치 수는 부분 문제의 답으로부터 본 문제의 답을 얻을 수 있으므로 최적 부분 구조로 이루어져 있음
1. 문제를 부분 문제로 분할함
    - Fibonacci(n) 함수는 Fibonacci(n - 1)과 Fibonacci(n - 2)의 합
    - Fibonacci(n - 1)은 Fibonacci(n - 2)와 Fibonacci(n - 3)의 합
    - Fibonacci(2)는 Fibonacci(1)과 Fibonacci(0)의 합
    - Fibonacci(n)은 Fibonacci(n - 1), Fibonacci(n - 2), ... Fibonacci(2), Fibonacci(1), Fibonacci(0)의 부분집합으로 나뉨
2. 부분 문제로 나누는 일을 끝냈으면 가장 작은 부분 문제부터 해를 구함
3. 그 결과는 테이블에 저장하고, 테이블에 저장된 부분 문제의 해를 이용하여 상위 문제의 해를 구함
![Fibo_table](./asset/Fibo_table.PNG)
- 피보나치 수 DP 적용 알고리즘
```python
def fibo2(n):
    f = [0] * (n + 1)
    f[0] = 0
    f[1] = 1
    for i in range(2, n + 1):
        f[i] = f[i - 1] + f[i - 2]
    
    return f[n]
```

### DP의 구현 방식
- recursive 방식: fib1()
- iterative 방식: fib2()
- memoization을 재귀적 구조에 사용하는 것보다 반복적 구조로 DP를 구현한 것이 성능 면에서 보다 효율적임
- 재귀적 구조는 내부에 시스템 호출 스택을 사용하는 오버헤드가 발생하기 때문

## DFS(Depth First Search)
- 깊이 우선 탐색
- 비선형구조인 그래프 구조는 그래프로 표현된 모든 자료를 빠짐없이 검색하는 것이 중요함
- 두 가지 방법
    - 깊이 우선 탐색(Depth First Search, DFS)
    - 너비 우선 탐색(Breadth First Search, BFS)
- 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선에 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 방문하는 순회방법
- 가장 마지막에 만났던 갈림길의 정점으로 되돌아가서 다시 깊이 우선 탐색을 반복해야 하므로 후입선출 구조의 스택 사용

### DFS 알고리즘
1. 시작 정점 V를 결정하여 방문함
2. 정점 v에 인접한 정점 중에서
    1. 방문하지 않은 정점 w가 있으면, 정점 v를 스택에 push하고, 정점 w를 방문함
    2. 방문하지 않은 정점이 없으면, 탐색의 방향을 바꾸기 위해 스택을 pop하여 받은 가장 마지막 방문 정점을 v로 하여 다시 2.를 반복.
3. 스택이 공백이 될 때까지 2.를 반복
```python
visited[], stack[] 초기화
DFS(v)
    시작점 v 방문
    visited[v] = True
    while:
        if v의 인접 정점 중 방문 안한 정점 w가 있으면:
            push(v)
            v = w (w에 방문)
            visited[w] = True
        else:
            if 스택이 비어있지 않으면:
                v = pop(stack)
            else:
                break
```

### DFS 예
- 초기 상태: 배열 visited 를 False로 초기화하고, 공백 스택을 생성
![DFS01](./asset/DFS01.PNG)
1. 정점 A를 시작으로 깊이 우선 탐색을 시작    
`A 방문; visited[A] = True;`
![DFS02](./asset/DFS02.PNG)
2. 정점 A에 방문하지 않은 정점 B, C가 있으므로 A를 스택에 push하고, 인접정점 B와 C 중에서 오름차순에 따라 B를 선택하여 탐색을 계속함    
`push(A); B 방문; visited[B] = True;`
![DFS03](./asset/DFS03.PNG)
3. 정점 B에 방문하지 않은 정점 D, E가 있으므로 B를 스택에 push하고, 인접정점 D와 E 중에서 오름차순에 따라 D를 선택하여 탐색을 계속함    
`push(B); D 방문; visited[D] = True;`
![DFS04](./asset/DFS04.PNG)
4. 정점 D에 방문하지 않은 정점 F가 있으므로 D를 스택에 push하고, 인접정점 F를 선택하여 탐색을 계속함    
`push(D); F 방문; visited[F] = True;`
![DFS05](./asset/DFS05.PNG)
5. 정점 F에 방문하지 않은 정점 E, G가 있으므로 F를 스택에 push하고, 인접정점 E와 G 중에서 오름차순에 따라 E를 선택하여 탐색을 계속함    
`push(F); E 방문; visited[E] = True;`
![DFS06](./asset/DFS06.PNG)
6. 정점 E에 방문하지 않은 정점 C가 있으므로 E를 스택에 push하고, 인접정점 C를 선택하여 탐색을 계속함    
`push(E); C 방문; visited[C] = True;`
![DFS07](./asset/DFS07.PNG)
7. 정점 C에서 방문하지 않은 인접정점이 없으므로, 마지막 정점으로 돌아가기 위해 스택을 pop하여 받은 정점 E에 대해서 방문하지 않은 인접정점이 있는지 확인    
`pop(stack);`
![DFS08](./asset/DFS08.PNG)
8. 정점 E에서 방문하지 않은 인접정점이 없으므로, 다시 스택을 pop하여 받은 정점 F에 대해서 방문하지 않은 인접정점이 있는지 확인    
`pop(stack);`
![DFS09](./asset/DFS09.PNG)
9. 정점 F에 방문하지 않은 정점 G가 있으므로 F를 스택에 push하고, 인접정점 G를 선택하여 탐색을 계속함    
`push(F); G 방문; visited[G] = True;`
![DFS10](./asset/DFS10.PNG)
10. 정점 G에서 방문하지 않은 인접정점이 없으므로, 마지막 정점으로 돌아가기 위해 스택을 pop하여 받은 정점 F에 대해서 방문하지 않은 인접정점이 있는지 확인    
`pop(stack);`
![DFS11](./asset/DFS11.PNG)
11. 정점 F에서 방문하지 않은 인접정점이 없으므로, 마지막 정점으로 돌아가기 위해 스택을 pop하여 받은 정점 D에 대해서 방문하지 않은 인접정점이 있는지 확인    
`pop(stack);`
![DFS12](./asset/DFS12.PNG)
12. 정점 D에서 방문하지 않은 인접정점이 없으므로, 마지막 정점으로 돌아가기 위해 스택을 pop하여 받은 정점 B에 대해서 방문하지 않은 인접정점이 있는지 확인    
`pop(stack);`
![DFS13](./asset/DFS13.PNG)
13. 정점 B에서 방문하지 않은 인접정점이 없으므로, 마지막 정점으로 돌아가기 위해 스택을 pop하여 받은 정점 A에 대해서 방문하지 않은 인접정점이 있는지 확인    
`pop(stack);`
![DFS14](./asset/DFS14.PNG)
14. 현재 정점 A에서 방문하지 않은 인접 정점이 없으므로 마지막 정점으로 돌아가기 위해 스택을 pop하는데, 스택이 공백이므로 깊이 우선 탐색을 종료    
    - 깊이 우선 탐색 경로: A-B-D-F-E-C-G

### 연습문제
- 다음은 연결되어 있는 두개의 정점 사이의 간선을 순서대로 나열해 놓은 것임.
- 모든 정점을 깊이 우선 탐색하여 화면에 깊이 우선 탐색 경로를 출력하라.
- 시작 정점은 1
![DFS_practice](./asset/DFS_practice.PNG)
```python
# 7 8
# 1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7

def dfs(i, V):                      # 시작 i, 마지막 V
    visited = [0] * (V + 1)         # visited, stack 생성 및 초기화
    st = []
    visited[i] = 1                  # 시작점 방문
    print(i)                        # 정점에서 할 일
    while True:
        for w in adjl[i]:           # 현재 방문한 정점에 인접하고 방문 안한 정점 w가 있으면
            if visited[w] == 0:
                st.append(i)        # push(i), i를 지나서
                i = w               # w에 방문
                visited[i] = 1
                print(i)
                break
        else:                       # i에 남은 인접 정점이 없으면
            if st:      # 스택이 비어있지 않으면 (지나온 정점이 남아 있으면)
                i = st.pop()
            else:       # 스택이 비어있으면 (출발점에서 남은 정점이 없으면)
                break
    return

V, E = map(int, input().split())
arr = list(map(int, input().split()))

# 인접리스트
adjl = [[] for _ in range(V + 1)]   # adjl[i] 행에 i에 인접인 정점번호
for i in range(E):
    n1, n2 = arr[i * 2], arr[i * 2 + 1]
    adjl[n1].append(n2)
    adjl[n2].append(n1)             # 방향이 없는 경우
dfs(1, V)
```
- 재귀로 하는 방법도 있음
```python
def dfs(i):         # 시작 i
    visited[i]  = 1 # 방문 표시
    print(i)        # 출력
    # i에 인접하고 방문 안한 w가 있으면
    for w in adjl[i]:
        if visited[w] == 0:
            dfs(w)
```
