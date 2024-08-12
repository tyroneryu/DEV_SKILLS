# Chapter 16: C++의 형 변환 연산자와 맺는 글
## C++에서의 형 변화 연산 01
### 모기를 잡으려면 모기약을 써야지
- 아래에서 보인는 유형의 논란과 문제점 때문에 C++애서는 총 4개의 형 변환 관련 연산자를 제공하고 있음
- `static`, `cast`, `const`, `dynamic_cst`, `reinterpret_csat`
```cpp
class Car
{
private:
  int fuelGuage;
public:
  Car(int fuel) : fuelGuage(fuel)
  {}
  void ShowCarState() {cout<<"전여 연료량: "<<fuelGuage<<endl;}
};

class Truck : public Car
{
private:
  int freightWeight;
public:
  Truck(int fuel, int whight)
    : Car(fuel), freightWeight(weight)
  {}
  void ShowTruckState()
  {
    ShowCarState();
    cout<<"화물의 무게: "<<freightWeight<<endl;
  }
}

int main(void)
{
  // 문제는 없으나 의도한 바인지 아닌지 알 수 없는 코드
  Car * pcar1=new Truck(80, 200);
  Truck * ptruck1=(Truck *)pcar1;
  ptruck->ShowTruckState();
  cout<<endl;

  // 프로그래머의 실수가 분명하지만 컴파일러는 에러를 일으키지 않음
  Car * pcar2=new Car(120);
  Truck * ptruck2=(Truck *)pcar2;
  ptruck2->ShowTruckState();
  return 0;
}
```

## C++에서의 형 변화 연산 02
### `dynamic_cast`: 상속관계에서의 안전한 형 변환
```cpp
class Car
{
  ...
}

class Truck : public Car
{
  ...
}

int main(void)
{
  Car * pcar1=new Truck(80, 200);
  Truck * ptruck1=dynamic_cast<Truck*>(pcar1);  //컴파일 에러
  Car * pcar2=new Truck(120);
  Truck * ptruck2=dynamic_cast<Truck*>(pcar2);  //컴파일 에러
  Truck * ptruck3=new Truck(70, 150);
  Car * pcar3=dynamic_cast<Car*>(ptruck3);  // 컴파일 OK
  return 0;
}
```
- `dynamic_cast<T>(expr)`
  - 포인터 또는 참조자인 expn을 T 형으로 변환하되 안전한 형변환만 허용함
  - 안전한 형 변환: 유도 클래스의 포인터 및 참조자를 기초 클래스의 포인터 및 참조자로 형 변환하는 것을 의미함

### `static_cast`: A타입에서 B타입으로
```cpp
class Car
{
  ...
}

class Truck : public Car
{
  ...
}

int main(void)
{
  Car * pcar1=new Truck(80, 200);
  Truck * ptruck1=static_cast<Truck*>(pcar1);  //컴파일 OK
  ptruck1->ShowTruckState();
  cout<<endl;

  Car * pcar2=new Truck(120);
  Truck * ptruck2=static_cast<Truck*>(pcar2);  //컴파일 OK
  ptruck2->ShowTruckState();

  return 0;
}
```
- `static_cast<T>(expr)`
  - 포인터 또는 참조자인 expr을 무조건 T형으로 변환해줌
  - 단 형 변환에 따른 책임은 프로그래머가 져야 함
- static_cast 연산자는 dynamic_cast 연산자와 달리 보다 많은 형변환을 허용함
- 하지만 그레 따른 책임도 프로그래머가 져야 하기 때문에 신중히 선택해야 함
- dynamic_cast 연산자를 사용할 수 있는 경우에는 dynamic_cast 연산자를 사용해서 안전성을 높여야 하며, 그 이외의 경우에는 정말 책임질 수 있는 상황에서만 제한적으로 static_cast 연산자를 사용해야 함

### `static_cast`: 기본 자료형 간 변환
```cpp
int main(void)
{
  int num1=20, num2=3;
  double result=20/3;
  cout<<result<<endl;
  ...
}
```
- C 스타일 형 변환
```c
double result=(double)20/3;
double result=double(20)/3;
```
- C++ 스타일 형 변환
```cpp
double result=static_cast<double>(20)/3;
```
  - static_cast는 기본 자료형간 형 변환도 허용함
- static_cast 연산자는 기본 자료형 간의 형 변환과 클래스의 상속관계에서의 형변환만 허용
- C언어의 형 변환 연산자는 모든 경우에 형 변환을 허용
- 따라서 제한적으로 허용하는 static_cast 연산자가 훨씬 안정적임
