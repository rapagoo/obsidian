# PCA는 여러 특징을 어떻게 줄이는가?

## 한 줄 설명

PCA는 데이터의 분산을 많이 보존하는 새로운 축인 주성분을 찾아 여러 특징을 더 적은 차원으로 바꾼다.

## 왜 필요한가

특징이 너무 많으면 계산과 시각화가 어렵다. 원본 정보를 가능한 한 유지하면서 2차원처럼 낮은 차원으로 줄일 수 있다.

## 핵심 코드

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)

print(X.shape)      # (1797, 64)
print(X_pca.shape)  # (1797, 2)
```

## 꼭 기억할 점

- 주성분은 원본 특징 일부를 고르는 것이 아니라 원본의 조합으로 만든 새 축이다.
- `explained_variance_ratio_`는 각 주성분이 설명하는 분산 비율이다.
- 수업에서는 digits의 64개 특징을 2개로 줄였고 설명 비율 합은 약 `0.29`였다.
- 차원 축소에는 정보 손실이 따른다.

## 관련 노트

- [[NumPy 배열은 Python 리스트와 어떻게 다른가]]
- [[지도학습과 비지도학습은 어떻게 다른가]]
- [[K-means는 데이터를 어떻게 묶는가]]

## 출처

- `day0723.ipynb`

