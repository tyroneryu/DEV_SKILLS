# Queue
## Queue의 특성
- 스택과 마찬가지로 삽입과 삭제의 위치가 제한적인 자료구조
- 큐의 뒤에서는 삽입만 하고, 큐의 앞에서는 삭제만 이루어지는 구조
- 선입선출 구조(FIFO: First In First Out)
- 큐에 삽입한 순서대로 원소가 저장되어, 가장 먼저 삽입된 원소는 가장 먼저 삭제됨
- 큐의 선입선출 구조
![Queue01](./asset/Queue01.PNG)
- 큐의 기본 연산
    - 삽입: enQueue
    - 삭제: deQueue

### 큐의 사용을 위해 필요한 주요 연산
|연산|기능|
|:---:|:---:|
|enQueue(item)|큐의 뒤쪽(rear 다음)에 원소를 삽입하는 연산|
|deQueue()|큐의 앞쪽(front)에서 원소를 삭제하고 반환하는 연산|
|createQueue()|공백 상태의 큐를 생성하는 연산|
|isEmpty()|큐가 공백 상태인지를 확인하는 연산|
|isFull()|큐가 포화 상태인지를 확인하는 연산|
|Qpeek()|큐의 앞쪽(front)에서 원소를 삭제없이 반환하는 연산|

### 큐의 연산 과정
1. 공백 큐 생성: crateQueue()
2. 원소 A 삽입: enQueue(A)
3. 원소 B 삽입: enQueue(B)
![Queue02](./asset/Queue02.PNG)
4. 원소 반환 / 삭제: deQueue()
5. 원소 C 삽입: enQueue(C)
6. 원소 반환 / 삭제: deQueue()
7. 원소 반환 / 삭제: deQueue()
![Queue03](./asset/Queue03.PNG)

## 큐의 구현
### 선형큐
- 1차원 배열을 이용한 큐
    - 큐의 크기 = 배열의 크기
    - front: 마지막으로 삭제된 인덱스
    - rear: 저장된 마지막 원소의 인덱스
- 상태 표현
    - 초기 상태: front = rear = -1
    - 공백 상태: front == rear
    - 포화 상태: rear == n - 1(n: 배열의 크기, n - 1: 배열의 마지막 인덱스)
- 초기 공백 큐 생성
    - 크기 n인 1차원 배열 생성
    - front와 rear를 -1로 초기화

### 삽입: enQueue(item)
- 마지막 원소 뒤에 새로운 원소를 삽입하기 위해
    1. rear 값을 하나 증가시켜 새로운 원소를 삽입할 자리를 마련
    2. 그 인덱스에 해당하는 배열원소 Q[rear] 에 item을 저장
```python
def enQueue(item):
    global rear
    if isFull(): print('Queue_Full')
    else:
        rear += 1
        Q[rear] = item
```

### 삭제: deQueue()
- 가장 앞에 있는 원소를 삭제하기 위해
    1. front 값을 하나 증가시켜 큐에 남아있는 첫번째 원소 이동
    2. 새로운 첫번째 원소를 리턴함으로써 삭제와 동일한 기능함
```python
def deQueue():
    if (isEmpty()) then Queue_Empty()
    else:
        front += 1
        return Q[front]
```

### 공백 상태 및 포화 상태 검사: isEmpty(), isFull()
- 공백 상태: front == rear
- 포화 상태: rear == n - 1(n: 배열의 크기, n - 1: 배열의 마지막 인덱스)
```python
def isEmpty():
    return front == rear

def isFull():
    return rear == len(Q) - 1
```

### 검색: Qpeek()
- 가장 앞에 있는 원소를 검색하여 반환하는 연산
- 현재 front의 한 자리 뒤(front + 1)에 있는 원소, 즉 큐의 첫번째에 있는 원소를 반환
```python
def Qpeek():
    if isEmpty():
        print('Queue_Empty')
    else:
        return Q[front + 1]
```

### 연습문제1
- 큐를 구현하여 다음 동작을 확인해 보자
- 세 개의 데이터 1, 2, 3을 차례로 큐에 삽입하고
- 큐에서 세 개의 데이터를 차례로 꺼내 출력
    - 1, 2, 3이 출력되어야 함
```python
N = 10
q = [0] * 10
front = rear = -1

rear += 1       # enqueue(1)
q[rear] = 1
rear += 1       # enqueue(2)
q[rear] = 2
rear += 1       # enqueue(3)
q[rear] = 3

while fron != rear:
    front += 1  # dequeue()
    print(q[front])
```

### 선형 큐 이용시 문제점
- 잘못된 포화상태 인식
    - 선형 큐를 이용하여 원소의 삽입과 삭제를 계속할 경우, 배열의 앞부분에 활용할 수 있는 공간이 있음에도 불구하고, rear = n - 1 인 상태 즉, 포화 상태로 인식하여 더 이상의 삽입을 수행하지 않게 됨
    ![wrong_queue01](./asset/wrong_queue01.PNG)
- 해결 방법 1
    - 매 연산이 이루어질 때마다 저장된 원소들을 배열의 앞부분으로 모두 이동시킴
    - 원소 이동에 많은 시간이 소요되어 큐의 효율성이 급격히 떨어짐
    ![solution_queue01](./asset/solution_queue01.PNG)
- 해결 방법 2
    - 1차원 배열을 사용하되, 논리적으로는 배열의 처음과 끝이 연결되어 원형 형태의 큐를 이룬다고 가정하고 사용
- 원형 큐의 논리적 구조
![wonqueue01](./asset/wonqueue01.PNG)

### 원형큐
- 초기 공백 상태: front = rear = 0
- Index의 순환
    - front와 rear의 위치가 배열의 마지막 인덱스인 n - 1을 가리킨 후, 그 다음에는 논리적 순환을 이루어 배열의 처음 인덱스인 0으로 이동해야 함
    - 이를 위해 나머지 연산자 mod를 사용함
- front 변수
    - 공백 상태와 포화 상태 구분을 쉽게 하기 위해 front가 있는 자리는 사용하지 않고 항상 빈자리로 둠
- 삽입 위치 및 삭제 위치

||삽입 위치|삭제 위치|
|:---:|:---:|:---:|
|선형큐|rear = rear + 1|front = front + 1|
|원형큐|rear = (rear + 1) mod n|front = (front + 1) mod n|

### 원형큐의 연산 과정
1. create Queue
2. enQueue(A)
![wonqueue02](./asset/wonqueue02.PNG)
3. enQueue(B)
4. deQueue()
![wonqueue03](./asset/wonqueue03.PNG)
5. enQueue(C)
6. enQueue(D)
![wonqueue04](./asset/wonqueue04.PNG)

### 원형 큐의 구현
- 초기 공백 큐 생성
    - 크기 n인 1차원 배열 생성
    - front와 rear를 0으로 초기화

### 공백 상태 및 포화 상태 검사: isEmpty(), isFull()
- 공백 상태: front == rear
- 포화 상태: 삽입할 rear의 다음 위치 == 현재 front
    - (rear + 1) mod n == front
```python
def isEmpty():
    return front == rear

def isFull():
    return (rear + 1) % len(cQ) == front
```

### 삽입: enQueue(item)
- 마지막 원소 뒤에 새로운 원소를 삽입하기 위해
    1. rear 값을 조정하여 새로운 원소를 삽입할 자리를 마련함:
    rear = (rear + 1) mod n
    2. 그 인덱스에 해당하는 배열 원소 cQ[rear]에 item을 저장
```python
def enQueue(item):
    global rear
    if isFull():
        print('Queue_Full')
    else:
        rear = (rear + 1) % len(cQ)
        cQ[rear] = item
```

### 삭제: deQueue(), delete()
- 가장 앞에 있는 원소를 삭제하기 위해
    1. front 값을 조정하여 삭제할 자리를 준비함
    2. 새로운 front 원소를 리턴함으로써 삭제와 동일한 기능함
```python
def deQueue():
    global front
    if isEmpty():
        print('Queue_Empty')
    else:
        front = (front + 1) % len(cQ)
        return cQ[front]
```

## 연결큐
### 연결큐의 구조
- 단순 연결 리스트(Linked List)를 이용한 큐
    - 큐의 원소: 단순 연결 리스트의 노드
    - 큐의 원소 순서: 노드의 연결 순서. 링크로 연결되어 있음
    - front: 첫번째 노드를 가리키는 링크
    - rear: 마지막 노드를 가리키는 링크
- 상태 표현
    - 초기 상태: front = rear = null
    - 공백 상태: front = rear = null
![Linkedqueue01](./asset/Linkedqueue01.PNG)

### 연결큐의 연산 과정
1. 공백 큐 생성: createLinkedQueue()
2. 원소 A 삽입: enQueue(A)
3. 원소 B 삽입: enQueue(B)      
![Linkedqueue02](./asset/Linkedqueue02.PNG)
4. 원소 삭제: deQueue()     
![Linkedqueue03](./asset/Linkedqueue03.PNG)
5. 원소 C 삽입: enQueue(C)      
![Linkedqueue04](./asset/Linkedqueue04.PNG)
6. 원소 삭제: deQueue()     
![Linkedqueue05](./asset/Linkedqueue05.PNG)
7. 원소 삭제: deQueue()     
![Linkedqueue06](./asset/Linkedqueue06.PNG)

## [참고] deque(덱)
- 컨테이너 자료형 중 하나
- deque 객체
    - 양쪽 끝에서 빠르게 추가와 삭제를 할 수 있는 리스트유 컨테이너
- 연산
    - append(x): 오른쪽에 x 추가
    - popleft(): 왼쪽에서 요소를 제거하고 반환, 요소가 없으면 IndexError
```python
from collections import deque

q = deque()
q.append(1)     # enqueue(1)
t = q.popleft() # dequeue()
```

```python
from collections import deque

q = []
for i in range(100000):
    q.append(i)
print('append')
while q:
    q.pop(0)
print('end')

dq = deque()
for i in range(200000):
    dq.append(i)
print('append')
while dq:
    dq.popleft()
print('end')
```

## 우선순위 큐(Priority Queue)
- 우선순위 큐의 특성
    - 우선순위를 가진 항목들을 저장하는 큐
    - FIFO 순서가 아니라 우선순위가 높은 순서대로 먼저 나가게 됨
- 우선순위 큐의 적용 분야
    - 시뮬레이션 시스템
    - 네트워크 트래픽 제어
    - 운영체제의 테스크 스케줄링
- 우선순위 큐의 구현
    - 배열을 이용한 우선순위 큐
    - 리스트를 이용한 우선순위 큐
- 우선순위 큐의 기본 연산
    - 삽입: enQueue
    - 삭제: deQueue
![PriorityQueue01](./asset/PriorityQueue01.PNG)

### 배열을 이용한 우선순위 큐
- 배열을 이용한 우선순위 큐 구현
    - 배열을 이용하여 자료 저장
    - 원소를 삽입하는 과정에서 우선순위를 비교하여 적절한 위치에 삽입하는 구조
    - 가장 앞에 최고 우선순위의 원소가 위치하게 됨
- 문제점
    - 배열을 사용하므로, 삽입이나 삭제 연산이 일어날 때 원소의 재배치가 발생
    - 이에 소요되는 시간이나 메모리 낭비가 큼

## 버퍼(Buffer)
- 데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역
- 버퍼링: 버퍼를 활용하는 방식 또는 버퍼를 채우는 동작을 의미함
- 버퍼의 자료 구조
    - 버퍼는 일반적으로 입출력 및 네트워크와 관련된 기능에서 이용됨
    - 순서대로 입력/출력/전달되어야 하므로 FIFO 방식의 자료구조인 큐가 활용됨

### 마이쮸
- Queue를 이용하여 마이쮸 나눠주기 시뮬레이션을 해보자
- 1번이 줄을 섬
- 1번이 한개의 마이쮸를 받음
- 1번이 다시 줄을 섬
- 새로 2번이 들어와 줄을 섬
- 1번이 두개의 마이쮸를 받음
- 1번 다시 줄을 섬
- 새로 3번이 들어와 줄을 섬
- 2번이 한 개의 마이쮸를 받음
- 2번이 다시 줄을 섬
- 새로 4번이 들어와 줄을 섬
- 20개의 마이쮸가 있을 때 마지막 것을 누가 가져갈까?
![mychew01](./asset/mychew01.PNG)
![mychew02](./asset/mychew02.PNG)
![mychew03](./asset/mychew03.PNG)
![mychew04](./asset/mychew04.PNG)
