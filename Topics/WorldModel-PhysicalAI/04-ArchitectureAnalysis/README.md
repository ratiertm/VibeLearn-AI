# M4: LLM + V-JEPA + π0 아키텍처 분석

## 개요
Triple Integration (LLM + V-JEPA + π0)의 핵심 아키텍처를 분석하고, 우리 프로젝트의 설계 근거를 수립합니다.

## 핵심 파이프라인

```
5개 센서 → M5 Encoders → M6 Fusion → M7 V-JEPA → M8 Flow Matching → 행동
              (각 16d)     (64d)    (미래 예측)  (부드러운 행동)
                                                        ↓
                                              M9 설명: LLM + V-JEPA
```

## 산출물

### concepts/ (개념 문서)
| 파일 | 내용 |
|---|---|
| [01_world_model_history.md](concepts/01_world_model_history.md) | World Model 발전: VAE→MuZero→JEPA→V-JEPA |
| [02_v_jepa_explained.md](concepts/02_v_jepa_explained.md) | V-JEPA 아키텍처 상세 (마스킹, EMA, 잠재 예측) |
| [03_pi0_architecture_analysis.md](concepts/03_pi0_architecture_analysis.md) | π0 분석 (VLM + Action Expert + Flow Matching) |
| [04_flow_matching_explained.md](concepts/04_flow_matching_explained.md) | Flow Matching 개념 (노이즈→목표 직선 경로) |
| [05_llm_as_sensor.md](concepts/05_llm_as_sensor.md) | LLM 양방향 센서 (상황해석 + 명령수신) |
| [06_vla_explained.md](concepts/06_vla_explained.md) | VLA 모델 비교 (RT-1, RT-2, Octo, π0) |
| [07_vjepa_vs_muzero_vs_jepa.md](concepts/07_vjepa_vs_muzero_vs_jepa.md) | 3가지 World Model 레이더 차트 비교 |
| [08_why_triple_integration.md](concepts/08_why_triple_integration.md) | 왜 세 가지를 합쳐야 하는가 |

### examples/ (실습 노트북)
| 파일 | 내용 |
|---|---|
| [01_paper_analysis.ipynb](examples/01_paper_analysis.ipynb) | 타임라인, 레이더 차트, 파이프라인 다이어그램 |
| [02_openpi_code_exploration.ipynb](examples/02_openpi_code_exploration.ipynb) | SimplifiedPi0, SimplifiedVJEPA, MiniTripleIntegration |

## 핵심 결론

| 컴포넌트 | 차용 원본 | 우리 프로젝트에서의 역할 |
|---|---|---|
| V-JEPA | Meta FAIR | World Model (잠재공간 미래 예측) |
| Flow Matching | π0 | 부드러운 행동 시퀀스 생성 |
| LLM | SayCan/RT-2 개념 | 양방향 의미 센서 + 설명 가능성 |
