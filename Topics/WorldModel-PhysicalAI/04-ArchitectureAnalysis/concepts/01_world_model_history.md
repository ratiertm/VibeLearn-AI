# World Model의 역사와 발전

## 핵심 질문
> "에이전트가 행동하기 전에 '상상'할 수 있다면, 더 안전하고 효율적인 행동이 가능하지 않을까?"

## 1. World Model이란?

World Model = 에이전트가 내부에 가진 **세상의 시뮬레이터**.
실제로 행동하지 않고도 "이렇게 하면 어떻게 될까?"를 예측할 수 있다.

**비유**: 체스 고수가 머릿속으로 수를 두어보는 것

```
현재 상태 → World Model → 예측된 미래 상태 → 최적 행동 선택
              (상상)
```

## 2. 세대별 발전

### 1세대: Ha & Schmidhuber (2018) - "World Models"

```
이미지 → VAE(압축) → 잠재 벡터 z → RNN(미래 예측) → Controller(행동)
         (64x64→32)   (z를 시간에 따라 전개)      (간단한 선형 모델)
```

**구조**:
- **V (Vision)**: VAE로 이미지를 잠재 벡터로 압축
- **M (Memory)**: MDN-RNN으로 잠재 벡터의 시간적 변화 예측
- **C (Controller)**: 잠재 벡터 → 행동 (간단한 선형 모델)

**성과**: 레이싱 게임에서 "꿈 속에서 학습" (dream training) 성공

**한계**:
- VAE가 **픽셀 복원**에 의존 → 배경색, 텍스처 등 불필요한 디테일까지 학습
- 복잡한 환경에서 잠재 표현 품질 저하
- 연속 제어에 제한적

---

### 2세대: MuZero (DeepMind, 2019)

```
관측 → Representation → 잠재 상태 h
                          ↓
                   Dynamics: h + action → h' (미래 상태)
                   Prediction: h → policy + value (보상+정책)
                          ↓
                   MCTS로 "상상 속 계획" → 최적 행동
```

**핵심 혁신**:
- 환경 규칙을 모르는 상태에서도 학습 (model-free → model-based)
- 잠재공간에서 미래 상태 + 보상 + 정책을 **동시에** 학습
- MCTS(몬테카를로 트리 탐색)로 "상상 속에서" 여러 수를 계획

**성과**: 바둑, 체스, Atari 게임에서 초인적 성능

**한계**:
- 이산적 행동(게임)에 특화 → 연속 제어(로봇)에 적용 어려움
- 트리 탐색 비용이 큼
- 학습에 대량의 self-play 데이터 필요

---

### 3세대: JEPA (LeCun / Meta FAIR, 2022) - 패러다임 전환

```
입력 x → x-Encoder → sx (x의 표현)
입력 y → y-Encoder → sy (y의 표현)

Predictor: sx → ŝy (x의 표현으로 y의 표현을 예측)
Loss: Energy(sy, ŝy) — 잠재공간에서의 거리
```

**핵심 혁신 - "추상적 예측"**:
- **기존**: 픽셀 → 픽셀 예측 (나뭇잎 하나하나 예측)
- **JEPA**: 표현 → 표현 예측 (나무가 있다는 것만 예측)
- 불필요한 디테일을 버리고 **구조와 의미**만 학습

**왜 중요한가**:
- 인간의 인지도 이와 유사: 우리는 미래를 픽셀 단위로 예측하지 않음
- "내가 걸어가면 벽에 부딪힐 것이다" (추상적 예측)

**비교**:
| | 생성 모델 (VAE, GAN) | JEPA |
|---|---|---|
| 예측 대상 | 픽셀 | 잠재 표현 |
| 학습 목표 | 원본 복원 | 표현 간 관계 |
| 불필요 정보 | 포함 (비효율) | 제거 (효율) |
| 비유 | 사진을 그려서 비교 | 핵심 특징만 비교 |

---

### 4세대: V-JEPA (Meta, 2024) - 비디오로 확장

JEPA를 **시간 축(비디오)**으로 확장한 것. → [V-JEPA 상세 분석](02_v_jepa_explained.md)

```
비디오 [t1, t2, t3, t4, t5]
마스킹:  [t1, ██, t3, ██, t5]  (t2, t4를 가림)
목표: 보이는 프레임으로 가려진 프레임의 "표현"을 예측
```

---

## 3. 우리 프로젝트에서의 위치

```
1세대 World Models:  VAE 복원 기반 → 단순 환경만 가능
2세대 MuZero:        게임 특화 → 로봇에 부적합
3세대 JEPA:          추상적 예측 → 개념적 기반
4세대 V-JEPA:        시간적 미래 예측 → ★ 우리 World Model의 핵심 ★
```

**우리가 V-JEPA를 선택한 이유**:
1. 로봇에는 **시간적 예측**이 필수 ("3초 후 장애물에 부딪힌다")
2. 픽셀 복원 불필요 → 연산 효율적
3. 5개 센서의 멀티모달 입력을 잠재공간에서 통합 가능
4. [π0의 Flow Matching](04_flow_matching_explained.md)과 자연스럽게 연결

**관련 문서**: [V-JEPA vs MuZero vs JEPA 비교](07_vjepa_vs_muzero_vs_jepa.md) | [왜 Triple Integration인가](08_why_triple_integration.md)

> **다음 문서**: [02_v_jepa_explained.md](02_v_jepa_explained.md) →

## 4. 한 줄 요약

> "World Model은 '픽셀 복원 → 잠재 예측 → 추상적 시간 예측'으로 발전했고, V-JEPA가 로봇용 World Model의 최적 후보다."

---
*참조: Ha & Schmidhuber 2018, MuZero (Schrittwieser et al. 2019), JEPA (LeCun 2022), V-JEPA (Bardes et al. 2024)*
