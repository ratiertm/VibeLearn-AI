# Topic 정보: WorldModel-PhysicalAI

## Topic 기본 정보

**Topic 이름**: WorldModel-PhysicalAI

**Topic 설명**:
LLM + V-JEPA + π0(Physical Intelligence)를 통합한 Triple Integration 아키텍처 구현.
5개 센서(카메라, 거리, 오디오, IMU, LLM)로 환경을 인식하고, V-JEPA 기반 World Model로
잠재공간에서 미래를 예측한 뒤, π0 스타일 Flow Matching으로 부드러운 행동을 생성하여
장애물을 회피하는 "설명 가능한" 에이전트 구축.

**학습 목적**:
- 소형 로봇과 자율주행차를 만들기 위한 기반 지식 습득
- LLM + V-JEPA + π0 Triple Integration 하이브리드 아키텍처 이해 및 구현
- "왜 이 행동을 했는가"를 설명할 수 있는 Physical AI 시스템 구현

**핵심 방향**:
- π0의 VLM Encoder + Flow Matching 행동 생성 방식 참고
- V-JEPA 기반 World Model로 잠재공간에서 시간적 미래 예측
- 5개 센서: 카메라(시각), 거리센서(촉각 대체), 마이크(청각), IMU(균형), LLM(언어/의미 해석)
- LLM 양방향 역할: 상황 해석("앞에 사람이 있어, 위험해") + 명령 수신("오른쪽으로 가")
- 설명 가능성: World Model 예측 + LLM 해석으로 행동의 이유를 설명
- 시뮬레이션 환경에서 환경을 바꿔가며 학습 가능한 시스템

**예상 학습 기간**: 10주 (주당 10시간, 총 100시간)

---

## 학습 목표

- [ ] LLM, V-JEPA, π0(VLA)의 아키텍처를 각각 비교 설명할 수 있다
- [ ] Physical AI가 무엇이고 왜 필요한지 사례와 함께 설명할 수 있다
- [ ] VLM + LLM 기반 Encoder로 5개 센서 데이터를 잠재공간으로 인코딩하고 융합할 수 있다
- [ ] V-JEPA 기반 World Model로 잠재공간에서 미래 상태를 예측할 수 있다
- [ ] π0 스타일 Flow Matching으로 부드러운 행동 시퀀스를 생성할 수 있다
- [ ] LLM 해석 + World Model 예측으로 "왜 이 행동인지" 설명 가능한 장애물 회피 시스템을 만들 수 있다

---

## 학습 환경

**운영 체제**: macOS (Darwin 23.5.0)

**주요 도구 및 기술 스택**:
- Python 3.x
- PyTorch (딥러닝 프레임워크)
- Jupyter Notebook
- VS Code / Claude Code
- 시뮬레이션 환경 (OpenAI Gym / MuJoCo 등)

**사전 지식**:
```
필수:
- Python 기초 (초보자 수준)
- 딥러닝 기본 개념 (아주 조금 경험)

권장:
- 선형대수 기초
- 확률/통계 기본 개념
- 강화학습 기초 개념
```

---

## 학습 접근 방식

- [x] 실습 중심, 필요한 이론만 (권장)
- 주당 학습 시간: 10시간
- 1회당 학습 시간: 2-3시간

## 특별히 집중하고 싶은 영역

- LLM + V-JEPA + π0 Triple Integration 아키텍처 구현
- VLM + LLM 기반 Encoder + Flow Matching 행동 생성
- 5모달 센서 융합 (카메라 + 거리 + 오디오 + IMU + LLM)
- LLM 양방향: 상황 해석 + 자연어 명령 수신
- V-JEPA 기반 시간적 미래 예측 World Model
- 설명 가능한 행동: World Model 예측 + LLM 해석으로 "왜 이 행동인지" 시각화
- 소형 로봇/자율주행차 적용 가능성
- 환경을 바꿔가며 학습/실험할 수 있는 시스템

---

**생성일**: 2026-03-09
**방법론**: VibeLearn AI
