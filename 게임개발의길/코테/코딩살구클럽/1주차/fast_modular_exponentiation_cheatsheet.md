# BOJ 1629 곱셈 — 빠른 거듭제곱 암기 핵심

## 1. 언제 쓰는 패턴인가?

아래 조건이 보이면 **빠른 거듭제곱**을 떠올린다.

```text
A^B % C
거듭제곱
나머지
B가 매우 큼
mod 1,000,000,007
```

---

## 2. 반드시 외울 문장

```text
a^b는 a^(b/2)를 구해서 제곱하면 된다.
b가 홀수면 a를 한 번 더 곱한다.
곱할 때마다 mod를 한다.
```

---

## 3. 핵심 수식

```text
b가 짝수:
a^b = a^(b/2) * a^(b/2)

b가 홀수:
a^b = a^(b/2) * a^(b/2) * a
```

---

## 4. 암기용 코드

```cpp
using ll = long long;

ll modPow(ll a, ll b, ll mod) {
    if (b == 0) return 1 % mod;

    ll ret = modPow(a, b / 2, mod);
    ret = ret * ret % mod;

    if (b % 2 == 1) ret = ret * a % mod;

    return ret;
}
```

---

## 5. 사용 예시

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll modPow(ll a, ll b, ll mod) {
    if (b == 0) return 1 % mod;

    ll ret = modPow(a, b / 2, mod);
    ret = ret * ret % mod;

    if (b % 2 == 1) ret = ret * a % mod;

    return ret;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    ll a, b, c;
    cin >> a >> b >> c;

    cout << modPow(a, b, c) << '\n';
}
```

---

## 6. 코드 해석 핵심

```cpp
ll ret = modPow(a, b / 2, mod);
```

`a^(b/2) % mod`를 구한다.

```cpp
ret = ret * ret % mod;
```

절반 거듭제곱을 제곱해서 `a^b`에 가깝게 만든다.

```cpp
if (b % 2 == 1) ret = ret * a % mod;
```

`b`가 홀수면 남은 `a` 하나를 더 곱한다.

---

## 7. 시간복잡도

```text
O(log B)
```

`B`를 매번 절반으로 줄이기 때문이다.

예시:

```text
11 → 5 → 2 → 1 → 0
```

---

## 8. 주의할 점

### 중간마다 mod를 해야 한다

```cpp
ret = ret * ret % mod;
```

거듭제곱 값은 너무 커져서 `long long`에도 담을 수 없다.  
그래서 곱할 때마다 나머지를 유지한다.

### 실전에서는 `b == 0` 종료 조건을 추천한다

```cpp
if (b == 0) return 1 % mod;
```

수학적으로 `a^0 = 1`이기 때문에 가장 범용적이다.

---

## 9. 최종 암기 요약

```text
빠른 거듭제곱 = 지수를 절반으로 줄이는 거듭제곱

1. b == 0이면 1 반환
2. ret = modPow(a, b / 2, mod)
3. ret = ret * ret % mod
4. b가 홀수면 ret = ret * a % mod
5. return ret
```
