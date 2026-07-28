# binary_crossentropy와 sparse_categorical_crossentropy는 언제 사용하는가?

## 한 줄 설명

`binary_crossentropy`는 두 class를 sigmoid 출력 하나로 분류할 때, `sparse_categorical_crossentropy`는 정수 label을 가진 다중 class를 softmax 출력으로 분류할 때 사용한다.

## 왜 필요한가

손실 함수는 모델의 출력 구조와 정답 label을 비교하는 규칙이다. 이진 분류인지 다중 class 분류인지, label이 정수인지 원-핫 벡터인지에 맞춰야 학습 오차가 올바르게 계산된다.

## 핵심 코드

```python
# 타이타닉 생존: label 0 또는 1
model.add(Dense(1, activation="sigmoid"))
model.compile(loss="binary_crossentropy", optimizer="adam")

# iris 품종: label 0, 1, 2
model.add(Dense(3, activation="softmax"))
model.compile(
    loss="sparse_categorical_crossentropy",
    optimizer="adam"
)
```

## 꼭 기억할 점

- `sparse`는 label이 원-핫 벡터가 아니라 `0`, `1`, `2` 같은 정수라는 뜻이다.
- 일반 `categorical_crossentropy`는 보통 정답도 class별 원-핫 벡터일 때 사용한다.
- 출력 노드 수는 class 수와 맞춰야 한다.

> [!warning]
> `day0723.ipynb`의 한 실험은 iris에 `Dense(1, softmax)`와 `categorical_crossentropy`를 사용했다. 이는 3-class 정수 label 구조와 맞지 않으며, 바로 앞 셀의 `Dense(3, softmax)`와 `sparse_categorical_crossentropy` 조합이 수업 데이터에 맞다.

## 관련 노트

- [[ReLU와 sigmoid와 softmax는 어떤 역할을 하는가]]
- [[손실 함수는 왜 필요한가]]

## 출처

- `day0723.ipynb`
