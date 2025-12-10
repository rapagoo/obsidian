
# Functions
## What is a Function?
함수를 쓰면, 코드를 모듈화할 수 있다. (Modularize)
cmath 라이브러리 학습
sqrt, cbrt, sin, cos, ceil, floor, round, pow
cstdlib 라이브러리 - rand()
ctime 라이브러리 - time()
## Functon Definition
함수를 정의할 때, 4가지가 필요하다.
name, parameter list, return type, body
함수는 사용되기 전 정의되어야 한다.
## Function Prototypes
함수가 사용되기 전 정의되어야 하는 이유는?
컴파일러가 매개변수와 리턴 타입을 알수가 없어서이다.
이를 해결하는 방법은 함수 프로토타입을 하는 것이다.
물론 모든 함수를 먼저 정의할 수도 있겠지만 이는 소규모 프로그램에서나 가능한 일이다.
## Function Parameters and the return Statement
function parameters는 number, order, and type 이 맞아야 한다.
함수에 데이터를 전달할 때는 값으로 전달된다.(Pass-by-value)
값이 복사되어 전달된다.
## Default Argument Values
default argument value 란? 함수 선언 시 매개변수에 기본값을 지정하는 기능이다.
```cpp
void print(int x, int y = 10, string msg = "default") {
    cout << x << " " << y << " " << msg << endl;
}

int main() {
    print(1);          // 1 10 "default"
    print(1, 20);      // 1 20 "default"
    print(1, 20, "hi"); // 1 20 "hi"
}
```
Parameter와 Argument는 다른 것이다.
Parameter는 함수 내부에서 사용되는 변수이고, Argument는 함수를 호출할 때 전달되는 실제 값으로, 매개변수에 복사되는 값이다.
함수 호출 시 Argument가 전달되지 않았을 때, 미리 지정해둔 Default Argument Value를 이용한다.
## Overloading Functions
같은 이름의 함수를 매개변수를 다르게 하여 여러 개 정의하는 것
```cpp
int add(int a, int b) {
    return a + b;
}

double add(double a, double b) {
    return a + b;
}

string add(string a, string b) {
    return a + b;
}
```
함수를 오버로딩할때, 리턴 타입은 고려되지 않는다.
## Passing Arrays to Functions
배열을 함수에 넘겨줄 때는, 배열의 크기도 알려줘야 한다.
배열을 넘겨주면 함수가 배열을 수정할 수도 있기 때문에, 그렇게 하고 싶지 않으면 const 를 사용한다.
## Pass by Reference
reference parameter를 함수에 전해줌으로써 그 변수의 복사된 값이 아닌 실제 위치를 함수에 전달할 수 있다.
