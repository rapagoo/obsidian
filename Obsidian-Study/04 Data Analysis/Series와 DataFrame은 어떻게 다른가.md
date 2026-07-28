# Series와 DataFrame은 어떻게 다른가?

## 한 줄 설명

`Series`는 인덱스와 값으로 이루어진 1차원 데이터이고, `DataFrame`은 여러 `Series`가 열로 모인 2차원 표다.

## 왜 필요한가

CSV나 엑셀 표처럼 행과 열이 있는 데이터를 열 단위로 계산하고 조건에 따라 선택하려면 pandas의 구조가 편리하다.

## 핵심 코드

```python
import pandas as pd

s1 = pd.Series([10, 20, 30, 40, 50])
df_train = pd.read_csv(
    "서울시 지하철호선별 역별 승하차 인원 정보.csv",
    encoding="EUC-KR"
)
```

## 꼭 기억할 점

- `df["역명"]`처럼 열 하나를 선택하면 보통 `Series`다.
- `df[["역명", "승차총승객수"]]`처럼 열을 리스트로 선택하면 `DataFrame`이다.
- `info()`는 열 이름·자료형·결측치 수를, `describe()`는 수치형 통계 요약을 보여 준다.
- 원본을 보존하려면 작업용 DataFrame을 `copy()`로 만든다.

## 관련 노트

- [[DataFrame의 행과 열은 어떻게 선택하는가]]
- [[얕은 복사와 깊은 복사는 어떻게 다른가]]

## 출처

- `day0721.ipynb`
- `day0722.ipynb`
- `인공지능_프로그래밍_6일차_온디1기.pdf`, 2~6쪽

