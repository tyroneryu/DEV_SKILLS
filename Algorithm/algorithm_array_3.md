# Array
- 배열: 2차원 배열
- 부분집합 생성
- 바이너리 서치 (Binary Search)
- 셀렉션 알고리즘 (Selection Algorithm)
- 선택 정렬 (Selection Sort)

## 2차원 배열
### 2차원 배열의 선언
- 1차원 List를 묶어놓은 List
- 2차원 이상의 다차원 List는 차원에 따라 Index를 선언
- 2차원 List의 선언: 세로 길이(행의 개수), 가로 길이(열의 개수)를 필요로 함
- Python 에서는 데이터 초기화를 통해 변수선언과 초기화가 가능함
- `arr = [[0, 1, 2, 3], [4, 5, 6, 7]]` : 2행 4열의 2차원 List

### 2차원 배열의 접근
- 배열 순회
    - n X m 배열의 n * m개의 모든 원소를 빠짐없이 조사하는 방법
- 행 우선 순회
```python
# i 행의 좌표
# j 열의 좌표

for i in range(n):
    for j in range(m):
        f(array[i][j])  # 필요한 연산 수행
```
- 열 우선 순회
```python
# i 행의 좌표
# j 열의 좌표

for j in range(m):
    for i in range(n):
        f(array[i][j])  # 필요한 연산 수행
```
- 지그재그 순회
```python
# i 행의 좌표
# j 열의 좌표

for i in range(n):
    for j in range(m):
        f(array[i][j + (m - 1 - 2 * j) * (i % 2)])  # 필요한 연산 수행
```
- 대각선 순회
```python
# i 행의 좌표
# j 열의 좌표

# 정방향
for i in range(n):
    f(array[i][j])  # 필요한 연산 수행

# 역방향
N = n + m
for i in range(n):
    f(array[i][N-1-i])  # 필요한 연산 수행
```
- 델타를 이용한 2차 배열 탐색
    - 2차 배열의 한 좌표에서 4방향의 인접 배열 요소를 탐색하는 방법
    - 인덱스 (i, j)인 칸의 상하좌우 칸 (ni, nj)
    ```python
    di = [0, 1, 0, -1]  # 방향별로 더할 값
    dj = [1, 0, -1, 0]
    N = 5
    arr = [[0] * N for _ in range(N)]
    for i in range(N):
        for j in range(N):
            for k in range(4):
                ni = i + di[k]
                nj = j + dj[k]
                if 0 <= ni < N and 0 <= nj < N:
                    print(arr[ni][nj], end = ' ')
            print()
    ```
    ```python
    
    ```
- 전치 행렬
```python
# i: 행의 좌표, len(arr)
# j: 열의 좌표, len(arr[0])

arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]] # 3 * 3 행렬

for i n range(3):
    for j in range(3):
        if i < j:
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
```

## 부분 집합
### 부분 집합 생성하기
- 완전검색 기법으로 부분집합 합 문제를 풀기 위해서는, 우선 집합의 모든 부분 집합을 생성한 후에 각 부분 집합의 합을 계산해야 함
- 주어진 집합의 부분 집합을 생성하는 방법에 대해 생각해보자
-  부분 집합의 수
    - 집합의 원소가 n개일 때, 공집합을 포함한 부분집합의 수는 2^n개임
    - 이는 각 원소를 부분집합에 포함시키거나 포함시키지 않은 2가지 경우를 모든 원소에 적용한 경우의 수와 같음
    - 예) {1, 2, 3, 4}
        - 2 X 2 X 2 X 2 = 16가지
- 각 원소가 부분집합에 포함되었는지를 loop 이용하여 확인하고 부분집합을 생성하는 방법
```python
bit == [0, 0, 0, 0]
for i in range(2):
    bit[0] = i                      # 0번 원소
    for j in range(2):
        bit[1] = j                  # 1번 원소
        for k in range(2):
            bit[2] = k              # 2번 원소
            for l in range(2):
                bit[3] = l          # 3번 원소
                print_subset(bit)   # 생성된 부분집합 출력
```

### 비트 연산자
|비트 연산자|설명|
|:---:|:---:|
|&|비트 단위로 AND 연산을 한다.|
|ㅣ(pipe)|비트 단위로 OR 연산을 한다.|
|<<|피연산자의 비트 열을 왼쪽으로 이동시킨다.|
|>>|피연산자의 비트 열을 오른쪽으로 이동시킨다.|

- << 연산자
    - 1 << n: 2^n 즉, 원소가 n개일 경우의 모든 부분집합의 수를 의미함
- & 연산자
    - i & (1 << j): i의 j번째 비트가 1인지 아닌지를 검사함
- 보다 간결하게 부분집합을 생성하는 방법
```python
arr = [3, 6, 7, 1, 5, 4]
n = len(arr)                            # n: 원소의 개수

for i in range(1 << n):                 # 1 << n: 부분집합의 개수
    for j in range(n):                  # 원소의 수만큼 비트를 비교함
        if i & (1 << j):                # i의 j번 비트가 1인 경우
            print(arr[j], end = ', ')   # j번 원소 출력
    print()
print()
```
```python
N = 5
arr = [1, 2, 3, 4, 5]

for i in range(1 << N):                 # 1 << n: 부분집합의 개수
    for j in range(N):                  # 원소의 수만큼 비트를 비교함
        if i & (1 << j):                # i의 j번 비트가 1인 경우
            print(arr[j], end = ' ')
    print()
```

### 연습 문제
- 부분집합의 합 문제 구현
- 10개의 정수를 입력 받아 부분집합의 합이 0이 되는 것이 존재하는지를 계산하는 함수를 작성해보자
```python
def f(arr, N):
    for i in range(1 << N):
        s = 0
        for j in range(N):
            if i & (1 << j):
                s += arr[j]
        if s == 0:
            return True
    return False
```
