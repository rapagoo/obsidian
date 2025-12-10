---
tags:
  - cpp
---
[모두의 코드 URL](https://modoocode.com/category/C++)
## 1 - 2. 첫 C++ 프로그램 분석하기
주로 namespace에 관한 내용 학습. 
"using namespace std" 는 가급적 사용하지 말자!
## 1 - 3. C++은 C 친구 - C와 공통점
Google에서는 변수의 이름을 지을 때는 띄어쓰기 부분에 \_\ 를 넣고,
함수의 경우는 대문자를 사용하는 방식을 취한다.
[[C++ Quick Note#Pointers]] 포인터 내용 참고
***
```cpp
// C++ 의 for 문
#include <iostream>

int main() {
  int i;

  for (i = 0; i < 10; i++) {
    std::cout << i << std::endl;
  }
  return 0;
}
```

```cpp
/* 1 부터 10 까지 합*/
#include <iostream>

int main() {
  int i, sum = 0;

  for (i = 1; i <= 10; i++) {
    sum += i;
  }

  std::cout << "합은 : " << sum << std::endl;
  return 0;
}
```
기존 C의 for문과 C++의 for문은 변한 것이 없다. 
다만 C++에서는 변수의 선언이 반드시 최상단에 있어야 되는 것은 아니다.
```cpp
/* while 문 이용하기 */
#include <iostream>

int main() {
  int i = 1, sum = 0;

  while (i <= 10) {
    sum += i;
    i++;
  }

  std::cout << "합은 : " << sum << std::endl;
  return 0;
}
```

```cpp
/* 행운의 숫자 맞추기*/
#include <iostream>

int main() {
  int lucky_number = 3;
  std::cout << "내 비밀 수를 맞추어 보세요~" << std::endl;

  int user_input;  // 사용자 입력

  while (1) {
    std::cout << "입력 : ";
    std::cin >> user_input;
    if (lucky_number == user_input) {
      std::cout << "맞추셨습니다~~" << std::endl;
      break;
    } else {
      std::cout << "다시 생각해보세요~" << std::endl;
    }
  }
  return 0;
}
```
while문과 if-else문 역시 C와 동일하다.
```cpp
// switch 문 이용하기
#include <iostream>

using std::cout;
using std::endl;
using std::cin;

int main() {
  int user_input;
  cout << "저의 정보를 표시해줍니다" << endl;
  cout << "1. 이름 " << endl;
  cout << "2. 나이 " << endl;
  cout << "3. 성별 " << endl;
  cin >> user_input;

  switch (user_input) {
    case 1:
      cout << "Psi ! " << endl;
      break;

    case 2:
      cout << "99 살" << endl;
      break;

    case 3:
      cout << "남자" << endl;
      break;

    default:
      cout << "궁금한게 없군요~" << endl;
      break;
  }
  return 0;
}
```
switch문 역시 동일하다.
## 2. C++ 참조자(레퍼런스)의 도입
```cpp
#include <iostream>

int change_val(int *p) {
  *p = 3;

  return 0;
}
int main() {
  int number = 5;

  std::cout << number << std::endl;
  change_val(&number);
  std::cout << number << std::endl;
}
//실행 결과
5
3
```
`change_val` 함수의 인자 `p` 에 `number` 의 주소값을 전달하여, `*p` 를 통해 `number` 를 참조하여 `number` 의 값을 3 으로 바꾸었다.
***
C 에서는 어떠한 변수를 가리키고 싶을 때 반드시 포인터를 사용해야 했다. 하지만 C++ 에서는 다른 변수나 상수를 가리키는 방법으로 또 다른 방식을 제공하는데, 이를 바로 **참조자(레퍼런스 - reference)** 라고 부른다.
```cpp
#include <iostream>

int main() {
  int a = 3;
  int& another_a = a;

  another_a = 5;
  std::cout << "a : " << a << std::endl;
  std::cout << "another_a : " << another_a << std::endl;

  return 0;
}
//실행 결과
a : 5
another_a : 5
```
`another_a` 는 `a` 의 또다른 이름이라고 컴파일러에게 참조자를 통해 알려준다.
**레퍼런스는 반드시 초기화를 해야 한다**
포인터는 `int* p;` 와 같은 문장이 가능하지만,
레퍼런스는 `int& another_a;` 와 같은 문장이 불가능하다.
**레퍼런스가 한 번 별명이 되면 절대로 다른 이의 별명이 될 수 없다**
포인터는 누구를 가리키는지 자유롭게 바꿀 수 있지만, 레퍼런스는 불가능하다.
**레퍼런스는 메모리 상에 존재하지 않을 수도 있다**
포인터는 정의하는 순간 메모리 상에서 8비트를 차지한다. 하지만 위의 `another_a` 와 같은 상황에서는, 레퍼런스가 굳이 메모리 상에 존재할 이유가 없다. `another_a` 가 존재하는 자리는 모두 `a` 로 바꾸면 되니까. 하지만 모든 상황에서 레퍼런스가 항상 메모리 상에 존재하지 않는 것은 아니다.
***
```cpp
#include <iostream>

int change_val(int &p) {
  p = 3;

  return 0;
}
int main() {
  int number = 5;

  std::cout << number << std::endl;
  change_val(number);
  std::cout << number << std::endl;
}
//실행 결과
5
3
```
`change_val` 함수 내에서, `int& p` 를 인자로 받기 때문에 사실상 `int& p = number;` 와 동일한 문장이 된다. 이후 `p = 3;` 은 `number = 3;` 과 완전히 동일하다.
c++ 문법에서는 참조자의 참조자를 만드는 것이 금지되어 있다.
```cpp
std::cin >> user_input;
```
`cin` 은 레퍼런스로 `user_input` 을 받는다.
```cpp
const int &ref = 4;
```
리터럴을 참조하려면 상수 참조자를 선언한다.
`int a = ref;` 는 `a = 4;` 와 동일하게 처리된다.
***
**레퍼런스의 배열과 배열의 레퍼런스**
레퍼런스의 배열은 불가능, 배열의 레퍼런스는 가능하다.
```cpp
int a, b;
int& arr[2] = {a, b}; //레퍼런스의 배열, 불가능
```

```cpp
int arr[3] = {1, 2, 3};
int (&ref)[3] = arr; //배열의 레퍼런스, 가능
```
배열의 레퍼런스를 사용할 때는 반드시 배열의 크기를 명시해야 한다.
***
**레퍼런스를 리턴하는 함수**
```cpp
int& function() {
  int a = 2;
  return a;
}

int main() {
  int b = function();
  b = 3;
  return 0;
}
//int b = function(); 문장은 사실상
int& ref = a;

// 근데 a 가 사라짐
int b = ref;  // !!!
```
function의 리턴 타입은 int&(참조자)인데, 정작 a는 함수의 리턴과 함께 사라진다.
이와 같이 레퍼런스는 있는데 원래 참조하려던 것이 사라진 레퍼런스를 댕글링 레퍼런스라고 부른다.
따라서 위 처럼 레퍼런스를 리턴하는 함수에서 지역 변수의 레퍼런스를 리턴하지 않도록 조심해야 한다.
```cpp
int& function(int& a) {
  a = 5;
  return a;
}

int main() {
  int b = 2;
  int c = function(b);
  return 0;
}
```
위 경우에는 인자로 받은 레퍼런스를 그대로 리턴하고 있기 때문에, 함수가 리턴한 참조자는 
아직 살아있는 변수인 `b`를 계속 참조하게 된다.
결국 c에 b의 값인 5를 대입하는 것과 동일하다.
그렇다면 이렇게 참조자를 리턴하는 경우의 장점이 무엇일까? 
C 언어에서 엄청나게 큰 구조체가 있을 때 해당 구조체 변수를 그냥 리턴하면 전체 복사가 발생해야 해서 시간이 오래 걸리지만, 해당 구조체를 가리키는 포인터를 리턴한다면 그냥 포인터 주소 한 번 복사로 매우 빠르게 끝난다.
마찬가지로 레퍼런스를 리턴하게 된다면 레퍼런스가 참조하는 타입의 크기와 상관 없이 딱 한 번의 주소값 복사로 전달이 끝나게 된다. 따라서 매우 효율적이다.
```cpp
int function() {
  int a = 5;
  return a;
}

int main() {
  int& c = function();
  return 0;
}
```
함수 리턴 시 사라지는 값을 레퍼런스로 받았기 때문에 댕글링 레퍼런스가 되어버린다. 불가능
하지만 중요한 예외 규칙이 있다.
```cpp
#include <iostream>

int function() {
  int a = 5;
  return a;
}

int main() {
  const int& c = function();
  std::cout << "c : " << c << std::endl;
  return 0;
}
```
const로 받았더니 문제없이 컴파일 된다. 
예외적으로 상수 레퍼런스로 리턴값을 받게 되면 레퍼런스가 사라질 때 까지 해당 리턴값의 생명이 연장된다.
## 3. C++의 세계로 오신 것을 환영합니다. (new, delete)
new/delete로 할당 및 해제
new를 통해 할당된 메모리 공간만 delete로 해제 가능
변수 선언 위치에 따른 차이점
## 4 - 1. 이 세상은 객체로 이루어져 있다
*객체란, 변수들과 참고 자료로 이루어진 소프트웨어 덩어리 이다.*
**인스턴스 변수/인스턴스 메소드**
객체에 정의되어 있는 변수/함수를 말한다.
![[Pasted image 20241004185827.png]]
해당 그림은 흔히 객체를 나타내기 위한 그림이다.
메소드가 변수를 감싸고 있는 것처럼 그리는 이유는, 실제로 변수들이 외부로부터 보호되고 있기 때문이다.
외부에서는 객체의 인스턴스 변수의 값을 바꿀 수 없고, 인스턴스 함수를 통해서 변경할 수 있다.
인스턴스 메소드를 통해서 간접적으로 인스턴스 변수의 값을 조절하는 것을 **캡슐화** 라고 부른다.
***
**클래스**
C++에서는 객체를 만들어낼 수 있는 장치, 다시 말해 설계도를 준비해 두었다.
그것이 바로 클래스(class)이다.
![[Pasted image 20241004190243.png]]
위와 같이 내용은 비어있는 빈 껍질로 이루어진 설계도라고 생각할 수 있다.
이 설계도를 통하여 객체를 만들어 낸다.
```cpp
#include <iostream>

class Animal {
 private:
  int food;
  int weight;

 public:
  void set_animal(int _food, int _weight) {
    food = _food;
    weight = _weight;
  }
  void increase_food(int inc) {
    food += inc;
    weight += (inc / 3);
  }
  void view_stat() {
    std::cout << "이 동물의 food   : " << food << std::endl;
    std::cout << "이 동물의 weight : " << weight << std::endl;
  }
};  // 세미콜론 잊지 말자!

int main() {
  Animal animal;
  animal.set_animal(100, 50);
  animal.increase_food(30);

  animal.view_stat();
  return 0;
}
```
`Animal` 인스턴스를 생성할 때는 구조체와 다르게 그냥 `int`나 `char`처럼 `Animal`이라고 적어주면 된다.
food, weight는 멤버 변수, `set_animal` 등은 멤버 함수라고 부른다.
## 4 - 2. 클래스의 세계로 오신 것을 환영합니다.(함수의 오버로딩, 생성자)
**함수의 오버로딩**
C++에서는 같은 이름을 가진 함수가 여러 개 존재할 수 있다.
함수 호출 시 사용하는 인자를 보고 구분한다.
**함수 오버로딩 과정**
1단계: 자신과 타입이 정확히 일치하는 함수를 찾는다.
2단계: 정확히 일치하는 타입이 없는 경우 형변환을 통해서 일치하는 함수를 찾는다.
- `Char, unsigned char, short` 는 `int` 로 변환된다.
- `Unsigned short` 는 `int` 의 크기에 따라 `int` 혹은 `unsigned int` 로 변환된다.
- `Float` 은 `double` 로 변환된다.
- `Enum` 은 `int` 로 변환된다.
3단계: 위와 같이 변환해도 일치하는 것이 없다면,좀 더 포괄적인 형변환을 통해 일치하는 함수를 찾는다.
- 임의의 숫자(numeric) 타입은 다른 숫자 타입으로 변환된다. (예를 들어 `float -> int)`
- `Enum` 도 임의의 숫자 타입으로 변환된다 (예를 들어 `Enum -> double)`
- `0` 은 포인터 타입이나 숫자 타입으로 변환된 0 은 포인터 타입이나 숫자 타입으로 변환된다
- 포인터는 `void` 포인터로 변환된다.
4단계: 유저 정의된 타입 변환으로 일치하는 것을 찾는다.
```cpp
// 모호한 오버로딩
#include <iostream>

void print(int x) { std::cout << "int : " << x << std::endl; }
void print(char x) { std::cout << "double : " << x << std::endl; }

int main() {
  int a = 1;
  char b = 'c';
  double c = 3.2f;

  print(a);
  print(b);
  print(c);

  return 0;
}
```
`print(c);` 를 하였을 때, 1단계에서 일치하는 것이 없음. 2단계로 넘어간다.
2단계에서는 `double` 의 캐스팅에 관한 내용이 없다. 3단계로 넘어간다.
3단계에서는 '임의의 숫자 타입이 임의의 숫자 타입' 으로 변환되서 생각되기 때문에 `double` 은 `char` 도, `int` 도 변환 될 수 있게 된다. 따라서 같은 단계에 두 개 이상의 가능한 일치가 존재하므로 오류가 발생한다.
이러한 오류가 발생하지 않도록 C++ 오버로딩 규칙을 잘 숙지하자.
***
```cpp
#include<iostream>

    class Date {
  int year_;
  int month_;  // 1 부터 12 까지.
  int day_;    // 1 부터 31 까지.

 public:
  void SetDate(int year, int month, int date);
  void AddDay(int inc);
  void AddMonth(int inc);
  void AddYear(int inc);

  // 해당 월의 총 일 수를 구한다.
  int GetCurrentMonthTotalDays(int year, int month);

  void ShowDate();
};

void Date::SetDate(int year, int month, int day) {
  year_ = year;
  month_ = month;
  day_ = day;
}

int Date::GetCurrentMonthTotalDays(int year, int month) {
  static int month_day[12] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
  if (month != 2) {
    return month_day[month - 1];
  } else if (year % 4 == 0 && year % 100 != 0) {
    return 29;  // 윤년
  } else {
    return 28;
  }
}

void Date::AddDay(int inc) {
  while (true) {
    // 현재 달의 총 일 수
    int current_month_total_days = GetCurrentMonthTotalDays(year_, month_);

    // 같은 달 안에 들어온다면;
    if (day_ + inc <= current_month_total_days) {
      day_ += inc;
      return;
    } else {
      // 다음달로 넘어가야 한다.
      inc -= (current_month_total_days - day_ + 1);
      day_ = 1;
      AddMonth(1);
    }
  }
}

void Date::AddMonth(int inc) {
  AddYear((inc + month_ - 1) / 12);
  month_ = month_ + inc % 12;
  month_ = (month_ == 12 ? 12 : month_ % 12);
}

void Date::AddYear(int inc) { year_ += inc; }

void Date::ShowDate() {
  std::cout << "오늘은 " << year_ << " 년 " << month_ << " 월 " << day_
            << " 일 입니다 " << std::endl;
}

int main() {
  Date day;
  day.SetDate(2011, 3, 1);
  day.ShowDate();

  day.AddDay(30);
  day.ShowDate();

  day.AddDay(2000);
  day.ShowDate();

  day.SetDate(2012, 1, 31);  // 윤년
  day.AddDay(29);
  day.ShowDate();

  day.SetDate(2012, 8, 4);
  day.AddDay(2500);
  day.ShowDate();
  return 0;
}
```
이 코드에서처럼 함수의 정의만 클래스 안에 넣어주고, 외부에서 멤버 함수를 정의해줄 수 있다.
main 함수를 살펴보면, day 인스턴스를 생성하고 `SetDate` 로 초기화를 해주었다.
만약 `SetDate` 를 하지 않는다면? 초기화 되지 않은 값에 덧셈과 출력 명령이 내려지기 때문에
이상한 쓰레기 값이 출력될 것이다.
만약 실수로 생성한 객체를 초기화 하지 않는다면?
이러한 일을 방지하고자 C++에 존재하는 것이 바로 생성자이다.
***
**생성자(Constructor)**
```cpp
#include <iostream>

class Date {
  int year_;
  int month_;  // 1 부터 12 까지.
  int day_;    // 1 부터 31 까지.

 public:
  void SetDate(int year, int month, int date);
  void AddDay(int inc);
  void AddMonth(int inc);
  void AddYear(int inc);

  // 해당 월의 총 일 수를 구한다.
  int GetCurrentMonthTotalDays(int year, int month);

  void ShowDate();

  Date(int year, int month, int day) {
    year_ = year;
    month_ = month;
    day_ = day;
  }
};

// 생략

void Date::AddYear(int inc) { year_ += inc; }

void Date::ShowDate() {
  std::cout << "오늘은 " << year_ << " 년 " << month_ << " 월 " << day_
            << " 일 입니다 " << std::endl;
}
int main() {
  Date day(2011, 3, 1);
  day.ShowDate();

  day.AddYear(10);
  day.ShowDate();

  return 0;
}
```
먼저, 생성자를 정의할 때는 클래스와 동일한 이름으로 정의해야 한다.
또한 생성자는 반환 타입이 없는 것도 특징이다.
Date 클래스의 객체인 day를 만들 때 (2011,3,1)로 생성자 `Date(int year...)` 를 호출한 것이다.
이는 암시적 방법으로, `Date day = Date(2012, 3, 1);` 이런 방식으로 선언하는 명시적 방법도 존재한다. 허나 암시적 방법으로 축약하여 사용할 수 있기에 많은 경우 암시적 방법을 사용한다.
**디폴트 생성자**
디폴트 생성자는 인자를 하나도 가지지 않는 생성자인데, 클래스에서 사용자가 어떠한 생성자도 명시적으로 정의하지 않았을 경우에 컴파일러가 자동으로 추가해주는 생성자이다.
사용자가 직접 디폴트 생성자를 정의할 수도 있다.
```cpp
// 디폴트 생성자 정의해보기
#include <iostream>

class Date {
  int year_;
  int month_;  // 1 부터 12 까지.
  int day_;    // 1 부터 31 까지.

 public:
  void ShowDate();

  Date() {
    year_ = 2012;
    month_ = 7;
    day_ = 12;
  }
};

void Date::ShowDate() {
  std::cout << "오늘은 " << year_ << " 년 " << month_ << " 월 " << day_
            << " 일 입니다 " << std::endl;
}

int main() {
  Date day = Date();
  Date day2;

  day.ShowDate();
  day2.ShowDate();

  return 0;
}
```
멤버 변수 초기화를 위해 디폴트 생성자를 직접 정의한 예시이다.
명시적으로 디폴트 생성자를 사용하는 방법도 존재하는데,
```cpp
class Test {
 public:
  Test() = default;  // 디폴트 생성자를 정의해라
};
```
이런 식으로 가능하다.
이전에는 디폴트 생성자를 사용하고 싶으면 그저 생성자를 정의하지 않는 방법 뿐이었으나,
C++11 이후로는 `= default` 를 생성자의 선언 뒤에 붙여주게 되면 디폴트 생성자를 정의하라고 컴파일러에게 명시적으로 알려줄 수 있다.
***
**생성자 오버로딩**
생성자 역시 함수이기 때문에 함수의 오버로딩이 적용될 수 있다.
쉽게 말해 해당 클래스의 객체를 여러 가지 방식으로 생성할 수 있다.
```cpp
#include <iostream>

class Date {
  int year_;
  int month_;  // 1 부터 12 까지.
  int day_;    // 1 부터 31 까지.

 public:
  void ShowDate();

  Date() {
    std::cout << "기본 생성자 호출!" << std::endl;
    year_ = 2012;
    month_ = 7;
    day_ = 12;
  }

  Date(int year, int month, int day) {
    std::cout << "인자 3 개인 생성자 호출!" << std::endl;
    year_ = year;
    month_ = month;
    day_ = day;
  }
};

void Date::ShowDate() {
  std::cout << "오늘은 " << year_ << " 년 " << month_ << " 월 " << day_
            << " 일 입니다 " << std::endl;
}
int main() {
  Date day = Date();
  Date day2(2012, 10, 31);

  day.ShowDate();
  day2.ShowDate();

  return 0;
}
```
## 4 - 3. 스타크래프트를 만들자 ① (복사 생성자, 소멸자)
```cpp fold title:Marine
#include <iostream>

class Marine {
  int hp;                // 마린 체력
  int coord_x, coord_y;  // 마린 위치
  int damage;            // 공격력
  bool is_dead;

 public:
  Marine();              // 기본 생성자
  Marine(int x, int y);  // x, y 좌표에 마린 생성

  int attack();                       // 데미지를 리턴한다.
  void be_attacked(int damage_earn);  // 입는 데미지
  void move(int x, int y);            // 새로운 위치

  void show_status();  // 상태를 보여준다.
};
Marine::Marine() {
  hp = 50;
  coord_x = coord_y = 0;
  damage = 5;
  is_dead = false;
}
Marine::Marine(int x, int y) {
  coord_x = x;
  coord_y = y;
  hp = 50;
  damage = 5;
  is_dead = false;
}
void Marine::move(int x, int y) {
  coord_x = x;
  coord_y = y;
}
int Marine::attack() { return damage; }
void Marine::be_attacked(int damage_earn) {
  hp -= damage_earn;
  if (hp <= 0) is_dead = true;
}
void Marine::show_status() {
  std::cout << " *** Marine *** " << std::endl;
  std::cout << " Location : ( " << coord_x << " , " << coord_y << " ) "
            << std::endl;
  std::cout << " HP : " << hp << std::endl;
}

int main() {
  Marine marine1(2, 3);
  Marine marine2(3, 5);

  marine1.show_status();
  marine2.show_status();

  std::cout << std::endl << "마린 1 이 마린 2 를 공격! " << std::endl;
  marine2.be_attacked(marine1.attack());

  marine1.show_status();
  marine2.show_status();
}
```
객체의 내부적 성질이나 상태 등은 private으로, 그 객체가 외부에 하는 행동들은 함수로 구현해 public으로 둔다.
이 코드의 문제는, 컴파일 시점에 몇 개의 마린을 생성할 지 정해야 한다는 것이다. 개별적으로 이름을 붙이기도 힘들고...
그럼 답은? marine들을 배열로 정해버리자.
```cpp fold title:Marines_Array
/* int main 전 까지 내용은 동일 */
int main() {
  Marine* marines[100];

  marines[0] = new Marine(2, 3);
  marines[1] = new Marine(3, 5);

  marines[0]->show_status();
  marines[1]->show_status();

  std::cout << std::endl << "마린 1 이 마린 2 를 공격! " << std::endl;

  marines[0]->be_attacked(marines[1]->attack());

  marines[0]->show_status();
  marines[1]->show_status();

  delete marines[0];
  delete marines[1];
}
```
 이러한 방식으로 배열을 설정해 마린들을 런타임 시점에서 생성해줄 수 있다.
 참고로 new 는 객체를 동적으로 생성하면서, 동시에 자동으로 생성자도 호출해 준다.
***
**소멸자**
각각의 마린에 이름을 지정해줄 수도 있다.
`Marine` 클래스에 이름을 저장할 수 있는 `name` 이라는 인스턴스 변수를 만들자
```cpp fold title:naming_marines 
// 마린의 이름 만들기
#include <string.h>
#include <iostream>

class Marine {
  int hp;                // 마린 체력
  int coord_x, coord_y;  // 마린 위치
  int damage;            // 공격력
  bool is_dead;
  char* name;  // 마린 이름

 public:
  Marine();                                       // 기본 생성자
  Marine(int x, int y, const char* marine_name);  // 이름까지 지정
  Marine(int x, int y);  // x, y 좌표에 마린 생성

  int attack();                       // 데미지를 리턴한다.
  void be_attacked(int damage_earn);  // 입는 데미지
  void move(int x, int y);            // 새로운 위치

  void show_status();  // 상태를 보여준다.
};
Marine::Marine() {
  hp = 50;
  coord_x = coord_y = 0;
  damage = 5;
  is_dead = false;
  name = NULL;
}
Marine::Marine(int x, int y, const char* marine_name) {
  name = new char[strlen(marine_name) + 1];
  strcpy(name, marine_name);

  coord_x = x;
  coord_y = y;
  hp = 50;
  damage = 5;
  is_dead = false;
}
Marine::Marine(int x, int y) {
  coord_x = x;
  coord_y = y;
  hp = 50;
  damage = 5;
  is_dead = false;
  name = NULL;
}
void Marine::move(int x, int y) {
  coord_x = x;
  coord_y = y;
}
int Marine::attack() { return damage; }
void Marine::be_attacked(int damage_earn) {
  hp -= damage_earn;
  if (hp <= 0) is_dead = true;
}
void Marine::show_status() {
  std::cout << " *** Marine : " << name << " ***" << std::endl;
  std::cout << " Location : ( " << coord_x << " , " << coord_y << " ) "
            << std::endl;
  std::cout << " HP : " << hp << std::endl;
}

int main() {
  Marine* marines[100];

  marines[0] = new Marine(2, 3, "Marine 2");
  marines[1] = new Marine(1, 5, "Marine 1");

  marines[0]->show_status();
  marines[1]->show_status();

  std::cout << std::endl << "마린 1 이 마린 2 를 공격! " << std::endl;

  marines[0]->be_attacked(marines[1]->attack());

  marines[0]->show_status();
  marines[1]->show_status();

  delete marines[0];
  delete marines[1];
}
```
그런데 이 코드에서, 생성하는 마린의 이름을 넣을 때 new를 사용했다.
그렇다면 동적 할당된 char 배열에 대한 delete는 언제 이루어 지는 걸까?
이는 자동으로 이루어지지 않는다.
그럼 그냥 main 함수 끝에서 Marine이 delete될 때, 우리가 생성했던 객체가 자동으로 소멸될 때 호출되는 함수는 없을까? 이것이 소멸자이다.
```cpp title:Destructor
#include <string.h>
#include <iostream>

class Marine {
  int hp;                // 마린 체력
  int coord_x, coord_y;  // 마린 위치
  int damage;            // 공격력
  bool is_dead;
  char* name;  // 마린 이름

 public:
  Marine();                                       // 기본 생성자
  Marine(int x, int y, const char* marine_name);  // 이름까지 지정
  Marine(int x, int y);  // x, y 좌표에 마린 생성
  ~Marine();

  int attack();                       // 데미지를 리턴한다.
  void be_attacked(int damage_earn);  // 입는 데미지
  void move(int x, int y);            // 새로운 위치

  void show_status();  // 상태를 보여준다.
};
Marine::Marine() {
  hp = 50;
  coord_x = coord_y = 0;
  damage = 5;
  is_dead = false;
  name = NULL;
}
Marine::Marine(int x, int y, const char* marine_name) {
  name = new char[strlen(marine_name) + 1];
  strcpy(name, marine_name);

  coord_x = x;
  coord_y = y;
  hp = 50;
  damage = 5;
  is_dead = false;
}
Marine::Marine(int x, int y) {
  coord_x = x;
  coord_y = y;
  hp = 50;
  damage = 5;
  is_dead = false;
  name = NULL;
}
void Marine::move(int x, int y) {
  coord_x = x;
  coord_y = y;
}
int Marine::attack() { return damage; }
void Marine::be_attacked(int damage_earn) {
  hp -= damage_earn;
  if (hp <= 0) is_dead = true;
}
void Marine::show_status() {
  std::cout << " *** Marine : " << name << " ***" << std::endl;
  std::cout << " Location : ( " << coord_x << " , " << coord_y << " ) "
            << std::endl;
  std::cout << " HP : " << hp << std::endl;
}
Marine::~Marine() {
  std::cout << name << " 의 소멸자 호출 ! " << std::endl;
  if (name != NULL) {
    delete[] name;
  }
}
int main() {
  Marine* marines[100];

  marines[0] = new Marine(2, 3, "Marine 2");
  marines[1] = new Marine(1, 5, "Marine 1");

  marines[0]->show_status();
  marines[1]->show_status();

  std::cout << std::endl << "마린 1 이 마린 2 를 공격! " << std::endl;

  marines[0]->be_attacked(marines[1]->attack());

  marines[0]->show_status();
  marines[1]->show_status();

  delete marines[0];
  delete marines[1];
}
```
소멸자의 사용법은 클래스 이름 앞에 ~만 붙여주면 된다.
예를 들어 Marine 클래스의 경우 `~Marine();` 이 된다.
소멸자가 생성자와 다른 점은, 인자를 아무것도 가지지 않는다는 점이다.
***
**복사 생성자**
스타크래프트의 "포토캐논 겹치기" 를 만들어본다고 생각했을 때,
각각의 캐논을 일일히 생성자로 생성하는 것은 쉽지 않다.
반면 1개만 생성해 놓고, 나머지를 복사 생성 할 수도 있다.
```cpp title:Photon_Cannon
// 포토캐논
#include <string.h>
#include <iostream>

class Photon_Cannon {
  int hp, shield;
  int coord_x, coord_y;
  int damage;

 public:
  Photon_Cannon(int x, int y);
  Photon_Cannon(const Photon_Cannon& pc);

  void show_status();
};
Photon_Cannon::Photon_Cannon(const Photon_Cannon& pc) {
  std::cout << "복사 생성자 호출 !" << std::endl;
  hp = pc.hp;
  shield = pc.shield;
  coord_x = pc.coord_x;
  coord_y = pc.coord_y;
  damage = pc.damage;
}
Photon_Cannon::Photon_Cannon(int x, int y) {
  std::cout << "생성자 호출 !" << std::endl;
  hp = shield = 100;
  coord_x = x;
  coord_y = y;
  damage = 20;
}
void Photon_Cannon::show_status() {
  std::cout << "Photon Cannon " << std::endl;
  std::cout << " Location : ( " << coord_x << " , " << coord_y << " ) "
            << std::endl;
  std::cout << " HP : " << hp << std::endl;
}
int main() {
  Photon_Cannon pc1(3, 3);
  Photon_Cannon pc2(pc1);
  Photon_Cannon pc3 = pc2;

  pc1.show_status();
  pc2.show_status();
}
```
위 코드에서 볼 수 있듯, 복사 생성자는 `T(const T& a);` 이런 식으로 정의한다.
여기서 a는 다른 T의 객체를 상수 레퍼런스로 받았기 때문에, 복사 생성자 내부에서 a의 데이터를 변경할 수 없다. 
근데 사실 컴파일러가 이미 디폴트 복사 생성자를 지원한다.
위 코드에서 복사 생성자를 지우고 코드를 실행해도 똑같은 결과가 나온다.
그래서 간단한 클래스의 경우 따로 복사 생성자를 만들어 주지 않아도 복사 생성이 가능하다.
***
**디폴트 복사 생성자의 한계**
```cpp title:LimitOfDefaultCopy
// 디폴트 복사 생성자의 한계
#include <string.h>
#include <iostream>

class Photon_Cannon {
  int hp, shield;
  int coord_x, coord_y;
  int damage;

  char *name;

 public:
  Photon_Cannon(int x, int y);
  Photon_Cannon(int x, int y, const char *cannon_name);
  ~Photon_Cannon();

  void show_status();
};

Photon_Cannon::Photon_Cannon(int x, int y) {
  hp = shield = 100;
  coord_x = x;
  coord_y = y;
  damage = 20;

  name = NULL;
}
Photon_Cannon::Photon_Cannon(int x, int y, const char *cannon_name) {
  hp = shield = 100;
  coord_x = x;
  coord_y = y;
  damage = 20;

  name = new char[strlen(cannon_name) + 1];
  strcpy(name, cannon_name);
}
Photon_Cannon::~Photon_Cannon() {
  // 0 이 아닌 값은 if 문에서 true 로 처리되므로
  // 0 인가 아닌가를 비교할 때 그냥 if(name) 하면
  // if(name != 0) 과 동일한 의미를 가질 수 있다.

  // 참고로 if 문 다음에 문장이 1 개만 온다면
  // 중괄호를 생략 가능하다.

  if (name) delete[] name;
}
void Photon_Cannon::show_status() {
  std::cout << "Photon Cannon :: " << name << std::endl;
  std::cout << " Location : ( " << coord_x << " , " << coord_y << " ) "
            << std::endl;
  std::cout << " HP : " << hp << std::endl;
}
int main() {
  Photon_Cannon pc1(3, 3, "Cannon");
  Photon_Cannon pc2 = pc1;

  pc1.show_status();
  pc2.show_status();
}
```
name을 디폴트 복사 생성자로 복사하게 되면, 문제가 발생한다.
기본 자료형들은 값을 복사해서 문제가 없지만, name 은 주소값이라서 주소만 복사된다.
이러면 두 객체가 같은 문자열을 가리키는 주소를 가진다. 
이후 복사시킨 객체가 삭제되면 복사된 객체의 name은 댕글링 포인터가 된다.
이처럼 주소값만 복사되는 것을 얕은 복사라고 한다.
문제가 발생하지 않도록 하려면, 값 자체를 복사해야 하겠다.
포인터 변수에 대해 새로운 메모리 공간을 할당하고 데이터를 복사하는 것을 깊은 복사라고 한다.
깊은 복사를 수행하게 되면 복사된 객체가 원본 객체와 독립적인 메모리를 가지게 되어, 한 객체가 소멸하더라도 다른 객체에서 문제가 발생하지 않는다.
```cpp title:deepCopy
// 복사 생성자의 중요성
#include <string.h>
#include <iostream>

class Photon_Cannon {
  int hp, shield;
  int coord_x, coord_y;
  int damage;

  char *name;

 public:
  Photon_Cannon(int x, int y);
  Photon_Cannon(int x, int y, const char *cannon_name);
  Photon_Cannon(const Photon_Cannon &pc);
  ~Photon_Cannon();

  void show_status();
};
Photon_Cannon::Photon_Cannon(int x, int y) {
  hp = shield = 100;
  coord_x = x;
  coord_y = y;
  damage = 20;

  name = NULL;
}
Photon_Cannon::Photon_Cannon(const Photon_Cannon &pc) {
  std::cout << "복사 생성자 호출! " << std::endl;
  hp = pc.hp;
  shield = pc.shield;
  coord_x = pc.coord_x;
  coord_y = pc.coord_y;
  damage = pc.damage;

  name = new char[strlen(pc.name) + 1];
  strcpy(name, pc.name);
}
Photon_Cannon::Photon_Cannon(int x, int y, const char *cannon_name) {
  hp = shield = 100;
  coord_x = x;
  coord_y = y;
  damage = 20;

  name = new char[strlen(cannon_name) + 1];
  strcpy(name, cannon_name);
}
Photon_Cannon::~Photon_Cannon() {
  if (name) delete[] name;
}
void Photon_Cannon::show_status() {
  std::cout << "Photon Cannon :: " << name << std::endl;
  std::cout << " Location : ( " << coord_x << " , " << coord_y << " ) "
            << std::endl;
  std::cout << " HP : " << hp << std::endl;
}
int main() {
  Photon_Cannon pc1(3, 3, "Cannon");
  Photon_Cannon pc2 = pc1;

  pc1.show_status();
  pc2.show_status();
}
```

## 4 - 4. 스타크래프트를 만들자 ② (const, static)

**생성자의 초기화 리스트**
***
**생성된 총 `Marine` 수 세기 (static 변수)**
static 변수란, 클래스의 모든 객체들이 공유하는 변수이다. 
메모리에 한 번만 할당되며, 객체가 여러 개 생성되어도 모든 객체가 같은 메모리 공간을 공유하게 된다.
```cpp title:static
// static 멤버 변수의 사용

#include <iostream>

class Marine {
  static int total_marine_num;

  int hp;                // 마린 체력
  int coord_x, coord_y;  // 마린 위치
  bool is_dead;

  const int default_damage;  // 기본 공격력

 public:
  Marine();              // 기본 생성자
  Marine(int x, int y);  // x, y 좌표에 마린 생성
  Marine(int x, int y, int default_damage);

  int attack();                       // 데미지를 리턴한다.
  void be_attacked(int damage_earn);  // 입는 데미지
  void move(int x, int y);            // 새로운 위치

  void show_status();  // 상태를 보여준다.

  ~Marine() { total_marine_num--; }
};
int Marine::total_marine_num = 0;

Marine::Marine()
    : hp(50), coord_x(0), coord_y(0), default_damage(5), is_dead(false) {
  total_marine_num++;
}

Marine::Marine(int x, int y)
    : coord_x(x), coord_y(y), hp(50), default_damage(5), is_dead(false) {
  total_marine_num++;
}

Marine::Marine(int x, int y, int default_damage)
    : coord_x(x),
      coord_y(y),
      hp(50),
      default_damage(default_damage),
      is_dead(false) {
  total_marine_num++;
}

void Marine::move(int x, int y) {
  coord_x = x;
  coord_y = y;
}
int Marine::attack() { return default_damage; }
void Marine::be_attacked(int damage_earn) {
  hp -= damage_earn;
  if (hp <= 0) is_dead = true;
}
void Marine::show_status() {
  std::cout << " *** Marine *** " << std::endl;
  std::cout << " Location : ( " << coord_x << " , " << coord_y << " ) "
            << std::endl;
  std::cout << " HP : " << hp << std::endl;
  std::cout << " 현재 총 마린 수 : " << total_marine_num << std::endl;
}

void create_marine() {
  Marine marine3(10, 10, 4);
  marine3.show_status();
}
int main() {
  Marine marine1(2, 3, 5);
  marine1.show_status();

  Marine marine2(3, 5, 10);
  marine2.show_status();

  create_marine();

  std::cout << std::endl << "마린 1 이 마린 2 를 공격! " << std::endl;
  marine2.be_attacked(marine1.attack());

  marine1.show_status();
  marine2.show_status();
}
```

모든 전역 및 `static` 변수들은 정의와 동시에 값이 0으로 초기화되기 때문에 따로 초기화하지 않아도 되지만 클래스 `static` 변수들의 경우
`int Marine::total_marine_num = 0;` 같은 방식으로 초기화 한다.
`static` 함수도 클래스 안에 정의할 수 있다.
`static` 함수는 객체의 개별 상태가 아닌 클래스 전체의 상태를 다룰 때 유용하다.
