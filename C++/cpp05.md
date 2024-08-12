# Chapter 05: 복사 생성자
## 복사 생성자와의 첫 만남
### C++ 스타일의 초기화
- C 스타일 초기화
```c
int num=20;
int &ref=num;
```
- C++ 스타일 초기화
```cpp
int num(20);
int &ref(num);
```
- 위처럼 아래의 두 문장은 실제로 동일한 문장으로 해석됨
- `SoSimple sim2=sim1;`
- `SoSimple sim2(sim1);`

```cpp
class SoSimple
{
private:
  int num1;
  int num2;
public:
  SoSimple(int n1, int n2) : num1(n1), num2(n2)
  { }
  void ShowSimpleData()
  {
    cout<<num1<<endl;
    cout<<num2<<endl;
  }
};

int main(void)
{
  SoSimple sim1(15, 20);
  SoSimple sim2=sim1;
  sim2.ShowSimpleData();
  return 0;
}
```
- 대입연산의 의미처럼 실제 멤버 대 멤버의 복사가 일어남
- 객체 생성시 따로 지정해 주지 않으면 디폴트 복사 생성자가 삽입됨

### `SoSimple sim2(sim1);`
- `SoSimple sim2(sim1);`의 해석
  - SoSimple형 갹체를 생성하라
  - 객체의 이름은 sim2로 정함
  - sim1을 인자로 받을 수 있는 생성자의 호출을 통해서 객체 생성을 완료함
- `SoSimple sim2=sim1`은 암묵적으로 `SoSimple sim2(sim1)`으로 해석됨
```cpp
class SoSimple
{
private:
  int num1;
  int num2;
public:
  SoSimple(int n1, int n2) : num1(n1), num2(n2)
  { }
  SoSimple(SoSimple &copy)
    : num1(copy.num1), num2(copy.num2)
  {
    cout<<"Called SoSimple(SoSimple &copy)"<<endl;
  }
};

int main(void)
{
  SoSimple sim1(15, 30);
  cout<<"생성 및 초기화 직전"<<endl;
  SoSimple sim2=sim1;   // SoSimple sim2(sim1); 으로 변환
  cout<<"생성 및 초기화 직후"<<endl;
  sim2.ShowSimpleData();
  return 0;
}
```
```
실행 결과

생성 및 초기화 직전
Called SoSimple(SoSimple &copy)
생성 및 초기화 직후
15
30
```

### 자동으로 삽입이 되는 디폴트 복사 생성자
```cpp
class SoSimple
{
private:
  int num1;
  int num2;
public:
  SoSimple(int n1, int n2) : num1(n1), num2(n2)
  {}
  ...
};
```
- 위 의 코드처럼 복사 생성자를 정의하지 않으면 멤버 대 멤버의 복사를 진행하는 디폴트 복사 생성자가 삽입됨
```cpp
class SoSimple
{
private:
  int num1;
  int num2;
public:
  SoSimple(int n1, int n2) : num1(n1), num2(n2)
  {}
  SoSimple(const SoSimple &copy) : num1(copy.num1), num2(copy.num2)
  {}
};
```

### 키워드 explicit
- `SoSimple sim2=sim1; -> SoSimple sim2(sim1);`
- 이러한 묵시적 형 변환은 복사 생성자를 explicit으로 선언하면 막을 수 있음
```cpp
explicit SoSimple(const SoSimple &copy)
  : num1(copy.num1), num2(copy.num2)
{
  // empty
}
```
- AAA 생성자를 ecplicit로 선언하면 `AAA obj=3`과 같은 형태로 객체 생성 불가
```cpp
class AAA
{
private:
  int num;
public:
  AAA(int n) : numm(n) {}
  ...
};
```

## 깊은 복사와 얕은 복사
### 디폴트 복사 생성자의 문제점
```cpp
class Person
{
private:
    char * name;
    int age;
public:
  Person(char * myname, int myage)
  {
    int len=strlen(myname)+1;
    name=new char[len];
    strcpy(name, myname);
    age=myage;
  }
  ...
  ~Person()
  {
    delete []name;
    cout<<"Called destructor"<<endl;
  }
};

int main(void)
{
  Person man1("Lee dong woo", 29);
  Person man2=man1;
  man1.ShowPersonInfo();
  man2.ShowPersonInfo();
  return 0;
}
```
```
실행 결과

이름: Lee dong woo
나이: 29
이름: Lee dong woo
나이: 29
Called destructor
```
- 객체 소멸 시 문제가 되는 구조: 얕은 복사

### 깊은 복사를 위한 복사 생성자의 정의
- 깊은 복사를 구성하는 복사 생성자
```cpp
Person(const Person& copy) : age(copy.age)
{
  name=new char[strlen(copy.name)+1];
  strcpy(name, copy.name);
}
```

## 복사 생성자의 호출 시점
### 복사 생성자가 호출되는 시점
- 메모리 공간의 할당과 초기화가 동시에 일어나는 상황
  1. 기존에 생성된 객체를 이용해서 새로운 객체를 초기화하는 경우
  2. Call-by-value 방식의 함수 호출 과정에서 객체를 인자로 전달하는 경우
  3. 객체를 반환하되, 참조형으로 반환하지 않는 경우
- Case 1
```cpp
Person man1("Lee dong woo", 29);
Person man2=man1;   // 복사 생성자 호출
```
- Case 2 & Case 3
```cpp
SoSimple SimpleFuncObj(SoSimple ob)
{
  ...
  return ob;
}
int main(void)
{
  SoSimple obj;
  SimpleFuncObj(obj);
}
```

### 복사 생성자의 호출 case의 확인 01
```cpp
class SoSimple
{
private:
  int num;
public:
  SoSimple(int n) : num(n)
  {}
  SoSimple(const SoSimple& copy) : num(copy.num)
  {
    cout<<"Called SoSimple(const SoSimple& copy)"<<endl;
  }
  void ShowData()
  {
    cout<<"num: "<<num<<endl;
  }
};

void SimpleFuncObj(SoSimple ob)
{
  ob.ShowData();
}

int main(void)
{
  SoSimple obj(7);
  cout<<"함수호출 전"<<endl;
  SimpleFuncObj(obj);
  cout<<"함수호출 후"<<endl;
  return 0;
}
```
```
실행 결과

함수호출 전
Called SoSimple(const SoSimple& copy)
num: 7
함수호출 후
```

### 복사 생성자의 호출 case의 확인 02
```cpp
class SoSimple
{
private:
  int num;
public:
  SoSimple(int n) : num(n)
  {}
  SoSimple(const SoSimple& copy) : num(copy.num)
  {
    cout<<"Called SoSimple(const SoSimple& copy)"<<endl;
  }
  SoSimple& AddNum(int n)
  {
    num+=n;
    return *this;
  }
  void ShowData()
  {
    cout<<"num: "<<num<<endl;
  }
};

SoSimple SimpleFuncOb(SoSimple ob)
{
  cout<<"return 이전"<<endl;
  return ob;
}
```
```
실행 결과

Called SoSimple(const SoSimple& copy)
return 이전
Called SoSimple(const SoSimple& copy)
num: 37
num: 7
```

### 반환할 때 만들어진 객체의 소멸 시점
```cpp
class Temporary
{
private:
  int num;
public:
  Temporary(int n) : num(n)
  {
    cout<<"create obj: "<<num<<endl;
  }
  ~Temporary()
  {
    cout<<"destroy obj: "<<num<<endl;
  }
  void ShowTempInfo()
  {
    cout<<"My num is "<<num<<endl;
  }
};

int main(void)
{
  Temporary(100);
  cout<<"*************** after make"<<endl;

  Temporary(200) ShowTempInfo();
  cout<<"*************** after make"<<endl;

  const Temporary &ref=Temporary(300);
  cout<<"*************** end of main"<<endl;
  return 0;
}
```
```
실행 결과

create obj: 100
destroy obj: 100
*************** after make
create obj: 200
My num is 200
destroy obj: 200
*************** after make
create obj: 300
*************** end of main
destroy obj: 300
```
- `Temporary(200).ShowTempInfo();`: `(임시객체의 참조 값).ShowTempInfo();`
