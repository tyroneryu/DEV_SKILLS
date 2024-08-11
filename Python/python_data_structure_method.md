# Data Structure
- 데이터 구조
- 여러 데이터를 효과적으로 사용, 관리하기 위한 구조
- str, list, dict 등

- 자료 구조
    - 컴퓨터 공학에서는 자료 구조라고 함
    - 각 데이터의 효율적인 저장, 관리를 위한 구조를 나눠 놓은 것

- 데이터 구조 활용
    - 문자열, 리스트, 딕셔너리 등 각 데이터 구조의 메서드를 호출하여 다양한 기능을 활용하기

## 메서드
- method
- 객체에 속한 함수
- 객체의 상태를 조작하거나 동작을 수행

### 메서드 특징
- 메서드는 클래스 (class) 내부에 정의되는 함수
- 클래스는 파이썬에서 '타입을 표현하는 방법' 이며 이미 은연 중에 사용해 왔음
- 예를 들어 help 함수를 통해 str을 호출해보면 class 였다는 것을 확인 가능
```python
print(help(str))

'''
Help on class str in module builtins:

class str(object)
 |  str(object='') -> str
 |  str(bytes_or_buffer[, encoding[, errors]]) -> str
 |  
 |  Create a new string object from the given object. If encoding or
 |  ...
'''
```
- 메서드는 어딘가(클래스)에 속해 있는 **함수**이며, 각 데이터 타입별로 다양한 기능을 가진 메서드가 존재

### 메서드 호출 방법
- 데이터 타입 객체.메서드()
```python
'hello'.capitalize()
```
- 메서드 호출 예시
```python
# 문자열 메서드 예시
print('hello'.capitalize())     # Hello

# 리스트 메서드 예시
numbers = [1, 2, 3]
numbers.append(4)

print(numbers)                  # [1, 2, 3, 4]
```

# 시퀀스 데이터 구조
## 문자열
### 문자열 조회/탐색 및 검증 메서드
| 메서드 | 설명 |
|:---:|:---:|
| s.find(x) | x의 첫번째 위치를 반환. 없으면 -1을 반환 |
| s.index(x) | x의 첫번째 위치를 반환. 없으면 오류 발생 |
| s.isalpha() | 알파벳 문자 여부 (단순 알파벳이 아닌 유니코드 상 Letter(한국어도 포함)) |
| s.isupper() | 대문자 여부 |
| s.islower() | 소문자 여부 |

- .find(x)
```python
print('banana'.find('a'))   # 1
print('banana'.find('z'))   # -1
```
- .index(x)
```python
print('banana'.index('a'))  # 1
print('banana'.index('a'))  # ValueError: substring not found
```
- .isupper(x) / .islower(x)
```python
string1 = 'HELLO'
string2 = 'Hello'
print(string1.isupper())    # True
print(string2.isupper())    # False
print(string1.islower())    # False
print(string2.islower())    # False
```
- .isalpha(x)
```python
string1 = 'Hello'
string2 = '123'
print(string1.isalpha())    # True
print(string2.isalpha())    # False
```

### 문자열 조작 메서드 (새 문자열 반환)
| 메서드 | 설명 |
|:---:|:---:|
| s.replace(old, new[, count]) | 바꿀 대상 글자를 새로운 글자로 바꿔서 반환 |
| s.strip([chars]) | 공백이나 특정 문자를 제거 |
| s.split(sep=None, maxsplit=-1) | 공백이나 특정 문자를 기준으로 분리 |
| 'seperator'.join([iterable]) | 구분자로 iterable을 합침 |
| s.capitalize() | 가장 첫번째 글자를 대문자로 변경 |
| s.title() | 문자열 내 띄어쓰기 기준으로 각 단어의 첫 글자는 대문자로, 나머지는 소문자로 변환 |
| s.upper() | 모두 대문자로 변경 |
| s.lower() | 모두 소문자로 변경 |
| s.swapcase() | 대 <-> 소문자 서로 변경 |

- .replace(old, new[, count])
```python
text = 'Hello, world!'
new_text = text.replace('world', 'Python')
print(new_text)     # Hello, Python!
```
- .strip([chars])
```python
text = '    Hello, world!    '
new_text = text.strip()
print(new_text)     # 'Hello, world!'
```
- .split(sep=None, maxsplit=-1)
```python
text = 'Hello, world!'
words = text.split(',')
print(words)    # ['Hello', 'world!']
```
- 'seperator'.join([iterable])
```python
words = ['Hello', 'world!']
text = '-'.join(words)
print(text)     # Hello-world!
```
- 나머지
```python
text = 'heLLo, woRld!'
new_text1 = text.capitalize()
new_text2 = text.title()
new_text3 = text.upper()
new_text4 = text.swapcase()

print(new_text1)    # Hello, world!
print(new_text2)    # Hello, World!
print(new_text3)    # HELLO, WORLD!
print(new_text4)    # HEllO, WOrLD!
```
- 메서드는 이어서 사용 가능
```python
text = 'heLLO, woRLd!'
new_text = text.swapcase().replace('l', 'z')

print(new_text)     # HEzzO, WOrlD!
```

## 리스트
### 리스트 값 추가 및 삭제 메서드
| 메서드 | 설명 |
|:---:|:---:|
| L.append(x) | 리스트 마지막에 항목 x를 추가 |
| L.extend(m) | Iterable m의 모든 항목들을 리스트 끝에 추가(+=과 같은 기능) |
| L.insert(i, x) | 리스트 인덱스 i에 항목 x를 삽입 |
| L.remove(x) | 리스트 가장 왼쪽에 있는 항목(첫번째) x를 제거. 항목이 존재하지 않는 경우 ValueError |
| L.pop() | 리스트 가장 오른쪽에 있는 항목(마지막)을 반환 후 제거 |
| L.pop(i) | 리스트의 인덱스 i에 있는 항목을 반환 후 제거 |
| L.clear() | 리스트의 모든 항목 삭제 |

- .append(x)
```python
my_list = [1, 2, 3]
my_list.append(4)
print(my_list)      # [1, 2, 3, 4]
```
- .extend(iterable)
```python
my_list = [1, 2, 3]
my_list.extend([4, 5, 6])
print(my_list)      # [1, 2, 3, 4, 5, 6]
```
- .insert(i, x)
```python
my_list = [1, 2, 3]
my_list.insert(1, 5)
print(my_list)      # [1, 5, 2, 3]
```
- .remove(x)
```python
my_list = [1, 2, 3]
my_list.remove(2)
print(my_list)      # [1, 3]
```
- .pop(i)
```python
my_list = [1, 2, 3, 4, 5]

item1 = my_list.pop()
item2 = my_list.pop(0)

print(item1)        # 5
print(item2)        # 1
print(my_list)      # [2, 3, 4]
```
- .clear()
```python
my_list = [1, 2, 3]
my_list.clear()
print(my_list)      # []
```
### 리스트 탐색 및 정렬 메서드
| 메서드 | 설명 |
|:---:|:---:|
| L.index(x, start, end) | 리스트에 있는 항목 중 가장 왼쪽에 있는 항목 x의 인덱스를 반환 |
| L.reverse() | 리스트의 순서를 역순으로 변경(정렬 X) |
| L.sort() | 리스트를 정렬(매개변수 이용가능) |
| L.count(x) | 리스트에서 항목 x의 개수를 반환 |

- .index(x)
```python
my_list = [1, 2, 3]
index = my_list.index(2)
print(index)    # 1
```
- .count(x)
```python
my_list = [1, 2, 2, 3, 3, 3]
count = my_list.count(3)
print(count)    # 3
```
- .sort()
```python
my_list = [3, 2, 1]
my_list.sort()
print(my_list)  # [1, 2, 3]

# 내림차순
my_list.sort(reverse=True)
print(my_list)  # [3, 2, 1]
```
- .reverse()
```python
my_list = [1, 3, 2, 8, 1, 9]
my_list.reverse()
print(my_list)  # [9, 1, 8, 2, 3, 1]
```

# 복사
### 데이터 타입과 복사
- 파이선에서는 데이터에 분류에 따라 복사가 달라짐
- '변경 가능한 데이터 타입'과 '변경 불가능한 데이터 타입'을 다르게 다룸

### 변경 가능한 데이터 타입의 복사
```python
a = [1, 2, 3, 4]
b = a
b[0 = 100]

print(a)    # [100, 2, 3, 4]
print(b)    # [100, 2, 3, 4]
```

### 변경 불가능한 데이터 타입의 복사
```python
a = 20
b = a
b = 10

print(a)    # 20
print(b)    # 10
```

## 복사 유형
1. 할당(Assignment)
2. 얕은 복사(Shallow copy)
3. 깊은 복사(Deep copy)

### 할당
- 리스트 복사 예시
```python
original_list = [1, 2, 3]
copy_list = original_list
print(original_list, copy_list)     # [1, 2, 3] [1, 2, 3]

copy_list[0] = 'hi'
print(original_list, copy_list)     # ['hi', 2, 3] ['hi', 2, 3]
```
- 할당 연산자 (=)를 통한 복사는 해당 객체에 대한 **객체 참조를 복사**

### 얕은 복사
- 리스트 얕은 복사 예시
```python
a = [1, 2, 3]
b = a[:]
print(a, b)     # [1, 2, 3] [1, 2, 3]

b[0] = 100
print(a, b)     # [1, 2, 3] [100, 2, 3]
```
- 슬라이싱을 통해 생성된 객체는 원본 객체와 독립적으로 존재
- 얕은 복사의 한계
    - 2차원 리스트와 같이 변경 가능한 객체 안에 변경 가능한 객체가 있는 경우
    ```python
    a = [1, 2, [1, 2]]
    b = a[:]
    print(a, b)     # [1, 2, [1, 2]] [1, 2, [1, 2]]

    b[2][0]= 100
    print(a, b)     # [1, 2, [100, 2]] [1, 2, [100, 2]]
    ```
    - a와 b의 주소는 다르지만 내부 객체의 주소는 같기 때문에 함께 변경됨

### 깊은 복사
- 리스트 깊은 복사 예시
```python
import copy

original_list = [1, 2, [1, 2]]
deep_copied_list = copy.deepcopy(original_copy)

deep_copied_list[2][0] = 100

print(original_list)        # [1, 2, [1, 2]]
print(deep_copied_list)     # [1, 2, [100, 2]]
```
- 내부에 중첩된 모든 객체까지 새로운 객체 주소를 참조하도록 함

# 참고
## 문자열에 포함된 문자들의 유형을 판별하는 메서드
- isdecimal()
    - 문자열이 모두 숫자 문자(0 ~ 9)로만 이루어져 있어야 True
- isdigit()
    - isdecimal()과 비슷하지만, 유니코드 숫자도 인식
- isnumeric()
    - isdigit()과 유사하지만, 몇 가지 추가적인 유니코드 문자들을 인식(분수, 지수, 루트 시호도 숫자로 인식)
- isdecimal() < isdigit() < isnumeric()

| isdecimal() | isdigit() | isnumeric() |
|:---:|:---:|:---:|
| True | True | True |
| False | True | True |
| False | False | True |
| False | False | False |
