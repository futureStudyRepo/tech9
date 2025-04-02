
---

# 🧠 딥러닝 이미지 분류와 객체 인식의 역사, YOLO 개요 및 데이터셋 구성

## 📌 1. 이미지 분류 vs 객체 인식

| 구분 | 이미지 분류 (Image Classification) | 객체 인식 (Object Detection) |
|------|-----------------------------------|------------------------------|
| 목표 | 이미지 전체를 하나의 클래스로 분류 | 이미지 내 여러 객체의 위치와 종류 식별 |
| 출력 | 클래스 라벨 (예: "고양이") | 바운딩 박스 + 클래스 (예: 고양이 위치와 종류) |
| 활용 | 필터링, 태깅, 자동 분류 | 자율주행, CCTV 분석, 얼굴인식 |

---

## 🕰️ 2. 딥러닝 기반 이미지 인식 역사

### 📷 이미지 분류 발전

- **2012** - **AlexNet**: ImageNet 대회에서 CNN의 가능성 증명.
- **2014** - **VGGNet**: 깊이 있는 네트워크 (16~19층), 간단한 구조.
- **2015** - **ResNet**: Residual Connection으로 152층 네트워크 학습 가능.
- **2017 이후** - EfficientNet, Vision Transformer 등 경량화, 트랜스포머 기반 발전.

### 🎯 객체 인식 발전

- **2014** - R-CNN (Selective Search + CNN)
- **2015** - Fast R-CNN (속도 개선)
- **2015** - Faster R-CNN (Region Proposal Network 도입)
- **2016** - YOLO (You Only Look Once) 등장
- **2017** - SSD (Single Shot MultiBox Detector)
- **2018~현재** - YOLOv3, v4, v5, YOLOv7, YOLOv8, YOLO-NAS, RT-DETR 등 지속 발전

---

## 🦾 3. YOLO (You Only Look Once)

### 🧩 개요
- **한 번의 CNN 추론으로 전체 이미지의 객체를 인식**
- 객체 위치와 클래스를 동시에 예측
- 실시간 객체 인식 가능 (30fps 이상 가능)

### 🚀 주요 YOLO 버전 요약

| 버전 | 발표년도 | 특징 |
|------|----------|------|
| YOLOv1 | 2016 | 첫 YOLO, 빠르지만 작은 객체 인식 약함 |
| YOLOv2 (YOLO9000) | 2017 | 다양한 해상도, 다중 클래스 학습 |
| YOLOv3 | 2018 | 다중 스케일 예측, 성능 향상 |
| YOLOv4 | 2020 | CSPDarknet 사용, 정확도/속도 개선 |
| YOLOv5 | 2020 | Ultralytics, PyTorch 기반, 경량화 |
| YOLOv6, v7, v8 | 2022~2023 | 작은 모델 최적화, 속도 및 정확도 향상 |
| YOLO-NAS | 2023 | Neural Architecture Search 기반 고성능 모델 |

---

## 📦 4. 객체 인식용 데이터셋 구성 방법

YOLO 학습을 위해서는 다음과 같은 형식이 필요합니다.

### 📁 기본 폴더 구조 (YOLOv5 기준)

```
dataset/
├── images/
│   ├── train/
│   │   ├── image1.jpg
│   │   └── ...
│   └── val/
│       ├── image2.jpg
│       └── ...
├── labels/
│   ├── train/
│   │   ├── image1.txt
│   │   └── ...
│   └── val/
│       ├── image2.txt
│       └── ...
└── data.yaml
```

### 📝 `labels/*.txt` 파일 포맷 (YOLO 형식)
```
<class_id> <x_center> <y_center> <width> <height>
```
- 모든 값은 **0~1 사이의 상대 좌표값**
- 예: `0 0.5 0.5 0.2 0.3` → 클래스 0번, 정중앙에 폭 20%, 높이 30%

### 📄 `data.yaml` 예시

```yaml
train: ./images/train
val: ./images/val

nc: 2  # 클래스 개수
names: ['cat', 'dog']  # 클래스 이름
```

---

## 🛠️ 5. 어노테이션 도구 추천

| 도구 | 특징 |
|------|------|
| [LabelImg](https://github.com/tzutalin/labelImg) | YOLO, Pascal VOC 지원, 직관적인 GUI |
| Roboflow | 웹 기반, 다양한 포맷 변환 지원 |
| CVAT | 대규모 작업 가능, 협업 지원 |

---

## 🔄 6. 데이터 증강(Augmentation) 팁

- **랜덤 크롭, 회전, 색상 변화** 등으로 일반화 성능 향상
- Albumentations, YOLO 내부 자동 증강 기능 활용

---

## 📚 참고할 공개 데이터셋

| 데이터셋 | 설명 |
|----------|------|
| COCO | 다양한 객체 인식 (80 클래스 이상) |
| Pascal VOC | 고전적 데이터셋, 비교적 간단 |
| Open Images | 구글 제공, 대규모 객체 주석 |
| BDD100K | 자율주행 차량 객체 인식용 |

---

# 🔍 YOLOv8 기본 사용법 (Ultralytics)

## 📦 설치

```bash
pip install ultralytics opencv-python cryptography
```

---

## ✅ 기본 사용 예제

```python
from ultralytics import YOLO
import cv2

# 모델 불러오기 (n: nano, s: small, m: medium, l: large, x: extra-large)
model = YOLO('yolov8n.pt')  # yolov8n은 경량 모델, 속도 빠름

# 이미지 읽기 및 리사이즈
img_path = 'road.JPG'
img = cv2.imread(img_path)
frame = cv2.resize(img, (320, 320))

# 객체 탐지 수행
results = model(frame)

# 탐지 결과 시각화
annotated_frame = results[0].plot(show=False)

# 결과 저장
cv2.imwrite('output2.JPG', annotated_frame)

# 클래스 이름 출력
print(model.names)
```

---

## 🧠 주요 메서드 및 사용법 요약

### 1. 모델 로드

```python
model = YOLO('yolov8n.pt')  # 사전 학습된 모델 불러오기
```

| 파일 | 설명 |
|------|------|
| `yolov8n.pt` | Nano 버전 |
| `yolov8s.pt` | Small |
| `yolov8m.pt` | Medium |
| `yolov8l.pt` | Large |
| `yolov8x.pt` | Extra-Large |

---

### 2. 객체 탐지 (이미지 또는 프레임 입력)

```python
results = model(img)
```

### 3. 결과 시각화

```python
annotated = results[0].plot()
cv2.imshow("Result", annotated)
```

---

### 4. 클래스 이름 확인

```python
print(model.names)  # {0: 'person', 1: 'bicycle', ...}
```

---

### 5. 탐지 결과 좌표 접근

```python
for box in results[0].boxes:
    cls = int(box.cls[0])
    name = model.names[cls]
    conf = float(box.conf[0])
    xyxy = box.xyxy[0].tolist()
    print(f"{name}: {conf:.2f}, box: {xyxy}")
```

---

## 🔁 추가 팁

- **비디오 스트림 처리**도 가능: `cv2.VideoCapture` 사용
- **커스텀 학습 모델**로도 교체 가능: `model = YOLO('runs/detect/train/weights/best.pt')`
- 이미지 크기 리사이즈는 정확도에 영향을 줄 수 있으니, 적절히 조절 필요

---
