# Brute force
## 학습목표
- 재귀적 알고리즘의 특성을 이해하고 이를 구현하기 위한 재귀 호출에 대해 학습함
- 완전 검색의 개념을 이해하고 완전 검색을 통한 문제 해결 방법에 대해 학습함
- 조합적 문제 (Combinatorial Problems)에 대한 완전 검색 방법에 대해 이해함
    - 순열, 조합, 부분집합을 생성하는 알고리즘을 학습함
- 탐욕 알고리즘 기법의 개념과 주요 특성을 이해함

## 반복(Iteration)과 재귀(Recursion)
- 반복과 재귀는 유사한 작업을 수행할 수 있음
- 반복은 수행하는 작업이 완료될 때까지 계속 반복
    - 루프 (for, while 구조)
    - 반복문은 코드를 n번 반복시킬 수 있음
- 재귀는 주어진 문제의 해를 구하기 위해 동일하면서 더 작은 문제의 해를 이용하는 방법
    - 하나의 큰 문제를 해결할 수 있는 (해결하기 쉬운) 더 작은 문제로 쪼개고 결과들을 결합함
    - 재귀 호출은 n중 반복문을 만들어낼 수 있음

### [도전] 1111 ~ 3333 까지 출력하는 코드
- 1111 ~ 3333까지 출력하는 코드를 작성하시오
```python
for a in range(1, 4):
    for b in range(1, 4):
        for c in range(1, 4):
            for d in range(1, 4):
                print(a, b, c, d)
```
- 중복 순열
- 숫자의 갯수가 많아지면 for문으로 구현 불가
- 재귀 호출로 구현할 수 있음
```python
path = []
N = 3

def run(lev):
    if lev == N:
        print(path)
        return
    
    for i in range(1, 4):
        path.append(i)
        run(lev + 1)
        path.pop()

run(0)
```

### 재귀를 연습하기 전 알아야 할 함수의 특징 1
- KFC 함수 호출할 때, int 타입 객체를 전달하면 값만 복사됨
- 아래 코드의 실행 결과는?
```python
def KFC(x):
    print(x)
    x += 1
    print(x)

x = 3
KFC(x + 1)
print(X)
```

### 재귀를 연습하기 전 알아야 할 함수의 특징 2
- 함수가 끝나면, Main으로 되돌아오는 것이 아니라, 해당 함수를 호출했던 곳으로 돌아옴
- 아래 코드의 실행 결과는?
```python
def BBQ(x):
    x += 10
    print(x)

def KFC(x):
    print(x)
    x += 3
    BBQ(x + 2)
    print(x)

x = 3
KFC(x + 1)
print(x)
```
- 재귀 호출 공부의 시작은 무한 재귀 호출을 막는 것부터 시작함
    - **기저 조건(base case)**을 잘 걸어야 함

### [도전] 0 1 2 3 4 5 5 4 3 2 1 0 재귀로 호출
- 0 1 2 3 4 5 5 4 3 2 1 0을 재귀 호출을 이용해 구현
```python
def KFC(x):
    if x == 6:
        return
    print(x. end = ' ')
    KFC(x + 1)
    print(x, end = ' ')

KFC(0)
```

### Branch
- 재귀 호출의 갯수만큼 가지 수가 늘어남
```python
def KFC(x):
    if x == 2:
        return
    KFC(x + 1)
    KFC(x + 1)
    print(x)

KFC(0)
```     
![Recursion01](./asset/Recursion01.PNG)
```python
def KFC(x):
    if x == 2:
        return
    KFC(x + 1)
    KFC(x + 1)
    KFC(x + 1)
    print(x)

KFC(0)
```     
![Recursion02](./asset/Recursion02.PNG)

- 깊이를 Level이라고 부를 수 있음
- Level은 3, Branch(나뭇가지)는 4인 형태인 코드를 아래와 같이 표현 가능
```python
def KFC(x):
    if x == 3:
        return
    
    KFC(x + 1)
    KFC(x + 1)
    KFC(x + 1)
    KFC(x + 1)

KFC(0)
```
```python
def KFC(x):
    if x == 3:
        return

    for i in range(4):
        KFC(x + 1)

KFC(0)
```
- 재귀 호출을 이용한 코딩 기법을 익히기 위해서는 먼저 위 코드의 원리를 이해해야함
- 여기까지 지식으로 **순열**을 학습할 준비를 마쳤음

### [도전] 트리 형태를 보고, 직접 재귀 호출 코드 구현하기
- Branch? Level? 파악 후, 직접 구현     
![Recursion03](./asset/Recursion03.PNG)

## 순열(Permutation)
- 서로 다른 N개에서, R개를 중복 없이, 순서를 고려하여 나열하는 것
- 예)
    - [0] [1] [2]로 구성된 3장의 카드들이 다량으로 존재
    - 이 중에서 2장을 뽑아, 순열을 나열하라
    - `[0 1] [0 2] [1 0] [1 2] [2 0] [2 1]`

### 중복 순열
- 서로 다른 N개에서 R개를 중복을 허용하고, 순서를 고려하여 나열하는 것
- 예)
    - [0] [1] [2]로 구성된 3장의 카드들이 다량으로 존재
    - 이 중에서 2장을 뽑아, 중복 순열을 나열하라
    - `[0 1] [0 1] [0 2] [1 0] [1 1] [1 2] [2 0] [2 1] [2 2]`

#### 중복 순열 구현 원리
1. 재귀 호출을 할 때마다, 이동 경로를 흔적으로 남김
2. 가장 마지막 레벨에 도착했을 때, 이동 경로를 출력함       
![Permutation03](./asset/Permutation03.PNG)
```python
path = []

def KFC(x):
    if x == 2:
        print(path)
        return

    for i in range(3):
        path.append(i)
        KFC(x + 1)
        path.pop()

KFC(0)
```
- 먼저 path라는 전역 리스트를 준비함
- 그리고, Level 2, Branch 3으로 동작되는 재귀 코드를 구현
- 재귀 호출을 하기 전에 이동할 곳의 위치를 path 리스트에 기록
- 재귀 호출 되었고, 다시 `path.append(i)`를 수행
- 두 번 재귀 호출 이후, 바닥에 도착했으니 출력하는 코드를 수행
- 함수가 리턴되고 돌아오자마자 기록을 삭제
- 다시 for문이 이어서 진행되고 값 i는 1이 됨
- 재귀 호출이 되고, path 값을 출력

### [도전] 증복 순열 출력하기
- 중복 순열 [1, 1, 1] ~ [6, 6, 6]까지 출력하는 코드를 재귀호출로 구현하자
```python
path = []

def KFC(x):
    if x == 3:
        print(path)
        return

    for i in range(1, 7):
        path.append(i)
        KFC(x + 1)
        path.pop()

KFC(0)
```

### 중복 허용않는 순열 구현
1. 중복 순열 코드를 작성
2. 중복을 제거하는 코드를 추가
- 중복을 제거하는 원리
    - 전역 리스트를 사용하면 이미 선택했던 숫자인지 아닌지 구분 가능
    - 이를 used 배열 또는 visited 배열이라고 함

- 이미 사용한 숫자인지 아닌지 구분하는 List 준비
    - 전역으로 used라는 리스트를 준비
    - `used = [False, False, False]`        
![Permutation04](./asset/Permutation04.PNG)
```python
path = []
used = [False, False, False]

def KFC(x):
    if x == 2:
        print(path)
        return
    
    for i in range(3):
        if used[i] == True:
            continue
        used[i] = True
        path.append(i)
        KFC(x + 1)
        path.pop()
        used[i] = False

KFC(0)
```
- 만약 이미 사용한 숫자인 경우, 재귀 호출을 생략하는 코드를 추가
- 처음 사용하는 숫자라면 used에 기록을 해줌
- 모든 처리가 끝나고 돌아왔다면 used에 기록을 지워줌

#### [도전] 중복 순열과 순열 구현하기
- n개의 주사위를 던져 나올 수 있는 모든 중복 순열 (Type 1)과 순열 (Type 2)를 출력하시오
- 입력
- `2 1  # N, Type`
    - N은 2이고, Type은 1이므로 중복 순열을 출력
```python
path = []
used = []
N = 0

def type1(x):
    if x == N:
        print(path)
        return

    for i in range(1, 7):
        path.append(i)
        type_1(x + 1)
        path.pop()

def type_2(x):
    if x == N:
        print(path)
        return

    for i in range(1, 7):
        if used i[i] == True:
            continue
        used[i] = True
        path.append(i)
        type_2(x + 1)
        path.pop()
        used[i] = False

used = [False for _ in range(7) ]
N, Type = map(int, input().split())

if Type == 1:
    type1(0)
if Type == 2:
    type2(0)
```

## 완전 탐색(Brute-Force)
- 모든 가능한 경우를 모두 시도를 해봐 정답을 찾아내는 알고리즘

### 완전 탐색 예시
- 자전거 열쇠 비밀번호 맞추기
    - 만약 1111 ~ 9999 네 자리 숫자를 맞춰야 한다면?
    - 4중 for문으로 모두 시도해보기
    - 만약 1 ~ 9까지 이루어진 N자리의 숫자를 맞춰야 한다면?
    - 순열 코드(재귀 호출)로 구현하여, 모두 시도

### 목표
- 완전 탐색 세 문제를 통해 재귀 호출을 이용한 완전 탐색 구현 방법에 대해 학습
1. 주사위 눈금의 합
2. 연속 3장의 트럼프 카드
3. Baby-jin (ver.완전 탐색)

#### 1. 주사위 눈금의 합
- 3개의 주사위를 던져 나올 수 있는 모든 경우에 대해, 합이 10 이하가 나오는 경우는 총 몇가지 인가?
- 먼저 합을 출력하는 코드를 작성
- 재귀 호출을 할 때마다 선택한 값의 누적합을 구하기
```python
path = []

def KFC(x, sum_):
    if x == 3:
        print(f'{path} = {sum_}')
        return
    
    for i in range(1, 7):
        path.append(i)
        KFC(x + 1, sum + i)
        path.pop()

KFC(0, 0)
```
- 파라미터에 sum을 추가하여 구현
    - sum = 지금까지 구한 합
    - i = 선택한 주사위 눈금
    - 재귀호출 할 때 sum + i 값을 전달
```python
path = []

def KFC(x, sum_):
    if x == 3:
        if sum <= 10:
            print(f'{path} = {sum_}')
        return
    
    for i in range(1, 7):
        path.append(i)
        KFC(x + 1, sum + i)
        path.pop()

KFC(0, 0)
```
- sum이 <= 10 이하일 때만 출력
    - 실제로는 모두 탐색하지만, 출력만 하지 않는 방법
- 이 경우는 모든 방법을 탐색하고 합이 10이 넘어가는 수는 출력만 생략
- 6 -> 5 이후는 10보다 크므로 사실상 재귀 호출을 할 필요가 없음

##### 가지치기
- 답이 아닌 것에 대해 즉시 되돌아감
```python
path = []

def KFC(x, sum_):
    if x == 3:
        if sum <= 10:
            print(f'{path} = {sum_}')
        return
    
    for i in range(1, 7):
        path.append(i)
        KFC(x + 1, sum + i)
        path.pop()

KFC(0, 0)
```
- 위 코드는 가지치기가 아님 (출력만 숨기는 것)
```python
path = []

def KFC(x, sum_):
    if sum > 10:
        return

    if x == 3:
        print(f'{path} = {sum_}')
        return
    
    for i in range(1, 7):
        path.append(i)
        KFC(x + 1, sum + i)
        path.pop()

KFC(0, 0)
```
- 위 코드가 가지치기
- 누적합이 10 넘어가는 순간 더 탐색할 필요 없이 return

- 따라서 완성된 코드
```python
path = []
cnt = 0

def KFC(x, sum_):
    global cnt
    if sum > 10:
        return

    if x == 3:
        cnt += 1
        return
    
    for i in range(1, 7):
        path.append(i)
        KFC(x + 1, sum + i)
        path.pop()

KFC(0, 0)
print(cnt)
```
