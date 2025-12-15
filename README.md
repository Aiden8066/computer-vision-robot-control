# 컴퓨터 비전을 통한 로봇 제어

ArUco 마커 기반 창고 네비게이션 및 YOLO 객체 감지를 통한 스마트 물류 로봇 제어 시스템

## 프로젝트 개요

이 프로젝트는 컴퓨터 비전 기술을 활용하여 창고 환경에서 자율 주행하는 로봇 제어 시스템을 구현합니다. ArUco 마커를 이용한 정밀한 위치 추적과 YOLOv5 기반 객체 감지를 통해 안전하고 효율적인 창고 내 네비게이션을 제공합니다.

### 주요 기능

- **ArUco 마커 기반 네비게이션**: 11개의 ArUco 마커를 이용한 정확한 위치 추적
- **YOLO 객체 감지**: 포크리프트와 사람을 실시간으로 감지하여 안전 보장
- **PyQt6 GUI**: 직관적이고 전문적인 사용자 인터페이스
- **실시간 영상 처리**: 카메라를 통한 실시간 환경 모니터링
- **안전 알림 시스템**: 위험 상황 감지 시 즉각적인 경고

## 시스템 구조

```
computer-vision-robot-control/
├── ArUco-Navigation-Robot-/          # 메인 네비게이션 시스템
│   ├── aruco_markers/                # ArUco 마커 이미지 파일들
│   │   ├── marker_00_robot_1.png     # 로봇 1 마커
│   │   ├── marker_01_robot_2.png     # 로봇 2 마커
│   │   ├── marker_02-09_warehouse_*.png # 창고 위치 마커 (1-8)
│   │   ├── marker_10_start_point.png # 시작점 마커
│   │   └── marker_info.txt           # 마커 정보
│   ├── config/                       # 설정 파일
│   │   └── navigation_config.json    # 네비게이션 매개변수
│   ├── models/                       # AI 모델
│   │   └── bestFok.pt               # YOLOv5 학습된 모델
│   ├── src/                         # 소스 코드
│   │   ├── warehouse_navigation_gui.py         # GUI 메인 애플리케이션
│   │   └── warehouse_navigation_system_fixed.py # 네비게이션 시스템 코어
│   ├── run_gui.py                   # GUI 실행 스크립트
│   ├── requirements_gui.txt         # Python 의존성
│   └── README_GUI.md               # GUI 사용 가이드
├── 객체 학습/                        # 모델 학습 관련
│   └── warehouse_yolov5_train.ipynb # YOLOv5 모델 학습 노트북
└── 실행 영상.mp4                     # 시스템 데모 영상
```

## 시작하기

### 필수 요구사항

- Python 3.8+
- OpenCV 4.8.0+
- PyQt6 6.5.0+
- 웹캠 또는 USB 카메라

### 설치 방법

1. **저장소 클론**
```bash
git clone https://github.com/[username]/computer-vision-robot-control.git
cd computer-vision-robot-control/ArUco-Navigation-Robot-
```

2. **의존성 설치**
```bash
pip install -r requirements_gui.txt
```

3. **GUI 실행**
```bash
python run_gui.py
```

또는 직접 실행:
```bash
cd src
python warehouse_navigation_gui.py
```

## 사용 방법

### GUI 인터페이스

1. **카메라 연결**: 시스템 시작 시 자동으로 카메라를 감지하고 연결
2. **목적지 선택**: GUI의 위치 버튼(1-10)을 클릭하여 목적지 설정
3. **네비게이션 시작**: "Start Navigation" 버튼으로 자율 주행 시작
4. **객체 감지 토글**: "Toggle Detection" 버튼으로 YOLO 감지 기능 제어
5. **신뢰도 조절**: 슬라이더를 통해 객체 감지 민감도 조정

### ArUco 마커 설정

| 마커 ID | 용도 | 설명 |
|---------|------|------|
| 0 | 로봇 1 | 첫 번째 로봇 식별자 |
| 1 | 로봇 2 | 두 번째 로봇 식별자 |
| 2-9 | 창고 위치 1-8 | 목적지 위치 마커 |
| 10 | 시작점 | 네비게이션 시작 위치 |

## 기술 스택

### 컴퓨터 비전
- **OpenCV**: ArUco 마커 감지 및 영상 처리
- **ArUco**: 4x4_50 딕셔너리 기반 마커 시스템
- **YOLOv5**: Ultralytics 프레임워크 기반 객체 감지

### AI/ML
- **PyTorch**: 딥러닝 프레임워크
- **Ultralytics**: YOLOv5 모델 구현
- **Custom Dataset**: 포크리프트와 사람 감지를 위한 맞춤형 데이터셋

### GUI 및 시스템
- **PyQt6**: 현대적이고 반응형 사용자 인터페이스
- **NumPy**: 수치 연산 및 배열 처리
- **Pillow**: 이미지 처리 및 한글 텍스트 렌더링

## 구성 설정

### navigation_config.json
```json
{
  "navigation": {
    "kp_distance": 0.4,        // 거리 제어 게인
    "kp_angle": 0.55,          // 각도 제어 게인
    "max_speed": 60,           // 최대 속도
    "distance_threshold": 80,   // 도착 판정 거리
    "warehouse_avoid_radius": 150  // 장애물 회피 반경
  },
  "camera": {
    "frame_width": 1280,       // 카메라 해상도 너비
    "frame_height": 720,       // 카메라 해상도 높이
    "fps": 30                  // 프레임률
  }
}
```

## AI 모델 학습

### YOLOv5 모델 학습 과정

1. **데이터셋 준비**: 창고 환경의 포크리프트와 사람 이미지 수집
2. **라벨링**: 객체 바운딩 박스 및 클래스 라벨 생성
3. **모델 학습**: Google Colab GPU를 활용한 YOLOv5 학습
4. **모델 최적화**: 실시간 추론을 위한 모델 경량화

학습 노트북: `객체 학습/warehouse_yolov5_train.ipynb`

### 감지 클래스
- **forklift**: 포크리프트 차량
- **person**: 작업자/사람

## 안전 기능

- **실시간 객체 감지**: 포크리프트와 사람의 위치 추적
- **충돌 방지**: 감지된 객체와의 안전 거리 유지
- **경고 알림**: 위험 상황 시 시각적/청각적 경고
- **비상 정지**: 긴급 상황 시 즉시 정지 기능

## 성능 지표

- **마커 감지 정확도**: 95%+
- **객체 감지 정확도**: 90%+ (mAP@0.5)
- **실시간 처리**: 30 FPS
- **네비게이션 정밀도**: ±5cm

## 문제 해결

### 일반적인 문제

1. **카메라 연결 실패**
   - USB 카메라 연결 상태 확인
   - 다른 애플리케이션에서 카메라 사용 중인지 확인

2. **마커 인식 불가**
   - 조명 조건 개선
   - 마커 크기 및 거리 조정
   - 카메라 초점 확인

3. **객체 감지 성능 저하**
   - 신뢰도 임계값 조정
   - 조명 환경 개선
   - GPU 메모리 확인

## 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.




⭐ 이 프로젝트가 도움이 되셨다면 Star를 눌러주세요!
