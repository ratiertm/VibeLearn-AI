# WorkLog: M2 - 딥러닝 기초 (PyTorch + Flow Matching 개념)

**날짜**: 2026-03-09
**모듈**: M2 - 딥러닝 기초
**사용 시간**: 약 3시간

---

## 오늘의 학습 목표

- [x] PyTorch 텐서 생성/연산/GPU 사용
- [x] 자동 미분 (autograd) 이해 및 실습
- [x] CNN으로 MNIST 숫자 분류 (95%+ 목표)
- [x] 센서 이미지에서 특징 벡터 추출 + 클러스터링 확인

---

## 진행 내용

### 1. 실습 1: PyTorch 텐서 연산 (60분)
- [x] 텐서 생성 4가지 방법
- [x] M1 센서 데이터 → 텐서 변환 (카메라 HWC→CHW 핵심)
- [x] 행렬 연산 + 신경망 수동 구현 (거리센서→행동)
- [x] MPS GPU 속도 비교
- [x] autograd: x² 기울기 + 거리→위험도 선형 학습
- [x] nn.Module로 DistanceToAction 신경망

### 2. 실습 2: MNIST CNN 분류기 (60분)
- [x] MNIST 데이터셋 로드 (60,000장)
- [x] CNN 모델: Conv(32)→Pool→Conv(64)→Pool→FC(128)→FC(10)
- [x] 5 에포크 MPS 학습 → **99.0% 정확도** (목표 95% 초과)
- [x] 예측 시각화 + Conv1 특징 맵 시각화

### 3. 실습 3: 센서 이미지 특징 추출기 (60분)
- [x] M1 CameraSensor로 3클래스 이미지 600장 생성
- [x] CameraEncoder: (3,64,64) → 16차원 특징 벡터
- [x] 30 에포크 학습 → **100% 정확도**
- [x] t-SNE 2D 시각화 (클러스터 분리 확인)
- [x] 코사인 유사도: 같은 클래스 0.999 vs 다른 클래스 -0.451
- [x] scikit-learn 추가 설치

### 4. README.md 작성 (15분)

---

## 문제 해결 로그

- MPS GPU 학습 정상 동작 (MNIST 5에포크, 특징추출 30에포크)
- scikit-learn 추가 설치 필요 (t-SNE용)
- 카메라 이미지 텐서 변환 시 HWC→CHW 순서 주의

---

## DoD 체크리스트 (M2 전체)

- [x] PyTorch 텐서 연산 및 autograd 실행
- [x] MNIST CNN 분류기 학습 완료 (99.0% > 95% 목표)
- [x] 센서 이미지 특징 추출기 구현
- [ ] 각 개념에 대한 직관적 설명 문서 작성 (concepts/)
- [x] Notebook 3개 완성
- [x] README.md 작성
- [x] WorkLog 작성 완료

**진행률**: M2 DoD ~85% (concepts 문서 제외 완료)

---

## Daily Retrospective

### What went well?
- MPS GPU로 MNIST 학습이 빠르게 완료됨
- CameraEncoder가 3클래스를 100% 분류 + t-SNE에서 깔끔하게 클러스터 형성
- M1 센서 데이터 → 텐서 변환 → CNN 입력까지 자연스럽게 연결됨

### What could be improved?
- concepts/ 문서 (neural_network_intuition.md, cnn_for_sensors.md) 미작성
- Flow Matching 개념 설명 아직 미진행 (M8에서 본격적으로)

### Insights
- PyTorch 이미지는 (C,H,W) 순서 — NumPy (H,W,C)와 반대
- CNN의 Conv→Pool 구조가 곧 Encoder의 특징 추출 부분
- 16차원 특징 벡터가 이미지의 "의미"를 압축 → 이것이 Fusion 입력이 됨
- autograd가 있어서 복잡한 미분을 직접 할 필요 없음 → 모델 설계에 집중 가능

### Next module
- **M3**: Gymnasium 시뮬레이션 환경 + LLM 연동

---

## 산출물

```
02-DeepLearningBasics/
├── README.md                                    ✅ 모듈 문서
├── examples/
│   ├── 01_tensor_operations.ipynb               ✅ 텐서 연산 + autograd
│   ├── 02_mnist_classifier.ipynb                ✅ MNIST CNN (99.0%)
│   └── 03_image_feature_extractor.ipynb         ✅ 센서 특징 추출 (100%)
└── (data/)                                      MNIST 데이터셋
```

**학습 결과**:
- MNIST CNN: 99.0% 정확도 (5 에포크, MPS)
- 센서 특징 추출: 100% 정확도 (30 에포크, MPS)
- 클러스터링: 같은 클래스 유사도 0.999, 다른 클래스 -0.451
