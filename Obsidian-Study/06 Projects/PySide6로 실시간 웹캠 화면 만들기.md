# PySide6로 실시간 웹캠 화면 만들기

## 실습 목표

OpenCV 웹캠 frame을 PySide6의 `QLabel`에 표시하고 시작·일시정지 버튼과 종료 시 자원 해제를 구현한다.

## 전체 흐름

1. `QTimer`의 `timeout`을 `update_frame()`에 연결한다.
2. 시작 버튼에서 웹캠을 열고 타이머를 시작한다.
3. OpenCV의 BGR frame을 RGB로 바꾼다.
4. NumPy 이미지 데이터를 `QImage`, `QPixmap`으로 변환한다.
5. 일시정지 버튼에서는 타이머를 멈춘다.
6. `closeEvent()`에서 타이머와 웹캠 자원을 정리한다.

## 핵심 코드

```python
self.timer = QTimer(self)
self.timer.timeout.connect(self.update_frame)

rgb_image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
h, w, ch = rgb_image.shape
bytes_per_line = ch * w

qt_image = QImage(
    rgb_image.data,
    w,
    h,
    bytes_per_line,
    QImage.Format.Format_RGB888
)

self.image_label.setPixmap(QPixmap.fromImage(qt_image))
```

```python
def closeEvent(self, event):
    self.timer.stop()
    if self.cap and self.cap.isOpened():
        self.cap.release()
    event.accept()
```

> [!note]
> 노트북의 `SystemExit: 0`은 창을 닫은 뒤 `sys.exit(app.exec())`가 정상 종료 상태를 Jupyter에 전달한 결과다.

## 이 실습에서 사용한 개념

- [[QTimer는 화면을 어떻게 주기적으로 갱신하는가]]
- [[OpenCV는 웹캠 프레임을 어떻게 읽는가]]
- [[클래스와 인스턴스는 어떤 관계인가]]
- [[PyInstaller는 Python 프로그램을 어떻게 배포하는가]]

## 출처

- `day0728.ipynb`

