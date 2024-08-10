# 모듈

## 개요
- 과학자, 수학자가 모든 이론을 새로 만들거나 증명하지 않는 것처럼 개발자 또한 프로그램 전체를 모두 혼자 힘으로 작성하는것은 드문 일
- 다름 프로그래머가 이미 작성해 놓은 수천, 수백만 줄의 코드를 사용하는 것은 생산성에서 매우 중요한 일

## 모듈
- Module
- 한 파일로 묶인 변수와 함수의 모음
- 특정한 기능을 하는 코드가 작성된 파이썬 파일(.py)
- 모듈 예시
    - 파이썬의 math 모듈
    - 파이썬이 미리 작성해 둔 수학 관련 변수와 함수가 작성된 모듈
    ```python
    import math
    print(math.pi)      # 3.141592653589793
    print(math.sqrt(4)) # 2.0
    ```

## 모듈 활용
### 모듈 import
- 모듈 가져오기
- 모듈 내 변수와 함수에 접근하려면 **import** 문이 필요
    `import math`
- 내장 함수 help를 사용해 모듈에 무엇이 들어있는지 확인 가능
```python
help(math)

"""
NAME
    math

DESCRIPTION
    This module provides access to the mathematical functions
    defined by the C standard.

FUNCTIONS
    acos(x, /)
        Return the arc cosine (measured in radians) of x.

        The result is between 0 and pi.
        ...
"""
```
- ' . (dot)'은 "점의 왼쪽 객체에서 점의 오른쪽 이름을 찾아라" 라는 의미의 연산자
```python
# 모듈명.변수명
print(math.pi)

# 모듈명.함수명
print(math.sqrt(4))
```
- 모듈 import하는 다른 방법
    - **from** 절을 활용해 특정 모듈을 미리 참조하고 어떤 요소를 import할지 명시
    ```python
    from math import pi, sqrt   # 권장하지 않는 방법

    print(pi)
    print(sqrt(4))
    ```
- 모듈 주의사항
    - 만약 서로 다른 모듈이 같은 이름의 함수를 제공할 경우 문제 발생
    - 마지막에 import된 이름으로 대체됨
    ```python
    from math import pi, sqrt
    from my_math import sqrt
    # 그래서 모듈 내의 모든요소를 한번에 import하는 *표기는 권장하지 않음
    from math import *
    ```

## 모듈 활용
- 직접 정의한 모듈 사용하기
    1. 모듈 my_math.py 작성
    2. 구 수의 합을 구하는 add 함수 작성
    3. my_math 모듈 import 후 add 함수 호출
    ```python
    # my_math.py
    def add(x, y):
        return x + y
    ```
    ```python
    # onsil.py
    import my_math

    print(my_math.add(2, 3))
    ```

## 파이썬 표준 라이브러리
- Python Standard Library
- 파이썬 언어와 함께 제공되는 다양한 모듈과 패키지의 모음
- [파이썬 표준 라이브러리](https://docs.python.org/ko/3/library/index.html)

### 패키지
- Package
- 관련된 모듈들을 하나의 디렉토리에 모아 놓은 것
- 패키지 사용하기
    - 아래와 같은 디렉토리 구조로 작성
    - 패키지 3개 : my_package, math, statistics
    - 모듈 2개 : my_math, tools  
    my_package  
    L math  
    ㅣ  L my_math.py  
    ㅏstatistics  
        L tools.py  
    ```python
    # my_math.py
    def add(x, y):
        return x + y
    ```
    ```python
    # tools.py
    def mod(x, y):
        return x % y
    ```
    - 각 패키지의 모듈을 import하여 사용하기
    ```python
    # sample.py
    from my_package.math import my_math
    from my_package.statistics import tools

    print(my_math.add(1, 2))    # 3
    print(tools.mod(1, 2))      # 1
    ```
- PSL 내부 패키지
    - 설치 없이 바로 import하여 사용
- 외부 패키지
    - **pip**를 사용하여 설치 후 import 필요

### pip
- 외부 패키지들을 설치하도록 도와주는 파이썬의 패키지 관리 시스템
- 파이썬 패키지 관리자(pip)
- PYPI(Python Package Index)에 저장된 외부 패키지들을 설치
    - [pip](https://pypi.org/)
- 패키지 설치
    - 최신 버전 / 특정 버전 / 최소 버전을 명시하여 설치할 수 있음
    ```python
    $ pip install SomePackage
    $ pip install SomePackage==1.0.5
    $ pip install SomePackage>=1.0.4
    ```
    - requests 외부 패키지 설치 및 사용 예시
    ```python
    $ pip install requests
    import requests

    url = 'https://random-data-api.com/api/v2/users'
    response = requests.get(url).json()

    print(response)
    ```

### 패키지 사용 목적
- 모듈들의 이름공간을 구분하여 충돌을 방지
- 모듈들을 효율적으로 관리하고 재사용할 수 있도록 돕는 역할

# 제어문
- Control Statement
- 코드의 실행 흐름을 제어하는 데 사용되는 구문
- **조건**에 따라 코드 블록을 실행하거나 **반복**적으로 코드를 실행

## 조건문
- Conditional Statement
- 주어진 조건식을 평가하여 해당 조건이 참(True)인 경우에만 코드 블록을 실행하거나 건너뜀
- if / elif / else
    - 파이썬 조건문에 사용되는 키워드
- if statement의 기본 구조
```python
if 표현식:
    코드 블록
elif 표현식:
    코드 블록
else:
    코드 블록
```
- 복수 조건문 예시
    - 조건식을 동시에 검사하는것이 아니라 순차적으로 비교
    ```python
    dust = 35

    if dust > 150:
        print('매우 나쁨')
    elif dust > 80:
        print('나쁨')
    elif dust > 30:
        print('보통')
    else:
        print('좋음')
    ```
- 중첩 조건문 예시
```python
dust = 480

if dust > 150:
    print('매우 나쁨')
    if dust > 300:
        print('위험해요! 나가지 마세요')
elif dust > 80:
    print('나쁨')
elif dust > 30:
    print('보통')
else:
    print('좋음')
```

## 반복문
- Loop Statement
- 주어진 코드 블록을 여러 번 반복해서 실행하는 구문
    - 특정 작업을 반복적으로 수행
    - 주어진 조건이 참인 동안 반복해서 실행
- for / while
    - 파이썬 반복문에 사용되는 키워드

### 'for' statement
- for
- 임의의 시퀀스의 항목들을 그 시퀀스에 들어있는 순서대로 반복
- for statement의 기본 구조
    ```python
    for 변수 in 반복 가능한 객체:
        코드 블록
    ```
- 반복 가능한 객체
    - iterable
    - 반복문에서 순회할 수 있는 객체
    - 시퀀스 객체 뿐만 아니라 dict, set 등도 포함
- for 문의 원리
    - 리스트 내 첫 항목이 반복 변수에 할당되고 코드 블록이 실행
    - 다음으로 반복 변수에 리스트의 2번째 항목이 할당되고 코드 블록이 다시 실행
    - ... 마지막으로 반복 변수에 리스트의 마지막 요소가 할당되고 코드 블록이 실행
    ```python
    items = ['apple', 'banana', 'coconut']
    for item in items:
        print(item)
    '''
    apple
    banana
    coconut
    '''
    ```
- 문자열 순회
```python
country = 'Korea'
for char in country:
    print(char)
'''
K
o
r
e
a
'''
```
- range 순회
```python
for i in range(5):
    print(i)
'''
0
1
2
3
4
'''
```
- 딕셔너리 순회
```python
my_dict = {
    'x': 10,
    'y': 20,
    'z': 30,
}
for key in my_dict:
    print(key)
    print(my_dict[key])
'''
x
10
y
20
z
30
'''
```
- 인덱스로 리스트 순회
    - 리스트의 요소가 아닌 인덱스로 접근하여 해당 요소들을 변경하기
    ```python
    numbers = [4, 6, 10, -8, 5]
    for i in range(len(numbers)):
        numbers[i] = numbers[i] * 2
    print(numbers)
    # [8, 12, 20, -16, 10]
    ```
- 중첩된 반복문
    - 중첩된 반복문에서의 출력 예상해보기
    ```python
    outers = ['A', 'B']
    inners = ['C', 'D']
    for outer in outers:
        for inner in inners:
            print(outer, inner)
    '''
    A C
    A D
    B C
    B D
    ```
    - 안쪽 반복문은 outers 리스트의 각 항목에 대해 한번씩 실행됨
    - print가 호출되는 횟수 -> **len(outers) * len(inners)**
- 중첩 리스트 순회
    - 안쪽 리스트 요소에 접근하려면 바깥 리스트를 순회하면서 중첩 반복을 사용해 각 안쪽 반복을 순회
    ```python
    elements = [['A', 'B'], ['C', 'D']]
    for element in elements:
        print(elment)
    '''
    ['A', 'B']
    ['C', 'D']
    ```
    ```python
    elements = [['A', 'B'], ['C', 'D']]
    for element in elements:
        for item in element:
            print(item)
    '''
    A
    B
    C
    D
    ```

### 'while' statement
- while
- 주어진 조건식이 참(True)인 동안 코드를 반복해서 실행
- == 조건식이 거짓(False)가 될 때 까지 반복
- while statement의 기본 구조
```python
while 조건식:
    코드 블록
```
- while 반복문 예시
```python
a = 0
while a < 3:
    print(a)
    a += 1
print('끝')
'''
0
1
2
끝
'''
```
- 사용자 입력에 따른 반복
    - while문을 사용한 특정 입력 값에 대한 종료 조건 활용하기
    ```python
    number = int(input('양의 정수를 입력해주세요.: '))

    while number <= 0:
        if number < 0:
            print('음수를 입력했습니다.')
        else:
            print('0은 양의 정수가 아닙니다.')
        
        number = int(input('양의 정수를 입력해주세요.: '))
    print('잘했습니다!')
    '''
    양의 정수를 입력해주세요.: 0
    0은 양의정수가 아닙니다.
    양의 정수를 입력해주세요.: -1
    음수를 입력했습니다.
    양의 정수를 입력해주세요.: 1
    잘했습니다!
    '''
    ```
- while문은 반드시 **종료 조건**이 필요

### 적절한 반복문 활용하기
- for
    - 반복 횟수가 명확하게 정해져 있는 경우에 유용
    - 예를 들어 리스트, 튜플, 문자열 등과 같은 시퀀스 형식의 데이터를 처리할 때
- while
    - 반복 횟수가 불명확하거나 조건에 따라 반복을 종료해야 할 때 유용
    - 예를 들어 사용자의 입력을 받아서 특정 조건이 충족될 때까지 반복하는 경우

## 반복 제어
- for 문과 while은 매 반복마다 본문 내 모든 코드를 실행하지만 때때로 일부만 실행하는 것이 필요할 때가 있음
- break
    - 반복을 즉시 중지
- continue
    - 다음 반복으로 건너뜀
- break 예시
    - 프로그램 종료 조건 만들기
    ```python
    number = int(input('양의 정수를 입력해주세요.: '))

    while number <= 0:
        if number == -9999:
            print('프로그램을 종료합니다.')
            break
        if number < 0:
            print('음수를 입력했습니다.')
        else:
            print('0은 양의 정수가 아닙니다.')
        
        number = int(input('양의 정수를 입력해주세요.: '))
    print('잘했습니다!')
    '''
    양의 정수를 입력해주세요.: -9999
    프로그램을 종료합니다.
    잘했습니다!
    ```
    - 리스트에서 첫번째 짝수만 찾은 후 반복 종료하기
    ```python
    numbers = [1, 3, 5, 6, 7, 9, 10, 11]
    found_even = False
    for num in numbers:
        if num % 2 == 0:
            print('첫번째 짝수를 찾았습니다:', num)
            found_even = True
            break
    if not found_even:
        print('짝수를 찾지 못했습니다.')
    # 첫번째 짝수를 찾았습니다: 6
    ```
- continue 예시
    - 리스트에서 홀수만 출력하기
    - 현재 반복문의 남은 코드를 건너뛰고 다음 반복으로 넘어감
    ```python
    numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    for num in numbers:
        if num % 2 == 0:
            continue
        print(num)
    '''
    1
    3
    5
    7
    9
    '''
    ```
- break와 continue 주의 사항
    - break와 continue를 남용하는 것은 코드의 가독성을 저하시킬 수 있음
    - **특정한 종료 조건**을 만들어 break을 대신하거나,     
    **if 문을 사용해** continue처럼 코드를 건너 뛸 수도 있음
    - 약간의 시간이 들더라도 가능한 코드의 가독성을 유지하고, 코드의 의도를 명확하게 작성하도록 노력하는 것이 중요
    ```python
    for number in range(1, 5):
        if number == 3:
            continue
        print(number)
    '''
    1
    2
    4
    '''
    ```
    - 위 코드를 아래로 바꿔줄 수 있음
    ```python
    for number in range(1, 5):
        if number != 3:
            print(number)
    '''
    1
    2
    4
    '''
    ```
    
## List Comprehension
- 간결하고 효율적인 리스트 생성 방법
- List Comprehension 구조
```python
[expression for 변수 in iterable]
list(expression for 변수 in iterable)
```
- List Comprehension 활용
    - 사용 전
    ```python
    numbers = [1, 2, 3, 4, 5]
    squared_numbers = []
    for num in numbers:
        squared_numbers.aappend(num**2)
    print(squred_numbers)
    # [1, 4, 9, 16, 25]
    ```
    - 사용 후
    ```python
    numbers = [1, 2, 3, 4, 5,]
    squared_numbers = [num ** 2 for num in numbers]
    print(squared_numbers)
    # [1, 4, 9, 16, 25]
    ```
- **참고** List Comprehension과 if 조건문
```python
[exprssion for 변수 initerable if 조건식]
list(expression for 변수 in iterable if 조건식)
```
- Comprehension을 남용하지 말자

## pass
- 아무런 동작도 수행하지 않고 넘어가는 역할
- 문법적으로 문장이 필요하지만 프로그램 실행에는 영향을 주지 않아야 할 때 사용
- pass 예시
    1. 코드 작성 중 미완성 부분
    - 구현해야 할 부분이 나중에 추가될 수 있고, 코드를 컴파일하는 동안 오류가 발생하지 않음
    ```python
    def my_function():
        pass
    ```
    2. 조건문에서 아무런 동작을 수행하지 않아야 할 때
    ```python
    if condition:
        pass    # 아무런 동작도 수행하지 않음
    else:
                # 다른 동작 수행
    ```
    3. 무한 루프에서 조건이 충족되지 않을 때 pass를 사용하여 루프를 계속 진행하는 방법
    ```python
    while True:
        if condition:
            break
        elif condition:
            pass    # 루프 계속 진행
        else:
            print('..')
    ```

## enumerate
- enumerate(iterable, start=0)
- iterable 객체의 각 요소에 대해 인덱스와 함께 반환하는 내장함수
- enumerate 예시
```python
fruits = ['apple', 'banana', 'cherry']
for index, fruit in enumerate(fruits):
    print(f'인덱스 {index}: {fruit}')
'''
인덱스 0: apple
인덱스 1: banana
인덱스 2: cherry
'''
```
