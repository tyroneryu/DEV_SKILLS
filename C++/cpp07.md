# Chapter 07: 상속(Inheritance)의 이해
## 상속에 들어가기에 앞서
### 상속의 이해를 위한 이 책의 접근방식
1. 문제의 제시
  - 상속과 더불어 다형성의 개념을 젹용해야만 해결 가능한 문제를 먼저 제시함
2. 기본개념 소개
  - 상속의 문법적 요소를 하나씩 소개해 나감
  - 그 과정에서 앞서 제시한 문제의 해결책을 함께 고민해 나감
3. 문제의 해결
  - 처음 제시한 문제를 상속을 적용하여 해결함
- 위의 흐름대로 상속을 이해해야 함
- 상속은 기존에 정의해 놓은 클래스의 재활용을 목적으로 만들어진 문법적 요소라고 이해하는 경우가 많음
- 하지만 상속에는 다른, 더 중요한 의미가 담겨 있음

### 문제의 제시를 위한 시나리오의 도입
```cpp
class PermanentWorker
{
private:
  char name[100];
  int salary;
public:
  PermanentWorker(char* name, int money)
    : salary(money)
  {
    strcpy(this->name, name);
  }
  int GetPay() const
  {
    return salary;
  }
  void ShowSalaryInfo() const
  {
    cout<<"name: "<<name<<endl;
    cout<<"slary: "<<GetPay()<<endl<<endl;
  }
};

class EmployeeHandler
{
private:
  PermanentWorker* empList[50];
  int empNum;
public:
  EmployeeHandler() : empNum(0)
  {}
  void AddEmployee(PermanentWorker* emp)
  {
    empList[empNum++]*emp;  // 신규 직원 등록 시
  }
  void ShowAllSalaryInfo() const  // 전체 급여 정보 출력
  {
    for(int i=0; i<empNum; i++)
      empList[i]->ShowSalaryInfo();
  }
  void ShowTotalSalary() const  // 급여 합계 정보 출력
  {
    int sum=0;
    for(int i=0; i<empNum; i++)
      sum+=empList[i]->GetPay();
    cout<<"salary sum: "<<sum<<endl;
  }
  ~EmployeeHandler()
  {
    for(int i=0; i<empNum; i++)
      delete empList[i];
  }
};
```
- 프로그램 전체 기능의 처리를, 프로그램의 흐름을 담당하는 클래스를 가리켜 **컨트롤 클래스라**고 함
- EmployeeHandler의 경우 컨트롤 클래스에 해당함

### 문제의 제시
- 프로그램에 추가할 직급의 형태
  - 영업직(Sales): 조금 특화된 형태의 공요직, 인센티브 개념 도입
  - 임시직(Temporary): 학생들을 대상으로 하는 임시고용 형태, 흔히 아르바이트라 함
- 확장 이후의 급여 지급 방식
  - 고용직 급여: 연봉제. 따라서 매달의 급여가 정해져 있음
  - 영업직 급여: 기본 급여 + 인센티브의 형태
  - 임시직 급여: 시간당 급여 X 일한 시간의 형태
- 이 문제는 영업직과 임시직에 해당하는 클래스의 추가로 끝나지 않음
- 컨트롤 클래스인 EmployeeHandler 클래스의 대대적인 변경으로 이어짐
- 좋은 코드는 요구사항의 변경 및 기능의 추가에 따른 변경이 최소화되어야 함
- 그리고 이를 위한 해결책으로 상속이 사용됨

## 상속의 문법적인 이해
### 상속의 방법과 그 결과
```cpp
class Person
{
private:
  int age;
  char name;
public:
  Person(int myage, char * myname) : age(myage)
  {
    strcpy(name, myname);
  }
  void WhatYourName() const
  {
    cout<<"My name is "<<name<<endl;
  }
  void HowOldAreYou() const
  {
    cout<<"I'm "<<age<<"years old"<<endl;
  }
};

class UnivStudent : public Person // Person 클래스 상속
{
private:
  char major[50];
public:
  UnivStudent(char * myname, int myage, char * mymajor) : Person(myage, myname)
  {
    strcpy(major, mymajor);
  }
  void WhoAreYou() const
  {
    // Person 클래스의 멤버
    WhatYourName();
    HowOldAreYou();
    cout<<"My major is "<<major<<endl<<endl;
  }
};
```
- 용어 정리
  - Person <-> UnivStudent
  - 상위 클래스 <-> 하위 클래스
  - 기초 클래스 <-> 유도 클래스
  - 슈퍼 클래스 <-> 서브 클래스
  - 부모 클래스 <-> 자식 클래스

### 상속받은 클래스의 생성자 정의
```cpp
UnivStudent(char * myname, int myage, char * mymajor) : Person(myage, myname)
{
  strcpy(major, mymajor)
}
```
- 이니셜라이저를 통해서 유도 클래스는 기초 클래스의 생성자를 명시적으로 호출해야 함
- 유도 클래스의 생성자는 기초 클래스의 멤버를 초기화 할 의무를 가짐
- 단, 기초 클래스의 생성자를 명시적으로 호출해서 초기화해야 함
```cpp
int main(void)
{
  UnivStudent ustd1("Lee", 22, "Computer eng.");
  ustd1.WhoAreYou();

  UnivStudent ustd2("Yoon", 21, "Electronic eng.");
  ustd2.WhoAreYou();
  return 0;
}
```
- 때문에 유도 클래스 UnivStudent는 기초 클래스의 생성자 호출을 위한 인자까지 함께 전달받아야 함
- private 멤버는 유도 클래스에서도 접근이 불가능하므로 생성자의 호출을 통해서 기초 클래스의 멤버를 초기화해야 함

### 유도 클래스의 객체 생성 과정
```cpp
class SoBase
{
private:
  int baseNum;
public:
  SoBase() : baseNum(20);
  {
    cout<<"SoBase()"<<endl;
  }
  SoBase(int n) : baseNum(n)
  {
    cout<<"SoBase(int n)"<<endl;
  }
  void ShowBaseData()
  {
    cout<<baseNum<<endl;
  }
};

class SoDerived : public SoBase
{
private:
  int derived;
public:
  SoDerived() : derivNum(30)
  {
    cout<<"SoDerived()"<<endl;
  }
  SoDerived(int n) : derivNum(n)
  {
    cout<<"SoDerived(int n)"endl;
  }
  SoDerived(int n1, int n2) : SoBase(n1), derived(n2)
  {
    cout<<"SoDerived(int n1, int n2)"<<endl;
  }
  void ShowDerivedData()
  {
    ShowBaseData();
    cout<<derivNum<<endl;
  }
};

int main(void)
{
  cout<<"case1..."<<endl;
  SoDerived dr1;
  dr1.ShowDerivedData();
  cout<<".........."<<endl;
  cout<<"case2..."<<endl;
  SoDerived dr2(12);
  dr2.ShowDerivedData();
  cout<<".........."<<endl;
  cout<<"case3..."<<endl;
  SoDerived dr3(23, 24);
  dr3.ShowDerivedData();
  return 0;
}
```
```
실행 결과

case1...
SoBase()
SoDerived()
20
30
..........
case2...
SoBase()
SoDerived(int n)
20
12
..........
case3...
SoBase(int n)
SoDerived(int n1, int n2)
23
24
```

### 유도 클래스의 객체 생성 과정 case1
1. 메모리 공간의 할당
2. 유도 클래스의 생성자 호출
3. 기초 클래스의 생성자 호출 및 실행
4. 유도 클래스의 생성자 실행

### 유도 클래스의 객체 생성 과정 case2
1. 메모리 공간의 할당
2. 유도 클래스의 void 생성자 호출
3. 이니셜라이저를 통한 기초 클래스의 생성자 호출이 명시적으로 정의되어 있지 않으므로 void 생성자 호출
4. 유도 클래스의 실행

### 유도 클래스 객체의 소멸 과정
```cpp
class SoBase
{
private:
  int baseNum;
public:
  SoBase(int n) : baseNum(n)
  {
    cout<<"SoBase() : "baseNum<<endl;
  }
  ~SoBase()
  {
    cout<<"~SoBase() : "<<baseNum<<endl;
  }
};

class SoDerived : public SoBase
{
private:
  int derivNum;
public:
  SoDerived(int n) : SoBase(n), derivNum(n)
  {
    cout<<"SoDerived() : "<<derivNum<<endl;
  }
  ~SoDerived()
  {
    cout<<"~SoDerived() : "<<derivNum<<endl;
  }
};
```
```
실행 결과

SoBase() : 15
SoDerived() : 15
SoBase() : 27
SoDerived() : 27
~SoDerived() : 27
~SoBase() : 27
~SoDerived() : 15
~SoBase() : 15
```
- 유도 클래스의 소멸자가 실행된 이후에 기초 클래스의 소멸자가 실행됨
- 스택에 생성된 객체의 소멸 순서는 생성 순서와 반대임

### 유도 클래스 정의 모델
```cpp
class Person
{
private:
  char * name;
public:
  Person(char * myname)
  {
    name=new char[strlen(myname)+1];
    strcpy(name, myname);
  }
  ~Person()
  {
    delete []name;
  }
  void WhatYourName() const;
  {
    cout<<"My name is "<<name<<endl;
  }
};

class UnivStudent : public Person
{
private:
  char * major;
public:
  UnivStudent(char * myname, char * mymajor)
    : Person(myname)
    {
      major=new char[strlen(mymajor)+1];
      strcpy(major, mymajor);
    }
    ~UnivStudent()
    {
      delete []major;
    }
    void WhoAreYou() const
    {
      WhatYourName();
      cout<<"My major is "<<major<<endl<<endl;
    }
};
```
- 기초 클래스의 멤버 대상의 동적 할당은 기초 클래스의 생성자를 통해서, 소멸 역시 기초 클래스의 소멸자를 통해서

## protected 선언과 세가지 형태의 상속
### protected로 선언된 멤버가 허용하는 접근의 범위
```cpp
class Base
{
private:
  int num1;
protected:
  int num2;
public:
  int num3;
  void ShowData()
  {
    cout<<num1<<", "<<num2<<", "<<num3;
  }
};

class Derived : public Base
{
public:
  void ShowBaseData()
  {
    cout<<num1; // 컴파일 에러
    cout<<num2; // 컴파일 OK
    cout<<num3; // 컴파일 OK
  }
};
```
- private < protected < public
- private을 기준으로 보면 protected는 private과 달리 상속관계에서의 접근을 허용함

### 세가지 형태의 상속
1. public 상속
```cpp
class Derived : public Base
{
  ...
}
```
  - 접근 제어 권한을 그대로 상속
  - 단, private은 접근 불가로 상속
2. protected 상속
```cpp
class Derived : protected Base
{
  ...
}
```
  - protected보다 접근의 범위가 넓은 멤버는 protected로 상속
  - 단, private은 접근 불가로 상속
3. private 상속
```cpp
class Derived : private Base
{
  ...
}
```
  - private보다 접근의 범위가 넓은 멤버는 protected로 상속
  - 단, private은 접근 불가로 상속

## 상속을 위한 조건
### 상속의 기본 조건인 IS-A 관계의 성립
- 전화기 -> 무선 전화기
- 컴퓨터 -> 노트북 컴퓨터
- 무선 전화기는 일종의 전화기이다
- 노트북 컴퓨터는 일종의 컴퓨터이다
- 무선 전화기 is a 전화기
- 노트북 컴퓨터 is a 컴퓨터
- 무선 전화기는 전화기의 기본 기능에 새로운 특성이 추가된 것
- 노트북 컴퓨터는 컴퓨터의 기본 기능에 새로운 특성이 추가된 것
- 이렇듯 is-a 관계는 논리적으로 상속을 기반으로 표현하기에 매우 적절함

### IS-A 기반의 예제
- NoteBookcomp(노트북 컴퓨터)는 Computer(컴퓨터)이다
- TabletNoteBook(타블렛 컴퓨터)는 NoteBookComp(노트북 컴퓨터)이다
- TabletNoteBook(타블렛 컴퓨터)는 Compter(컴퓨터)이다

### HAS-A 관계를 상속으로 구성하면
```cpp
class Gun
{
private:
  int bullet;
public:
  Gun(int bnum) : bullet(bnum)
  {}
  void Shut()
  {
    cout<<"BBANG!";
    bullet--;
  }
};

class Poice : public Gun
{
private:
  int handcuffs;
public:
  Police(int bnum, int bcuff)
    : Gun(bnum), handcuffs(bcuff)
  {}
  void PutHandcuff()
  {
    cout<<"SNAP!";
    handcuffs--;
  }
};
```
- 경찰은 총을 소유한다 : 경찰 has a 총
- has a 관계도 상속으로 구현이 가능함
- 하지만 이러한 경우 Police와 Gun은 강한 연관성을 띠게 됨
- 따라서 총을 소유하지 않은 경찰이나, 다른 무기를 소유하는 경찰을 표현하기가 쉽지 않아짐

### HAS-A 관계는 포함으로 표현함
```cpp
class Gun
{
private:
  int bullet;
public:
  Gun(int bnum) : bullet(bnum)
  {}
  void Shut()
  {
    cout<<"BBANG!";
    bullet--;
  }
};

class Poice
{
private:
  int handcuffs;
  Gun * pistol;
public:
  Police(int bnum, int bcuff)
    : handcuffs(bcuff)
  {
    if(bnum>0)
      pistol=new Gun(bnum);
    else
      pistol=NULL;
  }
  void PutHandcuff()
  {
    cout<<"SNAP!"<<endl;
    handcuffs--;
  }
  void Shut()
  {
    if(pistol==NULL)
      cout<<"Hut BBANG!"<<endl;
    else
      pistol->Shut();
  }
  ~Police()
  {
    if(pistol==NULL)
      delete pistol;
  }
};
```
- has a의 관계를 포함의 형태로 표현하면, 두 클래스 간 연관성은 낮아지며ㅡ 변경 및 확장이 용이해짐
- 즉, 총을 소유하지 않은 경찰의 표현이 쉬워지고, 추가로 무기를 소유하는 형태로의 확장도 간단해짐
