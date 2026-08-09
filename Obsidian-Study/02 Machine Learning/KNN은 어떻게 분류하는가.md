# KNN은 어떻게 분류하는가?

## 한 줄 설명

KNN은 새 데이터와 가까운 학습 샘플 `K`개를 찾고, 이웃에서 가장 많은 class로 분류한다.

## 왜 필요한가

“나는 누구와 가장 비슷한가”라는 직관으로 분류할 수 있어 첫 머신러닝 모델로 이해하기 쉽다.

## 핵심 코드

```python
from sklearn.neighbors import KNeighborsClassifier

knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train, y_train)
y_pred = knn.predict(X_test)
```

## 꼭 기억할 점

- `n_neighbors`는 모델이 자동으로 학습하지 않고 사람이 정하는 hyperparameter다.
- `K`가 너무 작으면 주변 잡음에 민감하고, 너무 크면 경계가 지나치게 단순해질 수 있다.
- 거리 기반 모델이므로 특징의 단위와 범위 차이에 영향을 받는다.
- 수업에서는 홀수 `K`를 바꾸며 테스트 정확도를 그래프로 비교했다.
- 갱신된 `day0728.ipynb`에서는 손 landmark 좌표나 관절 각도를 특징으로 만들어 `K=3`인 제스처 분류에도 사용했다.

## 관련 노트

- [[StandardScaler는 왜 필요한가]]
- [[과소적합과 과대적합은 어떻게 다른가]]
- [[손 랜드마크 좌표는 어떻게 제스처 특징이 되는가]]
- [[붓꽃 데이터로 KNN과 결정 트리와 SVM 비교하기]]
- [[MediaPipe와 KNN으로 손 제스처 분류하기]]

## 출처

- `day0722.ipynb`
- `day0728.ipynb`
