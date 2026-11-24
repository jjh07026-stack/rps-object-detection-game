# 객체 탐지 기반 실시간 가위바위보

카메라 영상에서 두 사람의 손동작을 객체 탐지로 인식하고, 3초 카운트다운 뒤 가위바위보 승패를 판정하는 2인용 게임입니다.

- **기간**: 2026.09 (대한상공회의소 AI 시스템반도체 설계 2기 미니 프로젝트)
- **인원**: 개인
- **기술**: Python, OpenCV, NumPy, Google AI Edge LiteRT (TFLite), YOLO 계열 객체 탐지 모델

## 주요 기능

- 가위 · 바위 · 보 3개 클래스 실시간 객체 탐지, 바운딩 박스와 confidence 표시
- 검출된 두 손을 화면 좌우 위치로 정렬해 P1 / P2 구분
- 3초 카운트다운 후 승 · 무 · 패 판정, 점수 누적

## 처리 흐름

```
카메라 → letterbox 전처리 (320×320) → TFLite 추론 → confidence 필터링 → NMS
      → 좌우 위치로 P1 / P2 지정 → READY → COUNTDOWN (3초) → RESULT
```

| 설정 | 값 |
|---|---|
| 모델 입력 크기 | 320 × 320 |
| Confidence 임계값 | 0.40 |
| NMS IoU 임계값 | 0.45 |
| 추론 주기 | 2프레임마다 1회 |
| 데이터셋 | 474장 (Train 80 % · Test 20 %) |

## 실행

```bash
pip install ai-edge-litert opencv-python numpy
python rps_game.py
```

- 학습한 모델 파일(`best.tflite`)은 저장소에 포함되어 있지 않습니다. 같은 폴더에 두고 실행하세요.
- 조작: `SPACE` 시작 / 다음 라운드 · `R` 점수 초기화 · `Q` / `ESC` 종료

## 파일

```
rps_game.py    게임 전체 (모델 로딩, 전처리, 추론, NMS, 게임 상태 관리)
docs/          report.pdf (결과 보고서)
```
