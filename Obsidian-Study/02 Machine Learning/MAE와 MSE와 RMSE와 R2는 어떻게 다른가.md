# MAE와 MSE와 RMSE와 R2는 어떻게 다른가?

## 한 줄 설명

MAE·MSE·RMSE는 예측 오차의 크기를 서로 다른 방식으로 계산하고, `R2`는 평균으로 예측한 기준보다 모델이 얼마나 잘 설명하는지 나타낸다.

## 왜 필요한가

같은 예측 결과도 큰 오차를 얼마나 강하게 벌점으로 볼지, 원래 값과 같은 단위로 해석할지에 따라 평가가 달라진다. 목적에 맞는 지표를 함께 보면 모델의 성능을 한쪽 기준으로만 판단하는 일을 줄일 수 있다.

## 핵심 코드

```python
from sklearn.metrics import (
    mean_absolute_error,
    mean_squared_error,
    root_mean_squared_error,
    r2_score
)

mae = mean_absolute_error(y_test, y_pred)
mse = mean_squared_error(y_test, y_pred)
rmse = root_mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
```

## 꼭 기억할 점

- MAE는 오차 절댓값의 평균이라 해석이 직관적이다.
- MSE는 오차를 제곱해 큰 오차에 더 큰 벌점을 준다.
- RMSE는 MSE에 제곱근을 적용해 정답과 같은 단위로 돌아온다.
- `R2`는 `1`에 가까울수록 좋고, `0`은 평균 예측 수준이며 음수가 될 수도 있다.
- 오차 지표는 작을수록 좋지만 `R2`는 클수록 좋다.

## 관련 노트

- [[선형 회귀는 무엇을 예측하는가]]
- [[손실 함수는 왜 필요한가]]

## 출처

- `day0723.ipynb`
