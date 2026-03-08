# VibeLearn AI Roadmap 생성 프롬프트

**버전**: 3.0
**생성일**: 2026-03-09
**방법론**: VibeLearn AI

---

## 사용 방법

이 프롬프트는 `topic_info.md`에서 입력한 Topic 정보를 바탕으로 학습 로드맵을 자동 생성합니다.

**사용 절차**:
1. 이 파일 전체를 AI에게 전달
2. AI가 VibeLearn AI 표준 로드맵 생성
3. 생성된 로드맵을 `vl_roadmap/YYYYMMDD_RoadMap_WorldModel-PhysicalAI.md`에 저장

---

## [1단계] Topic 정보

### 기본 정보

**Topic 이름**: `WorldModel-PhysicalAI`

**Topic 설명**:
```
LLM + V-JEPA + π0(Physical Intelligence)를 통합한 Triple Integration 아키텍처 구현.
5개 센서(카메라, 거리, 오디오, IMU, LLM)로 환경을 인식하고, V-JEPA 기반 World Model로
잠재공간에서 미래를 예측한 뒤, π0 스타일 Flow Matching으로 부드러운 행동을 생성하여
장애물을 회피하는 "설명 가능한" 에이전트 구축.
```

**학습 목적**:
```
- 소형 로봇과 자율주행차를 만들기 위한 기반 지식 습득
- LLM + V-JEPA + π0 Triple Integration 하이브리드 아키텍처 이해 및 구현
- "왜 이 행동을 했는가"를 설명할 수 있는 Physical AI 시스템 구현
```

**예상 학습 기간**: `10주 (주당 10시간, 총 100시간)`

---

### 환경 및 사전 지식

**운영 체제**: `macOS`

**주요 도구 및 기술 스택**:
```
- Python 3.x
- PyTorch
- Jupyter Notebook
- VS Code / Claude Code
- 시뮬레이션 환경 (OpenAI Gym / MuJoCo 등)
```

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

### 산출물 및 참조

**학습 목표** (달성하고 싶은 것):
```
- [ ] LLM, V-JEPA, π0(VLA)의 아키텍처를 각각 비교 설명할 수 있다
- [ ] Physical AI가 무엇이고 왜 필요한지 사례와 함께 설명할 수 있다
- [ ] VLM + LLM 기반 Encoder로 5개 센서 데이터를 잠재공간으로 인코딩하고 융합할 수 있다
- [ ] V-JEPA 기반 World Model로 잠재공간에서 미래 상태를 예측할 수 있다
- [ ] π0 스타일 Flow Matching으로 부드러운 행동 시퀀스를 생성할 수 있다
- [ ] LLM 해석 + World Model 예측으로 "왜 이 행동인지" 설명 가능한 장애물 회피 시스템을 만들 수 있다
```

**참조 자료**:
```
- π0 논문 (Physical Intelligence)
- V-JEPA 논문 (Meta FAIR)
- openpi (GitHub: Physical-Intelligence/openpi)
- JEPA / MuZero 논문
- SayCan, PaLM-E (LLM + Robot)
- NVIDIA Physical AI / Isaac Sim
- OpenAI Gym / Gymnasium 문서
- PyTorch 공식 튜토리얼
```

**vl_materials/ 폴더**:
```
논문 PDF, 참조 코드, 시뮬레이션 설정 파일 등 저장
```

---

## [2단계] AI에게 요청할 작업

위에 주입된 Topic 정보를 바탕으로 **VibeLearn AI 방법론**에 맞는 학습 로드맵을 생성해주세요.

### STEP 1: 학습 기간 적정성 검토 (필수)

사용자가 입력한 학습 기간 `10주 (주당 10시간, 총 100시간)`이 해당 Topic에 적절한지 분석하고 피드백을 제공하세요.

### STEP 2: 로드맵 생성

사용자가 기간을 최종 확정한 후, 모듈별 상세 로드맵을 생성하세요.

**모듈 구성 원칙**:
- 7-10개 모듈 (10주 기간에 맞게)
- 각 모듈은 독립적으로 완료 가능한 단위
- 난이도는 점진적 상승
- 마지막 모듈은 Capstone 프로젝트 (통합 실습)
- Python 초보자/딥러닝 아주 조금 경험에 맞는 난이도 설정

**각 모듈 필수 9가지 항목**:
1. 모듈 기본 정보 (번호, 제목, 예상 시간)
2. 학습 목표 (3-5개, 측정 가능)
3. 핵심 개념 (이론 20-30%)
4. 실습 과제 (실습 70-80%)
5. 예상 산출물
6. Definition of Done (DoD) 체크리스트
7. 자기 평가 체크리스트
8. 시간 배분
9. 참조 자료

**실습 설계 원칙**:
- 실습 우선 (70-80%)
- 점진적 복잡도
- 검증 가능성
- AI 시대 학습 범위
- 교과서 품질 산출물
- macOS 환경 고려

---

**생성자**: Claude with VibeLearn AI
**Template 버전**: 3.0
**방법론**: VibeLearn AI
