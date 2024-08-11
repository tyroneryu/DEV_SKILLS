# APS
## 일고리즘 설계 기법의 종류
### 1. Brute force(완전 탐색)
- 전체를 다 보기
- 배열: for문, while문
- 그래프: 관계가 있는 데이터
    - DFS, BFS
- 완전 탐색 구현 시 시간 or 메모리 초과 발생 가능

### 2. Greedy(그리디)
- 규칙 + 증명 -> 구현

### 3. Dynamic Programming(DP)
- 큰 문제를 작은 문제로 나누어 부분적으로 해결하기
- 분할 정복과 다르게 작은 문제가 중복
- 중복된 문제의 해답을 저장해놓고 재활용 (Memoization)

### 4. ETC
- 분할 정복: 큰 문제를 작은 문제로 나누어 부분적으로 해결
- Backtracking: 전체 중 가능성 없는 것을 빼기

## 분할 정복
### 학습 목표
- 문제를 분할해서 해결하는 분할 정복(Divide and Conquer) 기법을 이해
- 대표적인 알고리즘인 퀵 정렬과 병합 정렬에 대해 학습

### 문제: 가짜 동전 찾기
- n개의 동전들 중에 가짜 동전이 하나 포함되어 있다.
- 가짜 동전은 진짜 동전에 비해 아주 조금 가벼움
- 진짜 동전들의 무게가 동일하다고 할 때 양팔 저울을 이용해 가짜 동전을 찾아보자
- 양팔 저울을 최소로 이용해서 가짜 동전을 찾는 방법은?
- ex) 동전이 24(진자 23, 가짜 1)개 있다면
- 계속 반씩 나눠 가벼운 부분을 찾기

### 분할 정복 기법
#### 유래
- 1805년 12월 2일 아우스터리츠 전투에서 나폴레옹이 사용한 전략
- 전력이 우세한 연합군을 공격하기 위해 나폴레옹은 연합군의 중앙부로 쳐들어가 연합군을 둘로 나눔
- 둘로 나뉜 연합군을 한 부분씩 격파

#### 설계 전략
- 분할(Divide): 해결할 문제를 여러 개의 작은 부분으로 나눔
- 정복(Conquer): 나눈 작은 문제를 각각 해결
- 통합(Combine): 필요하다면 해결된 해답을 모음

#### Top-down approach 예시     
![APS01](./asset/APS01.PNG)

### 분할 정복 기법 예시
#### 거듭 제곱
- 반복 (Iterative) 알고리즘: O(n)
- C^2 = C * C
- C^3 = C * C * C
- C^n = C * C * C * ... * C
```python
def Iterative_Power(x, n):
    result = 1
    for i in range(1, n + 1):
        result *= x
    return result
```
- 분할 정복 기반 알고리즘: O(log2(n))
- C^8 = C * C * C * C * C * C * C * C
- C^8 = C^4 * C^4 = (C^4)^2 = ((C^2)^2)^2
- C^n = C^((n-1)/2) * C^((n-1)/2) * C
    - = (C^((n-1)/2))^2 * C
```python
def Recursive_Power(x, n):
    if n == 1:
        return x
    if n % 2 == 0:
        y = Recursive_Power(x, n//2)
        return y * y
    else:
        y = Recursive_Power(x, (n-1)//2)
        return y * y * x
```

## 병합 정렬(Merge sort)
- 여러 개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식
- 분할 정복 알고리즘 활용
    - 자료를 최소 단위의 문제까지 나눈 후에 차례대로 정렬하여 최종 결과를 얻어냄
    - top-down 방식
- 시간 복잡도: O(nlogn)

### 병합 정렬 과정
- [69, 10, 30, 2, 16, 8, 31, 22]를 병합 정렬하는 과정

#### 분할 단계
- 전체 자료 집합에 대해 **최소 크기의 부분집합이 될 때까지** 분할 작업을 계속함      
![APS02](./asset/APS02.PNG)
```python
def merge_sort(arr):
    if len(arr) == 1:
        return arr
    left = []
    right = []
    middle = len(arr) // 2

    for x in range(middle):
        left.append(arr[x])
    for y in range(middle, len(arr)):
        right.append(arr[y])
    
    left = merge_sort(left)
    right = merge_sort(right)

    return merge(left, right)
```

#### 병합 단계
- 2개의 부분집합을 정렬하면서 하나의 집합으로 병합
- 8개의 부분집합이 1개로 병합될 때까지 반복         
![APS03](./asset/APS03.PNG)
```python
def merg(left, right):
    result = []

    while len(left) > 0 or len(right) > 0:
        if len(left) > 0 and len(right) > 0:
            if left[0] <= right[0]:
                result.append(left.popleft())
            else:
                result.append(right.popleft())
        elif len(left) > > 0:
            result.append(left.popleft())
        elif len(right) > 0:
            result.append(right.popleft())
    return result
```

## 퀵 정렬
- 주어진 배열을 두개로 분할하고, 각각을 정렬
    - 병합 정렬과 동일??
- 다른 점 1: 병합 정렬은 그냥 두 부분으로 나누는 반면에, 퀵 정렬은 분할할 때, **기준 아이템(pivot item) 중심으로 분할**함
    - 기준보다 작은 것은 왼쪽, 큰 것은 오른쪽에 위치
- 다른 점 2: 각 부분 정렬이 끝난 후, 병합정렬은 **병합**이란 후처리 작업이 필요하지만 퀵 정렬은 필요 없음

### 알고리즘
- 퀵정렬
```python
def quicksort(arr, l, r):
    if l < r:
        s = partition(arr, l, r)
        quicksort(arr, l, s - 1)
        quicksort(arr, s + 1, r)
```
- Hoare-Partition 알고리즘
```python
def partition(arr, l, r):
    p = arr[0]
    i = 0
    j = len(arr) - 1
    while i <= j:
        while i <= j and arr[i] <= p:
            i += 1
        while i <= j and arr[j] >= p:
            j -= 1
        if i < j:
            arr[i], arr[j] = arr[j], arr[i]
    arr[l], arr[j] = arr[j], arr[l]
    return j
```

### 아이디어
- P(피봇) 값들보다 큰 값은 오른쪽, 작은 값은 왼쪽 집합에 위치하도록 함
![APS04](./asset/APS04.PNG)
- 피봇을 두 집합의 가운데에 위치시킴
![APS05](./asset/APS05.PNG)

### 피봇 선택
- 왼쪽 끝/오른쪽 끝/임의의 세 값 중에 중간 값               
![APS06](./asset/APS06.PNG)
![APS07](./asset/APS07.PNG)
![APS08](./asset/APS08.PNG)
![APS09](./asset/APS09.PNG)
![APS10](./asset/APS10.PNG)
![APS11](./asset/APS11.PNG)

### Lomuto partition 알고리즘
```python
def aprtition(arr, p, r):
    x = arr[r]
    i = p - 1
    for j in range(p, r - 1):
        if arr[j] <= x:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[r] = arr[r], arr[i + 1]
    return i + 1
```
![APS12](./asset/APS12.PNG)

### 연습 문제 1
- 배열의 데이터를 퀵 정렬하는 함수를 작성하고 테스트 해보기
- 입력
    - 11, 45, 23, 81, 28, 34
    - 11, 45, 22, 81, 23, 34, 99, 22, 17, 8
    - 1, 1, 1, 1, 1, 0, 0, 0, 0, 0

## 이진 검색
### 문제: 병뚜껑 속의 숫자 게임
- 술래가 병뚜껑 속 숫자를 확인 후 다음 사람부터 숫자를 맞히기 시작
- 술래는 up 또는 down을 통해 게임에 참여한 사람들이 병뚜껑 속 숫자에 가까워질 수 있도록 힌트 제시
- 이 게임은 숫자를 맞히는게 아니라 피하는게 핵심
- 최대한 빨리 당첨되려면 어떻게 함?
- 반으로 계속 나눠

### 이진 검색
- 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법
- 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여가면서 보다 빠르게 검색은 수행
- 이진 검색을 하기 위해서는 자료가 정렬된 상태여야 함

### 검색 과정
1. 자료의 중앙에 있는 원소를 고름
2. 중앙 원소의 값과 찾고자 하는 목표 값을 비교
3. 목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해 새로 검색을 수행하고, 크다면 자료의 오른쪽 반에 대해 새로 검색 수행
4. 찾고자 하는 값을 찾을 때까지 1 ~ 3의 과정을 반복

### 알고리즘
#### 반복 구조
```python
def binarySearch(n, arr, key):
    low = 0
    high = n - 1
    
    while low <= high:
        mid = (low + high) // 2

        if arr[mid] == key:
            return mid
        elif arr[mid] > key:
            high = mid - 1
        else:
            low = mid + 1
    return -1
```

#### 재귀 구조
```python
def binarySearch(arr, low, high, key):
    if low > high:
        return -1
    else:
        mid = (low + high) // 2

        if key == arr[mid]:
            return mid
        elif key < arr[mid]:
            return binarySearch(arr, low, mid - 1, key)
        else:
            return binarySearch(arr, mid + 1, high, key)
```

### 예시
```python
# 이진 검색
arr = [324, 32, 22114, 16, 48, 93, 422, 21, 316]

# 1. 정렬된 상태의 데이터
arr.sort()

# 2. 이진 검색: 반복문
def binarysearch_1(target):
    # 제일 왼쪽, 오른쪽 인덱스 구하기
    low = 0
    high = len(arr) - 1

    # 해당 숫자 찾으면 종료, 더 이상 쪼갤 수 없을 때까지 반복
    while low <= high:
        mid = (low + high) // 2

        # 가운데 숫자가 정답이면 종료
        if arr[mid] == target:
            return mid
        elif arr[mid] > target:
            high = mid - 1
        elif arr[mid] < target:
            low = mid + 1
    # 못찾으면 -1 반환
    return -1

# 3. 이진 검색: 재귀
def binarySearch_2(low, high, target):
    if low > high:
        return -1
    mid = (low + high) // 2
    if target == arr[mid]:
        return mid
    
    if target < arr[mid]:
        return binarySearch_2(low, mid - 1, target)
    else:
        return binarySearch_2(mid + 1, high, target)
```

## 정리
- 병합 정렬
    - 외부 정렬의 기본이 되는 정렬 알고리즘
    - 멀티 코어(Multi-Core) CPU나 다수의 프로세서에서 정렬 알고리즘을 병렬화하기 위해 병합 정렬 알고리즘이 활용됨
- 퀵 정렬은 매우 큰 입력 데이터에 대해서 좋은 성능을 보이는 알고리즘
