# K-fold 교차 검증은 왜 사용하는가?

## 한 줄 설명

K-fold 교차 검증은 데이터를 `K`개 묶음으로 나누고, 매번 다른 한 묶음을 검증에 사용해 `K`번의 성능을 평균내는 방법이다.

## 왜 필요한가

한 번의 학습·테스트 분할 결과는 우연히 쉽거나 어려운 샘플 구성에 영향을 받을 수 있다. 여러 분할에서 평가하면 성능의 안정성을 더 잘 볼 수 있다.

## 핵심 코드

```python
from sklearn.model_selection import KFold

kfold = KFold(
    n_splits=5,
    random_state=20,
    shuffle=True
)

for train_index, test_index in kfold.split(X):
    X_train, X_test = X[train_index], X[test_index]
    y_train, y_test = y[train_index], y[test_index]
```

## 꼭 기억할 점

- 각 fold마다 새 모델을 만들어 처음부터 학습한다.
- 수업의 5-fold에서는 iris 150개가 매번 학습 120개, 검증 30개로 나뉘었다.
- fold별 accuracy와 loss를 모아 평균을 계산했다.
- 최종 테스트셋을 따로 유지하는 흐름과 K-fold의 목적을 구분한다.

## 관련 노트

- [[학습 데이터와 테스트 데이터는 왜 나누는가]]
- [[validation_split은 왜 사용하는가]]
- [[붓꽃 신경망을 K-fold로 검증하기]]

## 출처

- `day0727.ipynb`
