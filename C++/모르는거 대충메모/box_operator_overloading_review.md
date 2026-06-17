# C++ 클래스 연산자 오버로딩 복습 노트

## 1. 이번에 내가 헷갈렸던 핵심

이번 `Box It!` 문제에서 막혔던 핵심은 **연산자 오버로딩 문법 자체**보다, **`<`의 비교 기준을 문제에서 어떻게 정의했는지**였다.

즉,

- `operator<`를 만든다는 것
- 그리고 그 안에서 **무엇을 기준으로 "작다"고 판단할지 정하는 것**

이 두 가지를 분리해서 이해해야 한다.

---

## 2. `operator<`의 기본 형태

보통 형태는 다음과 같다.

```cpp
bool operator<(const ClassName& other) const
{
    // 비교 기준 작성
}
```

### 의미

- `bool` : `<`의 결과는 참/거짓이므로 반환형은 `bool`
- `operator<` : `<` 연산자를 오버로딩
- `const ClassName& other` : 오른쪽 피연산자를 참조로 받음
- 함수 뒤의 `const` : 현재 객체를 바꾸지 않겠다는 의미

예를 들어:

```cpp
if (a < b)
```

는 내부적으로 거의 이렇게 생각하면 된다.

```cpp
a.operator<(b)
```

---

## 3. 내가 놓치고 있던 부분: `<`의 기준은 내가 정하는 것이다

기본 자료형 `int`는 `3 < 5`처럼 비교 기준이 이미 정해져 있다.

하지만 클래스는 다르다.

예를 들어 `Box`는

- length
- breadth
- height

세 값이 있다.

그러면

```cpp
box1 < box2
```

가 무슨 뜻인지 **문제나 설계자가 직접 정해야 한다.**

즉, `operator<`는 **정답이 하나로 정해져 있는 문법**이 아니라,
**비교 기준을 코드로 표현하는 도구**다.

---

## 4. `Box It!` 문제에서의 비교 기준

이 문제에서 `a < b`의 뜻은 다음이다.

1. `a.length < b.length`
2. 또는 `a.length == b.length` 이고 `a.breadth < b.breadth`
3. 또는 `a.length == b.length` 이고 `a.breadth == b.breadth` 이고 `a.height < b.height`

이것은 **사전식 비교(lexicographical comparison)** 이다.

즉,

- 먼저 `length` 비교
- 같으면 `breadth` 비교
- 그것도 같으면 `height` 비교

### 구현

```cpp
bool operator<(const Box& b) const
{
    if (length != b.length) return length < b.length;
    if (breadth != b.breadth) return breadth < b.breadth;
    return height < b.height;
}
```

---

## 5. 왜 `세 변이 모두 작다`가 아니었나?

처음에 흔히 떠올릴 수 있는 잘못된 기준은 이런 것이다.

```cpp
bool operator<(const Box& b) const
{
    return length < b.length && breadth < b.breadth && height < b.height;
}
```

하지만 이 문제는 그렇게 비교하지 않는다.

이 방식은 정렬 기준으로도 애매해질 수 있다.

예를 들어:

- `(1, 10, 10)`
- `(2, 1, 1)`

이 둘을 비교하면,
한쪽은 length는 작지만 breadth와 height는 크다.
그러면 어느 쪽이 "작다"고 할지 애매해진다.

그래서 이런 문제에서는 보통 **사전식 비교**를 사용한다.

---

## 6. 사전식 비교를 감각적으로 이해하기

문자열 사전순 비교와 비슷하다.

예를 들어:

- `apple`
- `banana`

를 비교할 때,
처음 다른 글자를 기준으로 판단한다.

`Box`도 비슷하다.

예를 들어:

- `(1, 5, 10)`
- `(1, 7, 2)`

라면,

- length는 둘 다 1로 같음
- breadth 비교: `5 < 7`
- 따라서 첫 번째가 더 작음

즉 뒤의 값 전체를 다 보는 게 아니라,
**앞에서부터 차례대로 비교하다가 처음 다른 곳에서 결정**한다.

---

## 7. `operator<<`는 왜 클래스 밖에 많이 쓰는가?

이 문제에서는 이것도 구현해야 했다.

```cpp
cout << temp << endl;
```

이걸 가능하게 하려면 `<<` 연산자를 오버로딩해야 한다.

보통 이렇게 쓴다.

```cpp
friend ostream& operator<<(ostream& out, const Box& B);
```

그리고 클래스 밖에서 구현한다.

```cpp
ostream& operator<<(ostream& out, const Box& B)
{
    out << B.length << ' ' << B.breadth << ' ' << B.height;
    return out;
}
```

### 왜 클래스 안의 멤버 함수로 잘 안 만드는가?

`cout << temp`에서 **왼쪽 피연산자**는 `cout`이다.

즉 개념적으로는 이런 쪽에 가깝다.

```cpp
cout.operator<<(temp)
```

왼쪽이 `Box`가 아니라 `ostream`이기 때문에,
`Box`의 멤버 함수로 만들기보다 **전역 함수 / friend 함수**로 만드는 것이 자연스럽다.

---

## 8. `friend`를 쓰는 이유

`length`, `breadth`, `height`는 `private` 멤버다.

그런데 클래스 밖의 `operator<<` 함수가 이 값들을 직접 출력하려면 접근 권한이 필요하다.

그래서:

```cpp
friend ostream& operator<<(ostream& out, const Box& B);
```

처럼 `friend`를 써서 접근을 허용한다.

---

## 9. `const Box&`로 받는 이유

연산자 오버로딩에서 매개변수를 이렇게 받는 이유는 두 가지다.

```cpp
const Box& b
```

### 1) 복사를 피하기 위해

값으로 받으면 객체가 복사된다.
참조(`&`)로 받으면 복사 비용이 없다.

### 2) 수정하지 않겠다는 뜻을 명확히 하기 위해

`const`를 붙이면 함수 안에서 `b`를 바꿀 수 없다.
비교 연산은 보통 상대 객체를 바꾸지 않으므로 `const`가 맞다.

---

## 10. 함수 뒤의 `const`는 무슨 뜻인가?

예:

```cpp
bool operator<(const Box& b) const
```

맨 뒤의 `const`는
**이 멤버 함수가 현재 객체(`*this`)를 바꾸지 않는다**는 뜻이다.

즉, 비교만 하고 객체 상태는 변경하지 않겠다는 의미다.

같은 이유로 getter도 보통 이렇게 쓴다.

```cpp
int getLength() const
```

---

## 11. `CalculateVolume()`에서 `1LL`를 붙이는 이유

네 코드의 이 부분은 이렇게 쓰는 것이 더 안전하다.

```cpp
long long CalculateVolume() const
{
    return 1LL * length * breadth * height;
}
```

### 이유

반환형이 `long long`이어도,

```cpp
length * breadth * height
```

가 먼저 `int * int * int`로 계산되면
중간 계산에서 오버플로우가 날 수 있다.

그래서 처음부터 `1LL`을 곱해서
전체 계산을 `long long` 기준으로 하도록 유도한다.

---

## 12. 내가 작성한 코드에서 같이 기억할 점

### 복사 생성자

```cpp
Box(const Box& other) : length(other.length), breadth(other.breadth), height(other.height) {}
```

이건 `Box NewBox(temp);` 같은 코드에서 사용된다.

### 대입과 복사는 다르다

```cpp
temp = NewBox;
```

이건 **복사 생성자**가 아니라 **복사 대입 연산자** 쪽 개념이다.

이번 문제에서는 기본 대입 연산자로도 충분해서 따로 구현하지 않아도 된다.

즉,

- `Box NewBox(temp);` → 복사 생성자
- `temp = NewBox;` → 복사 대입

이 둘은 구분해서 기억해야 한다.

---

## 13. 이번 문제 최종 핵심 코드

```cpp
class Box
{
private:
    int length{};
    int breadth{};
    int height{};

public:
    Box() {}
    Box(int a, int b, int c) : length(a), breadth(b), height(c) {}
    Box(const Box& other) : length(other.length), breadth(other.breadth), height(other.height) {}

    int getLength() const { return length; }
    int getBreadth() const { return breadth; }
    int getHeight() const { return height; }

    long long CalculateVolume() const
    {
        return 1LL * length * breadth * height;
    }

    bool operator<(const Box& b) const
    {
        if (length != b.length) return length < b.length;
        if (breadth != b.breadth) return breadth < b.breadth;
        return height < b.height;
    }

    friend ostream& operator<<(ostream& out, const Box& B);
};

ostream& operator<<(ostream& out, const Box& B)
{
    out << B.length << ' ' << B.breadth << ' ' << B.height;
    return out;
}
```

---

## 14. 이번에 복습해야 할 개념 체크리스트

아래 질문에 스스로 답할 수 있으면 이번 내용은 꽤 잡힌 것이다.

### 연산자 오버로딩
- [ ] `operator<`의 기본 선언 형태를 말할 수 있는가?
- [ ] `bool operator<(const Box& b) const` 각 부분의 의미를 설명할 수 있는가?
- [ ] `a < b`가 내부적으로 `a.operator<(b)`와 연결된다는 것을 이해했는가?

### 비교 기준
- [ ] 클래스의 `<` 기준은 자동으로 정해지는 것이 아니라, 내가 정의하는 것임을 이해했는가?
- [ ] `Box It!` 문제의 `<`가 부피 비교가 아니라 사전식 비교라는 것을 설명할 수 있는가?
- [ ] 왜 `length && breadth && height` 방식이 이 문제의 정답이 아닌지 설명할 수 있는가?

### 출력 연산자
- [ ] 왜 `operator<<`는 보통 클래스 밖에서 구현하는지 설명할 수 있는가?
- [ ] 왜 `friend`가 필요한지 설명할 수 있는가?

### const / 참조
- [ ] 왜 `const Box&`로 받는지 설명할 수 있는가?
- [ ] 함수 뒤의 `const`가 무엇을 의미하는지 설명할 수 있는가?

### 기타
- [ ] 왜 `1LL * length * breadth * height`로 쓰는지 설명할 수 있는가?
- [ ] 복사 생성자와 대입의 차이를 구분할 수 있는가?

---

## 15. 짧은 암기용 요약

### `operator<`

```cpp
bool operator<(const Box& b) const
{
    if (length != b.length) return length < b.length;
    if (breadth != b.breadth) return breadth < b.breadth;
    return height < b.height;
}
```

### `operator<<`

```cpp
friend ostream& operator<<(ostream& out, const Box& B);

ostream& operator<<(ostream& out, const Box& B)
{
    out << B.length << ' ' << B.breadth << ' ' << B.height;
    return out;
}
```

### 부피 계산

```cpp
return 1LL * length * breadth * height;
```

---

## 16. 다음에 비슷한 문제를 만나면 체크할 것

1. 이 연산자의 **비교/동작 기준이 문제에서 어떻게 정의되었는가?**
2. 멤버 함수로 구현할지, 전역/friend 함수로 구현할지?
3. `const`, `const&`를 붙여야 하는가?
4. 반환형은 무엇인가?
5. 큰 수 계산이면 `long long` 처리가 안전한가?

