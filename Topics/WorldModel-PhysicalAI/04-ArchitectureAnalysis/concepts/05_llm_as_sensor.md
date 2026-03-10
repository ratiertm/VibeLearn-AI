# LLM as Sensor: 언어를 센서로 활용하기

## 1. 핵심 아이디어

### 한 문장 요약
> "LLM을 '제6의 센서'로 사용하여, 카메라/거리/오디오/IMU가 감지하지 못하는 **의미적 정보**를 추가한다."

### 기존 로봇의 한계
```
카메라: "전방 2m에 높이 1m 물체" → 사각형 물체가 있다
거리센서: "1.8m 거리에 장애물"    → 뭔가 있다
오디오: "전방에서 소리"           → 소리가 난다
IMU: "현재 시속 0.5m/s"          → 움직이고 있다

→ 이 물체가 어린이인지 쓰레기통인지 모름!
→ "천천히 가"라는 사람의 명령을 이해할 수 없음!
```

### LLM을 추가하면
```
LLM (상황 해석): "전방 2m에 어린이가 서 있음. 위험도 높음."
LLM (명령 수신): "사용자 명령: '조심해서 우회해'" → 행동 힌트

→ 4개 센서가 못하는 "의미" 차원 추가
→ 자연어로 로봇을 제어 가능
```

## 2. LLM의 양방향 역할

### 방향 1: 상황 해석 (센서 → LLM → 의미)
```
[4개 센서 데이터] → LLM 프롬프트 생성 → LLM 추론 → 상황 텍스트
                    "거리센서: 전방 1.5m,       "앞에 움직이는 물체.
                     카메라: 작은 물체 감지,       어린이로 추정.
                     오디오: 웃음소리"              안전 우선 회피 권장."
```

**우리 프로젝트 M3에서의 구현**:
```python
# ObstacleAvoidanceEnv._get_info()
info['llm_interpretation'] = "SE방향 장애물 감지. 목표: 정면 10.8m. 이동 중."
```
현재는 규칙 기반이지만, 이후 실제 LLM으로 교체 가능.

### 방향 2: 명령 수신 (사용자 → LLM → 행동 힌트)
```
사용자: "오른쪽으로 가"
  ↓
LLM 파싱: {"command": "move_right", "urgency": "normal"}
  ↓
Language Encoder → 행동 조건 벡터 (16차원)
  ↓
Flow Matching이 이 조건을 반영하여 행동 생성
```

## 3. 관련 연구: LLM + 로봇 제어

### SayCan (Google, 2022)
```
사용자: "나 배고파"
LLM: "1. 부엌으로 가기 2. 냉장고 열기 3. 사과 집기 4. 가져오기"
로봇: 각 단계를 실행 가능성(affordance)과 결합
```
- **LLM 역할**: 고수준 계획 (plan) 생성
- **한계**: LLM과 로봇이 분리됨 → 실시간 반응 어려움

### PaLM-E (Google, 2023)
```
이미지 + 텍스트 → PaLM-E (562B) → 계획 + 로봇 명령
```
- **LLM 역할**: 시각 정보를 직접 이해 + 계획
- **한계**: 모델이 너무 큼 (562B params)

### RT-2 (Google DeepMind, 2023)
```
이미지 + 텍스트 → VLM → 직접 행동 토큰 출력
```
- **LLM 역할**: VLA (Vision-Language-Action) 모델의 일부
- **특징**: 언어 이해와 행동 생성이 하나의 모델

### Code as Policies (2022)
```
사용자: "빨간 블록을 파란 블록 위에 올려"
LLM: Python 코드 생성 → 로봇 API 호출
```
- **LLM 역할**: 코드 생성으로 로봇 제어

## 4. 우리 프로젝트에서의 LLM 통합

### 설계: Language Encoder

```
텍스트 입력 (2종류)
  ├── 상황 해석: "앞에 장애물, 위험"
  └── 명령: "오른쪽으로 가"
      ↓
  Tokenizer + Embedding
      ↓
  Language Encoder (작은 Transformer 또는 MLP)
      ↓
  잠재 벡터 (16차원) → Fusion에 합류
```

### 왜 LLM을 "센서"로 부르는가?

| 전통 센서 | LLM 센서 |
|---|---|
| 카메라: 빛 → 이미지 | LLM: 데이터 → 의미 해석 |
| 거리센서: 초음파 → 거리 | LLM: 텍스트 명령 → 행동 힌트 |
| 입력 → 정보 변환 | 입력 → 정보 변환 |

**공통점**: 환경 정보를 에이전트가 이해할 수 있는 형태로 변환
→ LLM도 "의미 센서"로 볼 수 있다

### 설명 가능성 기여

```
에이전트 행동: "오른쪽으로 회피"

설명 레이어 1 (LLM): "앞에 어린이가 있어서 안전 회피"
설명 레이어 2 ([V-JEPA](02_v_jepa_explained.md)): "직진하면 3스텝 후 충돌 예측됨"

→ 이중 설명 = [Triple Integration](08_why_triple_integration.md)의 핵심 가치
```

## 5. 한 줄 요약

> "LLM을 양방향 센서(상황 해석 + 명령 수신)로 활용하면, 기존 물리 센서가 놓치는 '의미'를 추가하고, V-JEPA 예측과 결합하여 '왜 이 행동인지' 설명할 수 있다."

**관련 문서**: [VLA 모델 비교](06_vla_explained.md) | [V-JEPA](02_v_jepa_explained.md) | [왜 Triple Integration인가](08_why_triple_integration.md)

> ← [04_flow_matching_explained.md](04_flow_matching_explained.md) | **다음 문서**: [06_vla_explained.md](06_vla_explained.md) →

---
*참조: SayCan (Ahn et al. 2022), PaLM-E (Driess et al. 2023), RT-2 (Brohan et al. 2023)*
