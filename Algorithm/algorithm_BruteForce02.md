# Brute-force
## 부분집합
- 집합에 포함된 원소들을 선택하는 것
- 부분 집합 예시
    - 표는 집합 {A, B, C}로 만들 수 있는 부분 집합의 예시
    - 부분집합에는 아무것도 선택하지 않은 경우도 집합에 포함됨 (공집합)     
![Subset03](./asset/Subset03.PNG)
- 집합에서 부분집합을 찾아내는 구현 방법
1. 완전 탐색
    - 재귀호출을 이용한 완전 탐색으로, 부분집합을 구할 수 있음
    - 실전보다는 완전 탐색 학습용으로 추천하는 방법
2. Binary Counting
    - 2진수 & 비트연산을 이용하여 부분집합을 구할 수 있음
    - 부분집합이 필요할 때 사용하는 추천 방법

### 완전탐색으로 부분집합 구하기
- 민철이에게는 세명의 친구가 있음
    - {MIN, CO, TIM}
- 함께 영화관에 갈 수 있는 멤버를 구성하고자 함
- 모든 경우를 출력해보자        
![Subset04](./asset/Subset04.PNG)
- 구현 방법
    - Branch: 2개
    - Level: 3개
```python
arr = ['O', 'X']
path = []
name = ['MIN', 'CO', 'TIM']

def print_name():
    print('{'. end = '')
    for i in range(3):
        if path[i] == 'O':
            print(name[i], end = ' ')
    print('}')

def run(lev):
    if lev == 3:
        print_name()
        return
    
    for i in range(2):
        path.append(arr[i])
        run(lev + 1)
        path.pop()

run(0)
```

### Binary Counting
- 원소 수에 해당하는 N개의 비트열을 이용
- 0 0 1이면 {A}임을 나타냄
- 1 1 0이면 {B, C}임을 나타냄       
![Subset05](./asset/Subset05.PNG)
- 집합의 총 개수
    - 만들 수 있는 집합의 총 개수는 2^n이며 n = 3이기 때문에 총 8개의 집합
    - 2^n은 1 << n 공식을 이용하여 빠르게 구할 수 있음
```python
print(pow(2, 3))        # 8
print(1 << 3)           # 8
```
- 0b110이 주어지면, BC 출력하는 함수
    - 6 (0b110)에서 비트연산을 이용하여 마지막 한 자리가 1인지 0인지 검사함
```python
arr = ['A', 'B', 'C']
n =len(arr)

def get_sub(tar):
    for i in range(n):
        if tar & 0x1:
            print(arr[i], end = '')
        tar >>= 1

get_sub(6)              # BC
```
- 완성된 부분집합 코드
```python
arr = ['A', 'B', 'C']
n =len(arr)

def get_sub(tar):
    for i in range(n):
        if tar & 0x1:
            print(arr[i], end = '')
        tar >>= 1

for tar in range(1 << n):
    print('{', end = '')
    get_sub(tar)
    print('}')
```

#### [도전] 친구와 카페 방문
- 민철이는 친구 {A, B, C, D, E}가 있음
- 이 중 최소 2명 이상의 친구를 선정하여 함께 카페에 가려 할 경우 몇가지 가능?   
```python
arr = ['A', 'B', 'C', 'D', 'E']
n = len(arr)

def det_count(tar):
    cnt = 0
    for i in range(n):
        # 1비트 1인지 확인
        if tar & 0x1:
            cnt += 1
        # right shift 비트 연산자 -> 오른쪽 끝 비트를 하나씩 제거
        tar >>= 1
    return cnt

result = 0
for tar in range(1 << n):
    if get_count(tar) >= 2:
        result += 1

print(result)
```

## 조합
- 서로 다른 n개의 원소 중 r개를 순서 없이 골라낸 것을 조합(combination)이라고 부름
- 순열과 조합의 차이
    - 순열: {A, B, C, D, E} 5명 중 1등, 2등, 3등 뽑기
    - A B C 와 C B A 는 다른 경우임
    - 조합: 5명 중 3명 뽑기
    - A B C 와 C B A 는 같은 경우임

### 조합 구현하기
- 5명 중 3명 뽑는 조합은 3중 for문으로 구현이 가능함
```python
arr = ['A', 'B', 'C', 'D', 'E']

for a in range(5):
    start1 = a + 1
    for b in range(start1, 5):
        start2 = b + 1
        for c in range(start2, 5):
            print(arr[a], arr[b], arr[c])

```
- 만약 5명 중 n명을 뽑는 코드는 몇 중 for문이 필요할까?
    - n중 for로 구현이 가능함
    - 즉 재귀호출 구현이 필요
    - Branch: 최대 5개
    - Level: n
```python
arr = ['A', 'B', 'C', 'D', 'E']
path = []

n = 3
def run(lev, start):
    if lev == n:
        print(path)
        return
    
    for i in range(start, 5):
        # 처음 run 함수의 start 값은 0임. 따라서 0 ~ 5까지 반복하면서 재귀 호출을 진행
        path.append(arr[i])
        run(lev + 1, i + 1)
        # 만약 i가 3이 선택되는 경우는 재귀 호출할 때 start는 4가 됨. 다음 for문은 4부터 수행
        path.pop()

run(0, 0)
```

#### [도전] 주사위 던지기
- 주사위 눈금 N개를 던져서 나올 수 있는 모든 조합을 출력하라
```python
N = 3
path = []

def func(lev, start):
    if lev == N:
        print(path)
        return
    
    for i in range(start, 7):
        path.append(i)
        func(len + 1, i)
        path.pop()
func(0, 1)
```

## Greedy
- 결정이 필요할 때, 현재 기준으로 가장 좋아보이는 선택지로 결정하여 답을 도축하는 알고리즘

### 대표적인 문제해결 기법
1. 완전탐색 (Brute-force)
    - 답이 될 수 있는 모든 경우를 시도해보는 알고리즘
2. Greedy
    - 결정이 필요할 때 가장 좋아보이는 선택지로 결정하는 알고리즘
3. DP
    - 현재에서 가장 좋아보이는 것을 선택하는 것이 아님
    - 과거의 데이터를 이용하여, 현재의 데이터를 만들어내는 문제해결 기법
4. 분할 정복
    - 큰 문제를 작은 문제로 나누어 해결하는 문제해결 기법

### 동전 교환 문제
- 10, 50, 100, 500원 짜리 동전이 있음
- 손님의 돈을 최소한의 동전 수를 사용해 교환해 주려 함
- 1730원 거슬러 주기 위해 필요한 최소 동전 개수?
```python
coin_list = [500, 100, 50, 10]
tar = 1730

cnt = 0
for coin in coin_list:
    possible_cnt = tar // coin
    cnt += possible_cnt
    tar -= coin * possible_cnt

print(cnt)
```
- 그리디 알고리즘으로 접근 시 다음의 케이스에서 예외 발생(10, 50, 70)
- 100원 거슬러줘야함
- 그리디로 접근시 동전 갯수 = 4개
- 정답 = 2개
- 모든 동전이 배수 관계인 경우는 그리디가 성립하지만 아닌 경우는 성립하지 않음

### 화장실 문제
- 하나의 화장실만 존재, A, B, C, D 학생들의 각자 평균 화장실 사용 시간은 다음과 같음
    - A: 15분
    - B: 30분
    - C: 50분
    - D: 10분
```python
person = [15, 30, 50, 10]
n = len(person)
person.sort()
sum = 0
left_person = n - 1

for turn in range(n):
    time = person[turn]
    # 누적합 += 남은사람 * 시간
    sum += left_person * time
    left_person -= 1

print(sum)
```

### 0-1 Knapsack
- 도둑은 보물들이 있는 창고에 침입하였음
- 도둑은 최대 30kg까지 짐을 담아갈 수 있음
- 물건의 개수 N, 물건 별 무게 W와 가격 P가 주어질 때, 어떤 물건을 담아야 도둑이 최대 이득을 보는가
    - 5kg: 50만원
    - 10kg: 60만원
    - 20kg: 140만원
- kg당 가치가 가장 높은 것을 먼저 담으면 안됨
    - 그리디로 접근하면 안되는 예외 케이스가 존재
    - 완전 탐색 혹은 DP로 접근해야함

### Fractional Knapsack
- 0-1 Knapsack과는 달리 물건을 원하는 만큼 자를 수 있음
    - 5kg: 50만원: 10만원/kg
    - 10kg: 60만원: 6만원/kg
    - 20kg: 140만원: 7만원/kg
- 그리디가 성립
- kg 당 가격이 가장 높은 물건을 최대한 담으면 됨
- 최대 수익
    - 가장 kg당 금액이 높은 물건 전체 사용 (50만원)
    - +두번째로 kg당 금액이 높은 물건 전체 사용 (140만원)
    - +세번째로 kg당 금액이 높은 물건 나머지 사용 (30만원)
    - = 220만원
```python
n = 3
target = 30
things = [(5, 50), (10, 60), (20, 140)]

things.sort(key = lambda x: (x[1] / x[0]), reverse = True)

sum = 0
for kg, price in things:
    per_price = price / kg

    if target < kg:
        sum += target * per_price
        break

    sum += price
    target -= kg

print(int(sum))
```

### 회의실 배정
- 회의실이 하나인 회사
- 여러 팀들이 원하는 회의실 예약 시간이 주어질 경우
- 가능한 많은 회의가 열리기 위해서는 회의를 어떻게 배정해야 하는가
- 회의 종료 시간이 가장 빠른 회의를 먼저 선택하면 됨
- 다른 예시
    - 총 10개의회의 요청이 존재
    - (5, 9), (6, 10), (8, 11), (1, 4), (3, 5), (1, 6), (5, 7), (3, 8), (2, 13), (12, 14)
    1. 위를 종료 시간 기준으로 오름차순으로 정렬
    - (1, 4), (3, 5), (1, 6), (5, 7), (3, 8), (5, 9), (6, 10), (8, 11), (2, 13), (12, 14)
    2. 빠르게 끝나는 회의를 선택해 확정함
    3. 이후로 가능한 회의 중, 빠르게 끝나는 회의를 선택하여 확정

## Review
### [연습문제 3] 부분집합의 합
- 아래의 10개의 정수 집합에 대한 모든 부분 집합 중 원소의 합이 0이 되는 부분 집합을 모두 출력하시오
- {-1, -3, -9, 6, 7, -6, 1, 5, 4, -2}
