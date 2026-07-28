# 붓꽃 데이터로 KNN과 결정 트리와 SVM 비교하기

## 실습 목표

같은 iris 데이터와 같은 분할을 사용해 KNN, 결정 트리, SVM의 학습 흐름과 전처리 차이를 비교한다.

## 전체 흐름

1. iris 데이터의 `data`와 `target`을 `X`, `y`로 지정
2. `random_state=20`으로 분할
3. 모델별 생성·학습·예측
4. `accuracy_score`로 평가
5. SVM에는 스케일링 적용

## 핵심 코드

```python
iris_dataset = load_iris()
X = iris_dataset.data
y = iris_dataset.target

X_train, X_test, y_train, y_test = train_test_split(
    X, y, random_state=20
)

knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train, y_train)

d_tree = DecisionTreeClassifier(max_depth=3, random_state=20)
d_tree.fit(X_train, y_train)

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

svc = SVC(kernel="rbf", C=1.0, random_state=20)
svc.fit(X_train_scaled, y_train)
```

## 비교 포인트

- KNN은 가까운 이웃을 사용한다.
- 결정 트리는 특징 질문을 만들며 `max_depth`로 복잡도를 제한했다.
- SVM은 결정 경계를 만들며 같은 scaler 기준이 필요하다.

## 이 실습에서 사용한 개념

- [[X와 y는 무엇인가]]
- [[KNN은 어떻게 분류하는가]]
- [[결정 트리는 어떻게 판단하는가]]
- [[SVM은 데이터를 어떻게 분류하는가]]
- [[StandardScaler는 왜 필요한가]]

## 출처

- `day0722.ipynb`
- `day0723.ipynb`

