# WorkLog: M4 아키텍처 분석

**날짜**: 2026-03-11
**모듈**: M4 - LLM + V-JEPA + π0 Architecture Analysis
**소요 시간**: ~3시간

## 진행 내용

### Day 1 (2026-03-11)

#### 실습 1: 논문/아키텍처 분석 (~2시간)
- World Model 발전사 정리 (Ha&Schmidhuber → MuZero → JEPA → V-JEPA)
- V-JEPA 심층 분석: 시공간 마스킹, EMA Target Encoder, 잠재공간 L2 Loss
- π0 분석: PaliGemma VLM + Action Expert + Flow Matching 구조
- Flow Matching 개념: 노이즈→목표 직선 경로, 속도장 학습, MSE Loss
- LLM as Sensor: 양방향(상황해석+명령수신), SayCan/PaLM-E/RT-2 비교
- VLA 모델 비교: RT-1 → RT-2 → Octo → π0 발전 과정
- V-JEPA vs MuZero vs JEPA 레이더 차트 비교
- Why Triple Integration: 각 컴포넌트 역할 분담 + 인간 의사결정 비유
- 실습 1 노트북: 타임라인, 레이더 차트, 파이프라인 다이어그램, Flow Matching/V-JEPA 시각화

#### 실습 2: 코드 구조 탐색 시작 (~1시간)
- SimplifiedPi0: Flow Matching 학습/추론 핵심 패턴 구현
- SimplifiedVJEPA: Context Encoder + EMA Target + Predictor 구현
- MiniTripleIntegration: End-to-End 프로토타입 (~25K params)
- 차용 포인트 vs 자체 구현 정리

## DoD 체크리스트
- [x] V-JEPA 분석 문서 작성
- [x] π0 분석 문서 작성
- [x] LLM as Sensor 분석 문서 작성
- [ ] π0 openpi + V-JEPA 코드 분석 완료 (코드 탐색 시작, 상세 분석 남음)
- [ ] Triple Integration 아키텍처 설계 문서 완성 (실습 3)
- [x] "왜 Triple Integration인가" 설명 문서 작성
- [x] README.md 작성
- [x] WorkLog 작성 완료

#### 문서 구조화 작업 (~30분)
- 8개 개념 문서에 번호 접두사 추가 (01_ ~ 08_)
- 파일 간 **관련 문서** 상호 참조 링크 추가 (08이 허브 역할)
- 순차 읽기용 **이전/다음 문서** 네비게이션 링크 추가
- M1~M4 README.md에 실습 파일 클릭 가능 링크 확인/추가

## Daily Retrospective

**잘한 점**:
- 8개 개념 문서 + 2개 노트북 = 체계적인 분석 산출물
- SimplifiedPi0/VJEPA/MiniTriple로 핵심 구조를 코드로 검증
- 차용/자체구현 구분이 명확

**개선점**:
- 실제 openpi/V-JEPA repo 코드를 직접 읽지 못함 (간소화 버전으로 대체)
- Flow Matching 수학적 이해가 직관 수준에 머무름

**내일 할 일**:
- M4 실습 3: Triple Integration 아키텍처 설계 문서 (입출력 차원 확정)
- M4 마무리 후 M5 시작: 개별 센서 Encoder 구현
