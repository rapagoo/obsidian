# SVM은 데이터를 어떻게 분류하는가?

## 한 줄 설명

SVM은 class 사이를 나누는 결정 경계를 찾되, 경계와 가장 가까운 샘플인 support vector까지의 margin을 크게 만들려는 모델이다.

## 왜 필요한가

특징 공간에서 class를 안정적으로 나누는 경계를 만들 수 있으며, kernel을 사용하면 직선으로 나누기 어려운 비선형 데이터도 다룰 수 있다.

## 핵심 코드

```python
from sklearn.svm import SVC

svc = SVC(kernel="rbf", C=1.0, random_state=20)
svc.fit(X_train_scaled, y_train)
y_pred = svc.predict(X_test_scaled)
```

## 꼭 기억할 점

- `kernel="rbf"`는 비선형 경계를 만들 때 사용하는 대표적인 kernel이다.
- `C`는 오분류 허용과 경계 복잡도 사이의 균형을 조절한다.
- SVM은 특징의 크기에 민감하므로 수업에서는 `StandardScaler`를 먼저 적용했다.
- 학습과 예측에 같은 scaler 기준을 사용해야 한다.

## 관련 노트

- [[StandardScaler는 왜 필요한가]]
- [[fit과 transform은 어떻게 다른가]]
- [[붓꽃 데이터로 KNN과 결정 트리와 SVM 비교하기]]

## 출처

- `day0723.ipynb`

