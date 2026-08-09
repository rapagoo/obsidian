# CNN과 YOLO는 이미지에서 무엇을 찾는가?

## 한 줄 설명

CNN은 이미지의 공간적 패턴을 학습하는 신경망이고, YOLO는 한 이미지에서 객체의 class와 위치를 함께 예측하는 객체 탐지 모델이다.

## 왜 필요한가

이미지 전체가 개나 고양이인지 맞히는 분류와, 이미지 안에서 여러 객체가 어디 있는지 사각형으로 찾는 탐지는 다른 문제다.

## 핵심 흐름

```python
result = model(frame)
detect = result.xyxyn[0].numpy()
```

## 꼭 기억할 점

- 이미지 분류는 보통 이미지 전체에 하나의 label을 예측한다.
- 객체 탐지는 여러 객체의 bounding box, confidence, class를 예측한다.
- 수업의 YOLOv5 사전학습 모델은 COCO 데이터셋의 class를 사용했다.
- 갱신된 수업에서는 `ultralytics.YOLO("yolov8s.pt")`를 이용한 YOLOv8 추론과 custom 학습도 실행했다.
- `day0728.ipynb`의 “CNN이 두 바퀴, YOLO가 한 바퀴” 설명은 직관적 비교이며 실제 모델 구조 전체를 엄밀히 설명한 것은 아니다.

## 관련 노트

- [[OpenCV는 웹캠 프레임을 어떻게 읽는가]]
- [[YOLOv5의 탐지 결과에는 무엇이 들어 있는가]]
- [[전이 학습은 사전 학습 모델을 어떻게 재사용하는가]]
- [[OpenCV와 YOLO로 객체 탐지하기]]
- [[고양이와 강아지 데이터로 YOLO를 학습하기]]

## 출처

- `day0728.ipynb`
