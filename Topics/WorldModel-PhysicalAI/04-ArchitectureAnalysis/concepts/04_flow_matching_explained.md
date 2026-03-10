# Flow Matching 해설

## 1. 핵심 아이디어

### 한 문장 요약
> "노이즈(랜덤)에서 목표 데이터로 직선 경로를 따라가는 생성 방식. 확산 모델보다 단순하고 빠르다."

### 직관적 비유

**확산 모델 (Diffusion)**:
- 깨끗한 그림에 먼지를 계속 뿌림 (1000단계) → 완전 노이즈
- 역방향: 먼지를 조금씩 제거 (1000단계) → 그림 복원
- 느림, 많은 단계 필요

**Flow Matching**:
- 노이즈 점에서 목표 점으로 **직선 경로**를 그림
- 네트워크가 "지금 위치에서 어느 방향으로 가야 하는지" 학습
- 빠름, 적은 단계로 충분

```
확산 모델:    노이즈 ~~~곡선곡선곡선~~~ → 목표    (1000 steps)
Flow Matching: 노이즈 ──────직선──────→ 목표    (10-50 steps)
```

## 2. 수학적 이해 (직관 수준)

### 기본 설정
- x₀: 노이즈 (가우시안 랜덤)
- x₁: 목표 데이터 (실제 행동)
- t: 시간 (0 → 1)
- xₜ: 시간 t에서의 중간 상태

### 선형 보간 (Optimal Transport Path)
```
xₜ = (1 - t) * x₀ + t * x₁
```
시간 t=0이면 순수 노이즈, t=1이면 목표 데이터.
중간에는 노이즈와 목표의 혼합.

### 속도장 (Velocity Field)
```
vₜ = dx/dt = x₁ - x₀
```
"지금 위치에서 목표로 가려면 어느 방향으로 얼마나 가야 하는가"

### 학습 목표
신경망 vθ(xₜ, t)가 진짜 속도장 vₜ를 근사하도록 학습:
```python
loss = MSE(v_theta(x_t, t), x1 - x0)
```

## 3. [π0](03_pi0_architecture_analysis.md)에서의 Flow Matching

π0는 **행동 시퀀스 생성**에 Flow Matching을 사용합니다.

```
학습 시:
  실제 행동 시퀀스 a₁ = [move_right, move_up, ...]  (목표)
  노이즈 a₀ = random                                (시작)
  중간 aₜ = (1-t)*a₀ + t*a₁                         (보간)
  네트워크가 vθ(aₜ, t, context) = a₁ - a₀ 학습       (속도)

추론 시:
  a₀ = random noise  (시작)
  for t in [0, 0.1, 0.2, ..., 1.0]:
      v = v_theta(aₜ, t, context)  (속도 예측)
      aₜ₊₁ = aₜ + v * dt           (한 걸음 이동)
  최종 a₁ ≈ 부드러운 행동 시퀀스
```

### 왜 행동 생성에 좋은가?

1. **부드러운 행동**: 직선 경로 → 급격한 변화 없는 연속적 행동
2. **멀티모달**: 같은 상황에서 여러 가능한 행동 생성 (왼쪽 회피 or 오른쪽 회피)
3. **빠른 추론**: 10-50 스텝이면 충분 (확산 모델 대비 20-100배 빠름)
4. **조건부 생성**: context(센서 상태)에 따라 다른 행동 생성

## 4. Flow Matching vs Diffusion vs GAN

| | GAN | Diffusion | Flow Matching |
|---|---|---|---|
| 학습 안정성 | 불안정 (mode collapse) | 안정 | **안정** |
| 생성 품질 | 좋음 | 매우 좋음 | **매우 좋음** |
| 추론 속도 | 빠름 (1 step) | 느림 (1000 steps) | **중간 (10-50 steps)** |
| 다양성 | 제한적 | 높음 | **높음** |
| 이론 | 적대적 학습 | 마르코프 체인 | **최적 수송** |
| 행동 생성 적합성 | 낮음 | 보통 | **높음** |

## 5. 우리 프로젝트에서의 활용

### M8에서 구현할 Flow Matching
```
[V-JEPA](02_v_jepa_explained.md) 예측 (미래 상태 잠재 벡터, 64차원)
  + 현재 상태 (fused 잠재 벡터, 64차원)
  → Flow Matching Network
  → 행동 시퀀스 [a1, a2, a3, a4] (4스텝 미래 행동)
```

### 간소화 포인트
| 원본 (π0) | 우리 버전 |
|---|---|
| 50Hz, 수백 차원 | 4스텝, 8방향(Discrete) 또는 2차원(Continuous) |
| 대규모 Transformer | 작은 MLP (3-4 layer) |
| 수만 시간 학습 | M3 환경에서 수천 에피소드 |

## 6. 코드 수준 이해 (의사코드)

```python
# 학습
def train_step(real_actions, context):
    t = torch.rand(batch_size)                    # 랜덤 시간
    noise = torch.randn_like(real_actions)         # 노이즈
    x_t = (1 - t) * noise + t * real_actions       # 보간
    target_v = real_actions - noise                 # 진짜 속도
    pred_v = model(x_t, t, context)                # 예측 속도
    loss = MSE(pred_v, target_v)
    return loss

# 추론
def generate_action(context, n_steps=20):
    action = torch.randn(action_dim)               # 노이즈에서 시작
    dt = 1.0 / n_steps
    for i in range(n_steps):
        t = i / n_steps
        v = model(action, t, context)               # 속도 예측
        action = action + v * dt                     # 한 걸음
    return action                                    # 부드러운 행동
```

## 7. 한 줄 요약

> "Flow Matching은 노이즈에서 목표로의 직선 경로를 학습하여 부드러운 행동 시퀀스를 빠르게 생성하며, π0의 로봇 제어 핵심이자 우리 프로젝트 M8의 행동 생성 기법이다."

**관련 문서**: [π0 아키텍처](03_pi0_architecture_analysis.md) | [V-JEPA](02_v_jepa_explained.md) | [왜 Triple Integration인가](08_why_triple_integration.md)

> ← [03_pi0_architecture_analysis.md](03_pi0_architecture_analysis.md) | **다음 문서**: [05_llm_as_sensor.md](05_llm_as_sensor.md) →

---
*참조: Lipman et al., "Flow Matching for Generative Modeling" (2022), π0 (2024)*
