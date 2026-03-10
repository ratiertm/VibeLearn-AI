# V-JEPA (Video Joint Embedding Predictive Architecture) 심층 분석

## 논문 정보
- **제목**: Revisiting Feature Prediction for Learning Visual Representations from Video
- **저자**: Adrien Bardes, Quentin Garrido, Jean Ponce, Xinlei Chen, Michael Rabbat, Yann LeCun, Mido Assran, Nicolas Ballas
- **소속**: Meta FAIR
- **출판**: 2024
- **링크**: https://arxiv.org/abs/2404.08471

## 1. 핵심 아이디어

### 한 문장 요약
> "비디오의 일부를 가리고, 보이는 부분으로 가려진 부분의 **추상적 표현**을 예측하여 시간적 패턴을 학습한다."

### 직관적 비유
영화를 보다가 장면 몇 개를 가렸을 때:
- **기존 방식**: 가려진 장면의 **모든 픽셀**을 정확히 복원 (배경, 색상, 텍스처까지)
- **V-JEPA**: 가려진 장면에서 **무슨 일이 일어나는지** 추상적으로 이해 ("주인공이 문을 열었다")

## 2. 아키텍처 상세

```
비디오 입력: [Frame1, Frame2, Frame3, Frame4, Frame5]
                ↓
         시공간 마스킹 (Spatio-Temporal Masking)
         [Frame1, ████, Frame3, ████, Frame5]
                ↓                    ↓
     Context Encoder              Target Encoder
     (보이는 프레임)              (가려진 프레임)
         ↓                           ↓
     context features            target features (sy)
         ↓
     Predictor (Transformer)
         ↓
     predicted features (ŝy)
         ↓
     Loss = L2(ŝy, sy)   ← 잠재공간에서의 거리
```

### 각 컴포넌트 설명

#### (1) Vision Transformer (ViT) Encoder
- 비디오 프레임을 **패치(patch)**로 분할
- 각 패치를 임베딩 → Transformer로 처리
- 출력: 각 패치의 잠재 표현 벡터

```
64x64 이미지 → 16x16 패치 16개 → 각 패치를 384차원 벡터로
→ Transformer → 16개의 384차원 feature vectors
```

#### (2) 시공간 마스킹 (Spatio-Temporal Masking)
- **공간 마스킹**: 이미지의 일부 영역을 가림 (MAE처럼)
- **시간 마스킹**: 비디오의 일부 프레임을 가림
- **시공간 마스킹**: 둘 다 동시에 적용 ← V-JEPA의 핵심

```
시간 →  t1    t2    t3    t4    t5
공간:  [□□■]  [□□□]  [■■■]  [□□□]  [■□■]
       □=보임, ■=가림
```

마스킹 비율: 보통 75-90% (대부분을 가림! → 더 강한 학습 신호)

#### (3) Context Encoder vs Target Encoder
- **Context Encoder**: 보이는 패치를 인코딩 (학습됨, gradient 업데이트)
- **Target Encoder**: 가려진 패치의 "정답" 표현 생성 (EMA 업데이트, gradient 없음)

```
Target Encoder 파라미터 = EMA(Context Encoder 파라미터)
θ_target ← τ * θ_target + (1-τ) * θ_context   (τ ≈ 0.996)
```

**왜 EMA?**: Target이 너무 빨리 바뀌면 학습이 불안정 → 천천히 따라가게 함
(BYOL, DINO에서 검증된 기법)

#### (4) Predictor (Transformer)
- Context features + 마스크 위치 정보 → 가려진 위치의 feature 예측
- 좁은(narrow) Transformer 사용 (Encoder보다 작음)
- **위치 임베딩이 핵심**: "t3의 왼쪽 위 패치를 예측해라"

#### (5) Loss Function
```python
loss = L2_norm(predicted_features - target_features)
# 잠재공간에서의 유클리드 거리
# 픽셀 복원 loss가 아님!
```

## 3. V-JEPA vs 기존 방법

| | MAE (Masked Autoencoder) | VideoMAE | V-JEPA |
|---|---|---|---|
| 예측 대상 | 픽셀 | 픽셀 (비디오) | **잠재 표현** |
| 마스킹 | 공간 | 시공간 | 시공간 |
| 디코더 | 픽셀 디코더 | 픽셀 디코더 | **없음** (잠재 예측만) |
| 학습 목표 | 원본 복원 | 원본 복원 | 표현 간 거리 |
| 불필요 정보 | 학습됨 | 학습됨 | **무시됨** |
| 연산 효율 | 보통 | 느림 | **빠름** (디코더 없음) |

## 4. V-JEPA의 시간적 예측이 우리 프로젝트에 중요한 이유

### 로봇 시나리오
```
현재(t): 에이전트 위치 (3, 2), 전방 장애물 거리 1.5m
  ↓ V-JEPA 예측
미래(t+3): "이대로 가면 장애물과 0.3m → 충돌 위험"
  ↓ [Flow Matching](04_flow_matching_explained.md)
행동: 오른쪽으로 회피 경로 생성
```

V-JEPA가 해주는 것:
1. **미래 상태 예측**: "3스텝 후 센서 상태는 이럴 것이다" (잠재공간에서)
2. **패턴 인식**: "이 패턴은 충돌 직전 패턴과 유사하다"
3. **효율적 표현**: 5개 센서 × 시간 = 고차원 → 저차원 잠재공간

### M3 환경과의 연결
```
M3 ObstacleAvoidanceEnv
  env.step(action) → obs (5 sensors) → [t1, t2, t3, ...]
                                              ↓
M7에서 구현할 V-JEPA:  [t1, t2] → 잠재공간에서 t3 예측
```

## 5. 우리 프로젝트에서의 간소화

원본 V-JEPA는 대규모 비디오 데이터셋(VideoMix2M)에서 사전학습하지만,
우리는 **핵심 메커니즘만 차용**합니다:

| 원본 V-JEPA | 우리 버전 |
|---|---|
| ViT-Large/Huge | 작은 Transformer (4-6 layer) |
| 비디오 프레임 (224x224) | 센서 잠재 벡터 (64차원) |
| 수백만 비디오 | M3 환경에서 생성한 에피소드 |
| 시공간 마스킹 | 시간 마스킹 (미래 스텝 예측) |
| 자기지도학습 | 환경 에피소드에서 학습 |

**차용하는 핵심**:
1. 잠재공간에서의 예측 (픽셀 복원 없음)
2. EMA Target Encoder
3. Predictor가 미래 표현을 예측하는 구조
4. 마스킹 기반 학습

## 6. 한 줄 요약

> "V-JEPA는 비디오의 가려진 부분을 잠재공간에서 예측하여 시간적 패턴을 학습하는 모델이며, 우리 프로젝트에서 '미래에 무슨 일이 일어날지' 예측하는 World Model의 핵심 기반이다."

**관련 문서**: [World Model 역사](01_world_model_history.md) | [V-JEPA vs MuZero vs JEPA 비교](07_vjepa_vs_muzero_vs_jepa.md) | [왜 Triple Integration인가](08_why_triple_integration.md)

> ← [01_world_model_history.md](01_world_model_history.md) | **다음 문서**: [03_pi0_architecture_analysis.md](03_pi0_architecture_analysis.md) →

---
*참조: Bardes et al., "Revisiting Feature Prediction for Learning Visual Representations from Video" (2024)*
