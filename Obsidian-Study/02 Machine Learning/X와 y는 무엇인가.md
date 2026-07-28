# X와 y는 무엇인가?

## 한 줄 설명

`X`는 모델이 규칙을 찾는 데 사용하는 입력 특징(feature)이고, `y`는 예측하려는 정답 또는 label이다.

## 왜 필요한가

붓꽃 품종을 맞히려면 꽃받침·꽃잎의 길이와 너비가 `X`, 품종 번호가 `y`가 된다. 입력과 목표를 분리해야 학습과 평가 흐름이 명확해진다.

## 핵심 코드

```python
from sklearn.datasets import load_iris

iris_dataset = load_iris()
X = iris_dataset.data
y = iris_dataset.target
```

## 꼭 기억할 점

- 여러 특징이 열로 모인 입력은 관례적으로 대문자 `X`를 쓴다.
- 한 개의 정답 열은 관례적으로 소문자 `y`를 쓴다.
- `X.shape`은 `(샘플 수, 특징 수)`, `y.shape`은 보통 `(샘플 수,)`다.
- `X`의 행과 같은 위치에 있는 `y`가 그 샘플의 정답이다.

## 관련 노트

- [[학습 데이터와 테스트 데이터는 왜 나누는가]]
- [[fit과 predict는 무엇을 하는가]]
- [[DataFrame의 행과 열은 어떻게 선택하는가]]

## 출처

- `day0722.ipynb`
- `day0723.ipynb`

