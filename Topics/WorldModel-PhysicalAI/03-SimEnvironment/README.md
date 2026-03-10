# M3. 시뮬레이션 환경 (Simulation Environment)

## 개요
OpenAI Gymnasium을 사용하여 강화학습 환경을 이해하고, 5개 센서가 통합된 **커스텀 장애물 회피 환경**을 구축합니다.

## 학습 목표
1. Gymnasium 환경의 observation-action-reward 루프 이해
2. `spaces.Dict`로 멀티센서 관측 공간 설계
3. 커스텀 환경(`gym.Env` 상속) 구현
4. 5개 센서 시뮬레이션 (카메라, 거리, 오디오, IMU, LLM)
5. 규칙 기반 에이전트로 성능 비교

## 실습 노트북

| 노트북 | 내용 | 핵심 개념 |
|--------|------|-----------|
| [01_gymnasium_basics.ipynb](examples/01_gymnasium_basics.ipynb) | Gymnasium 기초, CartPole, 규칙 에이전트 | `env.step()`, `spaces`, `gym.Env` |
| [02_obstacle_avoidance_env.ipynb](examples/02_obstacle_avoidance_env.ipynb) | 5센서 통합 장애물 회피 환경 | 레이캐스팅, 카메라 렌더링, JSON 설정 |

## ObstacleAvoidanceEnv 구조

```
env.reset() → obs(4센서 dict), info(LLM 해석)
env.step(action) → obs, reward, terminated, truncated, info

관측 (observation):
  camera:   (3, 64, 64)  RGB 이미지 (에이전트 시점)
  distance: (8,)         8방향 레이캐스팅 거리 (0~1)
  audio:    (2, 50)      스테레오 오디오 신호
  imu:      (6,)         속도/가속도/각도

정보 (info):
  llm_interpretation: str  규칙 기반 상황 해석 텍스트

행동 (action): 0~7 (N, NE, E, SE, S, SW, W, NW)
보상: 목표 도달(+100), 충돌(-50), 거리 기반 shaping
```

## 센서 시뮬레이션 방식

| 센서 | 방식 | 출력 |
|------|------|------|
| 카메라 | 상대 각도/거리로 장애물·목표를 64x64 이미지에 렌더링 | `(3,64,64)` |
| 거리 | 8방향 레이캐스팅 (레이-원 교차 + 벽 거리) | `(8,)` |
| 오디오 | 가장 가까운 장애물 방향·거리 → 좌우 채널 배분 | `(2,50)` |
| IMU | 이동 속도·가속도·방향을 직접 계산 + 노이즈 | `(6,)` |
| LLM | 센서 데이터 기반 규칙 → 한국어 상황 해석 텍스트 | `str` |

## 환경 설정 (JSON)

`configs/` 폴더에 3가지 난이도 설정:
- `env_simple.json`: 장애물 1개 (기본 테스트용)
- `env_medium.json`: 장애물 3개 (표준)
- `env_complex.json`: 장애물 8개 + 넓은 맵

## 파이프라인 연결
```
[M1] SensorManager         → 5개 센서 클래스 (독립 시뮬레이터)
[M2] CameraEncoder          → CNN으로 이미지 → 16차원 벡터
[M3] ObstacleAvoidanceEnv   → Gymnasium 환경 + 5개 센서 통합 ← 현재
[M4] ...                    → Encoder가 환경의 센서 데이터를 처리
```

## 산출물
- [x] Gymnasium 기초 노트북
- [x] 커스텀 장애물 회피 환경
- [x] 5개 센서 통합 시뮬레이션
- [x] JSON 환경 설정 파일 3종
- [x] 규칙 기반 에이전트 + 성능 비교
