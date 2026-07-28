# OpenCV와 YOLOv5로 객체 탐지하기

## 실습 목표

이미지와 웹캠 프레임을 OpenCV로 읽고, YOLOv5 모델이 찾은 객체의 위치·종류·신뢰도를 화면에 표시한다.

## 전체 흐름

1. `torch.hub.load()`로 YOLOv5 모델을 불러온다.
2. OpenCV로 이미지 또는 웹캠 프레임을 읽는다.
3. 모델에 프레임을 전달한다.
4. 탐지 결과의 좌표, 신뢰도, 클래스 번호를 꺼낸다.
5. 사각형과 라벨을 프레임에 그린다.
6. `q`를 누르면 반복을 끝내고 자원을 해제한다.

## 핵심 코드

```python
model = torch.hub.load(
    "ultralytics/yolov5",
    "yolov5s",
    pretrained=True
)

cap = cv2.VideoCapture(0)

while True:
    ret, frame = cap.read()
    if not ret:
        break

    results = model(frame)

    for *xyxy, conf, cls in results.xyxy[0]:
        x1, y1, x2, y2 = map(int, xyxy)
        label = f"{model.names[int(cls)]} {conf * 100:.2f}%"
        cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
        cv2.putText(
            frame,
            label,
            (x1, y1 - 10),
            cv2.FONT_HERSHEY_SIMPLEX,
            0.5,
            (0, 255, 0),
            2
        )

    cv2.imshow("YOLOv5", frame)
    if cv2.waitKey(1) & 0xFF == ord("q"):
        break

cap.release()
cv2.destroyAllWindows()
```

> [!warning]
> 정지 이미지 예제에는 `conf`가 0과 1 사이 값인데도 `f"{conf:.2f}%"`로 표시한 셀이 있었다. 백분율로 보이려면 실시간 예제처럼 `conf * 100`을 사용해야 한다.

## 이 실습에서 사용한 개념

- [[CNN과 YOLO는 이미지에서 무엇을 찾는가]]
- [[OpenCV는 웹캠 프레임을 어떻게 읽는가]]
- [[YOLOv5의 탐지 결과에는 무엇이 들어 있는가]]
- [[while과 for는 언제 사용하는가]]

## 출처

- `day0728.ipynb`

