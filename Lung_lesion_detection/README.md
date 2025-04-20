## 🫁 폐 병변 탐지 최적화 프로젝트

본 프로젝트는 폐 질환의 조기 진단을 위한 **X-ray 기반 자동 분석 시스템**을 개발하는 것을 목표로 합니다.<br>
특히, 전문가의 판독에만 의존하지 않고 **AI 모델을 통해 정상/비정상 여부를 분류하고, 병변의 위치를 탐지**하는 시스템을 구축하였습니다.<br>
<br>

## 📌 프로젝트 배경

폐 질환은 전 세계적으로 높은 사망률을 기록하는 질병 중 하나입니다.<br>
특히 폐암의 경우 조기 발견이 어려워 생존율이 낮습니다.<br>
X-ray 영상 판독에는 시간과 전문성이 필요하므로, 자동 분석 시스템의 필요성이 증가하고 있습니다.<br>
<br>

## 🎯 프로젝트 목표

- X-ray 이미지를 자동 분석하여 의료진을 보조하는 시스템 구축<br>
- 분류(Classification)와 객체 탐지(Object Detection)를 결합한 병변 인식 모델 설계<br>
- 높은 정확도와 빠른 속도의 진단 시스템 구현<br>
<br>

## 🗂 데이터셋 개요

- 출처: Kaggle - VinBigData Chest X-ray Dataset<br>
- 구성: X-ray 이미지 + 병변 위치 정보를 포함한 CSV 메타데이터<br>
- 형식: 의료 영상용 DICOM 포맷, 3000x3000 이상 고해상도 흑백 이미지<br>
<br>

## 🧹 데이터 전처리 및 분석 (EDA)

- 이미지 ID를 활용하여 이미지와 메타데이터 연결<br>
- 바운딩 박스를 기반으로 병변 시각화<br>
- 이미지당 평균 병변 수: 약 3.88개<br>
- 클래스 불균형을 고려하여 Downsampling 적용<br>
<br>

## 🧠 모델 아키텍처

### 1. EfficientNetV2-S (분류 모델)

- 기능: X-ray 이미지의 정상 / 비정상 분류<br>
- 입력 크기: 512x512<br>
- 학습 설정: `batch_size=32`, `epochs=30`, `optimizer=Adam`, `early_stopping(patience=5)`<br>
- 결과:<br>
  - Accuracy: 90.34%<br>
  - Precision, Recall, F1-score: 90.32%

### 2. YOLOv8s (탐지 모델)

- 기능: 비정상으로 분류된 이미지에서 병변 위치 탐지<br>
- 이미지 크기: 1024<br>
- 학습 설정: `epochs=20`, `optimizer=Adam`, `5-Fold Cross Validation`<br>
- 다양한 데이터 증강 기법 적용<br>
- YOLOv8n과 비교 시 YOLOv8s가 더 빠르고 정확한 탐지 성능을 보임<br>
<br>

## 🔄 모델 결합 구조

입력된 X-ray 이미지 → EfficientNet으로 정상/비정상 분류<br>
→ 비정상일 경우 YOLOv8s를 통해 병변 위치 탐지<br>
→ 바운딩 박스를 생성하여 병변 시각화<br>
<br>

## 🖼 탐지 결과 예시
![image](https://github.com/user-attachments/assets/034b4f4e-328c-41b7-bf81-6a19839e363b)

- 좌측: 원본 X-ray 이미지<br>
- 우측: YOLO가 예측한 병변 위치 바운딩 박스<br>
- 실제와 비교 시 유사하게 탐지되며, 일부 클래스는 검출 누락 발생<br>
<br>





