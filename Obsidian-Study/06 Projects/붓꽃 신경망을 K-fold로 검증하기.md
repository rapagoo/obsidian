# 붓꽃 신경망을 K-fold로 검증하기

## 실습 목표

iris 데이터를 5개 fold로 나누고 매 fold마다 같은 구조의 새 신경망을 학습해 accuracy와 loss를 평균낸다.

## 전체 흐름

1. 모델 생성 함수를 정의
2. `KFold(n_splits=5)`로 인덱스 분할
3. fold별 학습·검증 데이터 구성
4. 매번 새 모델 생성과 100 epoch 학습
5. fold별 평가값 저장과 평균 계산

## 핵심 코드

```python
def model_fn():
    model = Sequential()
    model.add(Dense(24, input_dim=4, activation="relu"))
    model.add(Dense(12, activation="relu"))
    model.add(Dense(3, activation="softmax"))
    model.compile(
        loss="sparse_categorical_crossentropy",
        optimizer="adam",
        metrics=["accuracy"]
    )
    return model

kfold = KFold(n_splits=5, random_state=20, shuffle=True)
accuracy_list = []

for train_index, test_index in kfold.split(X):
    X_train, X_test = X[train_index], X[test_index]
    y_train, y_test = y[train_index], y[test_index]

    model = model_fn()
    model.fit(X_train, y_train, epochs=100, batch_size=16)
    accuracy_list.append(model.evaluate(X_test, y_test)[1])

print(np.mean(accuracy_list))
```

수업 실행 결과의 평균 accuracy는 약 `0.9733`이었다.

## 이 실습에서 사용한 개념

- [[K-fold 교차 검증은 왜 사용하는가]]
- [[Dense 층은 무엇을 하는가]]
- [[epoch는 무엇인가]]
- [[batch_size는 무엇인가]]

## 출처

- `day0727.ipynb`
