# M1 - Python 기초 + 개발환경 세팅

**모듈**: M1 (WorldModel-PhysicalAI 학습 로드맵)
**난이도**: ⭐
**소요 시간**: ~4시간

---

## 이 모듈에서 배우는 것

1. Python 가상환경 생성 + PyTorch/NumPy 설치
2. NumPy로 5개 센서 데이터(카메라, 거리, 오디오, IMU, LLM) 생성 및 시각화
3. Python 클래스로 센서 시뮬레이터 설계 (객체지향 프로그래밍)

이 모듈의 센서 시뮬레이터는 전체 파이프라인의 **첫 번째 블록**입니다:

```
SensorManager.read_all()  ← 여기!
    │
    ├── camera   (64,64,3)
    ├── distance (8,)
    ├── audio    (2,100)
    ├── imu      (6,)
    └── llm      {text}
          │
    M5: Encoders → M6: Fusion → M7: V-JEPA → M8: Flow Matching → Action
```

---

## 실습 환경

| 항목 | 버전 |
|------|------|
| Python | 3.14.3 |
| PyTorch | 2.10.0 (MPS 지원) |
| NumPy | 2.4.3 |
| Matplotlib | 3.10.8 |
| OS | macOS (Apple Silicon) |

### 환경 세팅

```bash
# 가상환경 생성
python3 -m venv worldmodel-env
source worldmodel-env/bin/activate

# 패키지 설치
pip install torch numpy matplotlib jupyter
```

---

## 노트북 구성

### 01_environment_setup.ipynb
- PyTorch, NumPy, Matplotlib 버전 확인
- MPS (Mac GPU) 사용 가능 여부 확인

### 02_numpy_sensor_data.ipynb
- 5개 센서 데이터를 NumPy로 직접 생성 (하드코딩)
- 각 센서별 개별 시각화 + 통합 대시보드
- **시나리오**: 정면에 빨간 장애물, 오른쪽에서 소리

### 03_sensor_simulator.ipynb
- `Sensor` 기본 클래스 + 5개 하위 클래스 구현
- `SensorManager`로 통합 관리
- 3가지 시나리오별 시뮬레이션 및 대시보드 생성
- 파이프라인 연결 미리보기

---

## 센서 클래스 구조

```python
Sensor (기본 클래스)
├── CameraSensor     → read(obstacle_distance) → (64,64,3)
├── DistanceSensor   → read(obstacle_direction, obstacle_distance) → (8,)
├── AudioSensor      → read(sound_direction, sound_intensity) → (2,100)
├── IMUSensor        → read(speed, turn_rate) → (6,)
└── LLMSensor        → read(sensor_summary, user_command) → {text}

SensorManager
└── read_all(scenario) → {camera, distance, audio, imu, llm}
└── dashboard(data)    → 통합 시각화
```

---

## 산출물

```
01-PythonSetup/
├── README.md                              ← 이 파일
├── examples/
│   ├── 01_environment_setup.ipynb         환경 확인
│   ├── 02_numpy_sensor_data.ipynb         5개 센서 데이터 + 시각화
│   ├── 03_sensor_simulator.ipynb          센서 클래스 + SensorManager
│   ├── sensor_dashboard.png               실습 2 대시보드
│   └── sensor_dashboard_scenario3.png     실습 3 시나리오 3 대시보드
└── (worldmodel-env/)                      가상환경 (gitignore)
```

---

## 핵심 인사이트

- 모든 센서 데이터는 결국 **숫자 배열**로 표현됨 (LLM 텍스트 제외 → M5에서 벡터화)
- 시나리오 파라미터만 바꾸면 다양한 상황을 시뮬레이션 가능
- `SensorManager.read_all()` 반환값이 나중에 5개 Encoder의 입력이 됨
- MPS(Apple Silicon GPU) 사용 가능 → M2부터 GPU 가속 학습 활용
