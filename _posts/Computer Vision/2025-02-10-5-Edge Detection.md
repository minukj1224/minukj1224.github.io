---
title: "[영상처리 기초] OpenCV - 에지 검출"
last_modified_at: 2025-02-10
categories:
  - 영상처리 기초
  - 컴퓨터비전

tags:
  - Computer Vision
  - Image Processing
  - OpenCV
  - 영상처리

excerpt: "OpenCV를 활용한 영상의 에지 검출에 대해 설명합니다."
use_math: true
classes: wide
math_engine: katex
---

# 1. 에지 검출 개요

**에지(edge)**란 한쪽 방향으로 픽셀 값이 급격하게 변하는 부분을 의미한다. 컴퓨터 비전에서는 **에지를 검출하여 영상의 구조적 특징을 파악**하고 객체를 분리하는 데 활용된다.

## (1) 에지 검출의 원리: 미분과 그래디언트

영상에서 에지를 검출하는 기본 원리는 **미분(derivative) 연산**을 활용하여 픽셀 값 변화율이 큰 영역을 찾는 것이다. 일반적으로 **소벨(Sobel), 샤르(Scharr) 필터** 등을 활용하여 그래디언트 크기와 방향을 계산한다.

---

# 2. 마스크 기반 에지 검출

## (1) 소벨 필터 (Sobel Filter)

소벨 필터는 미분 연산을 수행하는 대표적인 필터로, x 및 y 방향으로 편미분을 수행하여 에지를 검출한다.

```python
import cv2
import numpy as np

img = cv2.imread('image.jpg', cv2.IMREAD_GRAYSCALE)
dx = cv2.Sobel(img, cv2.CV_64F, 1, 0, ksize=3)  # x 방향 미분
dy = cv2.Sobel(img, cv2.CV_64F, 0, 1, ksize=3)  # y 방향 미분
```

## (2) 샤르 필터 (Scharr Filter)

샤르 필터는 소벨 필터보다 더 정밀한 미분 연산을 수행하여 에지를 검출한다.

```python
dx = cv2.Scharr(img, cv2.CV_64F, 1, 0)
dy = cv2.Scharr(img, cv2.CV_64F, 0, 1)
```

---

# 3. 캐니 에지 검출 (Canny Edge Detection)

캐니 에지 검출은 **가우시안 필터링 + 그래디언트 계산 + 비최대 억제 + 이중 임계값 처리**의 과정을 거쳐 강한 에지만을 검출하는 알고리즘이다.

```python
edges = cv2.Canny(img, 50, 150)
```

- `threshold1`: 낮은 임계값
- `threshold2`: 높은 임계값

---

# 4. 허프 변환 (Hough Transform)

허프 변환은 **영상에서 직선이나 원을 검출**하는 방법이다.

## (1) 허프 변환 직선 검출 (Hough Line Transform)
```python
edges = cv2.Canny(img, 50, 150)
lines = cv2.HoughLinesP(edges, 1, np.pi/180, 100, minLineLength=50, maxLineGap=5)
```

## (2) 허프 변환 원 검출 (Hough Circle Transform)
```python
circles = cv2.HoughCircles(img, cv2.HOUGH_GRADIENT, 1, 50, param1=150, param2=40, minRadius=20, maxRadius=80)
```

---

# 5. 결론

에지 검출과 허프 변환을 활용하면 **객체 경계 검출, 텍스트 인식, 차선 검출 등 다양한 응용이 가능**하다. 🚀