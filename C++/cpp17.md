# Complex Data
## 배열과 문자열
- C++는 복합 데이터형을 제공함
- 사용자 정의대로 새로운 데이터형을 만들 수 있음
- 복합 데이터형: 기본 정수형과 부동 소수점형의 조합

### 배열
- 배열(array): 같은 데이터형의 집합
- 선언: `typename arrayName[arraysize];`
  - 예시: `short month[12];`
  1. 배열을 선언하면서 값을 대입하는 초기화의 방식
  2. 배열을 선언한 후 나중에 값을 대입하는 방식

#### 배열을 선언하면서 값을 대입하는 초기화의 방식
- 선언: `short month[12] = { 1, 2, 3 };`
- 배열 원소에 대입할 값들을 콤마로 구분하여 중괄호로 묶어 선언
- 초기화를 선언 이후 나중에 할 수는 없음
- 배열을 다른 배열에 통째로 대입할 수 없음
- 초기화 값의 개수를 배열 원소의 개수보다 모자라게 제공할 수 있음
- 배열을 부분적으로 초기화하면, 나머지 원소들은 모두 -으로 설정함
- 즉, 명시적으로 첫 번째 원소만 0으로 초기화하면, 나머지 원소들은 모두 0으로 초기화됨
- 배열을 초기화할 때 대괄호 속을 비워두면 컴파일러가 초기화 값의 개수를 헤아려 배열 원소 개수를 저장함
- 인덱스 접근 가능

### 문자열
- 문자열: 문자의 열(집합)
  - `char a[6] = { 'H', "e", "l", "l", "o", "\0" };`
- 위의 방식이 아닌 ""을 사용하여 초기화를 하면 따로 NULL 문자를 넣어주지 않아도 됨
  - `char a[] = "Hello";`

## 사용자 입력과 string
### String 예제
```cpp
#include <iostream>
#include <cstring>

using namespace std;

int main()
{
  const int size = 15;
  char name1[size];
  char name2[size] = "C++programing";

  cout<<"안녕하세요 저는 "<<name2;
  cout<<"입니다! 성함이 어떻게 되시나요?\n";
  cin>>name1;
  cout<<"음, "<<name1<<"씨, 당신의 이름은 ";
  cout<<strlen(name1)<<"자입니다\n";
  cout<<sizeof(name1)<<"바이트 크기의 배열에 저장되었습니다.\n";
  cout<<"이름이 "<<nane1[0]<<"자로 시작하는군요.\n";
  name2[3] = "\0";
  cout<<"제 이름의 처음 세 문자는 다음과 같습니다: ";
  cout<<name2<<endl;

  return 0;
}
```
- `#include <cstring>`: `strlen()` 함수를 사용하기 위해서
- `name2[3] = "\0";`: set to null character
- 위의 코드는 입력 값에 space가 있으면 이름이 space 이전까지만으로 인식됨
  - C++은 공백을 만나면 문자열이 끝난 것으로 인식
- `cin.getline(변수명, 입력 최대 크기)`: 를 사용해서 받으면 공백을 포함하여 입력을 받게됨
- `cin.get(변수명, 입력 최대 크기)`: getline과 동일

### String
- string: C++에서 문자열을 다루는 방법 중 하나
- C 스타일로 string 객체를 초기화할 수 있음
- cin을 사용하여 string 객체에 키보드 입력을 저장할 수 있음
- cout을 사용하여 string 객체를 디스플레이 할 수 있음
- 배열 표기를 사용하여 string 객체에 저장되어 있는 개별적인 문자들에 접근할 수 있음
- 배열을 다른 배열에 통째로 대입할 수 없지만 string은 가능
```cpp
#include <iostream>
using namespace std;

int main()
{
  char char1[20];
  char char2[20] = "jaguar";
  string str1;
  string str2 = "panda";

  char1 = char2;  // 불가능
  str1 = str2;    // 가능
  cout<<str1[0]<<endl;
}
```

## 구조체
### 구조체
- 구조체: 다른 데이터 형이 허용되는 데이터의 집합
- 배열은 같은 데이터 형들만의 집합
- 선언 및 사용 방법
```cpp
struct MyStruct
{
  string name;
  string position;
  float height;
  float weight;
}

// 이후 구조체 멤버 변수 사용 방법
MyStruct A;
A.name = "Son";
A.position = "Striker";
A.height = 183;
A.weight = 77;

MyStruct A = {
  "Son",
  "Striker",
  183,
  77
};
```
- 구조체 선언 시, 중괄호 이후에 구조체 이름을 선언하여 초기화 가능
- 구조체 선언 시, 빈 값들은 0으로 초기화 됨
- 구조체도 배열로 선언 가능
```cpp
MyStruct A[2] = {
  {"A", "A", 1, 1},
  {"B", "B", 2, 2}
};
```
- 위 경우 멤버변수 사용 시: `A[0].height` 같은 방식으로 사용 가능

## 공용체와 열거체
### 공용체 (Union)
- 공용체: 서로 다른 데이터 형을 한 번에 한 가지만 보관할 수 있는 집합
```cpp
union MyUnion
{
  int intVal;
  long longVal;
  float floatVal;
};
```
- int 형 보관할 때는 int 형만 보관하고, 이후에 다른 데이터 형이 들어오면 이전의 데이터를 제거하고 할당
- 메모리 절약 가능하다는 장점

### 열거체 (enum)
- 열거체: 기호 상수를 만드는 것에 대한 또다른 방법
```cpp
enum spectrum { red, orange, yellow, green, blue, violet, indigo }
```
- spectrum을 새로운 데이터형 이름으로 만듬
- red, orange, yellow, ... 0에서부터 7까지 정수 값을 각각 나타내는 기호 상수로 만듬
```cpp
spectrum a = orange;
cout<<a<<endl;

// 위 실행 시 1 출력 (상수니까)
```
- 열거자들 간의 산술 연산은 허용되지 않음
- 특별히 옵션을 걸지 않으면 0부터 할당됨
```cpp
enum spectrum { red = 0, orange = 2, yellow = 4, green, blue, violet, indigo }
```
- 위처럼 선언 시 green부터는 이전의 값 +1 값이 할당됨

## 포인터와 메모리 해제 01
### 변수 선언 과정
- 변수의 데이터형 + 변수명 + 변수의 초기화 값
- C++: 객체 지향 프로그래밍
- 컴파일 시간이 아닌 실행 시간에 어떠한 결정을 내릴 수 있음
- 배열을 하나 생성하고자 할 경우
  - 재래적 절차적 프로그래밍: 배열의 크기가 미리 결정
  - 객체 지향 프로그래밍: 배열의 크기를 실행 시간 결정
- 포인터: 사용할 주소에 이름을 붙임
- 즉, 포인터는 포인터의 이름이 주소를 나타냄
- 간접값 연산자, 간접 참조 연산자 *
```cpp
int main()
{
  int *a;     // C style
  int* b;     // C++ style
  int* c, d;  // c는 포인터 변수, d는 int형 변수

  return 0
}
```
```cpp
int main()
{
  int a = 6;
  int* b;
  b = &a;

  cout<<"a의 값: "<<a<<endl;
  cout<<"*b의 값: "<<*b<<endl;

  cout<<"a의 주소: "<<&a<<endl;
  cout<<"*b의 주소: "<<b<<endl;

  *b = *b + 1;
  cout<<"이제 a의 값은: "<<a<<endl;

  return 0;
}
```

## 포인터와 메모리 해제 02
### new 연산자
- new 연산자: 어떤 데이터형을 원하는지 new 연산자에게 알려주면, new 연산자는 그에 알맞은 크기의 메모리 블록을 찾아내고 그 블록의 주소를 리턴함
```cpp
int main()
{
  int* pointer = new int;
}
```

### delete 연산자
- 사용한 메모리를 다시 메모리 풀로 환수
- 환수된 메모리는 프로그램의 다른 부분이 다시 사용
```cpp
int main()
{
  int* ps = new int;
  // 메모리 사용
  delete ps;
}
```

#### delete를 사용하는 규칙
1. new로 대입하지 않은 메모리는 delete로 해제할 수 없음
2. 같은 메모리 블록을 연달아 두 번 delete로 해제할 수 없음
3. new[]로 메모리를 대입할 경우 delete[]로 해제함
4. 대괄호를 사용하지 않았다면 delete도 대괄호를 사용하지 않아야 함
```cpp
#include <iostream>

using namespace std;

int main(){
    // double형 데이터 3개를 저장할 수 있는 공간을 대입
    double* p3 = new double[3];
    p3[0] = 0.2;  // p3를 배열 이름처럼 취급함
    p3[1] = 0.5;
    p3[2] = 0.8;

    cout << "p3[1] is " << p3[1] << ".\n";

    p3 = p3 + 1;  // 포인터를 증가시킴

    cout << "Now p3[0] is " << p3[0] << " and ";
    cout << "p3[1] is " << p3[1] << "\n.";

    p3 = p3-1;    // 다시 시작 위치를 지시함
    delete[] p3;  // 배열 메모리를 해제
    // cin.get();
    return 0;
}
```

## 포인터 연산
### 포인터와 배열/문자열
```cpp
#include <iostream>
#define SIZE 20

using namespace std;

int main(){

    char animal[SIZE];
    char* ps;

    cout << "동물 이름을 입력하십시오.\n";
    cin >> animal;

    ps = new char[strlen(animal) +1];
    strcpy(ps, animal);

    cout << "입력하신 동물 이름을 복사하였습니다." << endl;
    cout << "입력하신 동물 이름은 " << animal << "이고, 그 주소는 " << (int*)animal << " 입니다." << endl;
    cout << "복사된 동물 이름은 " << ps << "이고, 그 주소는 " << (int*)ps << " 입니다." << endl;
     
    delete[] ps;
    return 0;
}
```
```cpp
#include <iostream>
#define SIZE 20

using namespace std;

struct MyStruct
{
    char name[20];
    int age;
};

int main(){

    MyStruct* temp = new MyStruct;

    cout << "당신의 이름을 입력하십시오.\n";
    cin >> temp->name;

    cout << "당신의 나이를 입력하십시오.\n";
    cin >> (*temp).age;

    cout << "안녕하세요! " << temp->name << "씨!\n";
    cout << "당신은 " << temp->age << "살 이군요!\n";
    
    delete temp;
    return 0;
}
```
- 구조체 멤버변수 접근 시 ->(화살표 연산자)로도 접근 가능
