# 혼동 행렬의 TP FN FP TN은 무엇인가?

## 한 줄 설명

혼동 행렬은 실제 class와 예측 class를 교차해 TP·FN·FP·TN 네 경우로 분류 결과를 센 표다.

## 왜 필요한가

암 환자가 매우 적은 데이터에서 모두 정상이라고 예측해도 정확도는 높을 수 있다. 어떤 종류의 오류가 발생했는지 따로 봐야 한다.

## 핵심 코드

```python
from sklearn.metrics import confusion_matrix

cm = confusion_matrix(y_test, y_pred, labels=[1, 0])

[[TP, FN],
 [FP, TN]] = cm
```

## 꼭 기억할 점

- Positive는 관심 class, True·False는 예측의 정답 여부다.
- TP: Positive로 예측했고 실제도 Positive
- FN: Negative로 예측했지만 실제는 Positive
- FP: Positive로 예측했지만 실제는 Negative
- TN: Negative로 예측했고 실제도 Negative
- `labels` 순서에 따라 행렬 배치가 달라질 수 있다.

## 관련 노트

- [[정확도 정밀도 재현율 F1은 어떻게 다른가]]
- [[로지스틱 회귀는 왜 분류 모델인가]]

## 출처

- `day0723.ipynb`
- `인공지능_프로그래밍_8일차_온디1기.pdf`, 12~26쪽

