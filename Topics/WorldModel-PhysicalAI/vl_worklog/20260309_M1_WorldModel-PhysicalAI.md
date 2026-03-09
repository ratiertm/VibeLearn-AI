# WorkLog: M1 - Python 기초 + 개발환경 세팅

**날짜**: 2026-03-09
**모듈**: M1 - Python 기초 + 개발환경 세팅
**사용 시간**: 약 4시간 (Day 1: 2h + Day 2: 2h)

---

## 오늘의 학습 목표

- [x] Python 가상환경 생성 + 패키지 설치
- [x] `import torch` 성공
- [x] NumPy로 가상 센서 데이터 생성 + 시각화

---

## 진행 내용

### 1. 개념 학습 (30분)
- [x] Python 가상환경 이해 (프로젝트별 독립 도구 상자)
- [x] NumPy/텐서 개념 (센서 데이터 = 숫자 배열)
- [x] 전체 파이프라인 구조: 5센서 → Encoder → Fusion → V-JEPA → Flow Matching

### 2. 실습 1: 개발환경 세팅 (45분)
- [x] Python 3.14.3 확인
- [x] 가상환경 생성 (`worldmodel-env/`)
- [x] PyTorch 2.10.0, NumPy 2.4.3, Matplotlib 3.10.8, Jupyter 설치
- [x] import 테스트 + MPS (Mac GPU) 확인

### 3. 실습 2: NumPy 센서 데이터 (45분)
- [x] 카메라 이미지 (64x64x3 RGB) 생성 + 시각화
- [x] 거리센서 (8방향) 레이더 차트 시각화
- [x] 오디오 (스테레오 2채널) 시각화
- [x] IMU (6개 값) 바 차트 시각화
- [x] LLM 센서 (텍스트: 상황 해석 + 명령) 표현
- [x] 5개 센서 통합 대시보드 생성 (`sensor_dashboard.png`)

### 4. 한글 폰트 수정 (Day 2)
- [x] Matplotlib 한글 폰트 깨짐 해결 (AppleGothic 설정)
- [x] 이모지 → 텍스트 태그 변환 ([CAM], [DIST], [MIC], [IMU], [LLM])
- [x] 대시보드 PNG 재생성 (한글 정상 출력)

### 5. 실습 3: 센서 시뮬레이터 클래스 (Day 2, 90분)
- [x] `Sensor` 기본 클래스 (name, read(), info())
- [x] `CameraSensor` - 장애물 거리에 따른 이미지 + 노이즈
- [x] `DistanceSensor` - 방향/거리 파라미터화
- [x] `AudioSensor` - 방향별 스테레오 신호
- [x] `IMUSensor` - 속도/회전 파라미터화
- [x] `LLMSensor` - 양방향 (센서 요약 → 해석, 사용자 → 명령)
- [x] `SensorManager` - 5개 센서 통합 관리 (read_all + dashboard)
- [x] 3가지 시나리오 테스트 (정면 장애물 / 옆면 회피 / 안전 직진)
- [x] 파이프라인 연결 미리보기 (M5~M8 흐름)

### 6. README.md 작성 (Day 2, 15분)
- [x] 모듈 개요, 환경 세팅, 노트북 설명, 클래스 구조, 산출물 목록

---

## 문제 해결 로그

- Day 1: 별다른 문제 없이 진행됨
- Python 3.14는 매우 최신 버전이지만 PyTorch 2.10.0이 정상 설치됨
- MPS (Apple Silicon GPU) 사용 가능 확인 → 나중에 학습 시 활용 가능
- **Day 2**: Matplotlib 한글 폰트 깨짐 → `mpl.rcParams['font.family'] = 'AppleGothic'`으로 해결
- **Day 2**: 이모지가 폰트 미지원으로 □ 표시 → 텍스트 태그로 대체

---

## DoD 체크리스트 (M1 전체)

- [x] Python 가상환경 생성 및 패키지 설치 완료
- [x] PyTorch import 성공
- [x] NumPy로 센서 데이터 생성 및 시각화
- [x] 5개 센서 시뮬레이터 클래스 구현
- [x] Jupyter Notebook 3개 작성
- [x] README.md 작성
- [x] WorkLog 작성 완료

**진행률**: M1 DoD 100% 달성!

---

## Daily Retrospective (Day 1)

### What went well?
- 환경 세팅이 문제 없이 한번에 완료됨
- 5개 센서 데이터를 NumPy로 직접 만들어보니 데이터 구조가 직관적으로 이해됨
- 센서 대시보드로 전체 시스템의 입력을 한눈에 볼 수 있게 됨

### What could be improved?
- LLM 센서는 아직 텍스트만 표현, 벡터 변환은 M5에서
- 센서 데이터가 하드코딩됨 → Day 2에서 클래스로 구조화

### Insights
- 모든 센서 데이터는 결국 "숫자 배열"로 표현됨 (LLM 텍스트 제외)
- MPS 사용 가능 → macOS에서도 GPU 가속 학습 가능
- 5개 센서를 딕셔너리로 묶는 패턴이 나중에 Gymnasium observation 구조와 연결

---

## Daily Retrospective (Day 2)

### What went well?
- 클래스 구조로 전환하니 시나리오 파라미터만 바꿔서 다양한 상황 생성 가능
- LLMSensor가 다른 센서 요약을 받아 자연어 해석을 생성하는 구조가 잘 동작
- Matplotlib 한글 폰트 문제를 AppleGothic으로 깔끔하게 해결

### What could be improved?
- LLMSensor의 해석이 템플릿 기반 → 실제 LLM API 연동은 M3에서
- CameraSensor 이미지가 단순함 → M3에서 시뮬레이션 환경 연동 시 개선

### Insights
- 클래스 상속 패턴 (Sensor → 하위 클래스)이 각 센서의 공통점/차이점을 명확하게 분리
- SensorManager.read_all()이 반환하는 dict 구조가 그대로 Encoder 입력이 됨
- 시나리오 dict 하나로 전체 센서 상태를 제어 → 나중에 Gymnasium 환경과 연결 용이

### Next module
- **M2**: 딥러닝 기초 (PyTorch + Flow Matching 개념)

---

## 산출물

```
01-PythonSetup/
├── README.md                              ✅ 모듈 문서
├── examples/
│   ├── 01_environment_setup.ipynb         ✅ 환경 확인 노트북
│   ├── 02_numpy_sensor_data.ipynb         ✅ 5개 센서 데이터 + 시각화
│   ├── 03_sensor_simulator.ipynb          ✅ 센서 클래스 + SensorManager
│   ├── sensor_dashboard.png               ✅ 실습 2 대시보드 (한글 수정)
│   └── sensor_dashboard_scenario3.png     ✅ 실습 3 시나리오 3 대시보드
└── (worldmodel-env/)                      가상환경 (gitignore)
```

**설치된 환경**:
- Python 3.14.3
- PyTorch 2.10.0 (MPS 지원)
- NumPy 2.4.3
- Matplotlib 3.10.8
- Jupyter Notebook
