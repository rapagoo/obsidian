# ReLU와 sigmoid와 softmax는 어떤 역할을 하는가?

## 한 줄 설명

활성화 함수는 Dense의 가중합을 다음 층에 전달할 값으로 바꾸며, ReLU는 은닉층, sigmoid는 이진 출력, softmax는 다중 class 출력에 주로 사용한다.

## 왜 필요한가

활성화 함수가 없으면 여러 Dense 층을 쌓아도 복잡한 비선형 관계를 배우기 어렵다. 출력층에서는 분류 문제에 맞는 활성화 함수를 사용해야 결과를 확률처럼 해석하고 알맞은 손실 함수와 연결할 수 있다.

## 핵심 구분

- `relu`: 음수는 `0`, 양수는 그대로 전달한다.
- `sigmoid`: 하나의 값을 `0~1` 범위로 바꾼다.
- `softmax`: 여러 출력값을 합이 1인 class별 확률 형태로 바꾼다.

## 핵심 코드

```python
model.add(Dense(12, activation="relu"))
model.add(Dense(1, activation="sigmoid"))

# 3-class 분류 출력
model.add(Dense(3, activation="softmax"))
```

## 꼭 기억할 점

- 이진 분류는 출력 노드 1개와 sigmoid를 사용했다.
- iris 3-class 분류는 출력 노드 3개와 softmax를 사용했다.
- 활성화 함수와 손실 함수, label 형태가 서로 맞아야 한다.
- `Dense(1, activation="softmax")`는 출력이 항상 1이 되므로 3-class 분류 출력으로 적합하지 않다.

## 관련 노트

- [[Dense 층은 무엇을 하는가]]
- [[binary_crossentropy와 sparse_categorical_crossentropy는 언제 사용하는가]]
- [[로지스틱 회귀는 왜 분류 모델인가]]

## 출처

- `day0723.ipynb`
- `인공지능_프로그래밍_9일차_온디1기.pdf`, 64~73쪽, 104~105쪽
