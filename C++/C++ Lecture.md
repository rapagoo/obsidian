#cpp
## 0923
메모리를 3군데에서 받을 수 있다.
`int n;`이 있을 때, 1. 지역 2.전역 3.동적
1. 지역 -> `{ int n; }`
2. 전역 -> `int n;`
3. 동적 -> `new int;`
동적 할당은 실행시에 메모리가 확보된다.
그래서 포인터 개념을 이용해 read/write 해야 한다.
Windows가 전역 변수의 크기를 제한하고 있기에, 모든 전역변수의 최대 메모리 크기는 2GB까지 사용할 수 있다.
함수 호출이 끝나고 돌아올 위치의 번지를 stack에 저장한다.
함수 정의는 메모리를 차지하는 동작이다.
```cpp
// 주소값을 저장하는 포인터
#include <iostream>
#include "save.h"
void change(int* num1, int* num2); // function declaration

void change(int* num1, int* num2) { // function definition
    int tmp{ *num1 };
    *num1 = *num2;
    *num2 = tmp;

}

int main() {
    // [문제] a와 b의 값을 바꾸는 함수 change를 호출하고 선언하고 정의하라.
    int a{ 1 };
    int b{ 2 };
    // 여기에서 a와 b를 인자로 하는 함수 change를 호출하였다.
    change(&a, &b); // function call
    std::cout << a << ", " << b << std::endl; //2, 1
    save("메인.cpp");
}
```
지역 변수 `a`와 `b`는 stack에 생기는 메모리다.
change를 호출할 때는, a와 b의 메모리 주소를 stack에 push한다.
포인터의 크기는 x64에서 8바이트를 차지한다.
포인터는 다른 지역에 있는 데이터를 건드리기 위해 사용한다.
타입에 상관없이, 포인터의 크기는 8바이트이다.
다음시간에 레퍼런스 진행 예정.
## 0926
```cpp
int main() {
    int num{ 100 };
    int& r = num; //r은 num의 alias이다.
    r = 123;
    std::cout << num << std::endl;
}
```
stack에 100이 들어가 있다.
num이라고 하는 번지수가 확정된다.
동일한 번지수의 별명을 r로 설정한다.
num과 r의 번지수를 동일하게 설정해준다(컴파일러가)
레퍼런스와 포인터가 다른 점: 실 객체가 있냐 없냐
`int* p = &num;` 에서의 p는 실제 메모리
`int& r = num;` 에서의 r은 이론적으로 실체가 없음
`int& r;` 은 불가능한 문장
둘 다 용도는 동일하다
버블 정렬에 대한 학습. ascending/descending order로 정렬해보았다.
quick sort
## 0930
qsort는 generic 함수이다. 자료형에 제한을 받지 않음.
```cpp
int 오름차순(const void* p, const void* q) {
    int a = *(int*)p;
    int b = *(int*)q;

    if (a < b) {
        return -1;
    }
    else if (a > b) {
        return 1;
    }
    else return 0;
}
```
void 포인터의 의미: 메모리 주소인데 자료형이 무엇이 될지는 네 의도대로 해석하면 된다.
const의 의미: 메모리 내용을 읽을 수는 있지만, 쓰기는 금지한다.(Read-Only Memory)
***
```cpp
int main() {
    char c[]{"quick brown fox jumps over the lazy dog"};
    //[문제]c를 오름차순으로 정렬한 후 화면에 출력하세요.
    qsort(c, strlen(c), sizeof(char), [](const void* a, const void* b) { 
        return *(char*)a - *(char*)b;
        });
    std::cout << c << std::endl;
    save("메인.cpp");
}
```
함수의 기능만 전달: 람다 함수
부를일이 없지만 함수의 기능만 필요할 때 사용
return값을 자동 추론할 수 있기 때문에 `->int` 생략 가능
```cpp
int main() {
    []() {
        std::cout << "hello" << std::endl;
        }();
    save("메인.cpp");
}
//결과: 5
```
`()` : 함수 호출 연산자
## 1003
동적 할당 메모리
지역 변수는 STACK에, 전역 변수는 DATA에 메모리가 생성된다.
이 두 변수들은 사용자가 이름을 붙이는 것이 허용된다.
왜? 컴파일할 때 메모리 주소가 결정되기 때문에.
그에 반해 동적 할당 메모리는 이름을 붙일 수가 없음. 런타임에서 주소가 결정되기 때문에.
동적할당된 메모리의 life cycle은 delete를 이용하여 반환하게 되면 끝난다.
***
```cpp
int main() {
	int n;
	int* p;
	cout << addressof(n) << endl;
	cout << addressof(p) << endl;
	p = new int;
	*p = 123;
	cout << "p가 가리키고 있는 곳의 값: " << *p << endl;
	cout << "p에 저장된 주소값: " <<  p << endl;
	int* x = p;
	*x = 333;
	cout << "같은 주소인가?" << *p << endl;
	save("메인.cpp");
}
```
***
```cpp
#include <iostream>
#include "save.h"
using namespace std;
int main() {
	// [문제] 사용자가 입력한 숫자만큼 int가 들어갈 메모리를 확보한다.
	// 메모리의 값을 1부터 시작하는 정수로 채워나간다.
	// 전체 정수의 합계를 계산하여 화면에 출력한다.

	while (true) {
		cout << "숫자를 입력하시오: ";
		int num;
		cin >> num;
		int* p = new int[num]; // 여기가 핵심
		int sum{ 0 };
		for (int i = 0; i < num; ++i) {
			p[i] = i + 1;
			sum += p[i];
		}
		cout << "전체 합계: " << sum << endl;
	}
	save("메인.cpp");
}
```
## 1007
동적 할당 메모리가 필요한 경우는 언제일까? 에 대해 생각해 보자.
동적: 프로그램이 실행될 때(run-time) 라고 이해하면 된다.
정적: compile-time
***
```cpp
while (true) {
	int* p = new int[1'000'000'000]; // 4GB 요청
	// 여기에서 할당 받은 메모리를 마음대로 사용
	for (int i = 0; i < 100; ++i) {
		cout << p[i] << " ";
	}
	cout << endl;

	delete[] p; // 사용 후 반드시 되돌려줘야 한다.
```
int* p: stack에 8바이트 메모리가 생김
free-store에 4gb 메모리 할당
4gb 메모리가 할당된 주소가 8바이트 공간 p에 저장되어 있음
delete[] p 하게 되면 주소를 더이상 사용하지 않겠다고 시스템에게 알려줌
free-store에 있는 4gb 메모리 할당 해제
***
```cpp
int main() {

	int* p;

	p = new int;

	cout << *p << endl;
	
	delete p;

	// 이런 경우가 없을 거 같나?
	delete p;
```
stack에 8바이트 크기의 메모리가 확보된다. 이름은 p
free-store에 4바이트 메모리를 요청한다. 그 메모리의 시작 번지를 리턴한다.
리턴한 주소값은 p에 들어감
`*p`가 출력됨
free-store에 있는 4바이트 메모리가 반환된다.
메모리는 반환됐는데 주소값은 p에 아직 남아있음
이때 포인터 p를 dangling pointer라고 부른다.
delete p를 한번 더 하게 되면, 이미 반환한 메모리를 또 반환하려 하는 것이고 그대로 프로그램 사망
이것을 방지하기 위해 만들어진 것이 스마트 포인터(RAII를 구현한 것. 리소스 관리를 자동화)
함수 포인터 공부하기 (참고: [[function pointer.png]]) / RAII도 공부하기
***
```cpp
char c[1'000'000'000]; 

int main() {
	save("메인.cpp");
}
```
전역 변수 c는 DATA에 들어간다. 1GB 할당. 디폴트 초기화
만약 `char c[1'000'000'000]{ 123 };` 하게 되면, 컴파일이 엄청 오래 걸린다.
왜 다르게 돌아갈까?
디폴트 초기화는 컴퓨터 입장에서 기계어를 만들어내기가 매우 쉽다.
그냥 1GB 확보하고 전부 디폴트 초기화 하는 것이라 명령어가 몇개 안쓰임
`char c[1'000'000'000]{ 123 };` 는 어떻게 진행될까?
1GB 메모리를 그대로 카피를 해서 실행 파일에 기록한다. 1GB짜리 실행 파일이 생성됨.
***
file i/o 공부해 보기

## 1010
10/28 월요일 중간시험 예정
```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main() {
	// file에 자료를 기록하기

	//[실습] 1부터 100까지 int 값을 파일에 기록하자.
	ofstream out{ "int 100개" }; // RAII

	for (int i = 0; i < 100; ++i) {
		out << i + 1 << endl;
	}
}
```
ofstream 타입의 메모리를 할당하고, out이라는 이름으로 파일에 액세스할 수 있다.
int 100개라는 이름의 파일이 생긴다.
파일을 읽고 쓰는데 확장자는 아무런 역할을 하지 않는다. 확장자는 힌트에 불과하다.
만약에 "int 100개" 라는 파일이 이미 있다면, 그 파일을 깨고 새로 만든다.
for 문에서 1부터 100까지가 기록되는 방식은, cout과 동일하다.
출력 창 또한 메모리기 때문에, 출력 창에 출력되는 방식과 동일하다고 생각하면 된다.
ofstream은 RAII이기 때문에, 자원 관리가 자동화된다.
`out.close()` 는 필요 없다. 자동으로 이루어지기 때문에
***
```cpp
	ifstream in{ "int 100개" };

	int num;
	for (int i = 0; i < 100; ++i) {
		in >> num;
		cout << num;
	}
```
ifstream으로 파일을 읽어오고, num이라는 메모리를 만든 다음
num에 파일의 데이터를 불러들여서 cout으로 화면에 출력한다.

ifstream 크기의 메모리를 STACK에 만든다. 이름은 in 이라고 액세스 하겠다.
in 메모리 내에 파일을 가리키는 포인터가 int 100개 파일을 가리킨다.
int num 메모리를 STACK에 만든다.
`in >> num;` 문장을 통해 num 메모리에 1이 들어온다.
cout 으로 1을 화면에 출력한다.
다시 다음 `in >> num;` 에서 다음 데이터인 2를 num 메모리에 넣는다.파일 포인터가 다음을 가리킴
cout으로 2를 화면에 출력한다.
이것을 100번 반복한다.
만약 100까지 읽었을 때 중간을 다시 읽을 수 있을까?
사실 파일 포인터는 임의의 위치로 움직일 수 있긴 하다.
근데 그럴 일은 많이 없긴 함. 일반적으로 사용을 안한다
왜? 보조기억장치는 너무 느려...
사실 데이터를 stack에 한방에 옮겨올 수도 있다. int num\[100] 이런 방식으로
***
파일을 읽을 때는 반드시 없을 때에 대비해야 한다.
```cpp
	ifstream in{ "int 100개" };
	if (not in) {
		cout << "파일을 열 수 없습니다. " << endl;
		exit(333);
	}

	int num;
	int count{ 0 };
	while (in >> num) {
		++count;
		cout << num << " ";
	}

	cout << endl;
	cout << "모두 " << count << " 개의 int 값을 읽었다. " << endl;
```
예외 처리를 먼저 해 주고, while 문을 사용하여 파일이 끝날 때 까지 읽어들였다.

## 1014
"랜덤값들" 파일에 있는 숫자들을 오름차순으로 정렬해 보자.
파일에 몇 개의 숫자가 있는 지 모르니까 정렬을 하려면 먼저 개수를 세어야 한다
근데 개수를 세고 또 파일을 읽어서 정렬해본다? 파일 포인터가 이미 진행된 상태인데...
그럼 파일을 읽는 부분을 괄호로 감싸서 지역으로 묶는다. 이러면 지역을 벗어나는 순간 파일 포인터 초기화
```cpp title:1014_1
#include <iostream>
#include <fstream>
#include <print>
#include "save.h"
using namespace std;

int main() {
	// [문제] 파일 "랜덤값들" 에는 몇 개인지 모르는 int가 있다
	// 오름차순으로 정렬한 후 출력하라

	int cnt{};

	{
		ifstream in{ "랜덤값들" };
		if (not in)
			return 0;

		// 개수를 센다. 몇 개인지 모르니까

		int num;

		while (in >> num) {
			++cnt;
		}
	}
	int* p = new int[cnt];

	ifstream in{ "랜덤값들" };
	if (not in)
		return 0;
	int num;
	int i{};
	while (in >> num) {
		p[i] = num;
		++i;
	}
	cout << "모두 " << i << " 개의 int값을 읽어 저장하였다." << endl;
	cout << "정렬 시작(오름차순)";
	qsort(p, cnt, sizeof(int), [](const void* a, const void* b) {
		return*(int*)a - *(int*)b;
		});
	cout << endl;
	cout << endl;

	cout << "오름차순으로 정렬한 후 " << endl;
	for (int i = 0; i < cnt; ++i) {
		print("{:16}", p[i]);
	}
	cout << endl;
	delete[] p;

	save("메인.cpp");
}
```
***
```cpp title:1014_2
// FILE I/O
#include <iostream>
#include <fstream>
#include "save.h"
using namespace std;

int main() {
	// [문제] 사용자가 원하는 문자 한 개를 입력받아라.
	// 파일 "메인.cpp" 에 찾는 문자가 몇 개 있는지 출력하라.
	// 없으면 없다고 출력하라.
	// 이 과정을 영원히 반복하라.

	while (true) {
		cout << "찾을 문자는? ";
		char toFind;
		cin >> toFind;

		ifstream in{ "메인.cpp" };
		if (not in)
			return 0;

		int cnt{};
		char ch;
		while (in >> ch) {
			if (ch == toFind) {
				++cnt;
			}
		}

		if (cnt)
			cout << toFind << " " << cnt << " 개 " << endl;
		else
			cout << toFind << " 메인.cpp에 없는 글자 " << endl;
	}
	save("메인.cpp");
}
```
근데 파일을 읽어 들어오는 것은 비용이 많이 발생한다.
이 코드에서는 심지어 while 루프를 이용해 파일을 몇 번이고 읽어들인다...
***
이제는 모두 몇 개의 단어가 있는지 출력해 보자.
근데 단어들은 모두 크기가 다르다.
가변 길이의 문자열을 읽어올 때는 string을 사용한다.
string은 읽어들인 글자 그대로 free store에 저장해서 처리한다.
ifstream은 공백이 있으면 알아서 처리해 준다. 프로그래머가 공복을 고려할 필요가 없음
```cpp title:1014_3
// FILE I/O
#include <iostream>
#include <fstream>
#include <string>
#include "save.h"
using namespace std;

int main() {
	// [문제] "메인.cpp" 에는 모두 몇 개의 단어가 있나 출력하라.
	// 여기서 단어란 공백으로 분리되지 않은 문자의 집합이다.
	
	ifstream in{ "메인.cpp" };
	if (not in)
		return 0;

	std::string str;
	int cnt{};
	while (in >> str) {
		cout << "읽은 단어: " << str << endl;
		++cnt;
	}

	cout << "모두 " << cnt << " 개의 단어가 있다. " << endl;
	save("메인.cpp");
}
```
실행해보면 in이 알아서 공백을 처리하고, str은 문자열 길이에 상관없이 알아서 처리한다.
***
```cpp title:1024_4
// FILE I/O
#include <iostream>
#include <fstream>
#include <string>
#include "save.h"
using namespace std;

int main() {
	// [문제] "메인.cpp"에 있는 소문자를 모두 대문자로 바꿔
	// "메인대문자.cpp" 에 저장하는 프로그램을 작성하라.

	ifstream in{ "메인.cpp" };
	if (not in)
		return 0;
	ofstream out{ "메인대문자.cpp" };

	char ch;

	in >> noskipws;
	while (in >> ch) {
		if (islower(ch)) {
			ch = toupper(ch);
		}
		out << ch;
	}
	

	save("메인.cpp");
}
```
***
```cpp title:1024_5
#include <iostream>
#include <fstream>
#include <string>

int main() {
	// [문제] "project.cpp"에 있는 단어를 
	// 길이 기준으로 하여 오름차순 정렬한 후 
	// 화면에 출력하라.

	int cnt{};
	{
		std::ifstream in{ "project.cpp" };
		if (not in) return 1;

		std::string str;
		while (in >> str) ++cnt;
		std::cout << cnt << " 개의 단어를 읽었다. " << "\n";
	}

	std::ifstream in{ "project.cpp" };
	if (not in) return 1;

	std::string* arr = new std::string[cnt];
	std::string str;
	int i{};
	while (in >> str) {
		arr[i++] = str;
	}

	qsort(arr, cnt, sizeof(std::string), [](const void* a, const void* b) {
		std::string s = *(std::string*)a;
		std::string t = *(std::string*)b;
		return int(s.length() - t.length());
		});

	for (int i{}; i < cnt; ++i) {
		std::cout << arr[i] << "\n";
	}
	delete[] arr;
}
```

## 1017
클래스 강의 시작
```cpp title:1017
// 사용자가 정의하는 자료형 - User-Defined Data Type
#include <iostream>
#include <string>
#include "save.h"

class Dog {
	char c;
	int n;
};

int main() {
	
	Dog dog;
	std::cout << sizeof Dog << "\n";
	save("메인.cpp");
}
```
Dog의 size는 5가 되어야 하는것 아닌가... 왜 8바이트라고 나올까?
만약 5바이트로 메모리를 할당하면, 5바이트 단위로 움직일텐데,
컴퓨터는 8바이트 단위로 움직이는게 훨씬 빠르다.
내부 구조를 보면, 1바이트 뒤에 3바이트가 완충재로(padding) 있고, 그 다음 4바이트가 있다.
메모리에 빈칸을 만들어서 속도를 얻기 위함이다.
**1031 목요일 시험 확정**
데이터 읽고 정렬시키기
설명하라 문제
***
사용자가 만드는 자료형은 무조건 앞글자를 대문자로 한다.
바깥세상에 보여줄 데이터는 struct로, 아닌건 class로 한다
바깥세상에 오픈한다? 밖에서 고유의 데이터를 바꿀 수 있다는 말
struct와 class의 유일한 차이점은 pubilc이 생략되어 있냐 private이 생략되어 있냐 하는 것

## 1021
```cpp title:1021_1
struct Point2D {
	int x{uid(dre)};
	int y{uid(dre)};

	void show() const {
		std::cout << "(" << x << ", " << y << ")" << "\n";
	}
};


int main() {
	Point2D point;
	point.show();
}
```
struct Point2D는 자료형을 정의한 것에 불과하다.
main 함수에서 Point2D point; 문장이 하는 일은...
STACK에 int 2칸짜리 메모리가 생긴다.
x는 63, y는 -73으로 초기화된다.
그 다음, show 라는 함수를 호출한다.
함수는 Code segment 영역에 쓰여진다.
point에 대한 정보를 show 함수가 알아야만 63, -73을 찍을 수 있다.
void show()에서 첫번째 인자로 객체의 정보를 전달한다. 숨어있는 정보
포인터가 전달되기 때문에 원격 제어가 가능하다.
숨어있는 인자 this 공부해 보기
그래서 사실은 show(Point2D* this) 이렇게 되는 거라고 볼 수 있다.
***
멤버 변수를 가리고 싶다면 class, 노출시키고 싶다면 struct를 쓴다.
class는 기본적으로 private: 이 들어간다. 그리고 열어두고 싶은 부분이 있다면 public: 을 쓴다.
예를 들어 함수에는 public을 사용하여 외부에서 접근할 수 있도록 한다.
***
```cpp title:1021_2
class Dog {
	std::string name;
	int age;
};

int main() {
	Dog dog;
	std::cout << sizeof dog << "\n";
	save("메인.cpp");
}
```
dog의 크기는 36바이트지만, 메모리를 40바이트로 하는 게 더 효율적이라고 컴파일러가 판단하여 STACK에 40바이트를 할당한다.
dog의 주소를 출력해 보면 어떨까? 나중에 해보기
근데 이거 초기화는 어떻게 할까? 멤버 변수는 감춰져 있는데...
그럴 때 생성자를 이용한다.
```cpp title:1021_3
class Dog {
	std::string name;
	int age;

public:
	Dog() {
		name = "몰라";
		age = 0;
		std::cout << "생성자 - creator(ctor) " << "\n";
	}
	void show() {
		std::cout << name << " - " << age << "\n";
	}
};

int main() {
	Dog dog;
	dog.show();
}
```
`Dog dog;` 는 2단계로 돌아간다.
1번: 메모리를 확보한다.
2번: 이 메모리를 채우기 위해서 code segment에 Dog::Dog 함수가 있는지 찾아본다.
`dog.show();` 에서는 이미 만들어진 객체 dog가 있기 때문에
this 포인터로 dog의 인자를 넘겨서 show 함수를 실행한다.
class는 객체를 찍어낼 수 있는 틀일 뿐이다.
생성자를 프로그래머가 만들지 않으면, 컴파일러가 자동으로 만들어 낸다. 디폴트 생성자
***
생성자는 오버로딩이 가능하다.
내가 원하는 방식으로 생성이 가능하다.
생성자는 함수이다.

## 1024
```cpp title:1024_1
class Dog {
	std::string name{};
	int age{};

public:
	//Dog() = default; // 컴파일러가 생성자를 자동으로 만든다.
	Dog() {  // 사용자가 제공한 default 생성자
		std::cout << "생성자 - constructor(ctor) " << "\n";
		PlaySound(L"barking", NULL, SND_SYNC);
	}

	~Dog() {
		std::cout << "소멸자 - destructor(dtor) " << "\n";
		PlaySound(L"wilhelm_scream", NULL, SND_SYNC);
	}
};

int main() {
	{
		Dog dog;
	}
	std::cout << "메인 끝나기 전 " << "\n";
	save("메인.cpp");
}
```
STACK에 sizeof Dog 만큼 메모리가 만들어 진다. 40바이트
40바이트 중 32바이트는 name, 4바이트는 age로 만든다. 나머지는 padding
addressof(dog)와 addressof(dog.name)은 같은 주소일 것이다.
여기까진 내 의지로 돌아가는 부분이 아니고 컴파일러가 만들어 낸다. instancing 단계
이 다음은 initialization 단계이다.
초기화하기 위해서 프로그래머가 지정한 함수를 쫓아가야 한다.
쫓아가는 함수가 "생성자"
생성자는 함수이기 때문에 무슨 일이든 할 수 있다.
지역이 시작되고 dog가 만들어 지며, 지역이 끝날 때 dog가 사라진다.
dog가 소멸되는 과정은 정확히 역순이다.
소멸될 때는, 1. 정리 2. 제거 의 과정으로 진행된다.
정리는 소멸자로 프로그래머가 진행한다.
제거는 프로그래머가 할 수 있는 동작이 아니라, 컴파일러가 기계어 코드로 진행한다.
제거가 실행되면 메모리에 만들어졌던 객체가 사라진다.
이 다음 코드 "메인 끝나기 전" 이 출력되고 프로그램이 종료된다.
***
```cpp title:1024_2
#include <iostream>

class MemoryMonster {
	int num;
	int* arr;
public:
	MemoryMonster(int num) : num{ num } {
		arr = new int[num];
		for (int i{}; i < num; ++i) {
			arr[i] = i + 1;
		}
	}
	~MemoryMonster() {
		delete[] arr;
	}
	void show() {
		for (int i{}; i < num; ++i) {
			std::cout << arr[i] << " ";
		}
		std::cout << "\n";
	}
};

int main() {
	// [문제] 생성 시에 전달된 int값 만큼 int를 저장할 memory를 확보하는
	// MemoryMonster를 코딩하라.
	// main() 이 의도한 대로 실행되도록 하자.
	MemoryMonster mm{ 100 };  // mm은 int 100개가 저장될 메모리를 확보한다.
	                          // 값을 1부터 100까지로 초기화하자.
	mm.show();  // 메모리에 있는 값을 화면에 출력한다.
}
```
`MemoryMonster mm{ 100 };` STACK에 mm이라고 하는 객체를 만든다. 크기는 16바이트
그 다음엔 초기화를 위해 생성자를 실행한다.
free-store에 400바이트를 확보한다. 그리고 이 배열의 시작 주소를 arr에 기록한다.
그 다음 for 루프를 돌면서 메모리에 숫자를 적는다.
이제 show 함수를 실행한다. code segment에 show 함수가 있고, mm의 포인터가 show 함수에 전달된다. this 포인터
show는 num이 누구인지를 아주 잘 알 수 있다. 또한 p의 위치도 알 수 있다. this 포인터로 주니까
코드가 끝나고 mm은 이제 stack에서 사라진다. 근데 소멸자가 없으면 free-store에서는 메모리가 사라지지 않는다.
이를 해결하기 위해서 소멸자를 만들면 stack에서 사라지기 전에 free-store에 있는 메모리를 없애고 지역을 벗어나면서 stack에서 mm이 사라진다.
stack은 LIFO
***
```cpp title:1024_3
int main() {
	MemoryMonster mm{ 100 };
	MemoryMonster aa{ mm };
```
mm 이라는 이름으로 stack에 16바이트 메모리가 생긴다. arr은 2468, num은 100
free-store에 예를 들어 2468번지에 int 100개 메모리가 생긴다.
aa에도 arr이 2468, num이 100이 된다.
문제는 main 함수가 끝날 때, aa의 소멸자가 호출되어 free-store 2468번지의 메모리가 삭제되고, mm의 소멸자가 호출될 때도 2468번지의 메모리가 삭제? 이미 없는 메모리니까 프로그램 펑

## 1028
생성자와 소멸자를 프로그램해야 하는 이유가 뭘까?
special 함수는 왜 special 한가?
***
복사 생성자 만들기
**깊은 복사란?** 객체의 메모리를 복사하는 것이 아니라, 객체가 확보한 메모리의 내용을 복사하는 것이다.
```cpp title:1028_1
	// 복사 생성자
	MemoryMonster(const MemoryMonster& other) : num{ other.num } {
		arr = new int[num]; // deep copy (깊은 복사)
		for (int i{}; i < num; ++i) {
			arr[i] = other.arr[i];
		}
		std::cout << "복사 생성자 " << "\n";
	}
```
확보한 메모리를 복사하는 코드의 예시이지만 실제로는 이렇게 코딩하지 않는다.
```cpp title:1028_2
	// 복사 생성자
	MemoryMonster(const MemoryMonster& other) : num{ other.num } {
		arr = new int[num]; // deep copy (깊은 복사)
		//DMA로 실행될 수 있다.
		memcpy(arr, other.arr, num * sizeof(int));
		std::cout << "복사 생성자 " << "\n";
	}
```
이렇게 하면 cpu 개입 없이 메모리 transfer가 가능하다.
`this` 를 이용해서 생성자와 소멸자가 호출될 때마다 해당 객체의 메모리 주소를 출력할 수 있다.
```cpp title:1028_3
// special 함수 - 생성/소멸/복사/할당/이동생성/이동할당
// 생성자/소멸자를 프로그램해야 하는 이유?
#include <iostream>
#include "save.h"

class MemoryMonster {
	int num;
	int* arr;
public:
	MemoryMonster(int num) : num{ num } {
		arr = new int[num];
		for (int i{}; i < num; ++i) {
			arr[i] = i + 1;
		}
		std::cout << "생성자 - " << this << "\n";
	}
	// 복사 생성자
	MemoryMonster(const MemoryMonster& other) : num{ other.num } {
		arr = new int[num]; // deep copy (깊은 복사)
		//DMA로 실행될 수 있다.
		memcpy(arr, other.arr, num * sizeof(int));
		std::cout << "복사 생성자 - " << this << "\n";
	}
	~MemoryMonster() {
		delete[] arr;
		std::cout << "소멸자 - " << this << "\n";
	}
	void show() {
		for (int i{}; i < num; ++i) {
			std::cout << arr[i] << " ";
		}
		std::cout << "\n";
	}
};

int main() {
	MemoryMonster mm{ 100 };
	MemoryMonster aa{ mm }; // aa가 복사생성되는 순간
	save("메인.cpp");
}
```
디폴트 생성자 학습해보기(추후에)
***
```cpp title:1028_4
int main() {
	MemoryMonster a{ 100 };
	MemoryMonster b{ 12345 };

	a = b;
	save("메인.cpp");
}
```
위 코드를 실행하면 오류가 발생한다.

stack에 16바이트 메모리 a가 생성된다.
free-store에 int 100개가 들어갈 메모리가 생성되고, 이 주소를 a에서 포인팅 한다.(시작 번지)
stack에 16바이트 메모리 b가 생성된다.
free-store에 int 12345개가 들어갈 메모리가 생성되고, 이 주소를 b에서 포인팅 한다.
`a = b;` 이 동작은 b를 a에 assign하라는 문장이다.
참고로 `MemoryMonster a = b;` 와는 다른 문장이다. 이것은 initialization.
`a = b;` 는 special 한 순간이다. 그렇기 때문에 프로그래머가 알려주지 않으면 컴파일러가 알아서 한다.
free-store에 있는 int 12345개가 있는 메모리 시작 번지를 a가 포인팅하게 된다.
LIFO로 인해 b가 먼저 소멸자를 호출하고, 소멸자에서 free-store에 있는 int 12345개 메모리를 반환한다.
이후 b는 stack에서 뽑혀나가게 되고, a가 free-store를 포인팅 하고 있는 부분은 dangling pointer가 된다.
이제 a를 반환하려고 하니까, 소멸자에서 이중 해제가 발생하고, 프로그램이 터진다.
또 다른 문제는, 아까 처음 a가 포인팅하던 free store- int 100개의 메모리를 반환할 수단을 잃어버린다.

이것을 해결하려면 어떻게 해야 할까?
`a = b;` 가 special 한 순간이라는 것만 이해하면 쉽다.
복사할당 연산자에서 제대로 coding 하면 된다.
a도 만들어져 있고 b도 만들어져 있는 상황에서 a를 b와 동일하게 해달라고 했을 때
a가 만들어질 때 확보한 메모리부터 해결해야 한다.
free-store에 있는 int 100개의 메모리를 반환한 뒤, num을 12345로 만들고
system에게 int 12345개가 들어갈 메모리를 달라고 요청하여 그것을 포인팅한다.
이후 b가 가리키는 부분과 a가 가리키는 부분을 deep copy 하도록 한다.
```cpp title:1028_5
	MemoryMonster& operator=(const MemoryMonster& other) {
		delete[] arr;
		num = other.num;
		arr = new int[num];
		memcpy(arr, other.arr, num * sizeof(int));
		return*this;
	}
```

## 1104

**중간시험 리뷰**
![[Pasted image 20241104154012.png]]
어느 메모리 영역에 생성될지에 대한 근거가 부족하므로, 알 수 없다. 가 정답이다
`{}` 는 0으로 초기화 가 아니라, "디폴트 초기화" 이다.

10개의 자료형 A 크기의 contiguous 메모리를 확보한다.
이 메모리의 시작번지를 B라는 이름으로 access 하겠다.
B가 어느 메모리 영역인지는 이 정보로 판단할 수 없다.

![[Pasted image 20241104154538.png]]
전역 변수를 값으로 초기화했으므로, 이 초기화 정보를 실행파일에 기록할 수 밖에 없다.
int 1000개가 저장될 크기인 4000바이트가 **디폴트 초기화 한 것에 비하여** 늘어날 것이다.(한개라도 초기화하는 순간 늘어남)
지금 보니 틀릴 이유가 없었는데, 그냥 답 자체를 잘못 적음. 긴장하지 말고 천천히 문제 보고 풀자

![[Pasted image 20241104155051.png]]
STACK의 크기를 a가 초과하였기 때문에 코드가 비정상적으로 종료된다. 

![[Pasted image 20241104155422.png]]
따로 적을 코멘트 x 쉽게 맞춘 문제

![[Pasted image 20241104160049.png]]
함수 포인터를 적는 문제인데, 너무 쉬운 문제라 코멘트 x

![[Pasted image 20241104160253.png]]
Claude AI 사용하여 해결하려 했는데, 결과값이 생각대로 나오지 않아
그냥 배운 것을 응용하여 스스로 해결하였다.

```cpp title:Midterm_6_My
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

int main() {
	int cnt{};
	ifstream in{ "단어들" };
	if (not in) {
		cout << "파일 \"단어들\"이 폴더에 없습니다." << endl;
		return -2024;
	}
	// 여기에 들어갈 코드를 작성하라.
	{
		ifstream in{ "단어들" };
		string str;
		while (in >> str) ++cnt;
	}
	string* p = new string[cnt];
	string str;
	int i{};
	while (in >> str) {
		p[i++] = str;
	}
	cout << p[cnt - 1].length() << endl;

	int zCount{};

	for (int i{}; i < cnt; ++i) {
		for (const char& c : p[i]) {
			if (c == 'Z') {
				++zCount;
				break;
			}
		}
	}
	cout << zCount << endl;

	qsort(p, cnt, sizeof(string), [](const void* a, const void* b) {
		string s = *(string*)a;
		string t = *(string*)b;
		return int(s.length() - t.length());
		});
	cout << p[cnt - 1].length() << endl;

	delete[] p;
}
```

교수님이 제시해준 코드를 보자
```cpp title:1
	int num;
	in >> num;
	cout << "단어의 개수: " << num << endl;

	string *words = new string[num];
	for (int i{}; i < num; ++i) {
		in >> words[i];
	}
	cout << "마지막 단어의 길이: " << words[num - 1].size() << endl;
```

```cpp title:2
	int cntZ{};
	for (int i{}; i < num; ++i) {
		for (char c : words[i]) {
			if (c == 'Z') {
				++cntZ;
				break;
			}
		}
	}
	cout << "Z의 개수: " << cntZ << endl;
```

```cpp title:3
	qsort(words, num, sizeof(string), [](const void* a, const void* b) {
		return int((*(string*)a).size() - (*(string*)b).size());
		});
	cout << "가장 긴 단어: " << words[num - 1].size() << endl;

	delete[] words;
```
***
오늘의 강의 시작

special 함수란 클래스 생사에 관련된 함수이다.
special 한 이유: 내가 아무런 짓을 하지 않아도 컴파일러가 만들어 낸다.

	MemoryMonster() = default;
	~MemoryMonster() = default;
	MemoryMonster(const MemoryMonster&) = default;
	MemoryMonster& operator=(const MemoryMonster&) = default;

이것들은 내가 직접 코딩할수 있지만, 그렇지 않으면 컴파일러가 default로 만들어 주는 특수 함수이다.

```cpp
class MemoryMonster {
	int num;
	int* arr;
```

클래스를 생성하면 클래스 메모리가 만들어진다. instancing
메모리 16바이트짜리 객체이다.
포인터는 뭐하려고 쓰는 물건이다? 어딘가의 메모리를 원격 조작하려고 쓴다.
MemoryMonster는 free-store의 메모리를 원격 조종하는 클래스이다.
원격 조종하기 위해 arr 을 쓴다.
멤버에 포인터가 포함되어 있는 객체는 바깥 세상과 반드시 관계를 맺는다.
그래서 태어나고, 복사되고 할 때 문제가 생긴다.
MemoryMonster xx=mm; 하게 되면 똑같은 크기만큼 메모리를 만들면 되는데,
똑같이 만들어달라는 소리는, mm이 원격 조종하는 부분을 가리키기 때문에 문제가 생긴다.
프로그램이 죽어버릴 위험이 생긴다는 뜻이다.
그래서 생성, 복사, 소멸 등을 일일히 프로그래밍해주지 않으면 문제가 생긴다.
***
```cpp
int main() {
	MemoryMonster a{ 100 };
	MemoryMonster b{ 200 };

	a = b;
```

`MemoryMonster a{ 100 };` 메모리가 잡히고 a라는 이름으로 access 가능.
int 100개가 들어갈 메모리를 free-store에 할당한다.
`MemoryMonster b{ 100 };` 메모리가 잡히고 b라는 이름으로 access 가능.
int 200개가 들어갈 메모리를 free-store에 할당한다.

a = b; 를 정확하게 설명해 보면,
b 의 내용을 a에 그대로 카피한다.
a에 200이라는 내용이 적힌다. b가 가리키는 내용을 a도 가리킨다.
free-store에 int 100개 메모리는 가리키는 포인터가 아예 사라졌다.
이제 반환이 절대 불가능하다. 포인터가 가리키는 시작 번지가 사라졌으므로.
LIFO에 의해서 stack에 있는 b가 먼저 소멸된다. 이후 a가 사라지면서 소멸자를 부르는데, dangling pointer로 의해서 시스템이 죽어버린다. 
이로써 사라지지 않는 메모리는 stack에 있는 a와, free-store에 있는 int 100개의 메모리 공간이다.
이것을 해결하려면 깊은 복사를 진행한다. 이러면 아무런 문제가 생기지 않겠다.
***


## 1107

`for (MemoryMonster mm : a)` 에서 시작

```cpp title:1107_1
int main() {
	MemoryMonster a[3]{ 10,30,20 };
	for (MemoryMonster mm : a)
		mm.show();
}
```

1단계 instancing 단계: 16바이트 객체 3개가 stack에 만들어짐
이제 어떻게 초기화할거지?
생성자를 코딩해 두었다. 
2단계로 생성자를 호출한다. num을 10으로 초기화하고 free-store에 int 10개짜리 메모리를 만들고 거기를 가리킨다.
두번째 객체의 생성자를 부른다. int 30개가 들어갈 메모리를 free-store에 만들고 포인팅한다.
세번째, int에 20을 넣고 20개가 들어갈 메모리를 확보해 가리킨다. 1부터 20까지 기록한다.
이제 main 함수의 첫 줄이 실행 완료되었다.

이제 범위 기반의 for loop를 살펴보자.
a에 있는 엘리먼트들을 순서대로 가져온다.
for loop 지역에서 MemoryMonster를 하나 만들어 달라고 요청한다. 이 mm 객체는 STACK에 만들어진다. 크기는 16바이트
16바이트 객체를 만드는데 복사 생성한다.
컴파일러가 instancing 진행
2단계로는 mm을 어떻게 채울지 본다.
여기서 for loop가 a의 엘리먼트들을 하나씩 복사 생성하라는 명령이기 때문에, 복사 생성자를 호출한다.
첫 번째 값인 10을 가져오고, free-store에 10개 들어갈 메모리를 확보한 뒤 깊은 복사를 진행해 데이터를 가져온다. 그 다음 show를 호출한다. mm의 시작 번지를 code segment에 있는 show 함수에게 this 포인터로 넘겨준다. 
그 다음 이 지역 객체가 메모리에서 사라진다. free-store의 메모리를 반환하고 stack에서 mm 객체가 사라진다. 이 과정이 a의 엘리먼트 개수만큼 반복된다.
main을 빠져나가는 순간 a의 메모리가 반환된다. 20개짜리 먼저, 그 다음 30개짜리, 그 다음 10개짜리.
이 프로그램은 이제 1바이트도 놓치지 않고 정상적으로 실행된다.

하지만 되짚어 올라가면 문제가 있다는 것을 알 수 있다.
stack에 있는 a 엘리먼트 3개를 화면에 찍기 위해 임시 객체를 만들어서 복사를 해서 출력하고... 이 과정을 3번 반복? 굉장히 이상하다.
이 데이터를 화면에 찍으려면, 복사할 필요 없이 그냥 원본 데이터에 직접 access 하는 것이 더 좋다.
STACK에 포인터나 레퍼런스를 하나 만들면 그냥 원격지에 있는 데이터를 마음대로 access 할 수 있으니까, 임시 객체를 만들지 않고도 찍고 싶은 메모리를 가리킬 수 있다.
이런 상황에서 cpp에서는 reference를 사용하는 것이 pointer를 사용하는 것보다 권장된다. 훨씬 쓰기 쉬움

```
// 실행 결과
생성자 - 00000002A297FB30 - 10
생성자 - 00000002A297FB40 - 30
생성자 - 00000002A297FB50 - 20
복사 생성자 - 00000002A297FB20 - 10
show 10 - 1 2 3 4 5 6 7 8 9 10
소멸자 - 00000002A297FB20 - 10
복사 생성자 - 00000002A297FB20 - 30
show 30 - 1 2 3 4 5 6 7 8 9 10
소멸자 - 00000002A297FB20 - 30
복사 생성자 - 00000002A297FB20 - 20
show 20 - 1 2 3 4 5 6 7 8 9 10
소멸자 - 00000002A297FB20 - 20
소멸자 - 00000002A297FB50 - 20
소멸자 - 00000002A297FB40 - 30
소멸자 - 00000002A297FB30 - 10
```

반면 레퍼런스 하나만 붙여주면?

```
// 실행 결과
생성자 - 000000480B3BFE40 - 10
생성자 - 000000480B3BFE50 - 30
생성자 - 000000480B3BFE60 - 20
show 10 - 1 2 3 4 5 6 7 8 9 10
show 30 - 1 2 3 4 5 6 7 8 9 10
show 20 - 1 2 3 4 5 6 7 8 9 10
소멸자 - 000000480B3BFE60 - 20
소멸자 - 000000480B3BFE50 - 30
소멸자 - 000000480B3BFE40 - 10
```

for 에서 레퍼런스의 의미: 복사하지 않고 원본에 직접 접근하겠다.
근데 원본에 직접 접근하면, for 루프에서 원본 데이터를 수정할 수 있지 않나?
이를 방지하기 위해 const를 사용하자.
const - 읽기만 할테니 바뀔까 걱정하지 않아도 된다.
& - 복사하지 않고 원본을 직접 access 하겠다.
한 번이라도 const를 사용했다면 일관성 있는 코드를 작성해야 한다.
이것을 const 일관성 이라고 한다. (Const Correctness?)
```cpp title:1107_2
// special 함수 - 생성/소멸/복사/할당/이동생성/이동할당
// 생성자/소멸자를 프로그램해야 하는 이유?
#include <iostream>
#include "save.h"

class MemoryMonster {
	int num;
	int* arr;

public:
	// MemoryMonster() = default;
	// ~MemoryMonster() = default;
	// MemoryMonster(const MemoryMonster&) = default;
	// MemoryMonster& operator=(const MemoryMonster&) = default;


	MemoryMonster() : num{}, arr{} {
		std::cout << "디폴트 생성자 - " << this << " - " << num << "\n";
	}
	MemoryMonster(int num) : num{ num } {
		arr = new int[num];
		for (int i{}; i < num; ++i) {
			arr[i] = i + 1;
		}
		std::cout << "생성자 - " << this << " - " << num << "\n";
	}
	// 복사 생성자
	MemoryMonster(const MemoryMonster& other) : num{ other.num } {
		arr = new int[num]; // deep copy (깊은 복사)
		//DMA로 실행될 수 있다.
		memcpy(arr, other.arr, num * sizeof(int));
		std::cout << "복사 생성자 - " << this << " - " << num << "\n";
	}
	// 복사할당연산자
	MemoryMonster& operator=(const MemoryMonster& other) {
		// 나를 나로 복사할당 하는 것은 막아야겠다. a = a;
		if (this == &other) return *this;
		delete[] arr; // 내가 확보한 메모리 반환
		num = other.num;
		arr = new int[num];
		memcpy(arr, other.arr, sizeof(int) * num);
		std::cout << "복사할당연산자 - " << this << " - " << num << "\n";
		return *this;
	}

	~MemoryMonster() {
		delete[] arr;
		std::cout << "소멸자 - " << this << " - " << num << "\n";
	}
	void show() const {
		std::cout << "show " << num << " - ";
		for (int i{}; i < 10; ++i) {
			std::cout << arr[i] << " ";
		}
		std::cout << "\n";
	}
};

int main() {
	MemoryMonster a[3]{ 10,30,20 };
	for (const MemoryMonster& mm : a)
		mm.show();

	save("메인.cpp");
}
```

const MemoryMonster& mm을 사용하기 위해, void show 에도 const를 붙여줬다.
***
MemoryMonster를 오름차순 정렬해 보자.
근데 오름차순 정렬하려면, num에 access 해야 하는데 num은 private이다.
이를 해결하기 위해 getter 함수를 만든다.
멤버 변수에 접근하기 위한 interface 함수 getter/setter
참고로 setter는 클래스로 코딩하는게 아니다. struct로 하는 것(충분한 필요성이 있는게 아니라면)
```cpp title:1107_3
// special 함수 - 생성/소멸/복사/할당/이동생성/이동할당
// 생성자/소멸자를 프로그램해야 하는 이유?
#include <iostream>
#include <random>
#include "save.h"
using namespace std;

default_random_engine dre;
uniform_int_distribution uid{ 10,99 };

class MemoryMonster {
	int* arr;
	int num;


public:
	// MemoryMonster() = default;
	// ~MemoryMonster() = default;
	// MemoryMonster(const MemoryMonster&) = default;
	// MemoryMonster& operator=(const MemoryMonster&) = default;


	MemoryMonster() : num{ uid(dre) } {
		arr = new int[num];
		for (int i{}; i < num; ++i)
			arr[i] = uid(dre);
		std::cout << "디폴트 생성자 - " << this << " - " << num << "\n";
	}
	MemoryMonster(int num) : num{ num } {
		arr = new int[num];
		for (int i{}; i < num; ++i) {
			arr[i] = i + 1;
		}
		std::cout << "생성자 - " << this << " - " << num << "\n";
	}
	// 복사 생성자
	MemoryMonster(const MemoryMonster& other) : num{ other.num } {
		arr = new int[num]; // deep copy (깊은 복사)
		//DMA로 실행될 수 있다.
		memcpy(arr, other.arr, num * sizeof(int));
		std::cout << "복사 생성자 - " << this << " - " << num << "\n";
	}
	// 복사할당연산자
	MemoryMonster& operator=(const MemoryMonster& other) {
		// 나를 나로 복사할당 하는 것은 막아야겠다. a = a;
		if (this == &other) return *this;
		delete[] arr; // 내가 확보한 메모리 반환
		num = other.num;
		arr = new int[num];
		memcpy(arr, other.arr, sizeof(int) * num);
		std::cout << "복사할당연산자 - " << this << " - " << num << "\n";
		return *this;
	}

	~MemoryMonster() {
		delete[] arr;
		std::cout << "소멸자 - " << this << " - " << num << "\n";
	}

	// 멤버 변수에 접근하기 위한 interface 함수 - getter / setter
	int getNum() const { return num; }

	void show() const {
		std::cout << "show " << num << " - ";
		for (int i{}; i < 10; ++i) {
			std::cout << arr[i] << " ";
		}
		std::cout << "\n";
	}
};

int main() {
	

	// [문제] 다음과 같이 MemoryMonster 30 마리가 있다. 
	// num 기준 오름차순으로 정렬하라.

	MemoryMonster mons[30];
	
	// 오름차순 정렬하라.
	qsort(mons, 30, sizeof(MemoryMonster), [](const void* a, const void* b) {
		MemoryMonster& m1 = *(MemoryMonster*)a;
		MemoryMonster& m2 = *(MemoryMonster*)b;
		return m1.getNum() - m2.getNum();
		});

	for (const MemoryMonster& mon : mons)
		mon.show();

	save("메인.cpp");
}
```
***

## 1111

```cpp
	class STRING {
		size_t len{};
		char* p{};

	public:
		STRING(const char* s) : len{ strlen(s) } {
			p = new char[len];
		}

		size_t size() const {
			return len;
		}
	};
	
	STRING s{ "The C++ Programming Language." };
```

마지막 문장이 돌아가게 하기 위해서 클래스 STRING을 만들었다.
s에서는 글자수와 내용을 save한다.
내용은 free-store 에 char가 29개 들어갈 공간을 만들어서 포인팅하고, 그 공간 안에는 글자의 내용을 저장한다.
참고로 널 포인터는 C++에서 다룰 필요가 없다.
***
```cpp
	for (STRING& s : s)
		cout << s.size() << endl;
```
range based for loop 쓸 때, 레퍼런스 사용 명심하기.
레퍼런스를 붙이지 않으면 복사 생성이 일어난다.
***
연산자 오버로딩
`int a = 2 + 3;` 이런 문장이 있을 때, + 를 operator라고 한다.
2와 3은 operand라고 부른다.
\+ 연산자는 binary operator이다.
단항 연산자 또한 존재한다. `++i;` 같은 경우가 있다. unary operator
2 + 3 을 함수라고 생각해본다면 더하기 연산이란 2와 3의 값을 더해서 리턴하는 함수처럼 생각할 수 있다.
int + (2, 3) {return 5};
이를 일반화해보자.
A c = a @ b; 이것이 적법한 문장이라고 정의한다면 어떻게 처리할 것인가?
A operator@(a, b); 이러한 함수가 있는지 컴파일러가 찾아본다.
만약 있다면 그것을 이용해 문장을 처리한다.

```cpp title:1111_1
// operator overloading - 표준 string 클래스를 만들며 이해한다.
#include <iostream>
#include <string>
#include "save.h"
#include "STRING.h"
using namespace std;
extern bool 관찰;		// 메시지를 보고 싶으면 true로 바꿔준다.

int main() {
	// [문제] 문제없이 실행되도록 필요한 코드를 추가하라.
	STRING s[5]{ "1", "333", "55555", "22", "4444" };

	관찰 = true;
	for (STRING& s : s)
		cout << s.size() << endl;
	관찰 = false;
	
	for (STRING& s : s)
		cout << s << endl;

	//for (int i{}; i < s.size(); ++i) {
	//	cout << s[i] << "-";
	//}
	//cout << endl;

	//s += " C++ 너무 재밌다.";
	//cout << s << endl;

	save("메인.cpp");
}
```

여기서 `cout << s << endl;` 이 문장을 보자.
`cout << s;` 를 먼저 살펴보면, `<<` 은 이항 연산자이다.
제일 먼저 찾는 것은 멤버 함수 우선이므로,
규칙 1. cout.operator<<(STRING); 를 먼저 찾아본다. 이러한 함수가 없다면...
규칙 2. operator<<(ostream,STRING); 이 있는지 찾아본다. 이것도 없으면 컴파일 실패
문제는 표준 cout 내부에서는 STRING 클래스가 뭔지 모른다.
이 문제를 해결하려면, 전역 함수가 필요하겠다.
입출력 함수를 선언할 땐, 친구 함수로 선언한다.
내 멤버 변수를 마음대로 읽고 쓰는 함수가 친구 함수이다.
```cpp
std::ostream& operator<<(std::ostream& os, const STRING& s)
{
	for (int i{}; i < s.len; ++i)
		os << s.p[i];
	return os;
}
```

이제 원래 메인 파일을 실행시켜보면 잘 작동한다.
연산자 오버로딩에 대한 학습이 더 필요

## 1114

```cpp
int main() {
	STRING s{ "오늘은 수능시험 날" };
	s = s + " - N수생 역대 최다";
	cout << s << endl;
}
```
이 코드를 작동하게 만들어 보자.
s에다가 뭔가를 더한다? 그럼 컴파일러가 제일 먼저 하는 것은
제일 먼저 찾는 것이 멤버 함수 우선이므로,
규칙 1. STRING s.operator+(const char*); 를 먼저 찾아본다. 이러한 함수가 없다면...
전역 함수를 찾아본다.
규칙 2. operator+(STRING, const char*); 이 있는지 찾아본다. 이것도 없으면 컴파일 실패
멤버 함수로 해결할 수 있다면 전역 함수로 만들 필요가 없다.

그럼 이제 연산자 오버로딩 함수를 만들어 보자.
`STRING operator+(const char*);` 이렇게 해 보았다.
이제 원본 코드를 보면,
s에 글자 수 18을 넣고,free store에 18글자 들어갈 메모리 만들고, "오늘은 수능시험 날" 글자를 거기에 넣는다. s가 그 메모리를 가리킨다.
그 다음, 더하기를 실행하면, free store에 오늘은 수능시험 날 옆에 N수생 역대 최다를 붙인다?
18칸은 new char\[18] 이런 식으로 한거니까 옆에다 붙일 수는 없을 것이다.
그럼 총 37칸이 필요하니까 free store에 공간을 다시 할당해 보자
이 과정에서 s는 변하지 않고 그대로 있다. s에 집어넣는 게 아니니까...
그러니까 이 함수가 리턴하는 것은 임시 리턴값이고, s에는 영향을 주지 않으니까 const를 붙이고 리턴값에는 레퍼런스가 붙지 않는다.
다시 정리해보면 임시 객체를 만들어서 두 문자열을 붙이고, 그 정보를 s.operator=() 로 s에 넣어준다.
```cpp
STRING STRING::operator+(const char* s) const
{
	size_t num = len + strlen(s);
	STRING tmp{ num };
	memcpy(tmp.p, p, len);
	memcpy(tmp.p + len, s, strlen(s));
	return tmp;			// RVO - Return Value Optimization
}
```
***
```cpp
int main() {
	관찰 = true;
	STRING s{ "오늘은 수능시험 날" };
	s = "2024년 11월 14일 - " + s;
	cout << s << endl;
}
```

이번엔 이 코드를 작동되게 만들어 보자.
const char* 가 왼쪽에 있다. (const char*). 이런 식으로는 불가능하니까 2단계로 넘어간다
전역 함수가 있는지 본다. operator+(const char*, const STRING&); 이런 함수가 있으면 작동하겠다.
메인.cpp에 전역 함수로 만드려니까, 멤버 변수에 접근할 수가 없다...
그래서 헤더파일에 friend를 주고,
```cpp
STRING operator+(const char* s, const STRING& STR)
{
	size_t slen = strlen(s);
	STRING tmp{ slen + STR.len };
	memcpy(tmp.p, s, slen);
	memcpy(tmp.p + slen, STR.p, STR.len);
	return tmp;
}
```
이렇게 해서 만들어보았다.
## 1118

`STRING s[5]{ "the", "C++" ,"programming", "language", "20241118" };` 
이러한 데이터가 있다고 했을 때, 이것을 오름차순 정렬하려면 어떻게 해야 할까?
먼저 시작 번지는 s이고 각각의 메모리 칸에 \*p와 len이 들어간다.
글자수 3글자, free-store에 t h e 가 있고 그걸 가리킨다. 이런 식으로

이제 qsort를 다섯 바퀴 돌린다.
***
```cpp
STRING s[5]{ "the", "C++" ,"programming", "language", "20241118" };

for (STRING& s : s) {
	for (int i{}; i < s.size(); ++i)
		s[i] = toupper(s[i]);
}
```
for루프를 돌면서 진짜 s에 접근하고 있다.
t h e 가 한글자씩 꺼내진다. 그리고 toupper 함수를 지나면 T H E가 된다. 이제 s\[i]에 넣으면 되는데
넣을 수가 없다. 왜 안될까?
연산자 오버로딩 함수를 살펴보면,
operator\[](int idx) const return p\[idx]; 이런 식으로 했었다.
const가 있기 때문에 내 데이터는 바꿀 수가 없다.

`s[i] = toupper(s[i]);` 이 문장을 다시 살펴보자.
t 가 toupper에 들어가고 T가 나온다. 이제 `s[i] = 'T';` 이 동작을 하려고 하는 건데,
"식이 수정할 수 있는 lvalue여야 합니다." 라는 오류가 나온다.
s\[i]는 메모리가 아니야 라고 컴파일러가 말해주는 것이다.
lvalue = location value 즉 메모리이다. `s[i]` 는 그저 char 일 뿐이다.
***
```cpp
	string s1{ "2024. 11. 18 이동문법" };
	string s2 = move(s1);
	cout << s1 << endl; //오류
	cout << s2 << endl;
```
string은 전체가 32바이트고 필드 2개가 있다. 글자 길이와 포인터
stack에 s1이라고 하는 객체를 만들었고, 21글자니까 21을 메모리에 넣고 포인터로 free-store를 가리킨다. free-store에 2021.11.18 ~ 들어간다.
s2에 s1을 이동시킨다. s1이 확보한 메모리를 s2가 가져가라고 하는 것이다.
s1은 xvalue 이기 때문에 오류가 발생한다. 사용하면 안되기로 약속했기 때문에 move가 가능한 것
***
이동 의미론
move가 지원되지 않으면 깊은 복사로 처리한다.
move는 프로그래머가 직접 쓰는게 아니고 알고리즘이 쓴다. 
이동을 지원하면 이동생성으로 처리하지만, 아니라면 복사로 삽질을 한다.

lvalue rvalue xvalue prvalue 이동생성 공부해보기
## 1121

12월 16일 기말시험 예정
후위증가 연산자를 만들 때는 int를 parameter로 사용하는 overloading을 사용한다.
후위 증가의 과정 1. 현재의 나를 save한다. 2. 나를 1 증가시킨다. 3. 증가되기 이전의 나를 return한다.
```cpp
	// post-increment
	INT operator++(int) {
		INT tmp{ *this };
		++num;
		return tmp;
	}
```
후위증가는 전위증가를 이용하여 코딩한다.
리턴값은 rvalue라서, n++++ 이런식으로 코딩할 수가 없다.
const는 앞에 쓰나 뒤에 쓰나 차이는 없다. const INT / INT const 똑같음
***
다음시간은 상속 진행할 예정
함수호출연산자 오버로딩
## 1125

"클래스 관계" 학습해 보기
상속을 하는 이유: 첫번째는 이미 부모가 다 해놓은 삽질을 내가 하지 않기 위해서
다른 말로는 코드를 재사용 하기 위해서이다.
두 번째는, 다형성을 구현하기 위해서이다.
다형성을 이용하지 않으면 게임을 코딩하기 어렵다
```cpp title:1125_1
// 상속
#include <iostream>
#include "save.h"
using namespace std;

class Animal {
public:
	Animal(const char* name, int age) : name{ name }, age{ age } {
		cout << "Animal 생성 - " << name << ", " << age << endl;
	}
	~Animal() {
		cout << "Animal 소멸 " << endl;
	}
	void move() {
		cout << "움직이니까 동물이다." << endl;
	}
private:
	int age;
	string name;
};

class Dog : public Animal {
public:
	Dog(const char* name, int age) : Animal(name, age) {
		cout << "Dog 생성 " << endl;
	}
	~Dog() {
		cout << "Dog 소멸 " << endl;
	}
	void bark() {
		cout << "개가 짖는다." << endl;
	};
private:
	double speed;
};

int main() {
	cout << "Animal의 크기: " <<  sizeof(Animal) << endl;
	cout << "Dog의 크기:    " <<  sizeof(Dog) << endl;
	Dog dog{ "코코",3 };

	save("메인.cpp");
}
```

위 코드에서, `Dog dog{"코코", 3};` 부분을 보자.
STACK에 3과 "코코" 가 들어간다.
나머지 빈자리에는 speed 값이 들어가는데, 값을 주지 않았으니 무슨 값인지는 모른다.
이렇게 48바이트 메모리가 스택에 딱 잡힌다. 내가 개입할 수 없는 1번 과정
그 다음에는 부모의 생성자를 먼저 호출한다. 그 다음 추가된 부분을 호출한다.
소멸자는 역순으로 진행한다.
***
```cpp
class Animal {
private:
	double speed{};
};

class Dog : public Animal {
private:
	string name{};
};

int main() {
	Animal a;		// 8
	Dog dog;		// 40
```

컴파일러가 main 함수 내의 두 문장을 어떻게 해석할까?
class Animal을 뒤져서, stack에 8바이트 메모리를 만든다.
class Dog를 뒤진다. 상속 관계로 엮여있으니까, 8바이트 + 32바이트 = 40바이트
이 40바이트 전체가 Dog이다. 그 안에 Animal의 일부가 있을 뿐
Dog를 가리키고 있는 포인터 p가 있다고 해보자.
stack에 8바이트 메모리가 생긴다.
p = &dog; 이렇게 된다면 아까 만든 40바이트 메모리의 시작번지를 가리킨다.
p 안에는 Animal 메모리가 있을까? 있다.
그런데 Animal을 가리키고 있는 포인터가 있다고 해보자. 이 안에는 자식의 메모리가 없다.
자식의 메모리 안에는 조상의 메모리가 있지만, 조상의 메모리 안에는 자식의 메모리가 없다.
## 1128

class Base : Derived {}; <---- Base is a Derived 관계가 된다.
상속을 통해서 얻고자 하는 것: 1. code 재사용 2. 다형성(Polymorphism)
다형성을 이루기 위한 핵심 키워드: virtual
C++ 핵심 키워드에 대해서 정리해 보자. 
1. const, &
2. virtual
3. template (STL)
키워드란? language가 정의하고 있는 예약어를 이야기 한다.
프로그램에서 다형성을 구현하기 위해서는 어떻게 해야 할까?
***
```cpp
class Animal {
public:
	void move() {
		cout << "움직이니까 동물이다" << endl;
	}
};
class Dog : public Animal {
	
};
int main() {
	Dog dog;
	dog.move();
}
```
stack에 1바이트 Dog dog 메모리를 만든다.
move 함수는 그럼 어디에?? code 섹션에 있다.
dog.move() 는 어떻게 호출되는 걸까? this 포인터를 이용해서 호출한다.
move(&dog) 이렇게 되는 것이다.
void move(Animal*) 원래 함수는 이렇게 되는 것
Dog는 Animal이니까 이러한 코드가 가능하다. Dog is a Animal
***
```cpp
class Animal {
public:
	void move() {
		cout << "움직이니까 동물이다" << endl;
	}
};
class Dog : public Animal {
public:
	void move() {
		cout << "개 달린다" << endl;
	}
};
int main() {
	Dog dog;
	dog.move();
}
```
***
```cpp
Dog dog;
Dog* pDog{ &dog };
pDog->move();
```
stack에 1바이트 메모리 dog이 만들어진다.
stack에 8바이트 메모리 pDog이 만들어진다. pDog은 dog의 시작 번지를 가리킨다.
pDog->move() 에서는 dog로 찾아가서 move를 찾는다.
그러니까 이 문장은 dog.move()와 동일한 문장이다.
code segment에 move가 3개 있다. move(Animal*) move(Dog*) move(Bird*)
***
```cpp
Dog dog;
Dog* pDog{ &dog };
Animal* pAnimal = (Animal*)(Dog*)pDog;
pAnimal->move();
```
pDog는 dog의 시작번지를 가리킨다.
이때 Animal* pAnimal을 stack에 만들고, pAnimal = pDog; 하게 되면
pAnimal은 dog의 시작 번지를 가리킨다. 문제가 없을까?
문제가 없다. Dog is a Animal이니까.
dog 안에는 Animal의 메모리가 포함되어 있기 때문에 자식 클래스를 부모 클래스로 간주해도 아무 이상이 없다.
## 1202

```cpp
int main() {
	Dog dog;
	Bird bird;

	dog.move();
	bird.move();
}
```
stack에 dog과 bird 메모리가 만들어진다.
dog.move() 는 code segment에서 부른다 static binding
이건 다형성이 아니다.
***
```cpp
// virtual void move() const

int main() {
	Dog dog1, dog2;
	Bird bird1, bird2, bird3;
	Animal* animals[5];
	animals[0] = &bird1;
	animals[1] = &bird2;
	animals[2] = &dog1;
	animals[3] = &bird3;
	animals[4] = &dog2;

	for (int i{}; i < 5; ++i)
		animals[i]->move();
}
```
animal, dog, bird가 각각 16, 24, 24바이트라는 크기로 변한다.
Animal 시작 번지에 virtual 포인터 하나 추가하고 원래 메모리를 붙인다.
8 + 4 + 4(패딩) = 16
Dog는 Animal을 먼저 붙인다. 8 + 4 + 4(패딩) + 8 = 24
Bird도 마찬가지 8 + 4 + 4(패딩) + 4 + 4(패딩) = 24
virtual 포인터란 무엇인가?
포인터란 무언가를 가리키려고 하는 것
virtual function table에서 각 멤버의 move 함수 시작 번지를 가리킨다.

다형성(Polymorphism)을 구현하는 핵심 키워드 virtual
클래스 멤버에 vptr이 추가된다.
메모리를 사용하여 편리함을 얻는다.
***
```cpp
cout << "모두 몇 마리인가요? ";
size_t num;
cin >> num;

Animal** animals = new Animal* [num];
for (int i{}; i < num; ++i) {
	int coin{ uid(dre) };
	if (1 == coin)
		animals[i] = new Dog;
	else
		animals[i] = new Bird;
}
```
`new Animal* [num]` 이 먼저 실행, free-store에 8바이트 메모리를 num개 만든다.
Animal* 의 포인터를 return 한다
stack에 Animal** animals 를 만든다. 8바이트 메모리이고 포인터들의 묶음을 가리키는 포인터이다
코인을 뒤집으면서 Animal* 의 i번째가 free store를 가리키게 한다. freestore에 24바이트 메모리 생성
이때 `animals[i]` 의 move() 를 실행하면 그 칸이 가리키는 동물의 move() 를 실행한다.
## 1205

const = 0 을 왜 사용할까?
Animal로 예를 들면 최상위 클래스인 Animal은 공통된 속성을 표현하기 위한 추상 클래스이다.
추상 클래스(Abstract class) / (Concrete class)
추상 클래스의 객체를 메모리에 instancing하는 것은 맞지 않다.
`virtual void move() const = 0` 과 같은 몸체가 없는 함수를 이용하여 실 객체를 만들지 못하게 한다.
이는 순수가상함수라고 부른다. pure virtual function
## 1209

