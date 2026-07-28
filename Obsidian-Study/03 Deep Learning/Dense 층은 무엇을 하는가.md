# Dense 층은 무엇을 하는가?

## 한 줄 설명

`Dense`는 이전 층의 모든 출력과 현재 층의 각 노드를 연결해 가중합을 계산하고 활성화 함수를 적용하는 완전 연결 층이다.

## 왜 필요한가

입력 특징을 여러 조합으로 변환하며 분류에 필요한 패턴을 학습한다. 여러 Dense 층을 쌓으면 단순한 선형 경계보다 복잡한 관계를 표현할 수 있다.

## 핵심 코드

```python
model = Sequential()
model.add(Dense(12, input_dim=7, activation="relu"))
model.add(Dense(8, activation="relu"))
model.add(Dense(1, activation="sigmoid"))
```

## 꼭 기억할 점

- 첫 Dense의 `input_dim=7`은 입력 특징 수다.
- `12`, `8`, `1`은 각 층의 출력 노드 수다.
- 첫 층은 입력을 받고, 중간은 은닉층, 마지막은 출력층 역할을 한다.
- 출력층 노드 수와 활성화 함수는 예측할 class 구조에 맞춰야 한다.

> [!warning]
> 수업 코드 일부는 두 번째 Dense에도 `input_dim`을 적었지만 첫 층 이후 입력 크기는 이전 층 출력에서 자동으로 정해지므로 보통 생략한다.

## 관련 노트

- [[ReLU와 sigmoid와 softmax는 어떤 역할을 하는가]]
- [[가중치와 편향은 언제 수정되는가]]

## 출처

- `day0723.ipynb`
- `인공지능_프로그래밍_9일차_온디1기.pdf`, 74~103쪽

