# MediaPipe와 KNN으로 손 제스처 분류하기

## 실습 목표

웹캠에서 손 landmark를 추출하고, 키보드로 label을 붙여 CSV에 저장한 뒤 KNN으로 손 제스처를 분류한다.

## 전체 흐름

1. MediaPipe `HandLandmarker`로 손의 21개 점을 찾는다.
2. 손목 기준 상대 좌표를 구하고 손 크기로 정규화한다.
3. 숫자 키와 함께 42개 특징을 CSV에 저장한다.
4. CSV의 특징을 `X`, label을 `y`로 나눠 KNN을 학습한다.
5. 새 웹캠 frame에도 같은 특징 변환을 적용해 제스처를 예측한다.

## 핵심 코드

```python
df = pd.read_csv("my_custom_gesture.csv")
X = df.iloc[:, 1:].values
y = df.iloc[:, 0].values

knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X, y)
```

```python
y_test = temp_array.flatten().reshape(1, -1)
y_pred = knn.predict(y_test)
```

> [!warning]
> `mp.solutions`를 사용한 셀은 설치된 MediaPipe `1.0.0`에서 `AttributeError`가 발생했다. 이후 셀은 `mediapipe.tasks.python.vision`의 `HandLandmarker`를 사용해 동작 방식을 바꿨다.

> [!warning]
> 한 셀은 42개 좌표 특징으로 만든 CSV를 불러온 뒤 15개 각도 특징을 예측에 전달해 OpenCV KNN의 열 개수 assertion 오류가 발생했다. 학습과 예측에는 반드시 같은 특징 표현을 사용해야 한다.

> [!warning]
> scikit-learn을 사용한 마지막 예제는 손이 감지된 경우에만 `y_pred`를 만든 뒤 반복문 밖에서 표시한다. 첫 frame에서 손을 찾지 못하면 `y_pred`가 아직 없을 수 있으므로 결과 표시도 손 감지 조건 안에서 처리하는 편이 안전하다.

## 이 실습에서 사용한 개념

- [[손 랜드마크 좌표는 어떻게 제스처 특징이 되는가]]
- [[KNN은 어떻게 분류하는가]]
- [[X와 y는 무엇인가]]
- [[OpenCV는 웹캠 프레임을 어떻게 읽는가]]

## 출처

- `day0728.ipynb`

