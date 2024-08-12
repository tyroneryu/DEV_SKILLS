# Chapter 14: 템플릿(Template) 02
## Chapter 13에서 공부한 내용의 확장
### Point 클래스 템플릿과 배열 클래스 템플릿
- 일반적인 배열 클래스 템플릿
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
```cpp
template <typename T>
class Point
{
private:
  T xpos, ypos;
public:
  Point(T x=0, T y=0);
  void ShowPosition() const;
};
```

### 템플릿 클래스 대상의 함수선언과 friend 선언
- 컴파일러가 생성해 내는 템플릿 클래스를 함수의 매개변수 및 반환형으로 지정하는 것도 가능하고 이러한 함수를 대상으로 friend 선언을 하는 것도 가능함
- 결론은 컴파일러가 생성하는 템플릿 클래스의 이름도 일반 자료형의 이름과 차별을 받지 않는다는 것
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
    cout<<xpos", "<<ypos<<endl;
  }

  friend Point<int> operator+(const Point<int>&, const Point<int>&);
  friend ostream& operator<< (ostream& os, const Point<int>& pos);
};

Point<int> operator+(const Point<int>& pos1, const Point<int>& pos2)
{
  return Point<int>(pos1.xpos+pos2.xpos, pos1.ypos+pos2.ypos);
}
```

## 클래스 템플릿의 특수화
### 클래스 템플릿 특수화
```cpp
template <typename T>
class SoSimple
{
public:
  T SimpleFunc(T num) {}
};
```
- 위의 template을 SoSimple 클래스 템플릿에 대해서 int 형에 대한 특수화
```cpp
template <>
class SoSimple<int>
{
public:
  int SimpleFunc(int num) {}
};
```
- 클래스 템플릿을 특수화하는 이유는 특정 자료형을 기반으로 생성된 객체에 대해, 구분이 되는 다른 행동 양식을 적용하기 위함
- 함수 템플릿을 특수화하는 방법과 이유, 그리고 클래스 템플릿을 특수화하는 방법과 이유는 동일함

### 클래스 템플릿의 부분 특수화
- MySimple 클래스 템플릿
```cpp
template <typename T1, typename T2>
class MySimple {}
```
- MySimple 클래스 템플릿의 `<char, int>`에 대한 특수화
```cpp
template <>
class MySimple<char, int> {}
```
- MySimple 클래스 템플릿의 `<T1, int>`에 대한 부분적 특수화
  - T2가 int인 경우에는 `MySimple<T1, int>`를 대상으로 인스턴스가 생성됨
```cpp
template <typename T1>
class MySimple<T1, int> {}
```
- 위와 같이 `<char, int>`형으로 특수화, 그리고 `<T1, int>`에 대해서 부분 특수화가 모두 진행된 경우 특수화가 부분 특수화에 앞섬
- 즉, `<char, int>`를 대상으로 객체 생성 시 특수화된 클래스의 객체가 생성됨

## 템플릿 인자
### 템플릿 매개변수에는 변수의 선언이 올 수 있음
- 템플릿의 인자로 변수의 선언이 올 수도 있음
```cpp
template <typename T, int len>
class SimpleArray
{
private:
  T arr[len];
public:
  T& operator[] (int idx)
  {
    return arr[idx];
  }
};
```
```cpp
class SimpleArray<int, 5>
{
private:
  int arr[5];
public:
  int& operator[] (int idx)
  {
    return arr[idx];
  }
};
```
- `SimplArray<int, 5>i5arr;`: `SimpleArray<int, 5>`형 템플릿 클래스
```cpp
class SimpleArray<double, 7>
{
private:
  double arr[7];
public:
  double& operator[] (int idx)
  {
    return arr[idx];
  }
};
```
- `SimplArray<double, 7>i7arr;`: `SimpleArray<double, 7>`형 템플릿 클래스

### 템플릿 매개변수는 디폴트 값 지정도 가능함
```cpp
template <typename T=int, int len=7>  // 디폴트 값 지정 가능
class SimpleArray
{
private:
  T arr[len];
public:
  T& operator[] (int idx)
  {
    return arr[idx];
  }
  SimpleArray<T, len>& operator=(const SimpleArray<T, len> &ref)
  {
    for(int i=0; i<len; i++)
      arr[i]=ref.arr[i];
  }
};

int main(void)
{
  SimpleArray<> arr;  // T에 int, len에 7의 디폴트 값 지정
  for(int i=0; i<7; i++)
    arr[i]=i+1;
  for(int i=0; i<7; i++)
    cout<<arr[i]<<" ";
  cout<<endl;
  return 0;
}
```
```
실행결과

1 2 3 4 5 6 7
```

## 템플릿과 static
### 함수 템플릿과 static 지역변수
```cpp
template <typename T>
void ShowStaticValue(void)
{
  static T num=0;
  num+=1;
  cout<<num<<" ";
}

int main(void)
{
  ShowStaticValue<int>();
  ShowStaticValue<int>();
  ShowStaticValue<int>();
  cout<<endl;
  ShowStaticValue<long>();
  ShowStaticValue<long>();
  ShowStaticValue<long>();
  cout<<endl;
  ShowStaticValue<double>();
  ShowStaticValue<double>();
  ShowStaticValue<double>();
  return 0;
}
```
```
실행 결과

1 2 3
1 2 3
1 2 3
```

### 클래스 템플릿과 static 멤버변수
- 클래스 템플릿의 static 변수는 템플릿 클래스 별로 독립적임
- 따라서 템플릿 클래스 별 객체들 사이에서만 공유가 이뤄짐
```cpp
template <typename T>
class SimpleStaticMem
{
private:
  static T mem;
public:
  void AddMem(int num) { mem+=num; }
  void ShowMem() { cout<<mem<<endl; }
};

// 아래는 템플릿 기반의 static 멤버 초기화 문장
template <typename T>
T SimpleStaticMem<T>::mem=0;
```

### `template <typename T>` vs `template <>`
- 쳄플릿임을 알리며 T가 무엇인지에 대한 설명도 필요한 상황
```cpp
template <typename T>
class SoSimple
{
public:
  T SimpleFunc(T num) {}
};
```
```cpp
template <typename T>
T simpleStaticMem<T>::mem=0;
```
- 템플릿과 관련 있음을 알리기만 하면 되는 상황
```cpp
template <>
class SoSimple<int>
{
public:
  int SimpleFunc(int num) {}
};
```
- static 멤버 초기화의 특수화
```cpp
template <>
long SimpleStaticMem<long>::mem=5;
```
