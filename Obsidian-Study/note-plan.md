# 노트 생성 계획

## 분석한 자료

### Jupyter Notebook

- `day0715.ipynb` - 변수, 자료형, 산술·비교·논리 연산자, 삼항 연산자와 내장 함수 이름을 변수로 덮어쓸 때의 오류
- `day0716.ipynb` - 형 변환, 입출력, 조건문, 리스트·튜플, 인덱싱·슬라이싱, 반복문, `range`, 버블 정렬과 이진 탐색
- `day0720.ipynb` - 딕셔너리, 함수, 재귀, 가변 인자, 모듈, 클래스, 상속, 다형성, 캡슐화와 생성자
- `day0721.ipynb` - 객체지향 실습, 배열·스택·큐·연결 리스트, 파일 입출력, 예외 처리, CSV·JSON·XML, API, pandas 입문
- `day0722.ipynb` - pandas 선택·그룹화·변환·결측치, 복사, 시각화, 상관계수, NumPy, KNN과 결정 트리
- `day0723.ipynb` - SVM, 스케일링, 회귀 평가, K-means, PCA, 원-핫 인코딩, 로지스틱 회귀, 혼동 행렬, Keras 신경망
- `day0724_2.ipynb` - PySide6 위젯·레이아웃·이벤트, 계산기와 날씨 앱, API, Selenium 이미지 수집
- `day0727.ipynb` - Keras 모델의 K-fold 교차 검증과 `validation_split`
- `day0728.ipynb` - OpenCV 웹캠, YOLOv5·YOLOv8 추론과 custom 학습, MediaPipe 손 landmark, KNN 제스처 분류, 얼굴 벡터·코사인 유사도, PySide6 웹캠 GUI와 PyInstaller 배포

### PDF

- `인공지능_프로그래밍_1일차_온디1기.pdf` - Python 소개, 개발 환경, 변수, 명명 규칙과 연산자
- `인공지능_프로그래밍_2일차_온디1기.pdf` - 리스트·튜플, 인덱싱·슬라이싱, 반복문과 `range`
- `인공지능_프로그래밍_3일차_온디1기.pdf` - 딕셔너리, 함수, 가변 인자와 모듈
- `인공지능_프로그래밍_4일차_온디1기.pdf` - 객체지향, 캡슐화, 상속, 다형성과 클래스 구조
- `인공지능_프로그래밍_5일차_온디1기.pdf` - 파일 입출력, 예외 처리, OS 파일 관리, CSV와 JSON
- `인공지능_프로그래밍_6일차_온디1기.pdf` - Series, DataFrame, 선택, `groupby`, `apply`, 결측치
- `인공지능_프로그래밍_7일차_온디1기.pdf` - 머신러닝의 흐름과 지도·비지도·강화 학습
- `인공지능_프로그래밍_8일차_온디1기.pdf` - 로지스틱 회귀, sigmoid, 혼동 행렬과 분류 평가 지표
- `인공지능_프로그래밍_9일차_온디1기.pdf` - 회귀, 손실 함수, 경사 하강, 신경망, 역전파와 활성화 함수

PDF는 노트북 코드에 등장한 개념의 공식 용어와 이론적 맥락을 확인하는 용도로만 반영한다.

## 생성할 개념 노트

### 01 Python

| 노트 제목 | 근거 자료 | 연결할 관련 노트 |
| --- | --- | --- |
| 변수는 왜 사용하는가 | `day0715.ipynb`, 1일차 PDF | 조건문, 함수 |
| 조건문은 실행 흐름을 어떻게 나누는가 | `day0715.ipynb`, `day0716.ipynb` | 반복문, 예외 처리 |
| 리스트와 튜플은 어떻게 다른가 | `day0716.ipynb`, 2일차 PDF | 인덱싱과 슬라이싱, 스택과 큐 |
| 인덱싱과 슬라이싱은 어떻게 동작하는가 | `day0716.ipynb`, 2일차 PDF | 리스트와 튜플, DataFrame 선택 |
| while과 for는 언제 사용하는가 | `day0716.ipynb`, 2일차 PDF | 조건문, `range`, 알고리즘 |
| 딕셔너리는 key와 value를 어떻게 관리하는가 | `day0720.ipynb`, 3일차 PDF | API, JSON |
| 함수의 매개변수와 반환값은 무엇인가 | `day0720.ipynb`, 3일차 PDF | 재귀, 모듈 |
| 재귀 함수는 언제 자신을 다시 호출하는가 | `day0720.ipynb` | 함수, 스택 |
| 모듈과 import는 코드를 어떻게 나누는가 | `day0720.ipynb`, 3일차 PDF | 함수, API |
| 클래스와 인스턴스는 어떤 관계인가 | `day0720.ipynb`, `day0721.ipynb`, 4일차 PDF | 상속, 캡슐화 |
| 상속과 오버라이딩은 어떻게 코드를 재사용하는가 | `day0720.ipynb`, 4일차 PDF | 클래스, 다형성 |
| 캡슐화에서 이중 밑줄은 무엇을 의미하는가 | `day0720.ipynb`, `day0721.ipynb`, 4일차 PDF | 클래스, getter |
| with open은 왜 사용하는가 | `day0721.ipynb`, 5일차 PDF | 예외 처리, CSV |
| 예외 처리는 왜 필요한가 | `day0721.ipynb`, 5일차 PDF | 파일 입출력, API |
| API 요청과 응답은 어떻게 동작하는가 | `day0721.ipynb`, `day0724_2.ipynb` | 딕셔너리, JSON |
| 얕은 복사와 깊은 복사는 어떻게 다른가 | `day0722.ipynb` | DataFrame 복사 |
| QTimer는 화면을 어떻게 주기적으로 갱신하는가 | `day0728.ipynb` | OpenCV, PySide6 |
| PyInstaller는 Python 프로그램을 어떻게 배포하는가 | `day0728.ipynb` | 모듈, PySide6 |

### 04 Data Analysis

| 노트 제목 | 근거 자료 | 연결할 관련 노트 |
| --- | --- | --- |
| Series와 DataFrame은 어떻게 다른가 | `day0721.ipynb`, `day0722.ipynb`, 6일차 PDF | 행과 열 선택 |
| DataFrame의 행과 열은 어떻게 선택하는가 | `day0722.ipynb`, 6일차 PDF | `loc`, `iloc`, 불리언 인덱싱 |
| loc과 iloc은 어떻게 다른가 | `day0722.ipynb`, 6일차 PDF | 행과 열 선택 |
| 불리언 인덱싱은 조건에 맞는 행을 어떻게 고르는가 | `day0722.ipynb` | `groupby`, 결측치 |
| groupby는 그룹별 통계를 어떻게 계산하는가 | `day0722.ipynb`, 6일차 PDF | 불리언 인덱싱, 시각화 |
| apply와 lambda는 어떻게 함께 사용하는가 | `day0722.ipynb`, 6일차 PDF | 원-핫 인코딩, 결측치 |
| 결측치는 왜 처리해야 하는가 | `day0722.ipynb`, 6일차 PDF | 원-핫 인코딩, 모델 학습 |
| 상관계수와 heatmap은 무엇을 보여주는가 | `day0722.ipynb` | 산점도, 특징 |
| NumPy 배열은 Python 리스트와 어떻게 다른가 | `day0722.ipynb` | X와 y, PCA |

### 02 Machine Learning

| 노트 제목 | 근거 자료 | 연결할 관련 노트 |
| --- | --- | --- |
| 지도학습과 비지도학습은 어떻게 다른가 | `day0723.ipynb`, 7일차 PDF | KNN, K-means |
| X와 y는 무엇인가 | `day0722.ipynb`, `day0723.ipynb` | 데이터 분할, `fit` |
| 학습 데이터와 테스트 데이터는 왜 나누는가 | `day0722.ipynb`, `day0723.ipynb` | `random_state`, 과적합 |
| fit과 predict는 무엇을 하는가 | `day0722.ipynb`, `day0723.ipynb` | X와 y, 평가 지표 |
| KNN은 어떻게 분류하는가 | `day0722.ipynb` | 스케일링, hyperparameter |
| 결정 트리는 어떻게 판단하는가 | `day0722.ipynb` | Gini impurity, 과적합 |
| 과소적합과 과대적합은 어떻게 다른가 | `day0722.ipynb` | 데이터 분할, K-fold |
| SVM은 데이터를 어떻게 분류하는가 | `day0723.ipynb` | StandardScaler |
| StandardScaler는 왜 필요한가 | `day0723.ipynb` | `fit`과 `transform`, SVM |
| fit과 transform은 어떻게 다른가 | `day0723.ipynb` | StandardScaler, 테스트 데이터 |
| 테스트 데이터에는 왜 fit하지 않는가 | `day0723.ipynb` | 데이터 분할, 데이터 누수 |
| 선형 회귀는 무엇을 예측하는가 | `day0723.ipynb`, 9일차 PDF | 회귀 평가 지표 |
| MAE와 MSE와 RMSE와 R2는 어떻게 다른가 | `day0723.ipynb` | 선형 회귀 |
| K-means는 데이터를 어떻게 묶는가 | `day0723.ipynb` | 비지도학습, 엘보우 |
| 엘보우 방법은 군집 개수를 어떻게 정하는가 | `day0723.ipynb` | K-means |
| PCA는 여러 특징을 어떻게 줄이는가 | `day0723.ipynb` | NumPy, 시각화 |
| 원-핫 인코딩은 왜 필요한가 | `day0723.ipynb` | pandas, 로지스틱 회귀 |
| 로지스틱 회귀는 왜 분류 모델인가 | `day0723.ipynb`, 8일차 PDF | sigmoid, 혼동 행렬 |
| 혼동 행렬의 TP FN FP TN은 무엇인가 | `day0723.ipynb`, 8일차 PDF | 정확도·정밀도·재현율·F1 |
| 정확도 정밀도 재현율 F1은 어떻게 다른가 | `day0723.ipynb`, 8일차 PDF | 혼동 행렬 |
| K-fold 교차 검증은 왜 사용하는가 | `day0727.ipynb` | 데이터 분할, 과적합 |
| 손 랜드마크 좌표는 어떻게 제스처 특징이 되는가 | `day0728.ipynb` | X와 y, KNN |
| 코사인 유사도는 두 벡터를 어떻게 비교하는가 | `day0728.ipynb` | NumPy, 얼굴 벡터 |

### 03 Deep Learning

| 노트 제목 | 근거 자료 | 연결할 관련 노트 |
| --- | --- | --- |
| Dense 층은 무엇을 하는가 | `day0723.ipynb`, 9일차 PDF | 입력층·은닉층·출력층 |
| ReLU와 sigmoid와 softmax는 어떤 역할을 하는가 | `day0723.ipynb`, 9일차 PDF | Dense, 손실 함수 |
| 손실 함수는 왜 필요한가 | `day0723.ipynb`, 9일차 PDF | optimizer, crossentropy |
| binary_crossentropy와 sparse_categorical_crossentropy는 언제 사용하는가 | `day0723.ipynb` | sigmoid, softmax |
| optimizer와 adam은 무엇을 하는가 | `day0723.ipynb`, 9일차 PDF | 손실 함수, 가중치 |
| epoch는 무엇인가 | `day0723.ipynb`, `day0727.ipynb` | batch size, 과적합 |
| batch_size는 무엇인가 | `day0723.ipynb`, `day0727.ipynb` | epoch, 가중치 갱신 |
| 가중치와 편향은 언제 수정되는가 | `day0723.ipynb`, 9일차 PDF | optimizer, 역전파 |
| compile과 fit과 evaluate는 무엇을 하는가 | `day0723.ipynb` | 손실 함수, epoch |
| validation_split은 왜 사용하는가 | `day0727.ipynb` | K-fold, 테스트 데이터 |
| CNN과 YOLO는 이미지에서 무엇을 찾는가 | `day0728.ipynb` | OpenCV, YOLO 결과 |
| OpenCV는 웹캠 프레임을 어떻게 읽는가 | `day0728.ipynb` | YOLO 실시간 탐지 |
| YOLOv5의 탐지 결과에는 무엇이 들어 있는가 | `day0728.ipynb` | 좌표, confidence, class |
| 전이 학습은 사전 학습 모델을 어떻게 재사용하는가 | `day0728.ipynb` | YOLO, epoch, batch size |

### 05 Algorithm

| 노트 제목 | 근거 자료 | 연결할 관련 노트 |
| --- | --- | --- |
| 버블 정렬은 어떻게 동작하는가 | `day0716.ipynb` | 이진 탐색 |
| 이진 탐색은 왜 정렬된 데이터가 필요한가 | `day0716.ipynb` | 버블 정렬 |
| 정적 배열과 동적 배열은 어떻게 다른가 | `day0721.ipynb` | 연결 리스트 |
| 스택과 큐는 데이터 순서가 어떻게 다른가 | `day0721.ipynb` | 괄호 검사, `deque` |
| 연결 리스트는 노드를 어떻게 연결하는가 | `day0721.ipynb` | 정적 배열, 스택과 큐 |
| 스택은 괄호의 유효성을 어떻게 검사하는가 | `day0721.ipynb` | 스택과 큐 |

## 생성할 실습 노트

| 노트 제목 | 저장 폴더 | 근거 자료 |
| --- | --- | --- |
| 포켓몬 배틀을 클래스로 만들기 | `06 Projects` | `day0721.ipynb` |
| 타이타닉 데이터를 전처리하고 모델 비교하기 | `06 Projects` | `day0722.ipynb`, `day0723.ipynb` |
| 붓꽃 데이터로 KNN과 결정 트리와 SVM 비교하기 | `06 Projects` | `day0722.ipynb`, `day0723.ipynb` |
| 붓꽃 신경망을 K-fold로 검증하기 | `06 Projects` | `day0727.ipynb` |
| PySide6로 계산기와 날씨 앱 만들기 | `06 Projects` | `day0724_2.ipynb` |
| Selenium으로 이미지 수집하기 | `06 Projects` | `day0724_2.ipynb` |
| OpenCV와 YOLO로 객체 탐지하기 | `06 Projects` | `day0728.ipynb` |
| 고양이와 강아지 데이터로 YOLO를 학습하기 | `06 Projects` | `day0728.ipynb` |
| MediaPipe와 KNN으로 손 제스처 분류하기 | `06 Projects` | `day0728.ipynb` |
| 얼굴 벡터의 코사인 유사도 비교하기 | `06 Projects` | `day0728.ipynb` |
| PySide6로 실시간 웹캠 화면 만들기 | `06 Projects` | `day0728.ipynb` |

## 생성할 인덱스 노트

- `90 Index/Python 학습 지도.md`
- `90 Index/Data Analysis 학습 지도.md`
- `90 Index/Machine Learning 학습 지도.md`
- `90 Index/Deep Learning 학습 지도.md`
- `90 Index/Algorithm 학습 지도.md`
- `90 Index/전체 수업 자료 목차.md`
- `99 Sources/원본 자료 목록.md`

## 통합하거나 제외할 내용

- 연산자 종류, `range`, `break`, 삼항 연산자는 독립 노트로 과도하게 분리하지 않고 조건문·반복문 노트에서 설명한다.
- 함수의 네 가지 형태, 기본 인자, `*args`, 여러 반환값은 `함수의 매개변수와 반환값은 무엇인가`에 통합한다.
- `self`, `__init__`, `__str__`은 클래스와 인스턴스의 생성·표현 흐름 안에서 함께 설명한다.
- Gini impurity는 결정 트리 노트, `random_state`는 데이터 분할 노트, hyperparameter는 KNN·결정 트리 관련 노트에 포함한다.
- 그래프 종류별 노트를 따로 만들지 않고 관련 데이터 분석·실습 노트에 필요한 시각화만 둔다.
- PySide6의 각 위젯, CSS 속성, Selenium 선택자 하나마다 별도 노트를 만들지 않는다.
- PDF에서만 자세히 다룬 미분 공식, 지수·로그 함수, 활성화 함수의 고급 수학, 모멘텀·AdaGrad·RMSProp은 노트북 코드의 우선순위를 지키기 위해 별도 노트로 확장하지 않는다.
- `day0724_2.ipynb`의 `SystemExit: 0`은 GUI 이벤트 루프가 종료되며 Jupyter에 표시된 것으로, 일반적인 실행 실패와 구분한다.
- 수업 코드의 공개 API 키는 보안상 노트에 복사하지 않고 환경 변수 또는 별도 설정 사용을 경고한다.
- 실행되지 않은 오탈자(`peak`/`peek`), 이진 탐색 변형의 잘못된 종료 조건, 분류 지표 주석의 잘못된 수식·수치, 3-class 신경망의 잘못된 출력층 실험, `input_dim` 중복, YOLO confidence의 퍼센트 표기를 경고로 기록한다.
- `day0728.ipynb`의 중단된 YOLO 학습, MediaPipe API 변경, 손 제스처 특징 수 불일치, `face_recognition` 설치 오류와 GUI의 정상 `SystemExit: 0`을 실행 실패 여부에 맞게 구분해 기록한다.
- YOLOv5와 YOLOv8의 추론은 하나의 실습 노트로 통합하고, custom 학습은 목적과 코드 흐름이 달라 별도 실습으로 분리한다.
