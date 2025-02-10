---
title: "[논문리뷰] EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks"
last_modified_at: 2025-02-10
categories:
  - 논문리뷰
  - 딥러닝
  - 컴퓨터비전

tags:
  - Computer Vision
  - AI
  - Image Classification
  - EfficientNet Model

excerpt: "EfficientNet 논문 리뷰"
use_math: true
classes: wide
math_engine: katex
---

> ICML 2019. [[논문 링크](https://arxiv.org/abs/1905.11946)]  
> 저자: Mingxing Tan, Quoc V. Le (Google Brain)  
> 출판일: 2019년

## 소개

EfficientNet은 기존 CNN 모델보다 더 적은 파라미터와 연산량으로 높은 성능을 달성한 모델이다. Google Brain 팀이 개발한 이 모델은 **컴퓨터 비전 모델을 확장(scaling)하는 새로운 방법론을 제안**하며, **균형 잡힌 모델 확장(Balanced Model Scaling)** 기법을 사용하여 성능을 최적화한다.

![EfficientNet 구조](/assets/img/efficientnet/efficientnet-architecture.webp)

### EfficientNet의 주요 기여
1. **Compound Scaling**: 너비(width), 깊이(depth), 해상도(resolution)를 균형 있게 확장하는 방법을 제안.
2. **MBConv 사용**: MobileNetV2의 Inverted Residual Block을 기반으로 한 연산 최적화.
3. **적은 연산량으로 높은 성능**: 기존 ResNet, DenseNet보다 적은 FLOPs로 높은 정확도를 달성.
4. **SOTA 모델 대비 효율성 증가**: ImageNet 데이터셋에서 최고 성능을 기록하며 모델 크기를 줄임.

## EfficientNet 구조

EfficientNet은 기본적으로 MobileNetV2의 **Inverted Residual Block**을 활용하며, 이를 기반으로 **Compound Scaling**을 적용하여 모델 크기를 최적화한다.

### 1. Baseline Network (EfficientNet-B0)
- 기본 구조는 MobileNetV2의 inverted residual block을 기반으로 설계됨.
- 채널 수, 깊이, 해상도를 균형 있게 조정하여 더 효율적인 네트워크 구축.

### 2. Compound Scaling 기법
EfficientNet은 기존 CNN 모델들이 단순히 깊이를 증가시키는 방식과 다르게, **너비(width), 깊이(depth), 해상도(resolution)을 균형적으로 조절**하는 방식을 제안한다.

$$
\alpha \cdot d, \beta \cdot w, \gamma \cdot r
$$

여기서, \( \alpha, \beta, \gamma \)는 scaling 계수를 의미하며, \( \phi \)는 전체 자원 증가 비율이다. 

### 3. MBConv (Mobile Inverted Bottleneck Convolution)
- MobileNetV2에서 사용된 Inverted Residual Block을 기반으로 최적화된 구조.
- 1x1 확장, Depthwise Conv, 1x1 축소를 수행하며, SE(Squeeze-and-Excitation) 기법을 적용하여 성능 향상.

![MBConv 구조](/assets/img/efficientnet/mbconv.webp)

## EfficientNet의 학습 방법

- **RMSProp Optimizer 사용**: 학습 안정성을 높이고 학습 속도를 향상.
- **Swish 활성화 함수 사용**: 기존 ReLU 대비 더 부드러운 비선형성을 제공하여 성능 향상.
- **AutoAugment 사용**: 데이터 증강 기법을 통해 일반화 성능 향상.
- **Stochastic Depth 적용**: 학습 과정에서 일부 레이어를 랜덤하게 생략하여 모델 일반화 능력 증가.

## 실험 및 결과

EfficientNet은 다양한 버전(B0~B7)으로 구성되며, ImageNet 및 다양한 데이터셋에서 기존 SOTA 모델 대비 더 적은 연산량과 파라미터 수로 높은 성능을 보였다.

| 모델  | 매개변수 수 | FLOPs | Top-1 정확도 |
|--------|------------|--------|-------------|
| ResNet-50 | 25.6M | 3.8B | 76.5% |
| ResNet-152 | 60.2M | 11.3B | 78.3% |
| EfficientNet-B0 | 5.3M | 0.39B | 77.1% |
| EfficientNet-B7 | 66.3M | 19B | 84.4% |

![EfficientNet 성능 비교](/assets/img/efficientnet/efficientnet-performance.webp)

## 결론

EfficientNet은 기존 CNN 모델을 단순 확장하는 것이 아닌, **Compound Scaling을 적용하여 모델을 최적화하는 방식**을 제안하며, 적은 연산량으로 높은 성능을 달성하였다.

1. **Compound Scaling 기법을 활용하여 균형 잡힌 모델 확장 제안**
2. **적은 파라미터 수와 연산량으로 높은 성능 달성**
3. **모바일 및 클라우드 환경에서도 효율적인 모델로 활용 가능**

EfficientNet은 현재에도 다양한 딥러닝 응용에서 활용되며, 네트워크 확장의 새로운 패러다임을 제시한 모델로 평가받고 있다.