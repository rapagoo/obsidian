# C++ 부트캠프 기초소양평가 대비 압축 정리

작성 목적: **이틀 뒤 기초소양평가/면접 전 빠르게 복습하기 위한 C++ 핵심 정리**  
현재 상태 기준: C++ 기본 문법과 STL 사용 경험은 있으나, 일부 세부 동작과 실수 포인트를 보완하면 좋음.

---

## 0. 지금 가장 중요한 방향

learncpp 전체를 읽는 것보다 아래 항목을 정확히 익히는 것이 효율적이다.

### 우선순위 높음

1. 입력/출력 처리: `cin`, `getline`, `stringstream`
2. 조건문/반복문/함수
3. `vector`, `string`, `map`, `set`, `stack`, `queue`
4. 문자열 처리와 빈도수 계산
5. 인덱스 범위 실수 방지
6. 포인터/참조자/메모리 기본 개념
7. 시간복잡도 기초

### 지금 깊게 파지 않아도 되는 것

- 템플릿 고급 문법
- 연산자 오버로딩
- 상속/다형성 심화
- 복잡한 포인터 산술
- 수동 메모리 관리 심화
- C 스타일 문자열 심화
- move semantics, perfect forwarding

---

## 1. 반드시 고쳐야 할 개인 취약 포인트

### 1.1 `a++`와 `++a`

```cpp
int a = 5;
int b = a++;
cout << a << " " << b;
```

출력:

```text
6 5
```

`a++`는 **현재 값을 먼저 사용한 뒤 증가**한다.

```cpp
int a = 5;
int b = ++a;
cout << a << " " << b;
```

출력:

```text
6 6
```

`++a`는 **먼저 증가한 뒤 사용**한다.

암기:

```text
a++ : 쓰고 증가
++a : 증가하고 씀
```

---

### 1.2 대입 `=`과 비교 `==`

```cpp
int x = 10;
if (x = 0) {
    cout << "A";
} else {
    cout << "B";
}
```

출력:

```text
B
```

이유:

```cpp
x = 0
```

은 비교가 아니라 대입이다. `x`에 `0`을 넣고, 조건식의 값도 `0`이 되므로 false로 판단된다.

비교하려면:

```cpp
if (x == 0) {
    cout << "A";
}
```

---

### 1.3 빈 `vector`에 인덱스로 접근하면 안 됨

```cpp
vector<int> v;
v[0] = 10; // 위험
```

`v`는 비어 있으므로 `v[0]`이라는 원소가 없다.

올바른 방법:

```cpp
vector<int> v;
v.push_back(10);
```

또는:

```cpp
vector<int> v(1);
v[0] = 10;
```

또는:

```cpp
vector<int> v;
v.resize(1);
v[0] = 10;
```

주의:

```cpp
vector<int> v;
v.reserve(10);
v[0] = 3; // 여전히 위험
```

`reserve`는 용량만 예약한다. 실제 원소를 만든 것이 아니다.  
`resize`는 실제 원소를 만든다.

```text
reserve: capacity 확보
resize : size 변경, 실제 원소 생성
```

---

### 1.4 `map[key]`는 없으면 자동 생성된다

```cpp
map<string, int> m;
m["apple"]++;
m["apple"]++;
cout << m["apple"] << " " << m["banana"];
```

출력:

```text
2 0
```

`m["banana"]`는 에러가 아니다.  
없는 key를 `[]`로 접근하면 자동으로 생성된다. `int`의 기본값은 `0`이다.

주의:

```cpp
m[key]
```

는 조회처럼 보여도, key가 없으면 새 원소를 추가한다.

존재 여부만 확인하려면:

```cpp
if (m.find("banana") != m.end()) {
    cout << "exists";
}
```

C++20 이상이라면:

```cpp
if (m.contains("banana")) {
    cout << "exists";
}
```

---

### 1.5 반복문 인덱스 범위

```cpp
vector<int> v = {1, 2, 3};

for (int i = 0; i <= v.size(); i++) {
    cout << v[i] << "\n";
}
```

문제:

```text
v.size()는 3
유효한 인덱스는 0, 1, 2
하지만 i <= v.size()면 i == 3까지 접근
v[3]은 범위 밖
```

수정:

```cpp
for (int i = 0; i < v.size(); i++) {
    cout << v[i] << "\n";
}
```

더 안전한 방식:

```cpp
for (int x : v) {
    cout << x << "\n";
}
```

---

## 2. 입력 처리

### 2.1 `cin`

```cpp
string s;
cin >> s;
```

공백 전까지만 입력받는다.

입력:

```text
hello world
```

결과:

```text
s = "hello"
```

---

### 2.2 `getline`

```cpp
string line;
getline(cin, line);
```

한 줄 전체를 입력받는다.

입력:

```text
hello world
```

결과:

```text
line = "hello world"
```

---

### 2.3 `cin` 다음 `getline`을 쓸 때 주의

```cpp
int n;
cin >> n;

string line;
getline(cin, line);
```

이 경우 `cin >> n` 뒤에 남은 개행 문자를 `getline`이 바로 읽어버릴 수 있다.

해결:

```cpp
int n;
cin >> n;
cin.ignore();

string line;
getline(cin, line);
```

더 안전하게:

```cpp
#include <limits>

cin.ignore(numeric_limits<streamsize>::max(), '\n');
```

---

### 2.4 한 줄을 단어 단위로 쪼개기: `stringstream`

```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <map>
using namespace std;

int main() {
    string line;
    getline(cin, line);

    stringstream ss(line);
    map<string, int> mp;

    string word;
    while (ss >> word) {
        mp[word]++;
    }

    for (auto p : mp) {
        cout << p.first << " " << p.second << "\n";
    }
}
```

이 패턴은 문자열 파싱 문제에서 자주 쓴다.

---

## 3. 함수, 값 전달, 참조 전달

### 3.1 값 전달

```cpp
void f1(vector<int> v) {
    v.push_back(10);
}
```

`v`가 복사되어 들어온다. 함수 안에서 수정해도 원본은 바뀌지 않는다.

---

### 3.2 참조 전달

```cpp
void f2(vector<int>& v) {
    v.push_back(10);
}
```

원본을 직접 수정한다.

---

### 3.3 읽기 전용 참조 전달

큰 객체를 복사하지 않고 읽기만 할 때는 `const &`를 쓴다.

```cpp
void printVector(const vector<int>& v) {
    for (int x : v) {
        cout << x << " ";
    }
}
```

장점:

```text
복사 비용 없음
함수 안에서 원본을 실수로 수정하지 못함
```

---

## 4. STL 핵심 정리

## 4.1 `vector`

동적 배열이다.

```cpp
vector<int> v;
v.push_back(3);
v.push_back(5);

cout << v[0]; // 3
cout << v.size(); // 2
```

자주 쓰는 함수:

```cpp
v.push_back(x); // 뒤에 추가
v.pop_back();   // 마지막 제거
v.size();       // 원소 개수
v.empty();      // 비었는지 확인
v.clear();      // 전체 제거
v.resize(n);    // 크기를 n으로 변경
```

주의:

```cpp
v[i]
```

는 범위 검사를 하지 않는다. `i`가 잘못되면 위험하다.

범위 검사를 원하면:

```cpp
v.at(i)
```

다만 알고리즘 문제에서는 보통 `v[i]`를 많이 쓴다.

---

## 4.2 `string`

문자열이다.

```cpp
string s = "banana";
cout << s[0]; // b
cout << s.size(); // 6
```

문자 순회:

```cpp
int count = 0;
for (char c : s) {
    if (c == 'a') {
        count++;
    }
}
```

문자열 뒤에 추가:

```cpp
s += 'x';
s += "hello";
```

부분 문자열:

```cpp
string t = s.substr(1, 3);
```

`substr(pos, len)`은 `pos` 위치부터 `len`개를 가져온다.

---

## 4.3 `map`

key-value 자료구조다. key를 기준으로 자동 정렬된다.

```cpp
map<string, int> mp;
mp["apple"]++;
mp["banana"] = 2;
```

순회:

```cpp
for (auto p : mp) {
    cout << p.first << " " << p.second << "\n";
}
```

구조적 바인딩, C++17 이상:

```cpp
for (auto [key, value] : mp) {
    cout << key << " " << value << "\n";
}
```

존재 확인:

```cpp
if (mp.find("apple") != mp.end()) {
    cout << "exists";
}
```

주의:

```cpp
mp["unknown"]
```

은 key가 없으면 새로 만든다.

---

## 4.4 `set`

중복 없는 집합이다. 자동 정렬된다.

```cpp
set<int> st;
st.insert(3);
st.insert(1);
st.insert(3);

for (int x : st) {
    cout << x << " ";
}
```

출력:

```text
1 3
```

중복 제거 + 정렬이 필요할 때 유용하다.

---

## 4.5 `stack`

후입선출, LIFO 구조다.

```cpp
stack<int> st;
st.push(1);
st.push(2);

cout << st.top(); // 2
st.pop();
cout << st.top(); // 1
```

자주 쓰는 함수:

```cpp
st.push(x);
st.pop();
st.top();
st.empty();
st.size();
```

주의:

```cpp
st.top();
st.pop();
```

은 stack이 비어 있으면 위험하다. 먼저 확인해야 한다.

```cpp
if (!st.empty()) {
    cout << st.top();
    st.pop();
}
```

---

## 4.6 `queue`

선입선출, FIFO 구조다.

```cpp
queue<int> q;
q.push(1);
q.push(2);

cout << q.front(); // 1
q.pop();
cout << q.front(); // 2
```

자주 쓰는 함수:

```cpp
q.push(x);
q.pop();
q.front();
q.empty();
q.size();
```

---

## 5. 자주 나오는 구현 패턴

### 5.1 최댓값/최솟값

```cpp
int n;
cin >> n;

int x;
cin >> x;

int mn = x;
int mx = x;

for (int i = 1; i < n; i++) {
    cin >> x;
    if (x < mn) mn = x;
    if (x > mx) mx = x;
}

cout << mx << " " << mn;
```

주의: 최댓값/최솟값 초기값을 무조건 `0`으로 잡으면 음수 입력에서 틀릴 수 있다.

---

### 5.2 문자 개수 세기

```cpp
string s;
cin >> s;

int count = 0;
for (char c : s) {
    if (c == 'a') {
        count++;
    }
}

cout << count;
```

---

### 5.3 단어 빈도수 세기

입력이 단어 개수 `n`과 함께 주어질 때:

```cpp
int n;
cin >> n;

map<string, int> mp;

for (int i = 0; i < n; i++) {
    string word;
    cin >> word;
    mp[word]++;
}

for (auto p : mp) {
    cout << p.first << " " << p.second << "\n";
}
```

한 줄 전체에서 단어를 세야 할 때:

```cpp
string line;
getline(cin, line);

stringstream ss(line);
map<string, int> mp;

string word;
while (ss >> word) {
    mp[word]++;
}

for (auto p : mp) {
    cout << p.first << " " << p.second << "\n";
}
```

---

### 5.4 올바른 괄호 판정

한 종류 괄호 `(`, `)`만 있을 때:

```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

bool isValid(string s) {
    stack<char> st;

    for (char c : s) {
        if (c == '(') {
            st.push(c);
        } else if (c == ')') {
            if (st.empty()) {
                return false;
            }
            st.pop();
        }
    }

    return st.empty();
}
```

핵심:

```text
여는 괄호는 push
닫는 괄호가 나왔는데 stack이 비어 있으면 실패
닫는 괄호가 나오면 pop
마지막에 stack이 비어 있으면 성공
```

여러 종류 괄호일 때:

```cpp
bool isValid(string s) {
    stack<char> st;

    for (char c : s) {
        if (c == '(' || c == '[' || c == '{') {
            st.push(c);
        } else {
            if (st.empty()) return false;

            if (c == ')' && st.top() != '(') return false;
            if (c == ']' && st.top() != '[') return false;
            if (c == '}' && st.top() != '{') return false;

            st.pop();
        }
    }

    return st.empty();
}
```

---

### 5.5 중복 제거

정렬된 결과가 필요하면 `set`:

```cpp
int n;
cin >> n;

set<int> st;
for (int i = 0; i < n; i++) {
    int x;
    cin >> x;
    st.insert(x);
}

for (int x : st) {
    cout << x << " ";
}
```

입력 순서를 유지하면서 중복 제거가 필요하면:

```cpp
int n;
cin >> n;

set<int> seen;
vector<int> result;

for (int i = 0; i < n; i++) {
    int x;
    cin >> x;

    if (seen.find(x) == seen.end()) {
        seen.insert(x);
        result.push_back(x);
    }
}

for (int x : result) {
    cout << x << " ";
}
```

---

## 6. 메모리와 포인터

## 6.1 주소

```cpp
int x = 10;
cout << &x;
```

`&x`는 `x`의 주소다.

---

## 6.2 포인터

포인터는 주소를 저장하는 변수다.

```cpp
int x = 10;
int* p = &x;
```

의미:

```text
x는 int 값 10을 저장
p는 x의 주소를 저장
```

---

## 6.3 역참조

```cpp
int x = 10;
int* p = &x;

cout << *p; // 10
```

`*p`는 p가 가리키는 위치의 값이다.

```cpp
*p = 20;
cout << x; // 20
```

---

## 6.4 `*`의 두 가지 의미

```cpp
int* p;
```

선언에서의 `*`: p는 int 포인터다.

```cpp
*p = 10;
```

사용에서의 `*`: p가 가리키는 곳의 값이다.

---

## 6.5 `&`의 두 가지 의미

```cpp
int x = 10;
int* p = &x;
```

`&x`: x의 주소.

```cpp
void f(int& x) {
    x++;
}
```

`int& x`: 참조자 선언.

문맥으로 구분한다.

---

## 6.6 참조자와 포인터 차이

참조자:

```cpp
void addOne(int& x) {
    x++;
}

int main() {
    int a = 10;
    addOne(a);
    cout << a; // 11
}
```

포인터:

```cpp
void addOne(int* p) {
    (*p)++;
}

int main() {
    int a = 10;
    addOne(&a);
    cout << a; // 11
}
```

비교:

```text
참조자: 기존 변수의 별명
포인터: 주소를 저장하는 변수

참조자: 선언 시 반드시 초기화
포인터: nullptr 가능

참조자: 다른 대상을 다시 가리킬 수 없음
포인터: 다른 주소를 다시 저장할 수 있음

참조자: 일반 변수처럼 사용
포인터: 역참조할 때 * 필요
```

---

## 6.7 `nullptr`

```cpp
int* p = nullptr;
```

아무것도 가리키지 않는 포인터다.

주의:

```cpp
cout << *p; // 위험
```

안전하게:

```cpp
if (p != nullptr) {
    cout << *p;
}
```

또는:

```cpp
if (p) {
    cout << *p;
}
```

---

## 6.8 stack 메모리와 heap 메모리

### stack

지역 변수는 보통 stack에 저장된다.

```cpp
void f() {
    int x = 10;
}
```

함수가 끝나면 `x`는 자동으로 사라진다.

### heap

`new`로 직접 할당하는 메모리다.

```cpp
int* p = new int;
*p = 10;

delete p;
p = nullptr;
```

`new`로 할당한 메모리는 직접 `delete`해야 한다.

---

## 6.9 memory leak

```cpp
int* p = new int;
*p = 10;

// delete p; 없음
```

동적으로 할당한 메모리를 해제하지 못하면 memory leak이 발생한다.

특히 다음 코드가 중요하다.

```cpp
int* p = new int;
*p = 10;

p = new int;
*p = 20;

delete p;
```

문제:

```text
첫 번째 new로 할당한 메모리 주소를 잃어버림
마지막 delete는 두 번째 메모리만 해제함
첫 번째 메모리는 해제할 방법이 없어짐
따라서 memory leak 발생
```

수정:

```cpp
int* p = new int;
*p = 10;
delete p;

p = new int;
*p = 20;
delete p;
p = nullptr;
```

---

## 6.10 dangling pointer

이미 해제된 메모리를 계속 가리키는 포인터다.

```cpp
int* p = new int(10);
delete p;

cout << *p; // 위험
```

`delete` 이후에는:

```cpp
delete p;
p = nullptr;
```

로 처리하는 습관이 좋다.

---

## 6.11 배열과 포인터

```cpp
int arr[3] = {10, 20, 30};
int* p = arr;

cout << *p << " " << *(p + 1);
```

출력:

```text
10 20
```

`arr`은 많은 상황에서 첫 번째 원소의 주소처럼 동작한다.

```text
*p       == arr[0]
*(p + 1) == arr[1]
*(p + 2) == arr[2]
```

실전 구현에서는 가능하면 C 배열보다 `vector`를 쓰는 것이 안전하다.

---

## 6.12 smart pointer 개념

직접 `new/delete`를 쓰면 memory leak, dangling pointer 같은 실수가 생기기 쉽다.  
현대 C++에서는 스마트 포인터를 사용해 메모리를 자동 관리하는 방식을 선호한다.

대표 예시:

```cpp
#include <memory>

unique_ptr<int> p = make_unique<int>(10);
```

핵심:

```text
new/delete: 직접 메모리 관리
smart pointer: 자동 메모리 관리
unique_ptr: 소유자가 하나
shared_ptr: 여러 곳에서 공유
```

면접 답변 예시:

```text
메모리 누수를 줄이기 위해 new/delete를 직접 쓰기보다 RAII와 smart pointer를 사용할 수 있습니다.
```

---

## 7. 시간복잡도 기초

### 7.1 O(1)

입력 크기와 상관없이 일정한 시간.

```cpp
cout << v[0];
```

---

### 7.2 O(n)

n개를 한 번 순회.

```cpp
for (int i = 0; i < n; i++) {
    cout << i;
}
```

---

### 7.3 O(n²)

이중 반복문.

```cpp
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        cout << i << " " << j << "\n";
    }
}
```

시간복잡도:

```text
O(n²)
```

---

### 7.4 `map`, `set`

일반적으로 `map`, `set`은 내부적으로 균형 이진 트리를 사용하므로 삽입/탐색이 보통 `O(log n)`이다.

```cpp
map<string, int> mp;
mp["apple"]++;
```

---

### 7.5 `unordered_map`, `unordered_set`

평균적으로 삽입/탐색이 `O(1)`이다.  
다만 정렬된 순서가 필요하면 `map`, `set`을 쓰는 것이 편하다.

기초 평가에서는 보통 `map`, `set`만 정확히 써도 충분하다.

---

## 8. 면접에서 말로 설명할 수 있어야 하는 답변

### 포인터란?

```text
포인터는 메모리 주소를 저장하는 변수입니다.
int* p는 int형 변수를 가리킬 수 있는 포인터이고, *p는 p가 가리키는 위치의 값을 의미합니다.
```

### 참조자와 포인터 차이

```text
참조자는 기존 변수의 별명이고, 선언할 때 반드시 초기화해야 합니다.
포인터는 주소를 저장하는 변수이고, nullptr일 수 있으며, 다른 대상을 다시 가리킬 수 있습니다.
```

### memory leak이란?

```text
동적으로 할당한 메모리를 해제하지 못하고 주소를 잃어버리는 경우입니다.
new로 할당한 메모리를 delete하지 않으면 memory leak이 발생할 수 있습니다.
```

### dangling pointer란?

```text
이미 해제되었거나 사라진 메모리를 계속 가리키고 있는 포인터입니다.
delete 이후 포인터를 사용하면 dangling pointer 문제가 생길 수 있습니다.
```

### `nullptr`이란?

```text
아무것도 가리키지 않는 포인터 값입니다.
nullptr을 역참조하면 안 되고, 포인터를 사용하기 전에 nullptr인지 확인하는 것이 안전합니다.
```

### stack과 heap 차이

```text
stack 메모리는 지역 변수처럼 자동으로 관리되는 메모리입니다.
함수가 끝나면 자동으로 사라집니다.
heap 메모리는 new 등을 통해 직접 할당하는 메모리이고, 직접 해제하거나 스마트 포인터로 관리해야 합니다.
```

### `vector`와 배열 차이

```text
배열은 크기가 고정되어 있고, vector는 동적으로 크기를 변경할 수 있는 컨테이너입니다.
vector는 push_back, size 같은 편리한 함수를 제공하고, 실전 구현에서 더 안전하게 사용할 수 있습니다.
```

### `map`은 언제 쓰나?

```text
key와 value를 연결해서 저장할 때 사용합니다.
예를 들어 단어 빈도수처럼 특정 문자열이 몇 번 나왔는지 셀 때 유용합니다.
```

### `stack`은 언제 쓰나?

```text
마지막에 들어온 것을 먼저 처리해야 할 때 사용합니다.
괄호 검사처럼 최근에 열린 괄호를 먼저 닫아야 하는 문제에 적합합니다.
```

---

## 9. 자주 하는 실수 체크리스트

코드 작성 후 아래를 확인한다.

```text
[ ] 배열/vector 인덱스가 범위를 넘지 않는가?
[ ] 반복문 조건에 <=를 잘못 쓰지 않았는가?
[ ] 빈 vector에 v[0]으로 접근하지 않았는가?
[ ] 빈 stack/queue에서 top/front/pop을 하지 않았는가?
[ ] cin 다음 getline을 쓸 때 개행 처리를 했는가?
[ ] 대입 = 과 비교 == 를 혼동하지 않았는가?
[ ] a++와 ++a의 차이를 확인했는가?
[ ] map[key]가 없는 key를 자동 생성한다는 점을 고려했는가?
[ ] 포인터를 초기화했는가?
[ ] nullptr을 역참조하지 않았는가?
[ ] new를 썼다면 delete를 했는가?
[ ] delete 이후 포인터를 다시 사용하지 않는가?
[ ] 함수에서 원본을 수정해야 하면 참조자 또는 포인터를 사용했는가?
[ ] 큰 객체를 읽기만 하면 const reference를 사용했는가?
```

---

## 10. 최소 코드 템플릿

### 기본 입출력 템플릿

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>
#include <set>
#include <stack>
#include <queue>
#include <sstream>
#include <algorithm>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    return 0;
}
```

부트캠프 평가 환경에서 `#include <bits/stdc++.h>`가 허용되면 편하게 쓸 수 있다.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    return 0;
}
```

다만 표준 C++ 헤더는 아니므로, 환경이 불확실하면 필요한 헤더를 직접 include하는 것이 안전하다.

---

## 11. 정렬과 탐색 기본

### 정렬

```cpp
vector<int> v = {3, 1, 2};
sort(v.begin(), v.end());

for (int x : v) {
    cout << x << " ";
}
```

출력:

```text
1 2 3
```

내림차순:

```cpp
sort(v.begin(), v.end(), greater<int>());
```

---

### `find`

```cpp
vector<int> v = {1, 2, 3};

auto it = find(v.begin(), v.end(), 2);

if (it != v.end()) {
    cout << "found";
}
```

---

### `sort + unique`로 중복 제거

```cpp
vector<int> v = {3, 1, 2, 3, 2, 1};

sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());

for (int x : v) {
    cout << x << " ";
}
```

출력:

```text
1 2 3
```

---

## 12. 면접 전날 학습 순서

### 1단계: 오답만 재확인

- `a++` vs `++a`
- `=` vs `==`
- 빈 `vector` 인덱스 접근
- `map[key]` 자동 생성
- stack 괄호 검사
- `new` 두 번으로 인한 memory leak

### 2단계: 핵심 패턴 눈으로 보기

- 단어 빈도수
- 괄호 검사
- 중복 제거
- 최댓값/최솟값
- 문자열 문자 개수 세기

### 3단계: 메모리/포인터 말로 설명

- 포인터
- 참조자
- nullptr
- memory leak
- dangling pointer
- stack vs heap
- smart pointer 개념

### 4단계: 새 내용 금지

면접 직전에는 새로운 고급 문법을 보지 말고, 이미 아는 내용에서 실수하지 않는 데 집중한다.

---

## 13. 문제를 더 풀어야 하는가?

반드시 문제를 많이 풀어야 하는 상태는 아니다.  
현재 포인터/참조자 기본은 괜찮고, STL도 어느 정도 사용할 수 있다.

다만 실제 평가가 코딩 문제를 포함한다면, 새로운 문제를 많이 풀기보다 아래 3개 패턴만 손으로 한 번씩 써보는 것이 좋다.

1. `map`으로 단어 빈도수 세기
2. `stack`으로 괄호 검사하기
3. `vector` 입력받고 최댓값/최솟값 구하기

시간이 정말 부족하면 문제 풀이보다 이 문서를 반복해서 보고, 특히 체크리스트를 확인하는 편이 더 효율적이다.

---

## 14. 마지막 10분 체크

```text
포인터는 주소 저장 변수
*p는 가리키는 값
&x는 x의 주소
int&는 참조자
nullptr 역참조 금지
new 했으면 delete
이미 delete한 포인터 사용 금지
빈 vector에 v[0] 금지
빈 stack에서 top/pop 금지
반복문은 i < size()
map[key]는 없으면 생성
cin 다음 getline은 ignore 필요
a++는 쓰고 증가, ++a는 증가하고 씀
```
