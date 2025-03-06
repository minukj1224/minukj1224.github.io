# EfficientDet – Scalable and Efficient Object Detection

## 1. 연구 개요
EfficientDet은 기존 객체 탐지(Object Detection) 모델보다 **더 작고 빠르면서도 높은 정확도**를 달성하기 위해 제안된 **스케일러블(Scalable)한 객체 탐지 모델**이다.

기존의 ResNet 기반 백본(Backbone) 대신 **EfficientNet**을 사용하며, Feature Pyramid Network(FPN)를 개선한 **BiFPN(Bi-directional Feature Pyramid Network)** 구조를 도입하여 성능을 향상시켰다. 또한, EfficientNet에서 사용된 **Compound Scaling** 개념을 객체 탐지 모델에 적용하여 모델의 크기와 성능을 균형 있게 확장할 수 있도록 설계되었다.

EfficientDet은 **모델 크기에 따라 D0 ~ D7까지 다양한 버전을 제공**하며, 연산량(FLOPs)을 최소화하면서도 높은 정확도를 유지하는 것이 특징이다.

---

## 2. 주요 기여

### 🔹 BiFPN (Bi-directional Feature Pyramid Network) 제안
EfficientDet에서는 기존의 FPN을 개선한 **BiFPN**을 도입하여 다중 해상도의 특징(feature)들을 더 효과적으로 활용할 수 있도록 설계하였다.

- **가중치 조정(weighted fusion) 기법 적용**
  - 기존 FPN보다 각 피처(feature)에 가중치를 부여하여 중요한 정보를 더 강조하도록 개선함.
- **상향식(Bottom-up) 및 하향식(Top-down) 연결 최적화**
  - 여러 단계의 특징 맵(feature map)을 효율적으로 연결하여 정보 전달을 최적화함.
  - 필요 없는 노드를 제거하고, 가중치를 학습할 수 있도록 개선하여 연산량을 줄이면서도 성능을 높임.

### 🔹 EfficientNet 백본(Backbone) 활용
EfficientDet에서는 기존 객체 탐지 모델들이 주로 사용하던 **ResNet 대신 EfficientNet을 백본으로 활용**하여 연산량을 줄이면서도 높은 성능을 유지하였다.

- **EfficientNet 특징**
  - 기존 ResNet보다 적은 연산량(FLOPs)으로 높은 성능을 제공함.
  - Depth, Width, Resolution을 동시에 스케일링하는 **Compound Scaling 기법**을 적용하여 최적의 성능을 달성함.

### 🔹 Compound Scaling 적용
EfficientNet에서 사용된 **Compound Scaling 기법**을 객체 탐지 모델에도 도입하여 **모델의 크기와 성능을 동시에 조절할 수 있도록 설계**하였다.

- 기존에는 Backbone(백본)과 FPN(Feature Pyramid Network), Head 부분이 개별적으로 조정되었음.
- EfficientDet에서는 **Backbone, BiFPN, Box/Class Prediction Head를 동시에 스케일링**하여 균형 잡힌 모델을 제공함.
- 이를 통해 **작은 연산량으로도 높은 성능을 유지할 수 있음**.

---

## 3. 실험 결과
EfficientDet의 성능을 기존 모델과 비교하기 위해 **COCO 데이터셋**을 활용한 실험을 진행하였다.

### 📌 COCO 데이터셋 실험 결과
- EfficientDet-D7 모델이 기존 **최고 성능 모델보다 적은 연산량(FLOPs)으로 더 높은 AP(Accuracy Precision) 성능을 기록**함.
- 다양한 크기의 EfficientDet 모델(D0 ~ D7)을 제공하여 **모바일 디바이스부터 고성능 서버까지 활용 가능**.
- **EfficientDet-D7 모델은 기존 최적 모델보다 연산량이 적으면서도 더 높은 성능을 보임.**
  - 기존의 RetinaNet, Faster R-CNN, YOLO 등과 비교하여 **성능 대비 연산량이 월등히 낮음**.

| Model | FLOPs | Parameters | COCO AP |
|--------|------------|------------|---------|
| YOLOv3 | 140B | 65M | 33.0 |
| RetinaNet | 250B | 34M | 39.1 |
| EfficientDet-D0 | 2.5B | 4M | 34.3 |
| EfficientDet-D7 | 410B | 52M | **52.2** |

EfficientDet은 **기존 모델보다 연산량을 줄이면서도 더 높은 정확도를 달성**할 수 있음을 보여준다.

---

## 4. 결론
EfficientDet은 **작은 연산량, 빠른 속도, 높은 정확도를 동시에 만족하는 객체 탐지 모델**로, 다양한 환경에서 활용할 수 있도록 설계되었다.

- **BiFPN을 통해 다중 해상도 피처를 효과적으로 활용하고, EfficientNet을 백본으로 사용하여 성능을 극대화함.**
- **Compound Scaling을 적용하여 모델을 균형 있게 확장할 수 있도록 설계됨.**
- **모델 크기에 따라 D0 ~ D7까지 다양한 버전을 제공하여, 모바일 디바이스부터 서버 환경까지 최적화 가능.**
- **COCO 데이터셋 실험 결과, 기존 객체 탐지 모델보다 적은 연산량으로 더 높은 성능을 보임.**

💡 **EfficientDet은 실무에서 경량화된 고성능 객체 탐지 모델이 필요한 경우 매우 유용할 것으로 보인다.**

---

