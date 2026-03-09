# M2 - 딥러닝 기초 (PyTorch + Flow Matching 개념)

**모듈**: M2 (WorldModel-PhysicalAI 학습 로드맵)
**난이도**: ⭐
**소요 시간**: ~5시간

---

## 이 모듈에서 배우는 것

1. PyTorch 텐서 연산 + MPS GPU 가속 + 자동 미분 (autograd)
2. CNN으로 이미지 분류 (MNIST 99% 정확도)
3. CNN Encoder로 센서 이미지 → 16차원 특징 벡터 추출 + t-SNE 시각화

```
M1: 센서 데이터 (NumPy)
    ↓
M2: 텐서 변환 + CNN Encoder 기초  ← 여기!
    ↓
M5: 5개 센서별 Encoder (VLM + LLM + 3종)
```

---

## 노트북 구성

### 01_tensor_operations.ipynb
- 텐서 생성 4가지 방법 (직접, NumPy변환, 랜덤, 특수)
- M1 센서 데이터 → 텐서 변환 (카메라: HWC→CHW)
- 행렬 연산 + reshape (신경망 핵심)
- 신경망 수동 구현: 거리센서(8) → 은닉(16) → 행동(4)
- MPS GPU 속도 비교
- 자동 미분: x² 기울기 + 거리→위험도 선형 학습
- nn.Module로 신경망 만들기

### 02_mnist_classifier.ipynb
- MNIST 60,000장 로드 + CNN 모델 정의
- Conv(32) → Pool → Conv(64) → Pool → FC(128) → FC(10)
- 5 에포크 학습: **99.0% 정확도**
- 예측 시각화 + Conv1 특징 맵 시각화

### 03_image_feature_extractor.ipynb
- M1 CameraSensor로 3클래스 이미지 600장 생성 (가까움/중간/멀리)
- CameraEncoder: (3,64,64) → Conv³ → FC → (16,)
- 30 에포크 학습: **100% 정확도**
- t-SNE로 특징 벡터 2D 시각화 (클러스터 형성 확인)
- 코사인 유사도: 같은 클래스 0.999, 다른 클래스 -0.451

---

## 핵심 개념

| 개념 | 설명 | 우리 파이프라인에서 |
|------|------|-------------------|
| 텐서 | NumPy + GPU + 자동미분 | 모든 데이터의 기본 단위 |
| CNN | 이미지에서 특징 추출 | 카메라 Encoder (M5) |
| autograd | 자동 기울기 계산 | 모든 학습의 핵심 |
| Encoder | 고차원 → 저차원 벡터 | 5개 센서 Encoder (M5) |

---

## 산출물

```
02-DeepLearningBasics/
├── README.md
├── examples/
│   ├── 01_tensor_operations.ipynb           텐서 연산 + autograd
│   ├── 02_mnist_classifier.ipynb            MNIST CNN 분류기
│   └── 03_image_feature_extractor.ipynb     센서 이미지 특징 추출기
└── (data/)                                  MNIST 데이터셋 (자동 다운로드)
```
