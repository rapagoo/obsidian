# epoch는 무엇인가?

## 한 줄 설명

epoch는 전체 학습 데이터를 모델이 한 번 모두 사용한 횟수다.

## 왜 필요한가

한 번만 데이터를 보는 것으로 충분히 패턴을 학습하지 못할 수 있어 같은 학습 데이터를 여러 번 반복한다.

## 핵심 코드

```python
model.fit(
    X_train,
    y_train,
    epochs=200,
    batch_size=64
)
```

## 꼭 기억할 점

- `epochs=200`은 전체 학습 데이터를 200회 반복한다는 뜻이다.
- 한 epoch 안에서 데이터는 `batch_size`만큼 나뉘어 처리된다.
- epoch가 많다고 항상 좋은 것은 아니며 검증 손실이 나빠지면 과적합일 수 있다.
- K-fold 실습에서는 각 fold마다 새 모델을 100 epoch씩 학습했다.

## 관련 노트

- [[batch_size는 무엇인가]]
- [[validation_split은 왜 사용하는가]]
- [[과소적합과 과대적합은 어떻게 다른가]]

## 출처

- `day0723.ipynb`
- `day0727.ipynb`

