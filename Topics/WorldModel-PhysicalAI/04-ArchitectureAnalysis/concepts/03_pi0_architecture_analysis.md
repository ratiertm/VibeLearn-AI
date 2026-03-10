# π0 (Physical Intelligence) 아키텍처 분석

## 논문 정보
- **제목**: π0: A Vision-Language-Action Flow Model for General Robot Control
- **저자**: Kevin Black, Noah Brown, Danny Driess, Adnan Esmail, Michael Equi, Chelsea Finn, Niccolo Fusai, Lachy Groom, et al.
- **소속**: Physical Intelligence
- **출판**: 2024
- **링크**: https://arxiv.org/abs/2410.24164

## 1. 핵심 아이디어

### 한 문장 요약
> "VLM(시각-언어 이해) + Flow Matching(부드러운 행동 생성)을 결합하여, 하나의 모델로 다양한 로봇 작업을 수행하는 범용 제어 모델."

### 직관적 비유
- **기존 로봇**: "접시 닦기" 모델, "빨래 개기" 모델, "물건 집기" 모델 → 각각 별도
- **π0**: 하나의 모델이 "접시 닦아", "빨래 개", "물건 집어"를 모두 이해하고 실행

## 2. 아키텍처 상세

```
┌─────────────────────────────────────────────────┐
│                    π0 모델                       │
│                                                  │
│  [이미지] ──→ ┐                                  │
│  [텍스트] ──→ ├→ PaliGemma (VLM) → 상태 토큰들    │
│  [로봇상태]─→ ┘     (3B params)                   │
│                        ↓                         │
│              Action Expert (Transformer)         │
│              + Flow Matching Head                │
│                        ↓                         │
│              행동 시퀀스 [a1...a8] (50Hz)         │
└─────────────────────────────────────────────────┘
```

### 각 컴포넌트

#### (1) PaliGemma (VLM Backbone)
- Google의 Vision-Language Model
- 이미지 + 텍스트를 동시에 이해
- **역할**: "앞에 빨간 컵이 있고, '컵을 집어'라는 명령" → 통합 이해

```
이미지 (224x224) → SigLIP Vision Encoder → 이미지 토큰
텍스트 ("pick up the cup") → Gemma Tokenizer → 텍스트 토큰
  ↓ 합쳐서
PaliGemma Transformer → 상태 토큰 시퀀스
```

#### (2) Action Expert
- VLM 출력을 받아 행동을 생성하는 전용 Transformer
- VLM과 **교차 어텐션(cross-attention)**으로 연결
- 왜 분리? → VLM은 "이해", Action Expert는 "행동 생성" 전문화

```
VLM 토큰 ────→ Cross-Attention ←──── Action 토큰 (노이즈)
                     ↓
              Denoised Action 토큰
                     ↓
              행동 시퀀스 [a1, a2, ..., a8]
```

#### (3) Flow Matching Head
- Action Expert의 출력을 **부드러운 연속 행동**으로 변환
- [별도 문서에서 상세 분석](04_flow_matching_explained.md)

### 학습 방식
1. **사전학습**: 대규모 로봇 데이터셋 (다양한 로봇, 다양한 작업)
2. **미세조정**: 특정 작업에 맞춰 fine-tuning
3. **Flow Matching 학습**: 노이즈 → 실제 행동으로의 흐름 학습

## 3. π0의 강점과 한계

### 강점
| 강점 | 설명 |
|---|---|
| 범용성 | 하나의 모델로 다양한 로봇 작업 |
| 자연어 명령 | "빨래 개" 같은 자연어로 제어 가능 |
| 부드러운 행동 | Flow Matching으로 jerky하지 않은 연속 제어 |
| VLM 기반 이해 | 이미지+텍스트 동시 이해로 맥락 파악 |
| 확장성 | 데이터가 많을수록 성능 향상 |

### 한계
| 한계 | 설명 | 우리의 해결 |
|---|---|---|
| 설명 불가 | "왜 이 행동인지" 모름 | [LLM 해석](05_llm_as_sensor.md) + [V-JEPA 예측](02_v_jepa_explained.md)으로 이중 설명 |
| 미래 예측 없음 | "3초 후 어떻게 될지" 모름 | [V-JEPA World Model](02_v_jepa_explained.md) 추가 |
| 단일 센서 | 카메라 + 텍스트만 | 5개 센서 멀티모달 확장 |
| 대규모 데이터 필요 | 수만 시간 로봇 데이터 | 시뮬레이션 환경(M3)에서 생성 |

## 4. π0에서 우리가 차용하는 것

### 차용 O
1. **VLM Encoder 구조**: 이미지 + 텍스트 → 통합 표현 (우리는 5센서로 확장)
2. **Flow Matching 행동 생성**: 노이즈 → 부드러운 행동 시퀀스
3. **Action Expert 분리**: 이해(Encoder)와 행동(Expert) 분리 설계

### 차용 X
1. **PaliGemma 3B**: 너무 큼 → 작은 Encoder들로 대체
2. **사전학습**: 대규모 로봇 데이터 없음 → M3 환경에서 학습
3. **범용 제어**: 우리는 장애물 회피 특화

### 우리 버전 vs π0

| | π0 | 우리 프로젝트 |
|---|---|---|
| VLM | PaliGemma 3B | 작은 CNN + MLP Encoders |
| 입력 | 카메라 + 텍스트 | 5개 센서 (카메라, 거리, 오디오, IMU, LLM) |
| World Model | 없음 | **V-JEPA 추가** (시간적 예측) |
| 설명 가능성 | 없음 | **LLM 해석 + V-JEPA 예측** |
| 행동 생성 | Flow Matching | Flow Matching (차용) |
| 규모 | 3B+ params | ~1M params |
| 데이터 | 수만 시간 로봇 | M3 환경 에피소드 |

## 5. openpi (오픈소스 구현)

π0의 공식 오픈소스 구현: https://github.com/Physical-Intelligence/openpi

```
openpi/
├── models/
│   ├── pi0.py          # 메인 모델
│   ├── vlm.py          # VLM (PaliGemma) 래퍼
│   └── flow_matching.py # Flow Matching 헤드
├── training/
│   ├── train.py        # 학습 스크립트
│   └── data.py         # 데이터 로더
└── inference/
    └── policy.py       # 추론 (행동 생성)
```

**실습 2에서 상세 탐색 예정**

## 6. 한 줄 요약

> "π0는 VLM으로 세상을 이해하고 Flow Matching으로 부드러운 행동을 생성하는 범용 로봇 모델이며, 우리는 여기에 V-JEPA(미래 예측)와 LLM(설명 가능성)을 추가하여 Triple Integration을 만든다."

**관련 문서**: [Flow Matching 상세](04_flow_matching_explained.md) | [VLA 모델 비교](06_vla_explained.md) | [왜 Triple Integration인가](08_why_triple_integration.md)

> ← [02_v_jepa_explained.md](02_v_jepa_explained.md) | **다음 문서**: [04_flow_matching_explained.md](04_flow_matching_explained.md) →

---
*참조: Black et al., "π0: A Vision-Language-Action Flow Model for General Robot Control" (2024)*
