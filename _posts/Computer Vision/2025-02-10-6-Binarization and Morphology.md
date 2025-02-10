---
title: "[영상처리 기초] OpenCV - 이진화와 모폴로지"
last_modified_at: 2025-02-10
categories:
  - 영상처리 기초
  - 컴퓨터비전

tags:
  - Computer Vision
  - Image Processing
  - OpenCV
  - 영상처리

excerpt: "OpenCV를 활용한 이진화 기법과 모폴로지 연산(침식, 팽창, 열기, 닫기)에 대해 설명합니다."
use_math: true
classes: wide
math_engine: katex
---

# 1. 영상 이진화 (Binarization)

영상 이진화(Binarization)는 영상의 각 픽셀 값을 **0 또는 255로 변환**하는 과정이다. 이진화를 수행하기 위해서는 특정 기준 값(임계값, Threshold)과 비교하여 픽셀 값을 결정한다.

## 1.1 기본 이진화 방법

이진화는 **픽셀 값이 임계값보다 크면 255, 작으면 0으로 설정**하는 방식으로 수행된다.

\[
    I'(x,y) = \begin{cases}
        255, & I(x,y) > T \\
        0, & I(x,y) \leq T
    \end{cases}
\]

OpenCV의 `threshold()` 함수를 사용하여 이진화를 수행할 수 있다.

```python
import cv2
import numpy as np

img = cv2.imread('image.jpg', cv2.IMREAD_GRAYSCALE)
_, binary = cv2.threshold(img, 128, 255, cv2.THRESH_BINARY)
```

## 1.2 자동 임계값 결정 (Otsu, Triangle)

영상의 히스토그램을 분석하여 **자동으로 최적의 임계값을 결정**하는 방법이 있다.

- **오츠(Otsu) 이진화:**
  - 클래스 간 분산(Inter-class Variance)을 최대로 하는 임계값을 자동 선택
  - `cv2.THRESH_OTSU` 사용

- **삼각(Triangle) 이진화:**
  - 히스토그램을 분석하여 삼각형 면적을 활용한 자동 임계값 결정
  - `cv2.THRESH_TRIANGLE` 사용

```python
_, binary_otsu = cv2.threshold(img, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)
```

## 1.3 적응형 이진화 (Adaptive Thresholding)

전역 임계값을 사용하는 대신, 작은 영역별로 임계값을 다르게 적용하여 지역적 조도를 고려하는 방법이다.

\[
    T(x,y) = \frac{1}{N} \sum_{(u,v) \in \mathcal{N}(x,y)} I(u,v) - C
\]

`cv2.adaptiveThreshold()`를 사용하여 적용 가능하다.

```python
binary_adaptive = cv2.adaptiveThreshold(img, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 11, 2)
```

---

# 2. 모폴로지 연산 (Morphological Operations)

모폴로지 연산은 영상의 구조적 특징을 유지하면서 객체의 형상을 조절하는 기법이다. **침식(Erosion), 팽창(Dilation), 열기(Opening), 닫기(Closing)** 등의 연산이 포함된다.

## 2.1 침식 (Erosion)

침식 연산은 객체의 경계를 깎아내며 작은 노이즈를 제거하는 데 사용된다.

\[
    I'(x,y) = \min_{(u,v) \in \mathcal{N}(x,y)} I(u,v)
\]

```python
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (3,3))
eroded = cv2.erode(img, kernel, iterations=1)
```

## 2.2 팽창 (Dilation)

팽창 연산은 객체의 경계를 확장하여 작은 구멍을 메우는 데 사용된다.

\[
    I'(x,y) = \max_{(u,v) \in \mathcal{N}(x,y)} I(u,v)
\]

```python
dilated = cv2.dilate(img, kernel, iterations=1)
```

## 2.3 열기 (Opening)

열기 연산은 **침식 후 팽창**을 적용하여 작은 객체를 제거하는 데 효과적이다.

\[
    I' = D(E(I))
\]

```python
opened = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)
```

## 2.4 닫기 (Closing)

닫기 연산은 **팽창 후 침식**을 적용하여 작은 구멍을 메우는 데 효과적이다.

\[
    I' = E(D(I))
\]

```python
closed = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)
```

## 2.5 모폴로지 그라디언트 (Morphological Gradient)

객체의 윤곽선을 강조하는 연산으로, 팽창과 침식의 차이를 계산한다.

\[
    I' = D(I) - E(I)
\]

```python
gradient = cv2.morphologyEx(img, cv2.MORPH_GRADIENT, kernel)
```

---

# 3. 결론

이진화는 영상의 분석 및 객체 검출을 위한 중요한 전처리 단계이며, 모폴로지 연산은 노이즈 제거와 윤곽선 추출 등에 필수적으로 활용된다. 🚀

