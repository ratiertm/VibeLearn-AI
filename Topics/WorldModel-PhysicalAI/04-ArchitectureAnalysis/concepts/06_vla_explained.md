# VLA (Vision-Language-Action) 모델 해설

## 1. VLA란?

### 한 문장 요약
> "이미지를 보고(Vision), 언어를 이해하고(Language), 행동을 생성(Action)하는 통합 모델."

### 발전 과정
```
Vision Model:     이미지 → 분류/검출         (뭐가 있는지 봄)
VLM:              이미지 + 텍스트 → 이해     (보면서 설명)
VLA:              이미지 + 텍스트 → 행동     (보면서 행동!)
```

## 2. VLA의 핵심 구조

```
┌───────────────────────────────────────┐
│              VLA 모델                  │
│                                        │
│  [이미지] → Vision Encoder ──┐          │
│                              ├→ 통합 → Action Head → 행동
│  [텍스트] → Language Model ──┘          │
│                                        │
└───────────────────────────────────────┘
```

## 3. 주요 VLA 모델 비교

| 모델 | 연도 | Vision | Language | Action | 특징 |
|---|---|---|---|---|---|
| **RT-1** | 2022 | EfficientNet | 토큰화된 명령 | 이산 토큰 | 최초의 대규모 로봇 학습 |
| **RT-2** | 2023 | ViT | PaLM-2 | 텍스트→행동 토큰 | LLM이 직접 행동 토큰 출력 |
| **Octo** | 2023 | ViT | 토큰 | Diffusion | 오픈소스, 범용 |
| **[π0](03_pi0_architecture_analysis.md)** | 2024 | SigLIP | Gemma | **[Flow Matching](04_flow_matching_explained.md)** | 부드러운 연속 행동 |

### RT-2의 핵심 아이디어
```
입력: [이미지] + "pick up the bottle"
     ↓
VLM (PaLM-2 + ViT): 이미지와 텍스트를 함께 이해
     ↓
출력: "1 128 91 241 1 000 000"  (행동을 텍스트 토큰으로!)
     ↓
디코딩: [x=0.1, y=0.28, z=0.91, gripper=open, ...]
```

**핵심**: 행동도 "단어"처럼 취급 → LLM의 시퀀스 생성 능력 활용

### π0의 차별점
- RT-2: 행동을 이산 토큰으로 → 해상도 제한, 부자연스러운 동작
- **π0**: Flow Matching으로 연속 행동 → 부드럽고 자연스러운 동작

## 4. 우리 프로젝트와의 관계

### 우리 프로젝트 = VLA 확장판

```
전통 VLA:     Vision + Language → Action
우리 프로젝트: Vision + Language + Distance + Audio + IMU
              → V-JEPA World Model → Flow Matching Action
              + 설명 가능성
```

### 차이점

| | 전통 VLA | 우리 Triple Integration |
|---|---|---|
| 센서 | 카메라 + 텍스트 | **5개 센서** |
| 미래 예측 | 없음 | **[V-JEPA](02_v_jepa_explained.md) World Model** |
| 행동 생성 | 토큰 or Diffusion | **[Flow Matching](04_flow_matching_explained.md)** (π0 차용) |
| 설명 가능성 | 제한적 | **[LLM](05_llm_as_sensor.md) + V-JEPA 이중 설명** |
| 규모 | 수십억 파라미터 | **~1M 파라미터** (교육용) |

## 5. 한 줄 요약

> "VLA는 '보고-이해하고-행동하는' 통합 모델의 패러다임이며, 우리 프로젝트는 여기에 멀티모달 센서, World Model, 설명 가능성을 추가한 확장판이다."

**관련 문서**: [π0 아키텍처](03_pi0_architecture_analysis.md) | [LLM as Sensor](05_llm_as_sensor.md) | [왜 Triple Integration인가](08_why_triple_integration.md)

> ← [05_llm_as_sensor.md](05_llm_as_sensor.md) | **다음 문서**: [07_vjepa_vs_muzero_vs_jepa.md](07_vjepa_vs_muzero_vs_jepa.md) →

---
*참조: RT-1 (Brohan et al. 2022), RT-2 (Brohan et al. 2023), Octo (Team et al. 2023), π0 (Black et al. 2024)*
