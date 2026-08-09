# 고양이와 강아지 데이터로 YOLO를 학습하기

## 실습 목표

Roboflow에서 받은 고양이·강아지 객체 탐지 데이터의 YAML 경로를 수정하고, 사전 학습된 YOLO 가중치로 custom 모델 학습을 시작한다.

## 전체 흐름

1. YOLO 저장소와 필요한 라이브러리를 준비한다.
2. 압축된 pet dataset을 푼다.
3. `data.yaml`의 dataset 경로와 train·validation 폴더를 수정한다.
4. `yolov5s.pt` 또는 `yolov8s.pt`를 출발점으로 학습한다.
5. 학습이 완료되면 저장된 가중치를 custom 추론에 사용한다.

## 핵심 코드

```python
with open("pet/data.yaml", "r") as file:
    data = yaml.safe_load(file)
    data["path"] = os.path.abspath("pet")
    data["train"] = "train/images"
    data["val"] = "valid/images"

with open("pet/data.yaml", "w") as file:
    yaml.dump(data, file)
```

```python
from yolov5.train import run

run(
    data="pet/data.yaml",
    epochs=5,
    batch_size=64,
    weights="yolov5s.pt",
    device="0"
)
```

YOLOv8에서는 `ultralytics.YOLO`의 `train()`을 사용했다.

```python
model = YOLO("yolov8s.pt")
model.train(
    data="pet/data.yaml",
    epochs=5,
    batch=64,
    device="0"
)
```

> [!warning]
> 저장된 실행 결과에서 YOLOv5 학습은 `KeyboardInterrupt`로 중단됐고, YOLOv8도 첫 epoch가 진행 중인 출력까지만 확인된다. 따라서 이 노트북 실행으로 5 epochs가 끝났거나 새 `best.pt`가 생성됐다고 단정할 수 없다.

> [!warning]
> 노트북의 dataset 다운로드 명령에는 접근 key가 포함되어 있어 이 노트에 복사하지 않았다. 공유용 코드에서는 key를 별도 설정으로 관리해야 한다.

## 이 실습에서 사용한 개념

- [[전이 학습은 사전 학습 모델을 어떻게 재사용하는가]]
- [[CNN과 YOLO는 이미지에서 무엇을 찾는가]]
- [[epoch는 무엇인가]]
- [[batch_size는 무엇인가]]
- [[OpenCV와 YOLO로 객체 탐지하기]]

## 출처

- `day0728.ipynb`

