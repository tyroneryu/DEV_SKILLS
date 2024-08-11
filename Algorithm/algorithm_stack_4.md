# Stack
## 부분집합 / 순열
### 연습문제2
- {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}의 부분집합 중 합이 10인 부분집합의 개수
```python
def f(i, k, t):        # k개의 원소를 가진 배열 A, 부분집합 합이 t인 경우
    if i == k:      # 모든 원소에 대해 결정하면
        ss = 0      # 부분집합 원소의 합
        for j in range(k):
            if bit[i]:      # A[j]가 포함된 경우
                ss += A[j]
        if ss == t:
            for j in range(k):
                if bit[j]:  # A[j]가 포함된 경우
                print(A[j], end = ' ')
            print()
    else:
        for j in range(1, -1, -1):
            bit[i] = j
            f(i + 1, k, t)
        # bit[i] = 1  # for문과 동일
        # f(i + 1, k)
        # bit[i] = 0
        # f(i + 1, k)

N = 10
A = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
bit = [0] * N
f(0, N, 10)
```

### 부분집합의 합
- 집합 {1, 2, 3} 의 원소에 대해 각 부분집합에서의 포함 여부를 트리로 표현
![Tree01](./asset/Tree01.PNG)
- i 원소의 포함 여부를 결정하면 i까지의 부분 집합의 합 Si를 결정할 수 있음
- S(i-1) 이 찾고자 하는 부분집합의 합보다 크면 남은 원소를 고려할 필요가 없음
![Tree02](./asset/Tree02.PNG)
- A[i] 원소를 부분집합의 원소로 고려하는 재귀함수 (A는 서로 다른 자연수의 집합)
```python
# i - 1 원소까지 고려한 합 s, 찾으려는 합 t
f(i, N, s, t)
    if s == t:      # i - 1 원소까지의 합이 찾는 값인 경우
        ...
    elif i == N:    # 모든 원소에 대한 고려가 끝난 경우
        ...
    elif s > t:     # 남은 원소를 고려할 필요가 없는 경우
        ...
    else:           # 남은 원소가 있고, s < t 인 경우
        subset[i] = 1
        f(i + 1, N, s + A[i], t)    # i 원소 포함
        subset[i] = 0
        f(i + 1, N, s, t)           # i 원소 미포함
```
```python
def f(i, k, s, t):
    global cnt
    cnt += 1
    if s == t:
        for j in range(k):
            if bit[j]:
                print(A[j], end = ' ')
    elif i == k:
        return
    elif s > t:
        return
    else:
        # for j in range(1, -1, -1):
        #     bit[i] = j
        #     f(i + 1, k, t)
        bit[i] = 1
        f(i + 1, k, s + A[i], t)
        bit[i] = 0
        f(i + 1, k, s, t)

N = 10
A = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
bit = [0] * N
f(0, N, 0, 10)
print(cnt)
```
- 추가 고려 사항
![Additional](./asset/Additional01.PNG)
- 남은 원소의 합을 다 더해도 찾는 값 T 미만인 경우 중단

### 순열
- A[1, 2, 3]의 모든 원소를 사용한 순열
    - 123, 132, 213, 231, 312, 321
    - 총 6가지 경우
```python
f(i, N)
    if i == N       # 순열 완성
        
    else:
        가능한 모든 원소에 대해
            P[i] 결정
            f(i + 1, N)
            P[i] 복구
```
![Permutation01](./asset/Permutation01.PNG)
![Permutation02](./asset/Permutation02.PNG)
```python
def f(i, k):
    if i == k:
        print(*P)
    else:
        for j in range(i, k):       # P[i] 자리에 올 원소 P[j]
            P[i], P[j] = P[j], P[i]
            f(i + 1, k)
            P[i], P[j] = P[j], P[i]

N = 3
P = [1, 2, 3]
f(0, N)
```
- 연습문제
```python
def f(i, k, s):     # s: i - 1까지 선택한 원소의 합
    global min_v
    global cnt
    cnt += 1
    if i == k:
        # print(*P)
        s = 0       # 선택한 원소의 합
        for j in range(k):
            s += arr[j][P[j]]   # j행에서 P[j] 열을 고른 경우의 합 구하기
        if min_v > s:
            min_v = s
    elif s >= min_v:
        return
    else:
        for j in range(i, k):       # P[i] 자리에 올 원소 P[j]
            P[i], P[j] = P[j], P[i]
            f(i + 1, k, s + arr[i][P[i]])
            P[i], P[j] = P[j], P[i]

N = int(input())
arr = [list(map(int, input().split())) for _ in range(N)]
P = [i for i in range(N)]
min_v = 100
cnt = 0
f(0, N, 0)
print(min_v, cnt)
```

## 분할 정복 알고리즘
- 유래
    - 1805년 12월 2일 아우스터리츠 전투에서 나폴레옹이 사용한 전략
    - 전력이 우세한 연합군을 공격하기 위해 나폴레옹은 연합군의 중앙부로 쳐들어가 연합군을 둘로 나눔
    - 둘로 나뉜 연합군을 한 부분씩 격파함
- 설계 전략
    - 분할(Divide): 해결할 문제를 여러 개의 작은 부분으로 나눔
    - 정복(Conquer): 나눈 작은 문제를 각각 해결
    - 통합(Combine): (필요하다면) 해결된 해답을 모음

### 거듭 제곱(Exponentiation)
- O(n)
- C^n = C * C * ... * C
```python
def Power(Base, Exponent):
    if Base == 0:
        return 1
    result = 1      # Base^0 은 1이므로
    for i in range(Exponent):
        result *= Base
    return result
```
- 분할 정복 기반의 알고리즘
    - O(log2n)
- C^n = C^((n-1)/2) * C^((n-1)/2) * C = (C^((n-1)/2))^2 * C
- C^n
    - n: 짝수 = C^(n/2) * C^(n/2)
    - n: 홀수 = C^((n-1)/2) * C^((n-1)/2) * C
```python
def Power(Base, Exponent):
    if Exponent == 0 or Base == 0:
        return 1
    
    if Exponent % 2 == 0:
        NewBase = Power(Base, Exponent / 2)
        return NewBase * NewBase
    else:
        NewBase = Power(Base, (Exponent -1) / 2)
        return (NewBase * NewBase) * Base
```

### 퀵 배열
- 주어진 배열을 두 개로 분할하고, 각각을 정렬함
    - 합병정렬과 동일?
- 차이점1: 합병정렬은 그냥 두 부분으로 나누는 반면에, 퀵정렬은 분할할 때, 기준 아이템(pivot item) 중심으로, 이보다 작은 것은 왼편, 큰 것은 오른편에 위치시킴
- 차이점2: 각 부분 정렬이 끝난 후, 합병정렬은 **합병**이란 후처리 작업이 필요하나, 퀵정렬은 필요로 하지 않음
-  알고리즘
```python
def quickSort(a, begin, end):
    if begin < end:
        p = partition(a, begin, end)
        quickSort(a, begin, p - 1)
        quickSort(a, p + 1, end)

def partition(a, begin, end):
    pivot = (begin + end) // 2
    L = begin
    R = end
    while L < R:
        while(L < r and a[L] < a[pivot]): L += 1
        while(L < R and a[R] >= a[pivot]): R -= 1
        if L < R:
            if L == pivot:
                pivot = R
            a[L], a[R] = a[R], a[L]
    a[pivot], a[R] = a[R], a[pivot]
    return R
```

#### 퀵 정렬 수행 과정
- 예제: {69, 10, 30, 2, 16, 8, 31, 22}
- 원소의 개수가 8개이므로 네번째 자리에 있는 원소2를 첫번째 피봇으로 선택하고 퀵 정렬 시작
![Quick01](./asset/Quick01.PNG)

1. 원소2를 피봇으로 선택하고 퀵 정렬 시작
    - L이 오른쪽으로 이동하면서 피봇보다 크거나 같은 원소를 찾고, R은 왼쪽으로 이동하면서 피봇보다 작은 원소를 찾음
    - L은 원소 69를 찾았지만, R은 피봇보다 작은 원소를 찾지 못한 채로 원소 69에서 L과 만나게 됨
    - L과 R이 만났으므로, 원소 69를 피봇과 교환하여 피봇 원소 2의 위치를 확정함
    ![Quick02](./asset/Quick02.PNG)
2. 피봇 2의 왼쪽 부분 집합은 공집합이므로 퀵 정렬을 수행하지 않고, 오른쪽 부분 집합에 대해서 퀵 정렬 수행
    - 오른쪽 부분 집합의 원소가 7개이므로 가운데 있는 원소 16을 피봇으로 선택
    - L이 찾은 30과 R이 찾은 8을 서로 교환
    ![Quick03](./asset/Quick03.PNG)
    - 현재 위치에서 L과 R의 작업을 반복
    - L은 원소 69를 찾았지만, R은 피봇보다 작은 원소를 찾지 못한 채로 원소 69에서 L과 만남
    - L과 R이 만났으므로, 원소 69를 피봇과 교환하여 피봇 원소 16의 위치를 확정
    ![Quick04](./asset/Quick04.PNG)
3. 피봇 16의 왼쪽 부분 집합에서 원소 10을 피봇으로 선택하여 퀵 정렬 수행
    - L의 원소 10과 R의 원소 8을 교환하는데, L의 원소가 피봇이므로 피봇원소에 대한 자리교환이 발생한 것이므로 교환한 자리를 피봇 원소 10의 위치로 확정
    ![Quick05](./asset/Quick05.PNG)
4. 피봇 10의 확정된 위치에서의 왼쪽 부분집합은 원소가 한개이므로 퀵 정렬을 수행하지 않고, 오른쪽 부분집합은 공집합이므로 역시 퀵 정렬을 수행하지 않음
    - 이제 1단계의 피봇이었던 원소 16에 대한 오른쪽 부분집합에 대해 퀵 정렬을 수행
    - 오른쪽 부분 집합의 원소가 4개이므로 두번째 원소 30을 피봇으로 선택
    - L이 찾은 69와 R이 찾은 22를 서로 교환
    ![Quick06](./asset/Quick06.PNG)
    - 현재 위치에서 L과 R의 작업을 반복
    - L은 오른쪽으로 이동하면서 피봇보다 크거나 같은 원소인 30을 찾고, R은 왼쪽으로 이동하면서 피봇보다 작은 원소를 찾다가 못 찾고 원소 30에서 L과 만남
    - L과 R이 만났으므로 피봇과 교환
    - 이 경우 R의 원소가 피봇이므로 피봇에 대한 자리교환이 발생한 것이므로 교환한 자리를 피봇의 자리로 확정함
    ![Quick07](./asset/Quick07.PNG)
5. 피봇 30의 확정된 위치에서의 왼쪽 부분 집합의 원소가 한 개이므로 퀵 정렬을 수행하지 않고, 오른쪽 부분집합에 대해서 퀵 정렬 수행
    - 오른쪽 부분집합의 원소 2개 중에서 원소 31을 피봇으로 선택
    - L은 오른쪽으로 이동하면서 원소 31을 찾고, R은 왼쪽으로 이동하면서 피봇보다 작은 원소를 찾다가 못 찾은 채로 원소 31에서 L과 만남
    - L과 R이 만났으므로 피봇과 교환하는데 R의 원소가 피봇이므로 결국 제자리가 확정됨
    ![Quick08](./asset/Quick08.PNG)
    - 피봇 31의 오른쪽 부분집합의 원소가 한 개이므로 퀵 정렬을 수행하지 않음
    - 이것으로써 전체 퀵 정렬이 모두 완성
- 퀵정렬의 최악의 시간 복잡도는 O(n^2)로, 합병정렬에 비해 좋지 못함
- 평균 복잡도 nlogn이라 사용

### 정렬 알고리즘 비교
|알고리즘|평균 수행 시간|최악 수행 시간|알고리즘 기법|비고|
|:---:|:---:|:---:|:---:|:---:|
|버블 정렬|O(n^2)|O(n^2)|비교와 교환|코딩이 가장 손쉬움|
|카운팅 정렬|O(n + k)|O(n + k)|비교환 방식|n이 비교적 작을 때만 가능함|
|선택 정렬|O(n^2)|O(n^2)|비교와 교환|교환의 회수가 버블, 삽입정렬보다 작음|
|퀵 정렬|O(n log n)|O(n^2)|분할 정복|최악의 경우 O(n^2)이지만, 평균적으로 가장 빠름|
|삽입 정렬|O(n^2)|O(n^2)|비교와 교환|n의 개수가 작을 때 효과적임|
|병합 정렬|O(n log n)|O(n log n)|분할 정복|연결리스트의 경우 가장 효율적인 방식|
