# Obsidian 노트 생성 결과

## 분석한 원본 파일 수

- ipynb: 9개
- pdf: 9개, 총 451쪽
- ppt: 0개
- pptx: 0개

원본 파일은 수정·삭제·복사하지 않았다. PDF는 노트북의 개념과 공식 용어를 보완하는 용도로만 사용했다.

## 생성한 노트 수

- 개념 노트: 70개
  - Python: 18개
  - Data Analysis: 9개
  - Machine Learning: 23개
  - Deep Learning: 14개
  - Algorithm: 6개
- 실습 노트: 11개
- 인덱스 노트: 6개
- 자료 목록: 1개
- 작업 관리 문서: 2개 (`note-plan.md`, `generation-report.md`)

## 주요 주제

- Python: 변수, 제어문, 컬렉션, 함수, 모듈, 객체지향, 파일, 예외, API, 복사, QTimer, PyInstaller
- 데이터 분석: pandas, NumPy, 선택, 그룹화, 변환, 결측치, 상관관계
- 머신러닝: 데이터 분할, KNN, 결정 트리, SVM, 회귀, 군집, PCA, 평가, 교차 검증, 손 landmark 특징, 코사인 유사도
- 딥러닝: Dense, 활성화 함수, 손실 함수, optimizer, epoch, batch, 검증, 전이 학습, YOLOv5·YOLOv8
- 알고리즘: 배열, 연결 리스트, 스택, 큐, 버블 정렬, 이진 탐색
- 기타 실습: PySide6, 날씨 API, Selenium, OpenCV, MediaPipe, 얼굴 벡터, custom 객체 탐지 학습

## 이번 갱신

- 2026-07-29에 갱신된 `day0728.ipynb`의 66개 code cell과 저장된 실행 결과를 다시 분석했다.
- 기존 YOLOv5 추론 실습은 YOLOv8 추론까지 포함하도록 통합했다.
- 새 개념 노트 5개와 실습 노트 4개를 추가했다.
- 기존 KNN·OpenCV·YOLO 노트, 분야별 학습 지도, 전체 목차, 자료 목록과 생성 계획을 증분 갱신했다.

## 확인이 필요한 내용

- PDF 9개는 검색 가능한 텍스트가 거의 없는 이미지 중심 자료였다. 총 451쪽을 이미지로 렌더링해 수업 용어와 흐름을 확인했으며, 노트북에 없는 주제를 PDF만으로 확장하지 않았다.
- `day0715.ipynb`에는 `print = 10`으로 내장 함수 이름을 덮어쓴 뒤 `TypeError`가 발생한 실행 결과가 있다.
- `day0716.ipynb`의 두 번째 이진 탐색 구현은 종료 조건 때문에 실제로 있는 `7`을 찾지 못한다. 정상 구현을 본문에 사용하고 오류는 경고로 남겼다.
- `day0721.ipynb`에는 리스트와 정수를 더해 발생한 `TypeError`와, `peek()`을 `peak()`으로 쓴 셀이 있다.
- `day0723.ipynb`에는 정밀도 설명의 수식·수치가 코드 및 PDF와 맞지 않는 부분이 있다. 노트에는 `TP / (TP + FP)`를 사용하고 불일치를 경고했다.
- `day0723.ipynb`에는 3개 클래스를 예측하면서 `Dense(1, activation="softmax")`를 사용한 실험 셀이 있다. 앞선 정상 코드인 `Dense(3, activation="softmax")`와 `sparse_categorical_crossentropy`를 기준으로 설명했다.
- `day0724_2.ipynb`의 여러 `SystemExit: 0`은 GUI 창을 닫을 때 기록된 정상 종료다. 날씨 앱의 `KeyError`는 예상과 다른 API 응답 구조로 보이며, 외부 API를 다시 실행해 확인하지는 않았다.
- `day0724_2.ipynb`에 직접 적힌 날씨 API 키는 노트에 복사하지 않았다. 실제로 사용 중인 키라면 폐기 후 재발급이 필요하다.
- `day0728.ipynb`의 정지 이미지 예제는 0~1 범위의 신뢰도에 `%` 기호만 붙였다. 실시간 예제처럼 `conf * 100`을 적용해야 백분율이 된다.
- 갱신된 `day0728.ipynb`의 YOLOv5 custom 학습은 첫 epoch 검증 중 `KeyboardInterrupt`로 중단됐으며 YOLOv8 학습도 첫 epoch 진행 중인 출력까지만 남아 있다. 새 학습이 완료되거나 `best.pt`가 생성됐다고 확정하지 않았다.
- YOLO custom dataset 다운로드 명령에 포함된 접근 key는 노트에 복사하지 않았다.
- `day0728.ipynb`의 한 YOLOv5 셀에는 독립된 `b` 한 줄이 남아 있다. 저장된 출력은 이전 코드 상태에서 실행됐을 가능성이 있어 현재 셀을 그대로 재실행하면 오류가 날 수 있다.
- MediaPipe `1.0.0` 환경에서 `mp.solutions`를 사용한 셀은 `AttributeError`가 발생했다. 이후 `mediapipe.tasks.python.vision.HandLandmarker`를 사용한 코드가 저장되어 있다.
- custom 손 제스처 실습의 한 셀은 학습 데이터의 42개 좌표 특징과 예측 데이터의 15개 각도 특징을 섞어 OpenCV KNN assertion 오류가 발생했다. 같은 특징 표현을 사용한 scikit-learn 예제를 기준으로 정리했다.
- `face_recognition` import 셀에는 `ModuleNotFoundError`가 기록되어 있어 현재 가상환경의 설치 상태를 별도로 확인해야 한다.
- PySide6 웹캠 GUI의 `SystemExit: 0`은 창을 닫은 뒤 이벤트 루프가 정상 종료된 기록으로 구분했다.
- GUI, 웹 자동화, 날씨 API, 웹캠, YOLO, MediaPipe와 얼굴 인식 코드는 외부 서비스·카메라·모델·가상환경이 필요하므로 재실행하지 않고 저장된 코드와 실행 결과를 분석했다.

## 링크 검사 결과

- 정상 링크 수: 344개
- 끊어진 링크 수: 0개
- 중복 가능성이 있는 노트: 0개
- 주요 개념·실습 중 고립 노트: 0개
- 파일명과 첫 번째 제목의 불일치: 0개

검사는 코드 블록과 인라인 코드 안의 대괄호를 제외하고 Obsidian 내부 링크만 대상으로 수행했다. Windows 파일명에서 사용할 수 없는 `?`는 파일명에서 생략하고, 개념 노트의 첫 번째 제목에는 질문 부호를 유지했다.
