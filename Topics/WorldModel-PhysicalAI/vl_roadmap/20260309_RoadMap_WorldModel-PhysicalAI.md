# WorldModel-PhysicalAI 학습 로드맵

**생성일**: 2026-03-09
**방법론**: VibeLearn AI
**버전**: 3.0 (LLM + V-JEPA + π0 Triple Integration)

---

## 학습 개요

### Topic 소개
LLM + V-JEPA + π0(Physical Intelligence)를 통합한 Triple Integration 아키텍처 구현.
5개 센서(카메라, 거리, 오디오, IMU, LLM)로 환경을 인식하고, V-JEPA 기반 World Model로
잠재공간에서 미래를 예측한 뒤, π0 스타일 Flow Matching으로 부드러운 행동을 생성하여
장애물을 회피하는 "설명 가능한" 에이전트 구축.

**핵심 방향**: LLM(양방향 언어 센서) + V-JEPA(시간적 미래 예측) + π0(Flow Matching 행동 생성) = 설명 가능한 Physical AI

### 학습 목표
- [ ] LLM, V-JEPA, π0(VLA)의 아키텍처를 각각 비교 설명할 수 있다
- [ ] Physical AI가 무엇이고 왜 필요한지 사례와 함께 설명할 수 있다
- [ ] VLM + LLM 기반 Encoder로 5개 센서 데이터를 잠재공간으로 인코딩하고 융합할 수 있다
- [ ] V-JEPA 기반 World Model로 잠재공간에서 미래 상태를 예측할 수 있다
- [ ] π0 스타일 Flow Matching으로 부드러운 행동 시퀀스를 생성할 수 있다
- [ ] LLM 해석 + World Model 예측으로 "왜 이 행동인지" 설명 가능한 장애물 회피 시스템을 만들 수 있다

### 예상 학습 기간
- **총 기간**: 10주
- **주당 시간**: 10시간
- **총 시간**: 100시간

### 학습 환경
- **OS**: macOS
- **도구**: Python 3.x, PyTorch, Jupyter Notebook, VS Code / Claude Code
- **시뮬레이션**: Gymnasium (OpenAI Gym 후속), PyBullet / MuJoCo
- **사전 지식**: Python 초보, 딥러닝 아주 조금 경험

### 핵심 파이프라인 (최종 구현 목표)
```
카메라 ────→ VLM Encoder (π0 스타일) ──┐
거리센서 ──→ Distance Encoder ─────────┤
마이크 ────→ Audio Encoder ────────────┤                                      ┌→ 설명: LLM "앞에 사람이 있어서 회피"
IMU ───────→ Motion Encoder ───────────┼→ 5-Modal → V-JEPA World Model ─────┤      + World Model "3스텝 후 충돌"
LLM ───────→ Language Encoder ─────────┘  Fusion    (시간적 미래 예측)       └→ π0 Flow Matching → 행동 시퀀스
  ↑ 상황해석: "앞에 사람이 있어"                                                   (부드러운 연속 제어)
  ↑ 명령수신: "오른쪽으로 가"
```

**Triple Integration 핵심**:
- **LLM**: 양방향 언어 센서 (상황 의미 해석 + 자연어 명령 수신)
- **V-JEPA**: 잠재공간에서 시간적 미래 예측 (픽셀 생성 없이 구조만 포착)
- **π0**: VLM Encoder + Flow Matching으로 부드러운 행동 시퀀스 생성

---

## 전체 로드맵 구조

| 모듈 | 모듈명 | 난이도 | 예상 시간 | 산출물 폴더 | v3.0 변경 |
|------|--------|--------|----------|------------|-----------|
| M1 | Python 기초 + 개발환경 세팅 | ⭐ | 10h | 01-PythonSetup/ | - |
| M2 | 딥러닝 기초 (PyTorch + Flow Matching 개념) | ⭐ | 10h | 02-DeepLearningBasics/ | - |
| M3 | 시뮬레이션 환경 + 센서 시뮬레이션 + LLM 연동 | ⭐⭐ | 10h | 03-SimEnvironment/ | 🔄 LLM 언어 인터페이스 추가 |
| M4 | LLM + V-JEPA + π0 아키텍처 분석 | ⭐⭐ | 10h | 04-ArchitectureAnalysis/ | 🔄 V-JEPA/LLM 분석 추가 |
| M5 | 5개 센서 Encoder (VLM + LLM + 3종) | ⭐⭐ | 10h | 05-SensorEncoders/ | 🔄 Language Encoder 추가 |
| M6 | 5-Modal Sensor Fusion | ⭐⭐⭐ | 10h | 06-SensorFusion/ | 🔄 5모달 융합 |
| M7 | V-JEPA 기반 Latent World Model | ⭐⭐⭐ | 10h | 07-LatentWorldModel/ | 🔄 V-JEPA 시간적 예측 |
| M8 | π0 Flow Matching 행동 생성 | ⭐⭐⭐ | 10h | 08-FlowController/ | - |
| M9 | 다양한 환경 실험 + LLM 명령 테스트 | ⭐⭐⭐ | 10h | 09-Experiments/ | 🔄 LLM 명령 실험 추가 |
| M10 | Capstone: LLM+V-JEPA+π0 Triple Integration | ⭐⭐⭐ | 10h | 10-Capstone/ | 🔄 Triple 통합 |

**총 예상 시간**: 100시간

---

## 모듈별 상세 계획

---

### M1 - Python 기초 + 개발환경 세팅

**난이도**: ⭐
**예상 시간**: 10h
**산출물 폴더**: `01-PythonSetup/`

#### 학습 목표
- [ ] Python 기본 문법(변수, 함수, 클래스, 리스트, 딕셔너리)을 사용할 수 있다
- [ ] NumPy로 배열/행렬 연산을 수행할 수 있다
- [ ] Matplotlib으로 데이터를 시각화할 수 있다
- [ ] 가상환경을 만들고 패키지를 설치할 수 있다
- [ ] Jupyter Notebook에서 코드를 실행하고 결과를 확인할 수 있다

#### 주요 개념
1. **Python 가상환경**: 프로젝트별 독립된 패키지 관리 공간. 프로젝트마다 다른 버전의 라이브러리를 쓸 수 있게 해줌
2. **NumPy 배열**: 수학적 연산에 최적화된 다차원 배열. 딥러닝의 텐서와 직결됨
3. **Matplotlib**: 데이터를 그래프로 시각화. 센서 데이터 분석에 필수
4. **Jupyter Notebook**: 코드 + 결과 + 설명을 한 곳에서 관리. 실험 기록에 최적

#### 실습 과제

**실습 1: 개발환경 세팅** ⭐
- **목적**: 전체 학습에 사용할 환경 구축
- **단계**:
  1. Python 3.11+ 설치 확인 (`python3 --version`)
  2. 가상환경 생성 (`python3 -m venv worldmodel-env`)
  3. PyTorch, NumPy, Matplotlib, Jupyter 설치
  4. Jupyter Notebook에서 "Hello World Model" 실행
- **예상 시간**: 90분
- **검증**: `import torch; print(torch.__version__)` 성공

**실습 2: NumPy로 센서 데이터 다루기** ⭐
- **목적**: 센서 데이터를 배열로 표현하고 조작하는 연습
- **단계**:
  1. 가상 카메라 이미지 (64x64 배열) 생성
  2. 가상 거리센서 데이터 (1D 배열) 생성
  3. 데이터 정규화, 리사이즈, 결합 연습
  4. Matplotlib으로 시각화
- **예상 시간**: 120분
- **검증**: 4개 센서 데이터를 생성하고 시각화한 그래프 출력

**실습 3: Python 클래스로 센서 시뮬레이터 만들기** ⭐⭐
- **목적**: 객체지향 프로그래밍으로 센서를 모델링
- **단계**:
  1. `Sensor` 기본 클래스 작성
  2. `CameraSensor`, `DistanceSensor`, `AudioSensor`, `IMUSensor` 하위 클래스
  3. 각 센서의 `read()` 메서드로 가짜 데이터 반환
  4. `SensorManager` 클래스로 4개 센서를 통합 관리
- **예상 시간**: 150분
- **검증**: `SensorManager.read_all()` 호출 시 4개 센서 데이터 딕셔너리 반환

#### 산출물
```
01-PythonSetup/
├── README.md
├── concepts/
│   └── python_basics_for_ai.md
├── examples/
│   ├── 01_environment_setup.ipynb
│   ├── 02_numpy_sensor_data.ipynb
│   └── 03_sensor_simulator.py
└── guides/
    └── setup_guide_macos.md
```

#### Definition of Done
- [ ] Python 가상환경 생성 및 패키지 설치 완료
- [ ] PyTorch import 성공
- [ ] NumPy로 센서 데이터 생성 및 시각화
- [ ] 4개 센서 시뮬레이터 클래스 구현
- [ ] Jupyter Notebook 3개 작성
- [ ] README.md 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] Python 가상환경이 왜 필요한지 설명할 수 있다
- [ ] NumPy 배열과 Python 리스트의 차이를 설명할 수 있다

**실무 활용**:
- [ ] AI에게 "이런 센서 데이터를 NumPy로 처리해줘"라고 구체적으로 요청할 수 있다
- [ ] 에러 발생 시 어떤 정보를 AI에게 제공해야 하는지 안다

**문제 해결**:
- [ ] 패키지 설치 에러 시 에러 메시지를 읽고 방향을 잡을 수 있다

#### 예상 시간 배분
- 개념 학습: 120분 (20%)
- 실습 1 (환경세팅): 90분
- 실습 2 (NumPy): 120분
- 실습 3 (센서 클래스): 150분
- 문서화: 60분
- 버퍼: 60분
- **합계**: 10h

#### 참조 자료
- Python 공식 튜토리얼: https://docs.python.org/3/tutorial/
- NumPy 퀵스타트: https://numpy.org/doc/stable/user/quickstart.html
- PyTorch 설치: https://pytorch.org/get-started/locally/

---

### M2 - 딥러닝 기초 (PyTorch + Flow Matching 개념)

**난이도**: ⭐
**예상 시간**: 10h
**산출물 폴더**: `02-DeepLearningBasics/`

#### 학습 목표
- [ ] 텐서(Tensor)를 생성하고 연산할 수 있다
- [ ] 신경망의 순전파/역전파 과정을 직관적으로 설명할 수 있다
- [ ] PyTorch로 간단한 신경망을 만들고 학습시킬 수 있다
- [ ] 이미지 데이터를 신경망에 입력하고 분류할 수 있다
- [ ] Flow Matching의 핵심 아이디어를 직관적으로 설명할 수 있다

#### 주요 개념
1. **텐서(Tensor)**: 다차원 배열의 딥러닝 버전. GPU 연산 가능. NumPy 배열과 유사하지만 자동 미분 지원
2. **신경망(Neural Network)**: 입력 → 은닉층 → 출력으로 데이터를 변환하는 함수의 연쇄. "학습"은 가중치를 조정하는 과정
3. **손실함수(Loss)**: 예측과 정답의 차이를 숫자로 표현. 이 값을 줄이는 것이 학습의 목표
4. **역전파(Backpropagation)**: 손실을 줄이기 위해 각 가중치를 얼마나 조정해야 하는지 계산하는 알고리즘
5. **CNN (Convolutional Neural Network)**: 이미지에서 특징을 추출하는 신경망. 카메라 센서 처리에 핵심
6. **Flow Matching (개념)**: 노이즈에서 목표 데이터로 "흐름(flow)"을 따라가는 생성 방식. Diffusion보다 효율적. π0가 행동 생성에 사용하는 핵심 기법. M8에서 본격 구현

#### 실습 과제

**실습 1: PyTorch 텐서 연산** ⭐
- **목적**: 딥러닝의 기본 데이터 단위인 텐서 조작 익히기
- **단계**:
  1. 텐서 생성 (from NumPy, random, zeros/ones)
  2. 텐서 연산 (덧셈, 행렬곱, reshape)
  3. GPU 텐서 vs CPU 텐서 확인
  4. autograd로 자동 미분 체험
- **예상 시간**: 90분
- **검증**: 간단한 함수의 기울기(gradient)를 자동으로 계산하여 출력

**실습 2: 첫 번째 신경망 - 숫자 분류** ⭐
- **목적**: 신경망 학습의 전체 과정(데이터→모델→학습→평가) 체험
- **단계**:
  1. MNIST 데이터셋 로드
  2. 간단한 CNN 모델 정의 (Conv → ReLU → Pool → FC)
  3. 학습 루프 작성 (forward → loss → backward → update)
  4. 정확도 평가 및 예측 결과 시각화
- **예상 시간**: 150분
- **검증**: MNIST 테스트 정확도 95% 이상

**실습 3: 센서 이미지 특징 추출기** ⭐⭐
- **목적**: 카메라 센서 데이터에서 특징을 추출하는 Encoder 맛보기
- **단계**:
  1. 간단한 환경 이미지 (장애물 있는 격자) 생성
  2. CNN으로 이미지 → 저차원 벡터 (특징) 추출
  3. 비슷한 이미지는 비슷한 벡터가 되는지 확인
  4. 특징 벡터를 2D로 시각화 (t-SNE)
- **예상 시간**: 150분
- **검증**: 장애물 위치가 다른 이미지들의 특징 벡터가 클러스터를 형성

#### 산출물
```
02-DeepLearningBasics/
├── README.md
├── concepts/
│   ├── neural_network_intuition.md
│   └── cnn_for_sensors.md
├── examples/
│   ├── 01_tensor_operations.ipynb
│   ├── 02_mnist_classifier.ipynb
│   └── 03_image_feature_extractor.ipynb
└── guides/
    └── pytorch_cheatsheet.md
```

#### Definition of Done
- [ ] PyTorch 텐서 연산 및 autograd 실행
- [ ] MNIST CNN 분류기 학습 완료 (95%+ 정확도)
- [ ] 센서 이미지 특징 추출기 구현
- [ ] 각 개념에 대한 직관적 설명 문서 작성
- [ ] Notebook 3개 완성
- [ ] README.md 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] 신경망이 "학습한다"는 것이 구체적으로 무엇을 의미하는지 설명할 수 있다
- [ ] CNN이 이미지에서 어떻게 특징을 추출하는지 직관적으로 설명할 수 있다

**실무 활용**:
- [ ] AI에게 "이런 구조의 신경망을 만들어줘"라고 구체적으로 요청할 수 있다
- [ ] 학습이 안 될 때 (loss가 안 줄어들 때) 확인할 점을 3가지 이상 말할 수 있다

**문제 해결**:
- [ ] 차원 불일치(shape mismatch) 에러를 스스로 해결할 수 있다

#### 예상 시간 배분
- 개념 학습: 120분 (20%)
- 실습 1 (텐서): 90분
- 실습 2 (MNIST): 150분
- 실습 3 (특징 추출): 150분
- 문서화: 60분
- 버퍼: 30분
- **합계**: 10h

#### 참조 자료
- PyTorch 공식 튜토리얼: https://pytorch.org/tutorials/beginner/basics/intro.html
- CNN Explainer (시각화): https://poloclub.github.io/cnn-explainer/

---

### M3 - 시뮬레이션 환경 + 센서 시뮬레이션 + LLM 연동

**난이도**: ⭐⭐
**예상 시간**: 10h
**산출물 폴더**: `03-SimEnvironment/`

#### 학습 목표
- [ ] Gymnasium 환경을 만들고 에이전트를 동작시킬 수 있다
- [ ] 커스텀 환경에 5개 센서(카메라, 거리, 오디오, IMU, LLM)를 시뮬레이션할 수 있다
- [ ] LLM을 통해 환경 상황을 자연어로 해석하고, 자연어 명령을 받을 수 있다
- [ ] 다양한 환경 설정(장애물 배치, 맵 크기)을 파라미터로 변경할 수 있다

#### 주요 개념
1. **Gymnasium**: OpenAI Gym의 후속. 에이전트가 환경에서 행동하고 보상을 받는 표준 인터페이스 (observation, action, reward, done)
2. **커스텀 환경**: 우리만의 장애물 회피 환경을 직접 정의. 맵, 장애물, 에이전트 위치를 설정
3. **센서 시뮬레이션**: 실제 센서 없이 가상 데이터 생성. 카메라=렌더링 이미지, 거리=레이캐스팅, 오디오=거리 기반 소리 세기, IMU=속도/가속도 계산
4. **LLM 언어 인터페이스**: LLM이 센서 데이터를 받아 상황을 자연어로 해석("앞에 장애물, 위험") + 사용자 명령을 행동 힌트로 변환("오른쪽으로 가" → direction_hint)
5. **환경 설정 파라미터화**: 장애물 수, 맵 크기, 소리 위치 등을 설정 파일로 관리 → 다양한 환경에서 학습 가능

#### 실습 과제

**실습 1: Gymnasium 기본 환경 체험** ⭐
- **목적**: 에이전트-환경 상호작용의 기본 루프 이해
- **단계**:
  1. Gymnasium 설치 및 CartPole 환경 실행
  2. observation, action, reward 출력 확인
  3. 랜덤 에이전트로 100 에피소드 실행
  4. 보상 그래프 시각화
- **예상 시간**: 90분
- **검증**: CartPole 환경이 렌더링되고 보상 그래프가 출력됨

**실습 2: 커스텀 장애물 회피 환경 만들기** ⭐⭐
- **목적**: 최종 프로젝트에 사용할 환경의 기반 구축
- **단계**:
  1. 2D 격자 맵 환경 정의 (에이전트, 장애물, 목표)
  2. 행동 공간: 상/하/좌/우 이동
  3. 보상: 목표 도달 +100, 장애물 충돌 -50, 스텝 -1
  4. 환경 설정을 JSON/YAML로 파라미터화
  5. 다양한 맵 설정 3개 이상 만들기
- **예상 시간**: 150분
- **검증**: 랜덤 에이전트가 3가지 다른 맵에서 동작

**실습 3: 5개 센서 시뮬레이션 통합 (LLM 포함)** ⭐⭐
- **목적**: 환경에서 5개 센서 데이터를 생성하는 시스템 구축 (LLM 양방향 포함)
- **단계**:
  1. 카메라: 에이전트 시점 64x64 RGB 이미지 렌더링
  2. 거리센서: 8방향 레이캐스팅으로 장애물까지 거리 측정
  3. 오디오: 장애물/목표까지 거리에 따른 소리 세기 + 방향 (2채널 스테레오)
  4. IMU: 에이전트의 속도, 가속도, 회전 상태
  5. LLM 센서 (양방향):
     - **상황 해석**: 센서 요약 데이터를 소형 LLM에 전달 → "앞 2m에 장애물, 오른쪽 열림" 텍스트 생성
     - **명령 수신**: "오른쪽으로 가" 같은 자연어 입력 → direction_hint 벡터로 변환
     - 시뮬레이션에서는 규칙 기반 텍스트 생성 + 소형 LLM (예: TinyLlama) 두 가지 모드
  6. 한 스텝에서 5개 센서 데이터를 동시에 반환
- **예상 시간**: 180분
- **검증**: `env.step(action)` 시 5개 센서 데이터 포함 observation + LLM 상황 설명 텍스트 반환

#### 산출물
```
03-SimEnvironment/
├── README.md
├── concepts/
│   ├── gymnasium_architecture.md
│   ├── sensor_simulation_design.md
│   └── llm_as_sensor.md
├── examples/
│   ├── 01_gymnasium_basics.ipynb
│   ├── 02_custom_obstacle_env.py
│   ├── 03_sensor_integration.py
│   ├── 04_llm_language_interface.py
│   └── configs/
│       ├── env_simple.yaml
│       ├── env_medium.yaml
│       └── env_complex.yaml
└── guides/
    └── custom_env_howto.md
```

#### Definition of Done
- [ ] Gymnasium 기본 환경 실행 성공
- [ ] 커스텀 장애물 회피 환경 구현
- [ ] 5개 센서 시뮬레이션 통합 완료 (LLM 양방향 포함)
- [ ] LLM 상황 해석 + 명령 수신 인터페이스 동작
- [ ] 3가지 이상의 환경 설정 파일 생성
- [ ] 센서 데이터 시각화 대시보드 구현
- [ ] README.md 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] Gymnasium의 observation-action-reward 루프를 그림으로 설명할 수 있다
- [ ] 각 센서(LLM 포함)가 물리 세계의 어떤 정보를 캡처하는지 설명할 수 있다
- [ ] LLM을 "센서"로 사용한다는 것이 무엇을 의미하는지 설명할 수 있다

**실무 활용**:
- [ ] AI에게 "이런 환경을 만들어줘"라고 구체적 스펙을 제시할 수 있다
- [ ] 센서 파라미터(해상도, 범위 등)를 상황에 맞게 조정할 수 있다

**문제 해결**:
- [ ] 환경이 예상대로 동작하지 않을 때 디버깅 방법을 알고 있다

#### 예상 시간 배분
- 개념 학습: 90분 (15%)
- 실습 1 (Gymnasium): 90분
- 실습 2 (커스텀 환경): 150분
- 실습 3 (센서 통합): 180분
- 문서화: 60분
- 버퍼: 30분
- **합계**: 10h

#### 참조 자료
- Gymnasium 문서: https://gymnasium.farama.org/
- 커스텀 환경 만들기: https://gymnasium.farama.org/tutorials/gymnasium_basics/environment_creation/

---

### M4 - LLM + V-JEPA + π0 아키텍처 분석

**난이도**: ⭐⭐
**예상 시간**: 10h
**산출물 폴더**: `04-ArchitectureAnalysis/`

#### 학습 목표
- [ ] LLM, V-JEPA, π0(VLA) 세 가지 아키텍처의 핵심을 각각 설명할 수 있다
- [ ] 세 아키텍처를 통합하는 Triple Integration의 설계 근거를 제시할 수 있다
- [ ] V-JEPA의 시간적 예측 메커니즘을 JEPA/MuZero와 비교할 수 있다
- [ ] LLM이 Physical AI에서 "센서"로 기능하는 방식을 설계할 수 있다

#### 주요 개념
1. **World Model (전통적)**: Ha & Schmidhuber 2018. VAE + RNN + Controller. 한계: 픽셀 복원에 의존
2. **JEPA**: Yann LeCun / Meta FAIR. 픽셀을 생성하지 않고 잠재공간에서 추상적 예측. "구조만 포착"
3. **V-JEPA (Video JEPA)**: JEPA의 비디오 버전. 시간 축에서 마스킹된 미래 프레임을 잠재공간에서 예측. 픽셀 디코딩 없이 시간적 패턴을 학습. 우리 World Model의 핵심 기반
4. **MuZero**: DeepMind. 잠재공간에서 미래 상태 + 보상 + 정책을 동시에 학습. "상상 속 계획"
5. **π0 (Physical Intelligence)**: VLM(PaliGemma) + Action Expert + Flow Matching. 범용 로봇 제어 모델. 행동은 잘하지만 "왜"를 설명하지 못함
6. **LLM as Sensor**: LLM이 센서 데이터를 의미적으로 해석("앞에 어린이") + 자연어 명령을 행동 힌트로 변환("천천히 가"). 기존 4개 센서가 못하는 "의미" 차원 추가
7. **Flow Matching**: 노이즈 → 목표 데이터로 흐름을 따라가는 생성 방식. π0가 부드러운 행동 시퀀스를 생성하는 핵심 기법

#### 실습 과제

**실습 1: LLM + V-JEPA + π0 논문/코드 분석** ⭐
- **목적**: 세 가지 핵심 아키텍처를 분석하고 비교
- **단계**:
  1. V-JEPA 논문 분석: 시간적 마스킹 → 잠재공간 예측 구조
  2. π0 논문 분석: VLM + Action Expert + Flow Matching 구조
  3. LLM의 Physical AI 활용 사례 조사 (SayCan, PaLM-E, RT-2 등)
  4. "V-JEPA vs MuZero vs JEPA" 비교표 + "LLM의 역할" 분석표 작성
- **예상 시간**: 180분
- **검증**: 세 아키텍처 비교 분석 문서 완성

**실습 2: π0 openpi + V-JEPA 코드 탐색** ⭐⭐
- **목적**: 실제 구현을 분석하여 우리 프로젝트에 적용할 요소 파악
- **단계**:
  1. openpi 저장소 구조 분석 (VLM Encoder, Action Expert, Flow Matching)
  2. lucidrains/pi-zero-pytorch (PyTorch 구현) 코드 분석
  3. Meta V-JEPA 코드 구조 분석 (GitHub: facebookresearch/jepa)
  4. 우리가 차용할 부분 vs 우리만의 부분 구분 문서 작성
- **예상 시간**: 150분
- **검증**: π0 + V-JEPA 코드 분석 + 차용 포인트 문서 완성

**실습 3: Triple Integration 아키텍처 설계** ⭐⭐
- **목적**: LLM + V-JEPA + π0 통합 전체 시스템 설계
- **단계**:
  1. 전체 파이프라인: 5 Encoders → 5-Modal Fusion → V-JEPA World Model → π0 Flow Matching
  2. π0에서 차용: VLM Encoder 개념, Flow Matching 행동 생성
  3. V-JEPA에서 차용: 잠재공간 시간적 미래 예측 (마스킹 기반)
  4. LLM 통합: 양방향 Language Encoder 설계 (상황 해석 + 명령 수신)
  5. 설명 가능성 설계: V-JEPA 예측 + LLM 해석 = "왜 이 행동인지" 이중 설명
  6. 각 컴포넌트의 입출력 크기(차원) 결정
  7. 설계 문서 + 아키텍처 다이어그램 작성
- **예상 시간**: 150분
- **검증**: Triple Integration 아키텍처 설계 문서 완성

#### 산출물
```
04-ArchitectureAnalysis/
├── README.md
├── concepts/
│   ├── world_model_history.md
│   ├── v_jepa_explained.md
│   ├── pi0_architecture_analysis.md
│   ├── llm_as_sensor.md
│   ├── vla_explained.md
│   ├── flow_matching_explained.md
│   ├── vjepa_vs_muzero_vs_jepa.md
│   └── why_triple_integration.md
├── examples/
│   ├── 01_paper_analysis.ipynb
│   ├── 02_openpi_code_exploration.ipynb
│   └── 03_vjepa_code_exploration.ipynb
└── guides/
    └── triple_integration_architecture_design.md
```

#### Definition of Done
- [ ] V-JEPA, π0, LLM 각각의 분석 문서 작성
- [ ] π0 openpi + V-JEPA 코드 분석 완료
- [ ] Triple Integration 아키텍처 설계 문서 완성
- [ ] "왜 Triple Integration인가" 설명 문서 작성
- [ ] README.md 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] LLM + V-JEPA + π0 각각의 역할을 비전공자에게 설명할 수 있다
- [ ] "왜 세 가지를 합쳐야 하는가"를 논리적으로 설명할 수 있다
- [ ] V-JEPA가 기존 World Model보다 시간적 예측에서 어떤 장점이 있는지 설명할 수 있다

**실무 활용**:
- [ ] Triple Integration의 각 모듈이 왜 필요한지 근거를 제시할 수 있다
- [ ] openpi, V-JEPA 코드에서 필요한 부분을 찾아 읽을 수 있다

**문제 해결**:
- [ ] 설계 단계에서 잠재적 문제점(차원 불일치, LLM 지연, 학습 불안정)을 사전에 파악할 수 있다

#### 예상 시간 배분
- 개념 학습 (논문/자료): 120분 (20%)
- 실습 1 (논문 분석): 180분
- 실습 2 (코드 탐색): 150분
- 실습 3 (아키텍처 설계): 150분
- 문서화: 30분
- 버퍼: 30분
- **합계**: 10h

#### 참조 자료
- π0 논문: https://arxiv.org/abs/2410.24164
- openpi (공식): https://github.com/Physical-Intelligence/openpi
- pi-zero-pytorch: https://github.com/lucidrains/pi-zero-pytorch
- V-JEPA 논문: https://arxiv.org/abs/2404.08471
- V-JEPA 코드: https://github.com/facebookresearch/jepa
- JEPA: https://openreview.net/pdf?id=BZ5a1r-kVsf
- MuZero: https://arxiv.org/abs/1911.08265
- SayCan (LLM + Robot): https://say-can.github.io/
- PaLM-E: https://palm-e.github.io/
- Flow Matching 해설: https://arxiv.org/abs/2210.02747

---

### M5 - 5개 센서 Encoder (VLM + LLM + 3종)

**난이도**: ⭐⭐
**예상 시간**: 10h
**산출물 폴더**: `05-SensorEncoders/`

#### 학습 목표
- [ ] 사전학습된 VLM을 Vision Encoder로 활용할 수 있다 (π0의 PaliGemma 개념)
- [ ] LLM 출력(텍스트)을 Language Encoder로 잠재 벡터화할 수 있다
- [ ] 거리/오디오/IMU 데이터를 MLP/1D-CNN으로 인코딩할 수 있다
- [ ] 5개 모달리티 각각의 Encoder 설계 근거를 설명할 수 있다

#### 주요 개념
1. **VLM (Vision-Language Model) Encoder**: π0가 PaliGemma를 사용하듯, 사전학습된 모델의 시각 이해 능력을 재활용
2. **Language Encoder (LLM 센서용)**: LLM의 텍스트 출력(상황 해석) + 사용자 명령을 잠재 벡터로 변환. Sentence Transformer 또는 소형 LLM의 임베딩 활용
3. **Transfer Learning**: 거대 모델의 일부를 가져와서 우리 태스크에 맞게 미세조정
4. **잠재 공간(Latent Space)**: 센서 데이터가 압축된 추상적 공간. 비슷한 상황은 가까운 점
5. **JEPA식 Encoder**: 픽셀을 복원하지 않음. 구조와 의미만 포착
6. **5개 모달리티별 Encoder**: 카메라=VLM, LLM=Language Encoder, 거리/오디오/IMU=경량 모델

#### 실습 과제

**실습 1: VLM 기반 Vision Encoder (π0 스타일)** ⭐⭐
- **목적**: 사전학습된 VLM을 활용하여 풍부한 시각 표현 획득
- **단계**:
  1. 소형 사전학습 Vision Model 선택 (예: SigLIP-small 또는 DINOv2-small)
  2. 모델 로드 후 M3 환경 이미지를 입력하여 특징 벡터 추출
  3. 추출된 특징의 품질 확인 (t-SNE 시각화, 클러스터링)
  4. CNN 직접 학습 vs VLM 전이학습 비교 실험
  5. VLM 특징을 우리 잠재공간 크기(32차원)로 projection
- **예상 시간**: 180분
- **검증**: VLM Encoder가 직접 학습 CNN보다 적은 데이터로 더 좋은 표현 생성

**실습 2: Language Encoder (LLM 센서용) + 3종 Encoder** ⭐⭐
- **목적**: LLM 텍스트 출력을 잠재 벡터로 변환 + 나머지 3개 센서 Encoder 구현
- **단계**:
  1. Language Encoder: LLM 텍스트(상황 해석 + 명령) → Sentence Transformer → latent_dim=16
     - 상황 해석 텍스트 ("앞 2m 장애물, 위험") → 의미 벡터
     - 사용자 명령 텍스트 ("오른쪽으로 가") → 방향 힌트 벡터
     - 두 벡터를 concat → Language latent (16차원)
  2. Distance Encoder: 8방향 거리값 → MLP → latent_dim=8
  3. Audio Encoder: 스테레오 소리 데이터 → 1D-CNN → latent_dim=8
  4. IMU Encoder: 속도/가속도/회전 → MLP → latent_dim=8
  5. 각 Encoder의 잠재 표현이 의미 있는지 검증
- **예상 시간**: 180분
- **검증**: Language Encoder가 비슷한 상황 설명은 비슷한 벡터로, 다른 상황은 다른 벡터로 매핑

**실습 3: 5개 Encoder 통합 테스트** ⭐⭐
- **목적**: 5개 Encoder가 M3 환경과 올바르게 연동되는지 확인
- **단계**:
  1. 환경에서 에피소드 실행하며 5개 센서 데이터 수집 (LLM 텍스트 포함)
  2. 각 Encoder에 실시간으로 통과시켜 잠재 벡터 생성
  3. 에피소드 동안 잠재 벡터의 변화 시각화 (시계열 플롯)
  4. 충돌 직전 vs 안전 상태의 잠재 벡터 비교
  5. LLM 명령이 Language Encoder 출력에 어떻게 반영되는지 시각화
- **예상 시간**: 120분
- **검증**: 위험 상황에서 잠재 벡터가 일관되게 변화 + LLM 명령이 벡터에 반영

#### 산출물
```
05-SensorEncoders/
├── README.md
├── concepts/
│   ├── encoder_design_principles.md
│   ├── jepa_encoder_approach.md
│   └── language_encoder_design.md
├── examples/
│   ├── 01_vision_encoder.ipynb
│   ├── 02_language_encoder.ipynb
│   ├── 03_other_sensor_encoders.ipynb
│   ├── 04_encoder_integration_test.ipynb
│   └── models/
│       ├── vision_encoder.py
│       ├── language_encoder.py
│       ├── distance_encoder.py
│       ├── audio_encoder.py
│       └── imu_encoder.py
└── guides/
    └── encoder_architecture_choices.md
```

#### Definition of Done
- [ ] Vision Encoder (카메라 → latent_dim=32) 구현
- [ ] Language Encoder (LLM 텍스트 → latent_dim=16) 구현
- [ ] Distance, Audio, IMU Encoder 각각 구현
- [ ] 5개 Encoder 통합 테스트 통과
- [ ] 잠재공간 시각화 완료
- [ ] 각 Encoder 설계 근거 문서 작성
- [ ] README.md 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] Encoder가 "정보를 버린다"는 것이 왜 중요한지 설명할 수 있다
- [ ] 텍스트를 잠재 벡터로 변환하는 방법(Sentence Transformer)을 설명할 수 있다

**실무 활용**:
- [ ] 새로운 센서가 추가되면 어떤 구조의 Encoder를 써야 하는지 판단할 수 있다

**문제 해결**:
- [ ] Encoder 출력이 의미 없어 보일 때 (다 비슷한 벡터) 원인을 파악할 수 있다

#### 예상 시간 배분
- 개념 학습: 60분 (10%)
- 실습 1 (Vision Encoder): 180분
- 실습 2 (기타 Encoder): 150분
- 실습 3 (통합 테스트): 120분
- 문서화: 60분
- 버퍼: 30분
- **합계**: 10h

#### 참조 자료
- I-JEPA (Meta): https://arxiv.org/abs/2301.08243
- Contrastive Learning 개요: https://lilianweng.github.io/posts/2021-05-31-contrastive/

---

### M6 - 5-Modal Sensor Fusion

**난이도**: ⭐⭐⭐
**예상 시간**: 10h
**산출물 폴더**: `06-SensorFusion/`

#### 학습 목표
- [ ] 멀티모달 데이터 융합(Fusion)의 주요 방법론을 설명할 수 있다
- [ ] 5개 센서(카메라+거리+오디오+IMU+LLM)의 잠재 표현을 하나의 통합 표현으로 합칠 수 있다
- [ ] Cross-attention으로 센서 간 관계를 학습시킬 수 있다 (특히 LLM↔Vision 상호작용)
- [ ] LLM 명령이 Fusion 결과에 어떻게 영향을 미치는지 시각화할 수 있다

#### 주요 개념
1. **Early Fusion vs Late Fusion**: Early = 원시 데이터 결합 후 처리. Late = 각각 처리 후 결과 결합. 우리는 Intermediate Fusion (잠재공간에서 결합)
2. **Cross-Attention**: 한 센서의 정보가 다른 센서 해석에 영향. 예: 소리가 나는 방향을 알면 카메라의 어디를 주목해야 하는지 결정
3. **Joint Embedding**: 4개 센서를 하나의 공유 잠재공간에 매핑. JEPA의 핵심 아이디어와 직결
4. **센서 가중치(Gating)**: 상황에 따라 어떤 센서를 더 신뢰할지 자동 결정. 예: 어두우면 카메라↓ 소리↑

#### 실습 과제

**실습 1: 기본 Fusion 구현 (Concatenation + MLP)** ⭐⭐
- **목적**: 가장 단순한 Fusion 방식부터 시작
- **단계**:
  1. 5개 Encoder 출력 (32+16+8+8+8=72차원) concatenation
  2. MLP로 72차원 → 64차원 통합 잠재 벡터 생성
  3. 통합 벡터로 "현재 상태 분류" (안전/주의/위험) 학습
  4. 단일 센서 vs 통합 센서 정확도 비교
  5. LLM 포함 vs 미포함 성능 차이 비교
- **예상 시간**: 150분
- **검증**: 5모달 통합 > 4모달(LLM 제외) > 단일 센서 정확도 확인

**실습 2: Cross-Attention Fusion (LLM↔Vision 상호작용)** ⭐⭐⭐
- **목적**: 센서 간 관계를 학습하는 고급 Fusion, 특히 LLM과 Vision의 상호작용
- **단계**:
  1. Multi-Head Attention 구현 (또는 PyTorch의 nn.MultiheadAttention 사용)
  2. 5개 센서 잠재 벡터를 Query/Key/Value로 교차 주의
  3. Attention 가중치 시각화: LLM이 어떤 센서에 주목하는지, Vision이 LLM 명령에 어떻게 반응하는지
  4. LLM 명령("왼쪽으로 가")이 다른 센서의 Attention에 미치는 영향 분석
  5. MLP Fusion과 성능 비교
- **예상 시간**: 180분
- **검증**: LLM 명령 변경 시 Attention 패턴이 변하는 것 확인

**실습 3: 센서 드롭아웃 + LLM 기여도 실험** ⭐⭐⭐
- **목적**: Fusion 모델의 로버스트니스 확인 + LLM 센서의 고유 기여도 분석
- **단계**:
  1. 학습 시 랜덤하게 1-2개 센서를 0으로 마스킹
  2. 테스트 시 각 센서를 하나씩 제거하며 성능 측정
  3. 특히 LLM 센서 제거 시 "의미적 이해"가 얼마나 떨어지는지 분석
  4. LLM 명령 유무에 따른 행동 변화 분석
  5. 센서 고장에 대한 내성(graceful degradation) 확인
- **예상 시간**: 120분
- **검증**: LLM 제거 시 "의미적 판단"(예: 사람 vs 벽 구분) 성능 하락 확인

#### 산출물
```
06-SensorFusion/
├── README.md
├── concepts/
│   ├── fusion_methods_comparison.md
│   └── cross_attention_explained.md
├── examples/
│   ├── 01_concat_fusion.ipynb
│   ├── 02_cross_attention_fusion.ipynb
│   ├── 03_sensor_dropout_experiment.ipynb
│   └── models/
│       ├── concat_fusion.py
│       └── attention_fusion.py
└── guides/
    └── fusion_architecture_guide.md
```

#### Definition of Done
- [ ] Concatenation + MLP Fusion 구현
- [ ] Cross-Attention Fusion 구현
- [ ] 단일 센서 vs 통합 센서 성능 비교 완료
- [ ] Attention 가중치 시각화
- [ ] 센서 드롭아웃 실험 완료
- [ ] Fusion 방법론 비교 문서 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] Early/Late/Intermediate Fusion의 차이를 설명할 수 있다
- [ ] Cross-Attention이 센서 간 관계를 어떻게 학습하는지 설명할 수 있다

**실무 활용**:
- [ ] 새 센서를 추가할 때 Fusion 모델을 어떻게 수정해야 하는지 안다

**문제 해결**:
- [ ] Fusion 후 성능이 단일 센서보다 낮을 때 원인을 분석할 수 있다

#### 예상 시간 배분
- 개념 학습: 60분 (10%)
- 실습 1 (Concat Fusion): 150분
- 실습 2 (Attention Fusion): 180분
- 실습 3 (드롭아웃 실험): 120분
- 문서화: 60분
- 버퍼: 30분
- **합계**: 10h

#### 참조 자료
- Attention Is All You Need: https://arxiv.org/abs/1706.03762
- Multimodal Learning Survey: https://arxiv.org/abs/2209.03430

---

### M7 - V-JEPA 기반 Latent World Model

**난이도**: ⭐⭐⭐
**예상 시간**: 10h
**산출물 폴더**: `07-LatentWorldModel/`

#### 학습 목표
- [ ] V-JEPA의 시간적 마스킹 예측 방식을 이해하고 적용할 수 있다
- [ ] 잠재공간에서 행동 조건부 미래 상태를 예측하는 모델을 구현할 수 있다
- [ ] N스텝 앞까지 "상상"으로 미래를 시뮬레이션할 수 있다
- [ ] V-JEPA 기반 예측의 품질을 MLP 기반과 비교 평가할 수 있다

#### 주요 개념
1. **V-JEPA 방식 예측**: 시간 축에서 미래 잠재 상태를 마스킹하고 예측. 픽셀 생성 없이 잠재공간에서만 동작. 시간적 패턴(동작, 변화)을 자연스럽게 학습
2. **Latent Dynamics Model**: 잠재공간에서 z(t) + action → z(t+1) 예측. V-JEPA의 예측 방식을 차용하여 시간적 일관성 보장
3. **Action-Conditioned Prediction**: 같은 상태에서도 행동에 따라 다음 상태가 달라짐. "왼쪽으로 가면 이렇게, 오른쪽으로 가면 저렇게"를 잠재공간에서 예측
4. **Multi-Step Rollout**: 여러 스텝 미래를 연쇄적으로 예측. V-JEPA의 시간적 예측 + MuZero의 상상 속 계획 결합
5. **LLM 조건부 예측**: LLM 명령("멈춰")이 World Model 예측에 영향. 같은 상태에서도 명령에 따라 다른 미래를 예측

#### 실습 과제

**실습 1: V-JEPA 스타일 1-Step Latent Dynamics Model** ⭐⭐
- **목적**: V-JEPA의 시간적 예측 방식을 적용한 잠재공간 한 스텝 예측
- **단계**:
  1. M6의 Fusion 출력 (64차원) + action (4차원 one-hot) → 입력
  2. V-JEPA 스타일 Predictor: 현재 잠재 상태의 일부를 마스킹하고 다음 상태 예측
  3. MLP 기반 Predictor와 비교 실험 (MLP vs V-JEPA 스타일)
  4. M3 환경에서 에피소드 데이터 수집 (state, action, next_state 쌍)
  5. 잠재공간 MSE Loss로 학습 (픽셀 복원 없이!)
  6. 예측 vs 실제 잠재 벡터 비교 시각화
- **예상 시간**: 150분
- **검증**: V-JEPA 스타일이 MLP보다 시간적 일관성에서 우수함을 확인

**실습 2: Multi-Step Rollout + LLM 조건부 예측** ⭐⭐⭐
- **목적**: 여러 스텝 미래를 "상상" + LLM 명령에 따른 분기 예측
- **단계**:
  1. 1-Step 모델을 연쇄 적용: z1 → z2 → z3 → ... → zN
  2. 행동 시퀀스 [a1, a2, ..., aN]에 대한 미래 궤적 예측
  3. LLM 명령 조건부 예측: 같은 상태에서 "전진" vs "멈춰" 명령 시 다른 미래 예측
  4. 실제 궤적 vs 상상 궤적 비교
  5. 예측 지평선(prediction horizon) 분석
- **예상 시간**: 180분
- **검증**: LLM 명령에 따라 다른 미래 궤적 예측 + 5-10스텝 rollout 비교 그래프

**실습 3: 보상 예측 + 설명 생성 (MuZero 스타일)** ⭐⭐⭐
- **목적**: 미래 상태/보상 예측 + World Model이 "왜"를 설명
- **단계**:
  1. Dynamics Model에 보상 예측 헤드 추가: z(t) + a → (z(t+1), r(t+1))
  2. 상태 예측 Loss + 보상 예측 Loss 합산하여 학습
  3. "이 행동을 하면 미래에 보상이 얼마나 될까?" 예측
  4. V-JEPA 예측 결과를 LLM에 전달 → 자연어 설명 생성 ("3스텝 후 충돌 예상, 회피 필요")
  5. 예측 보상 vs 실제 보상 비교
- **예상 시간**: 120분
- **검증**: World Model 예측 + LLM 설명이 결합된 "왜 이 행동인가" 데모

#### 산출물
```
07-LatentWorldModel/
├── README.md
├── concepts/
│   ├── latent_dynamics_explained.md
│   ├── multi_step_rollout.md
│   └── prediction_vs_reality.md
├── examples/
│   ├── 01_one_step_prediction.ipynb
│   ├── 02_multi_step_rollout.ipynb
│   ├── 03_reward_prediction.ipynb
│   └── models/
│       └── latent_world_model.py
└── guides/
    └── world_model_training_tips.md
```

#### Definition of Done
- [ ] 1-Step Latent Dynamics Model 학습 완료
- [ ] Multi-Step Rollout (5-10스텝) 구현
- [ ] 보상 예측 헤드 추가
- [ ] 예측 vs 실제 비교 시각화
- [ ] 예측 지평선 분석 문서 작성
- [ ] README.md 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] "잠재공간에서 예측"이 "픽셀 예측"보다 왜 나은지 설명할 수 있다
- [ ] Multi-Step Rollout의 오차 누적 문제를 설명할 수 있다

**실무 활용**:
- [ ] World Model의 예측 품질을 어떻게 평가해야 하는지 안다

**문제 해결**:
- [ ] 예측이 부정확할 때 원인(데이터 부족, 모델 용량, 학습 설정)을 구분할 수 있다

#### 예상 시간 배분
- 개념 학습: 60분 (10%)
- 실습 1 (1-Step): 150분
- 실습 2 (Multi-Step): 180분
- 실습 3 (보상 예측): 120분
- 문서화: 60분
- 버퍼: 30분
- **합계**: 10h

#### 참조 자료
- MuZero 해설: https://arxiv.org/abs/1911.08265
- Dreamer (V3): https://arxiv.org/abs/2301.04104
- World Models Interactive: https://worldmodels.github.io/

---

### M8 - Flow Matching 기반 행동 생성 (π0 스타일)

**난이도**: ⭐⭐⭐
**예상 시간**: 10h
**산출물 폴더**: `08-FlowController/`

#### 학습 목표
- [ ] Flow Matching의 원리를 이해하고 간단한 구현을 할 수 있다
- [ ] World Model 예측을 조건으로 한 Flow Matching 행동 생성을 구현할 수 있다
- [ ] π0 스타일의 Action Chunking (연속 행동 시퀀스 생성)을 구현할 수 있다
- [ ] World Model 기반 planning + Flow Matching 행동 생성의 시너지를 설명할 수 있다

#### 주요 개념
1. **Flow Matching**: 노이즈 분포에서 목표 행동 분포로 "흐름(flow)"을 학습. Diffusion보다 단순하고 효율적. 직선 경로(linear interpolation)를 따라가는 벡터장(vector field)을 학습
2. **Action Chunking**: 한 번에 하나의 행동이 아닌, 미래 H스텝의 행동 시퀀스를 한 번에 생성. π0는 50Hz로 50스텝을 한번에 생성. 부드러운 연속 제어의 핵심
3. **Conditioned Generation**: World Model의 미래 예측을 조건(condition)으로 행동을 생성. "3스텝 후 장애물이 있으니 지금 회피 행동 시작" → 설명 가능성의 핵심
4. **World Model + Flow Matching 시너지**: World Model이 "어떤 미래가 좋은가"를 판단하고, Flow Matching이 "그 미래로 가는 부드러운 행동"을 생성. π0(행동만 잘함) + World Model(이유를 앎)

#### 실습 과제

**실습 1: Flow Matching 기초 구현** ⭐⭐
- **목적**: Flow Matching의 핵심 메커니즘을 직접 구현하여 이해
- **단계**:
  1. 2D 데이터로 Flow Matching 시각화 (노이즈 → 목표 분포)
  2. Conditional Flow Matching (CFM) 학습: 직선 경로 + 벡터장
  3. ODE solver로 노이즈에서 목표까지 흐름 생성
  4. 생성 품질 시각화 (Diffusion과 비교)
- **예상 시간**: 150분
- **검증**: 2D 데이터에서 노이즈 → 목표 분포로의 흐름이 시각적으로 확인됨

**실습 2: World Model 조건부 행동 생성** ⭐⭐⭐
- **목적**: World Model 예측을 조건으로 Flow Matching 행동 생성
- **단계**:
  1. M7 World Model의 미래 상태 예측을 조건(condition)으로 사용
  2. Flow Matching Action Generator: 조건 + 노이즈 → 행동 시퀀스 (H=10스텝)
  3. 데이터 수집: M3 환경에서 (상태, 행동 시퀀스) 쌍
  4. 학습: World Model 예측 조건하에 전문가 행동 시퀀스를 생성하도록 학습
  5. 생성된 행동의 부드러움 vs 이산적 행동 비교
- **예상 시간**: 180분
- **검증**: Flow Matching이 부드러운 연속 행동 시퀀스를 생성

**실습 3: 전체 하이브리드 에이전트** ⭐⭐⭐
- **목적**: VLM Encoder → Fusion → World Model → Flow Controller 전체 연결
- **단계**:
  1. 전체 파이프라인 연결
  2. World Model로 여러 미래를 "상상" → 최선의 미래 선택
  3. 선택된 미래를 조건으로 Flow Matching이 행동 시퀀스 생성
  4. "왜 이 행동인가" 설명 시각화: World Model 예측 + 선택된 미래 표시
  5. 성공률 측정 + 설명 가능성 데모
- **예상 시간**: 180분
- **검증**: 장애물 회피 성공률 70%+ 이면서 "왜 이 행동인지" 시각화 가능

#### 산출물
```
08-FlowController/
├── README.md
├── concepts/
│   ├── flow_matching_deep_dive.md
│   ├── action_chunking.md
│   └── worldmodel_plus_flow.md
├── examples/
│   ├── 01_flow_matching_basics.ipynb
│   ├── 02_conditioned_action_generation.ipynb
│   ├── 03_hybrid_agent.ipynb
│   └── models/
│       ├── flow_matching.py
│       ├── flow_controller.py
│       └── hybrid_pipeline.py
└── guides/
    └── flow_vs_planning_comparison.md
```

#### Definition of Done
- [ ] Flow Matching 기초 구현 (2D 데모)
- [ ] World Model 조건부 행동 생성 구현
- [ ] 전체 하이브리드 파이프라인 연결
- [ ] 장애물 회피 성공률 70% 이상
- [ ] "왜 이 행동인가" 설명 시각화 구현
- [ ] Flow Matching vs CEM Planning 비교 문서
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] Flow Matching이 Diffusion과 어떻게 다른지 설명할 수 있다
- [ ] World Model + Flow Matching 조합이 왜 π0보다 "설명 가능"한지 설명할 수 있다

**실무 활용**:
- [ ] Action Chunking의 H (horizon)를 어떻게 정해야 하는지 판단할 수 있다

**문제 해결**:
- [ ] Flow Matching이 불안정한 행동을 생성할 때 원인을 분석할 수 있다

#### 예상 시간 배분
- 개념 학습: 60분 (10%)
- 실습 1 (Flow Matching 기초): 150분
- 실습 2 (조건부 행동 생성): 180분
- 실습 3 (전체 에이전트): 180분
- 문서화: 15분
- 버퍼: 15분
- **합계**: 10h

#### 참조 자료
- Flow Matching 논문: https://arxiv.org/abs/2210.02747
- Conditional Flow Matching: https://arxiv.org/abs/2302.00482
- π0 논문 (Flow Matching for actions): https://arxiv.org/abs/2410.24164
- pi-zero-pytorch 구현: https://github.com/lucidrains/pi-zero-pytorch

---

### M9 - 다양한 환경 실험 + LLM 명령 테스트 + 일반화

**난이도**: ⭐⭐⭐
**예상 시간**: 10h
**산출물 폴더**: `09-Experiments/`

#### 학습 목표
- [ ] 다양한 환경 설정에서 에이전트의 성능을 체계적으로 평가할 수 있다
- [ ] LLM 자연어 명령에 따른 행동 변화를 검증할 수 있다
- [ ] 학습한 환경과 다른 환경에서의 일반화 능력을 측정할 수 있다
- [ ] Triple Integration의 각 컴포넌트(LLM, V-JEPA, π0) 기여도를 분석할 수 있다

#### 주요 개념
1. **일반화(Generalization)**: 학습한 환경과 다른 환경에서도 잘 동작하는 능력. 진짜 World Model이면 새로운 환경에도 적응해야 함
2. **Domain Randomization**: 학습 시 환경을 매번 다르게 설정하여 특정 환경에 과적합하는 것을 방지
3. **Zero-Shot Transfer**: 한 번도 보지 못한 환경에서의 성능. 인과적 이해의 진정한 테스트
4. **Sim-to-Real Gap**: 시뮬레이션에서 학습한 것을 실제 로봇에 적용할 때의 차이. Physical AI의 핵심 과제

#### 실습 과제

**실습 1: 환경 다양화 + Domain Randomization** ⭐⭐
- **목적**: 다양한 환경에서 학습하여 일반화 능력 향상
- **단계**:
  1. 10가지 이상 환경 설정 생성 (장애물 수, 크기, 위치, 맵 크기 변경)
  2. Domain Randomization으로 매 에피소드 환경을 랜덤 변경
  3. 고정 환경 학습 vs 랜덤 환경 학습 성능 비교
  4. 학습 곡선 시각화
- **예상 시간**: 180분
- **검증**: 랜덤 환경 학습 에이전트가 새 환경에서 더 높은 성능

**실습 2: LLM 명령 테스트 + Zero-Shot Transfer** ⭐⭐⭐
- **목적**: LLM 자연어 명령에 따른 행동 변화 검증 + 새로운 환경 테스트
- **단계**:
  1. LLM 명령 테스트: 다양한 자연어 명령("천천히 가", "왼쪽으로 가", "멈춰")에 대한 행동 변화
  2. 학습에 사용하지 않은 5가지 "테스트 전용" 환경 생성
  3. 미로형, 동적 장애물, 좁은 통로 등 다양한 도전적 환경
  4. 각 환경에서 LLM 명령 유/무에 따른 성공률 비교
  5. Triple Integration 기여도 분석: LLM만 제거, V-JEPA만 제거, Flow Matching만 제거 시 성능 변화
  6. 실패 케이스 분석: "왜 이 상황에서 실패했는가?"
- **예상 시간**: 150분
- **검증**: LLM 명령에 따른 행동 변화 + Ablation Study 결과 문서

**실습 3: Physical AI 적용 분석** ⭐⭐
- **목적**: 시뮬레이션 → 실제 로봇 적용 시 고려사항 정리
- **단계**:
  1. Sim-to-Real Gap 분석: 시뮬레이션과 현실의 차이 정리
  2. 센서 노이즈 실험: 센서 데이터에 노이즈 추가 후 성능 변화
  3. 실시간 처리 속도 측정: 1스텝 inference 시간
  4. 소형 로봇 (예: Jetson Nano) 적용 시 필요한 모델 경량화 방향
  5. Physical AI 사례 분석: NVIDIA Isaac, Wayve, Tesla FSD
- **예상 시간**: 150분
- **검증**: Physical AI 적용 분석 문서 완성

#### 산출물
```
09-Experiments/
├── README.md
├── concepts/
│   ├── generalization_analysis.md
│   ├── sim_to_real_gap.md
│   └── physical_ai_applications.md
├── examples/
│   ├── 01_domain_randomization.ipynb
│   ├── 02_zero_shot_transfer.ipynb
│   ├── 03_physical_ai_analysis.ipynb
│   └── configs/
│       ├── test_env_maze.yaml
│       ├── test_env_dynamic.yaml
│       ├── test_env_narrow.yaml
│       └── ...
└── guides/
    └── experiment_results_summary.md
```

#### Definition of Done
- [ ] 10가지 이상 환경 설정 생성
- [ ] Domain Randomization 학습 완료
- [ ] Zero-Shot Transfer 테스트 (5개 환경)
- [ ] 센서 노이즈 실험 완료
- [ ] Physical AI 적용 분석 문서 작성
- [ ] 실험 결과 종합 요약 문서 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] World Model이 "이해"했는지 "외웠는지" 구분하는 방법을 설명할 수 있다
- [ ] Sim-to-Real Gap이 왜 발생하는지 3가지 이상 원인을 말할 수 있다

**실무 활용**:
- [ ] 실제 로봇에 적용하려면 어떤 단계가 더 필요한지 로드맵을 제시할 수 있다

**문제 해결**:
- [ ] 새 환경에서 성능이 급락할 때 개선 방향을 제시할 수 있다

#### 예상 시간 배분
- 개념 학습: 60분 (10%)
- 실습 1 (Domain Randomization): 180분
- 실습 2 (Zero-Shot Transfer): 150분
- 실습 3 (Physical AI 분석): 150분
- 문서화: 30분
- 버퍼: 30분
- **합계**: 10h

#### 참조 자료
- Domain Randomization: https://arxiv.org/abs/1703.06907
- NVIDIA Isaac Sim: https://developer.nvidia.com/isaac-sim
- Wayve GAIA-1: https://wayve.ai/thinking/scaling-gaia-1/

---

### M10 - Capstone: LLM+V-JEPA+π0 Triple Integration 시스템

**난이도**: ⭐⭐⭐
**예상 시간**: 10h
**산출물 폴더**: `10-Capstone/`

#### 학습 목표
- [ ] LLM+V-JEPA+π0 Triple Integration 시스템을 완성된 프로젝트로 통합할 수 있다
- [ ] "왜 이 행동인가"를 V-JEPA 예측 + LLM 해석으로 이중 설명하는 데모를 만들 수 있다
- [ ] 자연어 명령으로 에이전트를 제어하는 데모를 만들 수 있다
- [ ] Triple Integration의 의의와 각 컴포넌트의 역할을 설명할 수 있다

#### 주요 개념
1. **시스템 통합**: VLM Encoder → Fusion → World Model → Flow Controller 전체 파이프라인 end-to-end 동작
2. **설명 가능성 (Explainability)**: World Model의 미래 예측을 시각화하여 "이 행동의 이유"를 사람이 이해할 수 있게 표현
3. **실험 관리**: 다양한 실험 설정과 결과를 체계적으로 기록하고 비교
4. **프로젝트 문서화**: 전체 시스템을 다른 사람이 이해하고 재현할 수 있는 수준

#### 실습 과제

**실습 1: 전체 파이프라인 리팩토링 + 통합** ⭐⭐⭐
- **목적**: M3-M9의 코드를 깔끔하게 정리하여 하나의 시스템으로 통합
- **단계**:
  1. 프로젝트 구조 정리 (models/, envs/, utils/, configs/)
  2. 설정 파일 기반 학습: `python train.py --config configs/experiment1.yaml`
  3. 학습 → 평가 → 시각화 파이프라인 자동화
  4. 체크포인트 저장/로드 구현
- **예상 시간**: 180분
- **검증**: 설정 파일 하나로 전체 학습-평가 파이프라인 실행 가능

**실습 2: 시각화 대시보드** ⭐⭐
- **목적**: 학습 과정과 결과를 직관적으로 확인
- **단계**:
  1. 에이전트 행동 애니메이션 (matplotlib animation 또는 pygame)
  2. World Model의 "상상" 과정 시각화 (예측 궤적 vs 실제 궤적)
  3. 센서 데이터 실시간 표시 (4개 센서 대시보드)
  4. 학습 곡선, 성공률, 환경별 성능 비교 차트
- **예상 시간**: 150분
- **검증**: 에이전트가 장애물을 피하는 모습 + World Model 상상 과정이 함께 시각화

**실습 3: 최종 데모 + 프로젝트 문서** ⭐⭐⭐
- **목적**: 프로젝트를 포트폴리오/발표 수준으로 완성
- **단계**:
  1. 3가지 환경에서의 데모 영상/GIF 생성:
     - 자율 장애물 회피 + V-JEPA 예측 시각화
     - LLM 자연어 명령 ("오른쪽으로 가") → 행동 변화 데모
     - LLM 상황 해석 ("앞에 사람이 있어, 천천히 가") → 설명 데모
  2. README.md: 프로젝트 소개, Triple Integration 아키텍처, 실행 방법, 결과
  3. 핵심 문서: "LLM이 의미를 알고, V-JEPA가 미래를 예측하고, π0가 부드럽게 행동한다"
  4. 한계와 개선 방향: 실제 로봇 적용, 더 큰 LLM 통합, 실시간 처리 등
  5. Topic Retrospective 작성
- **예상 시간**: 150분
- **검증**: README를 읽은 사람이 "왜 LLM+V-JEPA+π0인지"를 이해할 수 있는 수준

#### 산출물
```
10-Capstone/
├── README.md                    # 전체 프로젝트 소개 (포트폴리오 수준)
├── train.py                     # 학습 실행 스크립트
├── evaluate.py                  # 평가 스크립트
├── visualize.py                 # 시각화 스크립트
├── models/
│   ├── encoders.py              # 5개 센서 Encoder (VLM+LLM+3종)
│   ├── fusion.py                # Multimodal Fusion
│   ├── world_model.py           # Latent World Model
│   ├── flow_matching.py         # Flow Matching Action Generator
│   └── controller.py            # Hybrid Controller (WorldModel + Flow)
├── envs/
│   ├── obstacle_env.py          # 커스텀 환경
│   └── sensor_sim.py            # 센서 시뮬레이션
├── configs/
│   ├── default.yaml
│   ├── easy.yaml
│   ├── hard.yaml
│   └── transfer_test.yaml
├── results/
│   ├── demo_easy.gif
│   ├── demo_hard.gif
│   └── experiment_summary.md
└── docs/
    ├── architecture.md          # π0+WorldModel 하이브리드 아키텍처 설명
    ├── why_hybrid.md            # 왜 π0+World Model 하이브리드인가
    ├── explainability.md        # 설명 가능성: World Model이 주는 가치
    └── future_directions.md     # 향후 방향 (실제 로봇 적용, π0 풀 모델 통합 등)
```

#### Definition of Done
- [ ] LLM+V-JEPA+π0 Triple Integration 파이프라인 통합 및 리팩토링
- [ ] 설정 파일 기반 학습/평가 시스템
- [ ] 이중 설명 시각화 (V-JEPA 미래 예측 + LLM 자연어 설명 + Flow Matching 행동)
- [ ] LLM 자연어 명령 데모 생성
- [ ] 3가지 환경 데모 생성
- [ ] 포트폴리오 수준 README 작성
- [ ] "왜 LLM+V-JEPA+π0 Triple Integration인가" 설명 문서
- [ ] Topic Retrospective 작성
- [ ] WorkLog 작성 완료

#### Self-Assessment
**개념 이해**:
- [ ] 전체 시스템의 아키텍처를 비전공자에게 5분 안에 설명할 수 있다
- [ ] World Model이 "이해"한 것과 하지 못한 것을 구분하여 설명할 수 있다

**실무 활용**:
- [ ] 이 프로젝트를 실제 로봇에 적용하려면 무엇이 더 필요한지 설명할 수 있다
- [ ] 다음 학습/연구 방향을 3가지 이상 제시할 수 있다

**문제 해결**:
- [ ] 시스템의 병목(bottleneck)이 어디인지 파악하고 개선 방향을 제시할 수 있다

#### 예상 시간 배분
- 실습 1 (통합 리팩토링): 180분
- 실습 2 (시각화): 150분
- 실습 3 (데모 + 문서): 150분
- Topic Retrospective: 60분
- 버퍼: 60분
- **합계**: 10h

#### 참조 자료
- PyTorch 프로젝트 구조 모범 사례: https://github.com/victoresque/pytorch-template
- Weights & Biases (실험 관리): https://wandb.ai/

---

## WorkLog 작성 가이드

각 학습 세션마다 WorkLog를 작성하여 진행 상황을 추적합니다.

**파일명 규칙**: `vl_worklog/YYYYMMDD_MX_WorldModel-PhysicalAI.md`
- 예: `vl_worklog/20260310_M1_WorldModel-PhysicalAI.md`

**WorkLog 필수 섹션**:
1. 오늘의 학습 목표 (체크리스트)
2. 진행 내용 (실습별 상세 기록)
3. 문제 해결 로그
4. DoD 체크리스트 (모듈 완료 기준)
5. Daily Retrospective
6. 참조 및 산출물

---

## Retrospective 가이드

### Daily Retrospective (매일, 5-10분)
WorkLog 내에 작성:
- What went well?
- What could be improved?
- Insights
- Tomorrow's focus

### Module Retrospective (모듈 완료 시, 15-20분)
`vl_worklog/YYYYMMDD_MX_Retrospective.md`:
- 계획 대비 실제 비교
- 핵심 학습 내용
- 발생한 문제와 해결
- Roadmap 정확도 평가
- 다음 모듈 준비사항

### Topic Retrospective (전체 완료 시, 30-60분)
`vl_worklog/YYYYMMDD_WorldModel-PhysicalAI_Final_Retrospective.md`:
- 전체 학습 여정 통계
- VibeLearn AI 방법론 효과성 평가
- 산출물 품질 평가
- 향후 학습 개선 사항

---

## 전체 폴더 구조

```
Topics/WorldModel-PhysicalAI/
├── topic_info.md
├── vl_prompts/
│   ├── roadmap_prompt.md
│   └── daily_learning_prompt.md
├── vl_roadmap/
│   └── 20260309_RoadMap_WorldModel-PhysicalAI.md  ← 이 파일
├── vl_worklog/
│   ├── YYYYMMDD_M1_WorldModel-PhysicalAI.md
│   ├── YYYYMMDD_M2_WorldModel-PhysicalAI.md
│   └── ...
├── vl_materials/
│   └── (논문 PDF, 참조 코드 등)
├── 01-PythonSetup/
├── 02-DeepLearningBasics/
├── 03-SimEnvironment/
├── 04-WorldModelConcepts/
├── 05-SensorEncoders/
├── 06-SensorFusion/
├── 07-LatentWorldModel/
├── 08-FlowController/
├── 09-Experiments/
└── 10-Capstone/
```

---

## 학습 진행 상황 추적

| 모듈 | 시작일 | 종료일 | 상태 | DoD 달성률 | 비고 |
|------|--------|--------|------|-----------|------|
| M1 | | | ⏳ | 0% | |
| M2 | | | ⏳ | 0% | |
| M3 | | | ⏳ | 0% | |
| M4 | | | ⏳ | 0% | |
| M5 | | | ⏳ | 0% | |
| M6 | | | ⏳ | 0% | |
| M7 | | | ⏳ | 0% | |
| M8 | | | ⏳ | 0% | |
| M9 | | | ⏳ | 0% | |
| M10 | | | ⏳ | 0% | |

**범례**:
- ⏳ 대기
- 🔄 진행 중
- ✅ 완료

---

## 성공 기준

전체 Topic 완료 기준:
- [ ] 모든 모듈 완료 (DoD 100%)
- [ ] 최소 10개 산출물 폴더 생성
- [ ] LLM+V-JEPA+π0 Triple Integration 시스템 동작
- [ ] 5개 센서 (카메라+거리+오디오+IMU+LLM) + Flow Matching 행동 생성 통합
- [ ] LLM 자연어 명령으로 에이전트 제어 가능
- [ ] 3가지 이상 환경에서 장애물 회피 성공
- [ ] 이중 설명 구현: V-JEPA 미래 예측 시각화 + LLM 자연어 설명
- [ ] "왜 LLM+V-JEPA+π0 Triple Integration인가" 설명 문서 완성
- [ ] Topic Retrospective 작성
- [ ] Capstone 프로젝트 완성 (데모 가능 수준)

---

**생성자**: Claude with VibeLearn AI
**Roadmap 버전**: 3.0 (LLM + V-JEPA + π0 Triple Integration)
**방법론 버전**: VibeLearn AI 2.0
