# fit과 transform은 어떻게 다른가?

## 한 줄 설명

전처리기의 `fit()`은 변환에 필요한 기준을 학습하고, `transform()`은 그 기준을 실제 데이터에 적용한다.

## 왜 필요한가

StandardScaler는 먼저 평균과 표준편차를 알아야 값을 바꿀 수 있다. 기준을 배우는 단계와 값을 바꾸는 단계를 구분하면 데이터 누수를 막을 수 있다.

## 핵심 코드

```python
scaler = StandardScaler()

scaler.fit(X_train)
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

`fit_transform(X_train)`은 학습 데이터에 대한 `fit()`과 `transform()`을 연속으로 실행한 것이다.

## 꼭 기억할 점

- `fit()`만 호출하면 원본 데이터 값은 아직 변하지 않는다.
- `transform()`은 이미 학습된 기준이 있어야 한다.
- 학습·테스트 데이터 모두 열의 의미와 순서가 같아야 한다.
- 모델의 `fit()`은 예측 규칙을 배우고 전처리기의 `fit()`은 변환 기준을 배운다.

## 관련 노트

- [[StandardScaler는 왜 필요한가]]
- [[테스트 데이터에는 왜 fit하지 않는가]]
- [[fit과 predict는 무엇을 하는가]]

## 출처

- `day0723.ipynb`

