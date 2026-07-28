# optimizer와 adam은 무엇을 하는가?

## 한 줄 설명

optimizer는 손실을 줄이는 방향으로 모델의 가중치와 편향을 업데이트하며, `adam`은 학습률을 적응적으로 조절하는 대표적인 optimizer다.

## 왜 필요한가

손실 함수는 현재 오차를 알려 주지만 파라미터를 직접 바꾸지는 않는다. optimizer가 역전파로 얻은 gradient를 이용해 실제 수정을 수행한다.

## 핵심 코드

```python
model.compile(
    loss="binary_crossentropy",
    optimizer="adam",
    metrics=["accuracy"]
)
```

## 꼭 기억할 점

- optimizer는 학습 단계에서만 가중치를 바꾼다.
- 학습률이 너무 크면 최적점을 지나칠 수 있고 너무 작으면 학습이 느리다.
- 수업 모델에서는 직접 optimizer를 구현하지 않고 문자열 `"adam"`으로 선택했다.
- PDF에는 경사 하강과 여러 optimizer가 소개되지만 노트북 실습은 `adam`을 사용했다.

## 관련 노트

- [[손실 함수는 왜 필요한가]]
- [[가중치와 편향은 언제 수정되는가]]
- [[batch_size는 무엇인가]]

## 출처

- `day0723.ipynb`
- `인공지능_프로그래밍_9일차_온디1기.pdf`, 106~110쪽

