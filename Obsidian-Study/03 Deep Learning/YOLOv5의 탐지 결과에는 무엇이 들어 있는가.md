# YOLOv5의 탐지 결과에는 무엇이 들어 있는가?

## 한 줄 설명

수업에서 사용한 `result.xyxyn[0]`의 각 행에는 정규화된 bounding box 좌표 4개, confidence, class 번호가 들어 있다.

## 왜 필요한가

모델의 숫자 출력만으로는 객체가 화면 어디에 있고 무엇인지 바로 알기 어렵다. 좌표를 pixel 값으로 바꾸고 confidence와 class를 해석해야 탐지 상자와 이름을 영상에 표시할 수 있다.

## 핵심 코드

```python
detect = result.xyxyn[0].cpu().numpy()

x1 = int(detect[i][0] * frame.shape[1])
y1 = int(detect[i][1] * frame.shape[0])
x2 = int(detect[i][2] * frame.shape[1])
y2 = int(detect[i][3] * frame.shape[0])

conf = detect[i][4]
name = data["names"][int(detect[i][5])]
```

## 꼭 기억할 점

- `xyxyn`의 `n`은 좌표가 이미지 크기 대비 `0~1`로 정규화됐음을 뜻한다.
- 실제 pixel 좌표는 x에 이미지 너비, y에 이미지 높이를 곱한다.
- GPU tensor일 수 있으므로 갱신된 코드처럼 `.cpu().numpy()`로 CPU에 옮긴 뒤 NumPy 배열로 변환할 수 있다.
- confidence는 `0~1` 값이므로 퍼센트 표시는 `conf * 100`을 사용한다.
- class 번호 `16`은 수업의 COCO YAML에서 `"dog"`로 확인했다.

> [!warning]
> 이미지 한 장 실습 셀의 `f"{conf:.2f}%"`는 `0.84%`처럼 잘못 보일 수 있다. 실시간 셀의 `f"{conf*100:.2f}%"`가 올바른 퍼센트 표기다.

## 관련 노트

- [[OpenCV는 웹캠 프레임을 어떻게 읽는가]]
- [[CNN과 YOLO는 이미지에서 무엇을 찾는가]]
- [[OpenCV와 YOLO로 객체 탐지하기]]
- [[고양이와 강아지 데이터로 YOLO를 학습하기]]

## 출처

- `day0728.ipynb`
