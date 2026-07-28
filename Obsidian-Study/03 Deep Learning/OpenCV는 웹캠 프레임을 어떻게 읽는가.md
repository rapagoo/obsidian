# OpenCV는 웹캠 프레임을 어떻게 읽는가?

## 한 줄 설명

OpenCV의 `VideoCapture`는 카메라를 열고 `read()`를 반복 호출해 동영상의 각 순간을 이미지 frame으로 가져온다.

## 왜 필요한가

실시간 객체 탐지는 한 장의 이미지가 아니라 웹캠에서 계속 들어오는 frame마다 모델을 실행해야 한다.

## 핵심 코드

```python
import cv2

cap = cv2.VideoCapture(0)

while True:
    ret, frame = cap.read()
    if not ret:
        break

    cv2.imshow("TEST", frame)
    if cv2.waitKey(1) & 0xFF == ord("q"):
        break

cap.release()
cv2.destroyAllWindows()
```

## 꼭 기억할 점

- `0`은 보통 기본 웹캠 장치 번호다.
- `ret`이 `False`이면 frame을 읽지 못한 것이다.
- 종료할 때 카메라를 `release()`하고 창을 닫는다.
- `waitKey(1)`은 화면 갱신과 키 입력 확인에 사용된다.

## 관련 노트

- [[CNN과 YOLO는 이미지에서 무엇을 찾는가]]
- [[YOLOv5의 탐지 결과에는 무엇이 들어 있는가]]

## 출처

- `day0728.ipynb`

