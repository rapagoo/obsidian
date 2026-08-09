# QTimer는 화면을 어떻게 주기적으로 갱신하는가?

## 한 줄 설명

PySide6의 `QTimer`는 정해진 간격마다 `timeout` 신호를 보내 연결된 함수를 실행한다.

## 왜 필요한가

GUI의 이벤트 처리를 멈추는 무한 반복문 대신 타이머가 주기적으로 웹캠 frame을 읽게 하면, 화면을 갱신하면서 버튼 클릭과 창 닫기 같은 이벤트도 함께 처리할 수 있다.

## 핵심 코드

```python
self.timer = QTimer(self)
self.timer.timeout.connect(self.update_frame)

def start_webcam(self):
    if not self.timer.isActive():
        self.timer.start(30)

def pause_webcam(self):
    if self.timer.isActive():
        self.timer.stop()
```

## 꼭 기억할 점

- 수업 코드의 `30`은 약 30ms 간격으로 `update_frame()`을 호출한다.
- `timeout.connect()`에는 호출 결과가 아니라 실행할 함수 자체를 전달한다.
- 일시정지는 타이머를 멈추고, 프로그램 종료 시에는 웹캠도 `release()`해야 한다.
- 실제 화면 갱신 속도는 카메라와 처리 시간의 영향을 받으므로 항상 정확히 같은 FPS가 보장되지는 않는다.

## 관련 노트

- [[클래스와 인스턴스는 어떤 관계인가]]
- [[OpenCV는 웹캠 프레임을 어떻게 읽는가]]
- [[PySide6로 실시간 웹캠 화면 만들기]]

## 출처

- `day0728.ipynb`

