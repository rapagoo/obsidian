# 테스트 데이터에는 왜 fit하지 않는가?

## 한 줄 설명

테스트 데이터에 전처리기의 `fit()`을 수행하면 평가 전에 테스트 분포의 정보를 사용하게 되어 데이터 누수가 생긴다.

## 왜 필요한가

시험 문제 전체의 평균과 표준편차를 미리 보고 공부하는 것과 비슷하다. 실제 운영에서는 미래 데이터의 통계를 미리 알 수 없으므로 평가가 낙관적으로 바뀔 수 있다.

## 핵심 코드

```python
scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

## 꼭 기억할 점

- 학습 데이터에서 얻은 평균과 표준편차를 테스트 데이터에도 그대로 적용한다.
- `X_test_scaled = scaler.fit_transform(X_test)`로 별도 기준을 만들면 안 된다.
- 결측치 대체, 인코딩, 특징 선택처럼 데이터에서 기준을 배우는 전처리도 같은 원칙을 따른다.
- pipeline을 사용하면 전처리와 모델 학습 흐름을 함께 관리할 수 있지만 수업 코드에서는 직접 단계를 구분했다.

## 관련 노트

- [[학습 데이터와 테스트 데이터는 왜 나누는가]]
- [[fit과 transform은 어떻게 다른가]]
- [[StandardScaler는 왜 필요한가]]

## 출처

- `day0723.ipynb`
- 추가 설명: 일반적인 scikit-learn 데이터 누수 방지 원칙

