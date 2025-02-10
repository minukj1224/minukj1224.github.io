---
title: "[논문리뷰] Going Deeper with Convolutions (InceptionNet / GoogLeNet)"
last_modified_at: 2025-02-10
categories:
  - 논문리뷰
  - 딥러닝
  - 컴퓨터비전

tags:
  - Computer Vision
  - AI
  - Image Classification
  - InceptionNet Model

excerpt: "InceptionNet (GoogLeNet) 논문 리뷰"
use_math: true
classes: wide
math_engine: katex
---

> CVPR 2015. [[논문 링크](https://arxiv.org/abs/1409.4842)]  
> 저자: Christian Szegedy, Wei Liu, Yangqing Jia, Pierre Sermanet, Scott Reed, Dragomir Anguelov, Dumitru Erhan, Vincent Vanhoucke, Andrew Rabinovich (Google Research)  
> 출판일: 2014년 9월 17일

## 소개

InceptionNet(GoogLeNet)은 Google Research에서 개발한 CNN 기반 모델로, **ILSVRC 2014**에서 우승을 차지한 신경망이다. 이 모델은 전통적인 CNN보다 깊고 효율적인 구조를 갖추었으며, **Inception Module**을 통해 계산 효율성을 극대화하였다.

InceptionNet의 주요 기여는 다음과 같다:
1. **Inception Module 도입**: 다양한 크기의 필터를 병렬로 적용하여 다중 스케일 특징을 추출.
2. **네트워크 깊이 증가**: 성능 향상을 위해 네트워크를 깊게 설계하였으나, 연산량을 최적화.
3. **1×1 Convolution 사용**: 연산량 감소 및 비선형성 추가.
4. **Auxiliary Classifier 적용**: 학습 속도 향상 및 Gradient Vanishing 문제 해결.

## InceptionNet 구조

InceptionNet은 전통적인 CNN 모델과 달리, 다양한 크기의 필터를 동시에 적용하는 **Inception Module**을 도입하였다.

### 1. 입력 레이어
- \( 224 \times 224 \) RGB 이미지 입력.
- 평균 제거 후 정규화된 데이터 사용.

### 2. Inception Module
Inception Module은 **1×1, 3×3, 5×5 합성곱**을 병렬로 수행한 후, **Max Pooling**과 결합하는 구조로 이루어진다. 이를 통해 다양한 크기의 특징을 효과적으로 추출할 수 있다.

$$
\text{Output} = \text{Concat} \Big( \text{Conv}_{1\times1}, \text{Conv}_{3\times3}, \text{Conv}_{5\times5}, \text{Pooling} \Big)
$$

### 3. 1×1 Convolution 활용
- **차원 축소**: 연산량을 줄이면서도 네트워크의 표현력을 유지하기 위해 사용.
- **비선형성 추가**: ReLU 활성화 함수를 적용하여 비선형성을 강화.

### 4. Auxiliary Classifier (보조 분류기)
- 중간 계층에서 추가적인 분류기를 사용하여 **Gradient Vanishing** 문제를 해결.
- Softmax를 활용하여 네트워크가 빠르게 수렴할 수 있도록 도움.

### 5. Fully Connected Layer & Softmax
- Global Average Pooling 적용 후, Softmax 함수를 사용하여 최종 예측 수행.

### 6. GoogLeNet 구조 요약
GoogLeNet(Inception v1)은 **22개의 레이어**로 구성되었으며, 기존 모델보다 더 깊지만 연산량은 적다.

| 모델 | 레이어 개수 | 주요 특징 |
|------|------------|-------------|
| AlexNet | 8 | 단순한 CNN 구조 |
| VGG-16 | 16 | 3×3 Convolution 반복 |
| GoogLeNet | 22 | Inception Module 도입, 연산량 최적화 |

## InceptionNet의 학습 방법

- **Batch Normalization 미적용** (Inception v2부터 도입)
- **Momentum 기반 SGD 사용 (Momentum = 0.9)**
- **Weight Decay 적용 (0.0002)**
- **Dropout 사용 (0.4)으로 과적합 방지**

## 실험 및 결과

InceptionNet은 **ILSVRC 2014에서 6.67% Top-5 Error Rate**를 기록하며 우승하였다.

### 주요 성과
- **기존 CNN 대비 높은 정확도 달성**
- **Inception Module을 통한 연산량 감소**
- **Auxiliary Classifier 도입으로 학습 안정성 증가**

## 결론

InceptionNet(GoogLeNet)은 CNN의 깊이를 증가시키면서도 연산량을 최적화한 혁신적인 모델로, 이후 등장한 **Inception v2, Inception v3, Xception** 등의 발전된 네트워크에 큰 영향을 미쳤다.

1. **Inception Module 도입으로 다양한 스케일의 특징 추출**
2. **1×1 Convolution을 활용한 차원 축소 및 연산 최적화**
3. **Auxiliary Classifier를 통한 학습 안정성 향상**

현재에도 InceptionNet은 다양한 이미지 분류 및 객체 검출 작업에서 활용되며, CNN 모델의 중요한 발전 과정 중 하나로 평가받는다.

