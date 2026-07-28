# StandardScaler는 왜 필요한가?

## 한 줄 설명

`StandardScaler`는 각 특징에서 평균을 빼고 표준편차로 나누어 평균이 0, 표준편차가 1에 가까운 범위로 맞춘다.

## 왜 필요한가

키와 몸무게처럼 단위와 숫자 범위가 다르면 큰 숫자의 특징이 거리나 경계 계산에 더 강하게 영향을 줄 수 있다.

## 핵심 코드

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

## 꼭 기억할 점

- scaler의 평균과 표준편차는 학습 데이터에서만 구한다.
- 테스트 데이터에는 학습된 같은 기준으로 `transform()`만 적용한다.
- 스케일링은 값의 단위를 맞추지만 데이터의 순서를 섞지는 않는다.
- 수업에서는 SVM에 반드시 스케일링을 적용했고, 거리 기반 KNN에도 영향을 줄 수 있다.

## 관련 노트

- [[fit과 transform은 어떻게 다른가]]
- [[테스트 데이터에는 왜 fit하지 않는가]]
- [[SVM은 데이터를 어떻게 분류하는가]]

## 출처

- `day0723.ipynb`

