---
title: "[영상처리 기초] OpenCV와 영상의 기하학적 변환"
last_modified_at: 2025-02-10
categories:
  - 영상처리 기초
  - 컴퓨터비전

tags:
  - Computer Vision
  - Image Processing
  - OpenCV
  - 영상처리

excerpt: "OpenCV를 활용한 영상의 기하학적 변환(이동, 전단, 크기 변환, 회전, 대칭, 투시 변환)에 대해 설명합니다."
use_math: true
classes: wide
math_engine: katex
---

# 1. 영상의 기하학적 변환 개요

영상의 기하학적 변환(Geometric Transformation)은 **픽셀의 값은 유지하면서 위치를 변경하는 변환 과정**을 의미한다. 대표적인 기하학적 변환에는 **이동, 전단, 크기 변환, 회전, 대칭, 투시 변환** 등이 있다.

# 2. 이동 변환 (Translation)

이동 변환은 영상을 **가로(x) 및 세로(y) 방향으로 일정 크기만큼 이동**시키는 연산으로, 어파인 변환 행렬은 다음과 같이 표현된다.

\[
  M_{translation} = \begin{bmatrix} 1 & 0 & a \\ 0 & 1 & b \end{bmatrix}
\]

```python
import cv2
import numpy as np

img = cv2.imread('image.jpg')
M = np.float32([[1, 0, 50], [0, 1, 100]])  # x 방향으로 50, y 방향으로 100 이동
translated = cv2.warpAffine(img, M, (img.shape[1], img.shape[0]))
```

---

# 3. 전단 변환 (Shear Transformation)

전단 변환(Shearing)은 영상을 **평행사변형 형태로 변형**시키는 변환이다.

### (1) x축 전단 변환
\[
  M_{shear-x} = \begin{bmatrix} 1 & m & 0 \\ 0 & 1 & 0 \end{bmatrix}
\]

```python
M = np.float32([[1, 0.2, 0], [0, 1, 0]])
sheared_x = cv2.warpAffine(img, M, (img.shape[1] + 100, img.shape[0]))
```

### (2) y축 전단 변환
\[
  M_{shear-y} = \begin{bmatrix} 1 & 0 & 0 \\ m & 1 & 0 \end{bmatrix}
\]

```python
M = np.float32([[1, 0, 0], [0.2, 1, 0]])
sheared_y = cv2.warpAffine(img, M, (img.shape[1], img.shape[0] + 100))
```

---

# 4. 크기 변환 (Scaling)

크기 변환(Scaling)은 영상을 확대 또는 축소하는 변환이다.

\[
  M_{scale} = \begin{bmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \end{bmatrix}
\]

```python
scaled = cv2.resize(img, None, fx=2, fy=2, interpolation=cv2.INTER_CUBIC)
```

---

# 5. 회전 변환 (Rotation)

회전 변환은 영상의 특정 기준점을 중심으로 일정 각도 회전시키는 연산이다.

\[
  M_{rotation} = \begin{bmatrix} \cos\theta & -\sin\theta & x_0 \\ \sin\theta & \cos\theta & y_0 \end{bmatrix}
\]

```python
M = cv2.getRotationMatrix2D((img.shape[1]//2, img.shape[0]//2), 45, 1)
rotated = cv2.warpAffine(img, M, (img.shape[1], img.shape[0]))
```

---

# 6. 대칭 변환 (Flip)

영상의 좌우 또는 상하를 뒤집는 대칭 변환이다.

- 좌우 대칭: `cv2.flip(img, 1)`
- 상하 대칭: `cv2.flip(img, 0)`

```python
flipped_lr = cv2.flip(img, 1)  # 좌우 대칭
flipped_ud = cv2.flip(img, 0)  # 상하 대칭
```

---

# 7. 투시 변환 (Perspective Transformation)

투시 변환은 직선의 평행 관계를 유지하지 않는 변환으로, **사다리꼴 형태의 영상도 정사각형으로 보정할 수 있다.**

```python
pts1 = np.float32([[50,50], [200,50], [50,200], [200,200]])
pts2 = np.float32([[10,100], [180,50], [50,250], [200,200]])
M = cv2.getPerspectiveTransform(pts1, pts2)
perspective = cv2.warpPerspective(img, M, (300,300))
```

---

# 8. 결론

영상의 기하학적 변환은 다양한 이미지 처리 및 컴퓨터 비전 응용에서 중요한 역할을 한다. 특히, **객체 정렬, 원근 왜곡 보정, 카메라 캘리브레이션** 등에서 필수적인 기술이다. 🚀

