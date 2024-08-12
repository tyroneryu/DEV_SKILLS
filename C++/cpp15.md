# Chapter 15: 예외처리(Exception Handling)
## 예외상황과 예외처리의 이해
### 예외상황을 처리하지 않았을 때의 결과
- 예외상황은 프로그램 실행 중에 발생하는 문제의 상황을 의미함
- 예외상황의 예
  - 나이를 입력하라고 했는데 0보다 작은 값이 입력됨
  - 나눗셈을 위해서 두 개의 숫자를 입력 받았는데, 제수로 0이 입력됨
  - 주민등록번호 13자리만 입력하라고 했는데 중간에 -가 삽입됨
- 이렇듯 예외는 문법적 오류가 아닌, 프로그램 논리에 맞지 않는 오류를 뜻함
```cpp
int main(void)
{
  int num1, num2;
  cout<<"두 개의 숫자 입력: ";
  cin>>num1>>num2;

  cout<<"나눗셈의 몫: "<<num1/num2<<endl;
  cout<<"나눗셈의 나머지: "<<num1%num2<<endl;
  return 0;
}
```
- 입력값이 7, 0이면 프로그램이 중단됨

### if문을 이용한 예외처리
- if문을 이용해서 예외를 발견하고 처리하면, 예외처리 부분과 일반적인 프로그램의 흐름을 쉽게 구분할 수 없음
- if문은 일반적인 프로그램의 논리를 구현하는데 주로 사용됨
- 그래서 C++은 별도의 예외처리 메커니즘을 제공하고 있음

## C++의 예외처리 메커니즘
### C++의 예외처리 메커니즘 이헤: try, catch, throw
```cpp
try
{
  ...
  if(예외가 발생한다면)
    throw expn;
  ...
}
catch (type exn)
{
  // 예외처리
}
```
- try 블록은 예외발생에 대한 검사범위를 지정하는데 사용됨
- catch 블록은 try 블록에서 발생한 예외를 처리하는 영역으로 그 형태가 마치 반환형 없는 함수와 같음
- throw는 예외의 발생을 알리는 역할을 함
- try-catch는 하나의 문장이므로 그 사이에 다른 문장이 삽입될 수 없음

### 예외처리 메커니즘의 적용
```cpp
int main(void)
{
  int num1, num2;
  cout<<"두 개의 숫자 입력: ";
  cin>>num1>>num2;

  try
  {
    if(num2==0)
      throw num2;
    cout<<"나눗셈의 몫: "<<num1/num2<<endl;
    cout<<"나눗셈의 나머지: "<<num1%num2<<endl;
  }
  catch(int expn)
  {
    cout<<"제수는 "<<expn<<"이 될 수 없습니다."<<endl;
    cout<<"프로그램을 다시 실행하세요."<<endl;
  }
  cout<<"end of main"<<endl;
  return 0;
}
```
```
실행 결과 01
두 개의 숫자 입력: 9 2
나눗셈의 몫: 4
나눗셈의 나머지: 1
end of main

실행 결과 02
두 개의 숫자 입력: 7 0
제수는 0이 될수 없습니다.
프로그램을 다시 실행하세요.
end of main
```
- 예외의 발생으로 인해서 try 블록 내에서 throw절이 실행이 되면, try 블록의 나머지 부분은 실행이 되지 않음
- 실행의 흐름을 이해하는 것이 중요
- 실행의 흐름은 try 블록 안으로 들어감
- 그 안에서 예외가 발생하면 이후에 등장하는 catch 블록을 실행하게 됨
- 예외가 발생하지 않으면 try 블록을 빠져나와 try-catch 블록 이후를 실행함

### try 블록을 묶는 기준
- 잘 묶인 경우
```cpp
try
{
  if(num2==0)
    throw num2;
  cout<<"나눗셈의 몫: "<<num1/num2<<endl;
  cout<<"나눗셈의 나머지: "<<num1%num2<<endl;
}
catch(int expn) {...}
```
- 잘못 묶인 경우
```cpp
try
{
  if(num2==0)
    throw num2;
}
catch(int expn) {...}

cout<<"나눗셈의 몫: "<<num1/num2<<endl;
cout<<"나눗셈의 나머지: "<<num1%num2<<endl;
```
- 예외와 연관이 있는 부분을 모두 하나의 try 블록으로 묶어야 함

## Stack Unwinding(스택 풀기)
### 예외의 전달
```cpp
void Divide(int num1, int num2)
{
  if(num2==0)
    throw num2;
  ...
}

int main(void)
{
  ...
  try
  {
    // 함수 호출에서 예외 데이터 전달
    Divide(num1, num2);
    cout<<"나눗셈을 마쳤습니다."<<endl;
  }
  catch(int expn)
  {
    ...
  }
  return 0;
}
```
- Divide 함수 내에서 발생함
- 그런데 발생한 지점을 (throw 절을)감싸는 try 블록이 존재하지 않음
- 예외가 처리되지 않으면, 예외가 발생한 함수를 호출한 영역으로 예외 데이터가(더불어 예외처리에 대한 책임까지) 전달됨

### 예외의 발생위치와 처리위치가 달라야 하는 경우
- 함수의 비정상 종료에 따른 처리를 main 함수에서 해야하므로 예외의 처리는 main 함수에서 진행되어야 함
```cpp
int main(void)
{
  char str1[100];
  char str2[100];
  while(1)
  {
    cout<<"두 개의 숫자 입력";
    cin>>str1>>str2;
    try
    {
      cout<<str1<<" + "<<str2<<" = "<<StoI(str1)+StoI(str2)<<endl;
      break;
    }
    catch(char ch)
    {
      cout<<"문자 "<<ch<<"가 입력되었습니다."<<endl;
      cout<<"재입력 진행합니다."<<endl<<endl;
    }
  }
  cout<<"프로그램을 종료합니다."<<endl;
  return 0;
}
```
- 예외의 데이터가 전달되면 함수는 더 이상 실행되지 않고 종료됨
```cpp
int StoI(char * str)
{
  int len=strlen(str);
  int num=0;

  for(int i=0; i<len; i++)
  {
    if (str[i]<'0' || str[i]>'9')
      throw str[i];
    num+=(int)(pow((double)10, (len-1)-i) * (str[i]+(7-'7')));
  }
  return 0;
}
```

### 스택 풀기(Stack Unwinding)
- main 함수에서조차 예외를 처리하지 않으면, terminate 함수(프로그램을 종료시키는 함수)가 호출되면서 프로그램이 종료됨

### 자료형이 일치하지 않아도 예외 데이터는 전달
```cpp
int SimpleFunc(void)
{
  ...
  try
  {
    if(...)
      throw -1;   // int 형 예외 데이터의 발생
  }
  catch(char expn) {...}  // char 형 예외 데이터를 전달하다
}
```
- 형 변환 발생하지 않아서 예외 데이터는 SimpleFunc 함수를 호출한 영역으로 전달됨

### 하나의 try 블록과 다수의 catch 블록
```cpp
try
{
  out<<str1<<" + "<<str2<<" = "<<StoI(str1)+StoI(str2)<<endl;
  break;
}
catch(char ch)
{
  cout<<"문자 "<<ch<<"가 입력되었습니다."<<endl;
  cout<<"재입력 진행합니다."<<endl<<endl;
}
catch(int expn)
{
  if(expn==0)
    cout<<"0으로 시작하는 숫자는 입력불가."<<endl;
  else
    cout<<"비정상적 입력이 이루어졌습니다."<<endl;
  cout<<"재입력 진행합니다."<<endl;
}
```
- 하나의 try 영역 내에서 종류가 다른 둘 이상의 예외가 발생할 수 있기 때문에, 하나의 try 블록에 다수의 catch 블록을 추가할 수 있음

### 전달되는 예외의 명시
```cpp
int ThrowFunc(int num) throw(int, char)
{
  ...
}
```
- 함수 내에서 예외상황의 발생으로 인해서 int형 예외 데이터와 char형 예외 데이터가 전달될 수 있음을 명시한 선언
- int, char 이외의 예외 데이터가 전달되면, terminate 함수의 호출로 인해 프로그램이 종료됨
- 프로그램의 종료는 대비하지 못한 예외상황의 처리를 알리는 의미로 받아들여짐
```cpp
int SimpleFunc(void) throw()
{
  ...
}
```
- 어떠한 예외가 발생해도 프로그램은 종료가 됨
- 이 함수는 어떠한 예외상황도 발생하지 않는다는 의미의 함수 선언
