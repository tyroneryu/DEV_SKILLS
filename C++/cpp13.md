# Chapter 13: 템플릿(Template) 01
## 템플릿(Template)에 대한 이해와 함수 템플릿 01
### 함수를 대상으로 템플릿 이해하기
- 일반함수
```cpp
int Add(int num1, int num2)
{
  return num1+num2;
}
```
- 템플릿화의 중간 단계
```cpp
T Add(T num1, T num2)
{
  return num1+num2;
}
```
- 템플릿화 완료
```cpp
template <typename T>
T Add(T num1, T num2)
{
  return num1+num2;
}
```
```cpp
template <typename T>
T Add(T num1, T num2)
{
  return num1+num2;
}

int main(void)
{
  // T를 int로 하여 만들어진 함수를 호출하면서 15와 20을 전달
  cout<< Add<int>(15, 20)         <<endl;
  // T를 double로 하여 만들어진 함수를 호출하면서 2.9와 3.7을 전달
  cout<< Add<double>(2.9, 3.7)    <<endl;
  cout<< Add<int>(3.2, 3.2)       <<endl;
  cout<< Add<double>(3.14, 2.75)  <<endl;
  return 0;
}
```

### 컴파일러가 생성하는 템플릿 기반의 함수
- `Add<int>()`의 함수 호출 문을 처음 컴파일 할 때 이 함수가 만들어짐
```cpp
int Add<int>(int num1, int num2)
{
  return num1+num2;
}
```
- `Add<double>()`의 함수 호출 문을 처음 컴파일 할 때 이 함수가 만들어짐
```cpp
double Add<double>(double num1, double num2)
{
  return num1+num2;
}
```

### 호출하기가 좀 불편함
- 호출하기 불편하지 않음
- 컴파일러가 전달인자의 자료형을 통해서 호출해야 할 함수의 유형을 자동으로 결정해주기 때문
```cpp
template <typename T>
T Add(T num1, T num2)
{
  return num1+num2;
}

int main(void)
{
  // 전달되는 인자를 통해서 컴파일러는 아래를 Add<int>(15, 20);으로 해석
  cout<< Add<int>(15, 20)         <<endl;
  // 전달되는 인자를 통해서 컴파일러는 아래를 Add<double>(2.9, 3.7);으로 해석
  cout<< Add<double>(2.9, 3.7)    <<endl;
  cout<< Add<int>(3.2, 3.2)       <<endl;
  cout<< Add<double>(3.14, 2.75)  <<endl;
  return 0;
}
```

### 함수 템플릿과 템플릿 함수
- 함수의 형태로 정의된 템플릿이기 때문에 함수 템플릿이라고 함
- 즉, 아래는 템플릿이지 호출이 가능한 형태의 함수가 아님
```cpp
template <typename T>
T Add(T num1, T num2)
{
  return num1+num2;
}
```
- 아래는 템플릿 함수
- 템플릿을 기반으로 컴파일러에 의해서 생성된 함수이기 때문
- 즉, 이는 함수이지 템플릿이 아님
```cpp
int Add<int>(int num1, int num2)
{
  return num1+num2;
}

double Add<double>(double num1, double num2)
{
  return num1+num2;
}
```

### 둘 이상의 형(Type)에 대해 템플릿 선언하기
- 아래처럼 콤마를 사용해 둘 이상의 형에 대해 템플릿을 선언 가능
```cpp
template <class T1, class T2>
void ShowData(double num)
{
  cout<<(T1)num<<", "<<(T2)num<<endl;
}

int main(void)
{
  ShowData<char, int>(65);
  ShowData<char, int>(67);
  ShowData<char, double>(68.9);
  ShowData<short, double>(69.2);
  ShowData<short, double>(70.4);
  return 0;
}
```
```
실행 결과

A, 65
C, 67
D, 68.9
69, 69.2
70, 70.4
```
- 템플릿의 선언에 있어서 키워드 `typename`과 `class`는 같은 의미로 사용됨

## 템플릿(Template)에 대한 이해와 함수 템플릿 02
### 함수 템플릿의 특수화(Specialization): 도입
```cpp
template <typename T>
T Max(T a, T b)
{
  return a > b ? a : b;   // 대소비교 함수 템플릿으로 큰 값을 반환함
}

int main(void)
{
  cout<< Max(11, 15)            <<endl;
  cout<< Max("T", "Q")          <<endl;
  cout<< Max(3.5, 7.5)          <<endl;
  cout<< Max("Simple", "Best")  <<endl;
  return 0;
}
```
- 정수, 실수, 그리고 문자를 대상으로는 Max 함수의 호출의 의미를 가짐
- 그러나 문자열을 대상으로 호출이 되면 의미를 갖지 않음
- 아래는 문자열의 길이비교가 목적인 경우 어울리는 함수
```cpp
const char * Max(const char* a, const char* b)
{
  return strlen(a) > strlen(b) ? a : b;
}
```
- 일반적인 상황에서는 Max 템플릿 함수가 호출되고, 문자열이 전달되는 경우에는 문자열의 길이를 비교하는 Max 함수를 호출하게 할 수 있지 않나?

### 함수 템플릿의 특수화(Specialization): 적용
```cpp
template <typename T>
T Max(T a, T b)
{
  return a > b ? a : b;   // 대소비교 함수 템플릿으로 큰 값을 반환함
}

template <>
char* Max(char* a, char* b)
{
  cout<<"char* Max<char*>(char* a, char* b)"<<endl;
  return strlen(a) > strlen(b) ? a : b;
}

template <>
const char* Max(const char* a, const char* b)
{
  cout<<"const char* Max<char*>(const char* a, const char* b)"<<endl;
  return strlen(a) > strlen(b) ? a : b;
}

int main(void)
{
  cout<< Max(11, 15)            <<endl;
  cout<< Max("T", "Q")          <<endl;
  cout<< Max(3.5, 7.5)          <<endl;
  cout<< Max("Simple", "Best")  <<endl;

  char str1[]="Simple";
  char str2[]="Best";
  cout<< Max(str1, str2)        <<endl;
  return 0;
}
```
```
실행 결과

15
T
7.5
const char* Max<char*>(const char* a, const char* b)
Simple
char* Max<char*>(char* a, char* b)
Simple
```

### 함수 템플릿의 특수화(Specialization): 비교
- 특수화하는 자료형의 정보가 생략된 형태
```cpp
template <>
char* Max(char* a, char* b)
{}

template <>
const char* Max(const char* a, const char* b)
{}
```
- 특수화하는 자료형의 정보를 명시한 형태
```cpp
template <>
char* Max<char*>(char* a, char* b)
{}

template <>
const char* Max<const char*>(const char* a, const char* b)
{}
```

## 클래스 템플릿(Class Template)
### 클래스 템플릿의 정의
- 일반 클래스
```cpp
class Point
{
private:
  int xpos, ypos;
public:
  Point(int x=0, int y=0) : xpos(x), ypos(y)
  {}
  void ShowPosition() const
  {
    cout<<"["<<xpos<<", "<<ypos<<"]"<<endl;
  }
};
```
- 클래스의 템플릿화
```cpp
template <typename T>
class Point
{
private:
  T xpos, ypos;
public:
  Point(T x=0, T y=0) : xpos(x), ypos(y)
  {}
  void ShowPosition() const
  {
    cout<<"["<<xpos<<", "<<ypos<<"]"<<endl;
  }
};
```
- 템플릿 클래스의 객체 생성시 자료형의 정보는 생략이 불가능함

### 클래스 템플릿의 선언과 정의의 분리
```cpp
template <typename T>
class SimpleTemplate
{
public:
  T SimpleFunc(const T& ref); // 함수의 선언
}
```
- 템플릿 외부의 함수 정의
```cpp
template <typename T>
T SimpleTemplate<T>::SimpleFunc(const T& ref)
{
  ...
}
```
- `SimpleTemplate::SimpleFunc()`
  - SimpleTemplate 클래스의 멤버함수 SimpleFunc를 의미
- `SimpleTemplate<T>::SimpleFunc()`
  - T에 대해서 템플릿으로 정의된 SimpleTemplate의 멤버함수 SimpleFunc를 의미
- `template <typename T>`
  - `<T>`가 들어가면 이 T가 의미하는 바를 항상 설명해야 함

### 파일을 나눌 때에 고려할 사항
1. 클래스 템플릿의 정의 부를 담고 있는 소스파일을 포함시킴
2. 헤더파일에 클래스 템플릿의 정의 부를 포함시킴

### 배열 클래스의 템플릿화
- 자료형에 따른 각각의 배열 클래스
```cpp
class BoundCheckIntArray {...};
class BoundCheckPointArray {...};
class BoundCheckPointPtrArray {...};
```
- 위의 배열들을 대체할 수 있는 배열 기반의 클래스 템플릿
```cpp
template <typename T>
class BoundCheckArray
{
private:
  T * arr;
  int arrlen;

  BoundCheckArray(const BoundCheckArray& arr) {}
  BoundCheckArray& operator=(const BoundCheckArray& arr) {}
public:
  BoundCheckArray(int len);
  T& operator[] (int len);
  T operator[] (int idx);
  int GetArrLen() const;
  ~BoundCheckArray();
};
```
- 지금까지 설명한 내용을 바탕으로 배열 클래스 템플릿을 직접 완성해보자
- 이는 지금까지 공부한 내용의 복습 또는 연습에 해당함
