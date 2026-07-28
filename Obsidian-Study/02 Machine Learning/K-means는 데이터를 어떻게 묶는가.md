# K-means는 데이터를 어떻게 묶는가?

## 한 줄 설명

K-means는 `K`개의 중심점을 두고 각 데이터를 가장 가까운 중심에 할당한 뒤, 군집 평균으로 중심을 반복 이동한다.

## 왜 필요한가

정답 label이 없는 데이터에서도 서로 비슷한 샘플을 묶어 구조를 탐색할 수 있다.

## 핵심 코드

```python
from sklearn.cluster import KMeans

kmeans = KMeans(
    n_clusters=4,
    n_init="auto",
    random_state=20
)
kmeans.fit(X)

centers = kmeans.cluster_centers_
labels = kmeans.labels_
```

## 꼭 기억할 점

- `n_clusters`는 미리 정해야 하는 군집 수다.
- 군집 번호는 정답 class의 의미와 직접 대응하지 않는 임의 번호다.
- 거리 기반이므로 특징 범위의 영향을 받을 수 있다.
- 수업의 `centors` 변수명은 오탈자지만 해당 셀 내부에서 일관되게 사용됐다.

## 관련 노트

- [[지도학습과 비지도학습은 어떻게 다른가]]
- [[엘보우 방법은 군집 개수를 어떻게 정하는가]]
- [[PCA는 여러 특징을 어떻게 줄이는가]]

## 출처

- `day0723.ipynb`
