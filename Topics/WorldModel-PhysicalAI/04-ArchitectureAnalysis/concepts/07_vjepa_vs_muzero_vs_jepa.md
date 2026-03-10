# V-JEPA vs MuZero vs JEPA 비교 분석

## 1. 3가지 핵심 비교

### 한눈에 보기

```
JEPA:    입력 표현 → Predictor → 대상 표현 (정적)
V-JEPA:  시간적 표현들 → Predictor → 미래 표현 (동적, 비디오)
MuZero:  잠재 상태 + 행동 → Dynamics → 미래 상태 + 보상 + 정책
```

## 2. 상세 비교표
ㅍ
| 항목 | JEPA | V-JEPA | MuZero |
|---|---|---|---|
| **소속** | Meta FAIR (LeCun) | Meta FAIR | DeepMind |
| **연도** | 2022 (개념) / 2023 (I-JEPA) | 2024 | 2019 |
| **입력** | 이미지 (정적) | **비디오 (시간적)** | 게임 관측 |
| **예측 대상** | 마스킹된 이미지 영역의 표현 | **마스킹된 시간 구간의 표현** | 미래 상태 + 보상 + 정책 |
| **예측 공간** | 잠재공간 | **잠재공간** | 잠재공간 |
| **마스킹** | 공간 (이미지 패치) | **시공간 (프레임 + 패치)** | 없음 (행동 기반 전이) |
| **학습 방식** | 자기지도학습 (SSL) | **자기지도학습 (SSL)** | 강화학습 (RL) |
| **행동 개입** | 없음 | **없음** | 있음 (action 입력) |
| **Target Encoder** | EMA | **EMA** | 없음 |
| **계획 능력** | 없음 | **없음 (표현 학습만)** | MCTS (트리 탐색) |
| **행동 공간** | 해당 없음 | **해당 없음** | 이산적 (게임 액션) |
| **연속 제어** | 해당 없음 | **간접 지원** | 어려움 |
| **규모** | ViT-Large | **ViT-Large/Huge** | 비교적 작음 |
| **데이터** | ImageNet | **VideoMix2M** | Self-play |

## 3. 아키텍처 비교 다이어그램

### JEPA (이미지)
```
이미지 x ──→ [x-Encoder] ──→ sx
                                  ↘
                              [Predictor] → ŝy ←비교→ sy
                                  ↗
이미지 y ──→ [y-Encoder*] ──→ sy     (*EMA 업데이트)
  (마스킹)
```

### V-JEPA (비디오)
```
비디오 [...t1...t2...t3...t4...t5...]
           ↓ 시공간 마스킹
보이는 부분 → [Context Encoder] → context features
                                      ↓
                                 [Predictor] → predicted features
                                      ↓
                                 Loss: L2(predicted, target)
                                      ↑
가려진 부분 → [Target Encoder*] → target features  (*EMA)
```

### MuZero
```
관측 ──→ [Representation] ──→ h₀ (잠재 상태)
                                  ↓ + action₀
                          [Dynamics] → h₁ + reward₁
                                  ↓ + action₁
                          [Dynamics] → h₂ + reward₂
                                  ↓
각 hᵢ → [Prediction] → policy_i + value_i
         ↓
       MCTS → 최적 행동 선택
```

## 4. 핵심 차이점 분석

### (1) "무엇을 예측하는가"

| | 예측 대상 | 비유 |
|---|---|---|
| JEPA | 이미지의 가려진 부분의 의미 | "사진의 가려진 부분에 뭐가 있을까?" |
| V-JEPA | **미래 시간의 의미** | "다음에 무슨 일이 일어날까?" |
| MuZero | **행동 후 미래 상태 + 보상** | "이렇게 하면 어떻게 될까? 얼마나 좋을까?" |

### (2) "행동이 개입하는가"

- **JEPA/V-JEPA**: 행동 없이 순수하게 관측만으로 학습 (passive observation)
- **MuZero**: "이 행동을 하면" 미래가 어떻게 되는지 학습 (active intervention)

→ **우리 프로젝트**: V-JEPA(관측 기반) + Flow Matching(행동 생성) 분리 설계

### (3) "시간을 어떻게 다루는가"

- **JEPA**: 시간 개념 없음 (단일 이미지)
- **V-JEPA**: 시간적 마스킹으로 미래 예측 ← **시간적 추론의 핵심**
- **MuZero**: 행동-상태 전이로 시간 진행

### (4) "학습 데이터"

- **JEPA/V-JEPA**: 레이블 없는 이미지/비디오 (자기지도)
- **MuZero**: 자기 자신과의 대전 (self-play, 강화학습)

## 5. 왜 우리 프로젝트에 V-JEPA인가?

| 요구사항 | JEPA | V-JEPA | MuZero |
|---|---|---|---|
| 시간적 미래 예측 | ❌ | ✅ | ✅ |
| 연속 제어 (로봇) | ❌ | ✅ | ❌ (이산적) |
| 자기지도학습 (레이블 불필요) | ✅ | ✅ | ❌ (보상 필요) |
| 멀티모달 확장 가능 | ⚠️ | ✅ | ❌ |
| 잠재공간 예측 (효율적) | ✅ | ✅ | ✅ |
| 행동 생성과 분리 가능 | ✅ | ✅ | ❌ (통합) |

**결론**:
- JEPA: 시간 개념 없음 → 로봇에 부적합
- MuZero: 이산 행동 + 강화학습 필요 → 우리 설계와 맞지 않음
- **V-JEPA**: 시간적 예측 + 자기지도 + 연속 제어 호환 → **최적 선택**

## 6. Triple Integration에서의 역할 분담

```
LLM:      "의미"를 이해 (왜?)        → 설명 가능성  → 05_llm_as_sensor.md
V-JEPA:   "미래"를 예측 (앞으로?)     → World Model → 02_v_jepa_explained.md
π0 FM:    "행동"을 생성 (어떻게?)     → 부드러운 제어 → 04_flow_matching_explained.md

세 가지가 각각 다른 질문에 답하고, 합쳐서 완전한 시스템
```

## 7. 한 줄 요약

> "JEPA는 정적 이해, MuZero는 게임 계획, V-JEPA는 시간적 미래 예측에 특화되며, 연속 제어 로봇의 World Model로는 V-JEPA가 최적이다."

**관련 문서**: [World Model 역사](01_world_model_history.md) | [V-JEPA 상세](02_v_jepa_explained.md) | [왜 Triple Integration인가](08_why_triple_integration.md)

> ← [06_vla_explained.md](06_vla_explained.md) | **다음 문서**: [08_why_triple_integration.md](08_why_triple_integration.md) →

---
*참조: LeCun (JEPA, 2022), Bardes et al. (V-JEPA, 2024), Schrittwieser et al. (MuZero, 2019)*
