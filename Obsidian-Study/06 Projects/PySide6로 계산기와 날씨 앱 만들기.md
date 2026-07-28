# PySide6로 계산기와 날씨 앱 만들기

## 실습 목표

PySide6의 위젯과 이벤트를 이용해 계산기와 날씨 조회 화면을 만들고, 버튼 클릭이 Python 함수 실행으로 이어지는 흐름을 익힌다.

## 전체 흐름

1. `QApplication`과 화면 위젯을 만든다.
2. 버튼의 `clicked` 신호를 함수에 연결한다.
3. 입력값을 읽어 계산하거나 API를 호출한다.
4. 결과를 `QLabel`이나 `QLineEdit`에 표시한다.
5. 이벤트 루프를 실행한다.

## 핵심 코드

```python
app = QApplication(sys.argv)

window = QWidget()
layout = QVBoxLayout()

input_box = QLineEdit()
result_label = QLabel("결과")
button = QPushButton("계산")

button.clicked.connect(calculate)

layout.addWidget(input_box)
layout.addWidget(button)
layout.addWidget(result_label)
window.setLayout(layout)
window.show()

sys.exit(app.exec())
```

날씨 앱에서는 같은 버튼 이벤트 안에서 `requests.get()`으로 응답을 받고, 필요한 JSON 값을 화면에 표시했다.

```python
response = requests.get(url)
data = response.json()
weather = data["weather"][0]["description"]
```

> [!warning]
> 수업 노트북의 날씨 API 키는 코드에 직접 포함되어 있었다. 노트에는 키를 복사하지 않았으며, 외부에 노출된 키라면 폐기하고 새 키를 발급하는 것이 안전하다.

> [!warning]
> 계산기에서 문자열을 곧바로 `eval()`에 전달하면 임의의 Python 표현식이 실행될 수 있다. 학습용 예제 밖에서는 허용할 연산을 제한해야 한다.

> [!note]
> 노트북에 기록된 `SystemExit: 0`은 창을 닫으면서 `sys.exit()`가 정상 종료를 전달한 결과다. 반면 날씨 데이터에서 발생한 `KeyError`는 예상한 JSON 구조가 오지 않았다는 뜻이므로 응답 상태와 오류 메시지를 먼저 확인해야 한다.

## 이 실습에서 사용한 개념

- [[클래스와 인스턴스는 어떤 관계인가]]
- [[예외 처리는 왜 필요한가]]
- [[API 요청과 응답은 어떻게 동작하는가]]

## 출처

- `day0724_2.ipynb`

