# API 요청과 응답은 어떻게 동작하는가?

## 한 줄 설명

API는 정해진 주소와 형식으로 요청을 보내면 서버가 상태 코드와 데이터를 응답하는 약속이다.

## 왜 필요한가

날씨나 박스오피스처럼 다른 서비스가 가진 데이터를 직접 저장하지 않고도 프로그램에서 사용할 수 있다.

## 핵심 코드

```python
import requests

url = "https://api.example.com/weather?q=Seoul"
response = requests.get(url)

if response.status_code == 200:
    data = response.json()
```

## 꼭 기억할 점

- `2xx`는 보통 성공, `4xx`는 요청 문제, `5xx`는 서버 문제를 뜻한다.
- `response.text`, `response.content`, `response.json()`은 각각 문자열, 바이트, JSON 변환 결과다.
- 응답이 항상 예상 구조라는 보장은 없다. `data["weather"]` 접근 전 상태 코드와 오류 응답을 확인해야 한다.
- API 키를 노트나 공개 저장소에 직접 적지 말고 환경 변수나 별도 설정 파일로 관리한다.

> [!warning]
> 수업 노트북에는 실제 API 키가 문자열로 노출되어 있다. 재사용 중인 키라면 폐기·재발급하는 것이 안전하다.

## 관련 노트

- [[딕셔너리는 key와 value를 어떻게 관리하는가]]
- [[예외 처리는 왜 필요한가]]
- [[PySide6로 계산기와 날씨 앱 만들기]]

## 출처

- `day0721.ipynb`
- `day0724_2.ipynb`

