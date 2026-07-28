# compile과 fit과 evaluate는 무엇을 하는가?

## 한 줄 설명

`compile()`은 학습 규칙을 설정하고, `fit()`은 가중치를 학습하며, `evaluate()`는 정답이 있는 데이터에서 손실과 metric을 계산한다.

## 왜 필요한가

신경망을 만드는 단계, 실제로 학습하는 단계, 학습하지 않은 데이터로 확인하는 단계를 구분해야 결과를 올바르게 해석할 수 있다. 세 메서드는 이 흐름에서 서로 다른 책임을 맡는다.

## 핵심 코드

```python
model.compile(
    loss="binary_crossentropy",
    optimizer="adam",
    metrics=["accuracy"]
)

model.fit(X_train, y_train, epochs=200, batch_size=64)
score = model.evaluate(X_test, y_test)
```

## 꼭 기억할 점

- 층을 구성한 뒤 학습 전에 `compile()`한다.
- `fit()`은 학습 데이터로 파라미터를 업데이트한다.
- `evaluate()`는 파라미터를 바꾸지 않고 손실과 metric을 반환한다.
- `metrics=["accuracy"]`이면 `score[1]`이 accuracy다.
- 새 입력의 class나 확률이 필요하면 `predict()`를 사용한다.

## 관련 노트

- [[손실 함수는 왜 필요한가]]
- [[epoch는 무엇인가]]
- [[학습 데이터와 테스트 데이터는 왜 나누는가]]

## 출처

- `day0723.ipynb`
