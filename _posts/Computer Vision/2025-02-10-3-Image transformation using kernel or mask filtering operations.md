---
title: "[영상처리 기초] OpenCV와 영상 필터링"
last_modified_at: 2025-02-10
categories:
  - 영상처리 기초
  - 컴퓨터비전

tags:
  - Computer Vision
  - Image Processing
  - OpenCV
  - 영상처리

excerpt: "OpenCV를 활용한 영상 필터링 기법(엠보싱, 블러링, 샤프닝, 잡음 제거)에 대해 설명합니다."
use_math: true
classes: wide
math_engine: katex
---

# 1. 영상 필터링 개요

영상 필터링은 원하는 특징을 강조하고 불필요한 정보를 제거하는 과정이다. 필터링의 핵심은 **컨볼루션(Convolution)** 연산으로, 작은 **커널(Kernel)** 행렬을 활용하여 이미지에서 특정 특징을 강조하거나 제거한다.

필터링의 대표적인 예시:
- **엠보싱(Embossing):** 윤곽선 강조
- **블러링(Blurring):** 노이즈 제거 및 부드러운 효과
- **샤프닝(Sharpening):** 경계를 뚜렷하게 만듦
- **잡음 제거(Denoising):** 불필요한 신호 제거

# 2. 엠보싱 필터링

엠보싱(Embossing)은 영상의 윤곽을 강조하여 3D 효과를 부여하는 기법이다. 필터링 과정에서 픽셀 값 변화가 적은 평탄한 영역은 **회색(128로 설정)**이 되고, 경계 부분은 더 밝거나 어두운 값으로 변환된다.

### (1) 엠보싱 커널 정의

\[
  K_{emboss} = \begin{bmatrix} -1 & -1 & 0 \\ -1 & 0 & 1 \\ 0 & 1 & 1 \end{bmatrix}
\]

### (2) OpenCV를 활용한 엠보싱 적용

```python
import cv2
import numpy as np

img = cv2.imread('image.jpg', 0)
kernel = np.array([[-1, -1, 0], [-1, 0, 1], [0, 1, 1]])

dst = cv2.filter2D(img, -1, kernel) + 128  # 엠보싱 효과 적용 후 중간값 보정

cv2.imshow('Embossed Image', dst)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

---

# 3. 블러링 (Blurring)

블러링(Blurring)은 영상의 선명도를 낮추고 노이즈를 제거하는 데 사용된다. 대표적인 블러링 기법에는 **평균값 필터(Mean Filter)**, **가우시안 필터(Gaussian Filter)** 등이 있다.

## (1) 평균값 필터 (Mean Filter)

평균값 필터는 주변 픽셀 값의 평균을 계산하여 블러링을 수행한다.

```python
kernel = np.ones((3,3), np.float32) / 9
img_blurred = cv2.filter2D(img, -1, kernel)
```

OpenCV의 `cv2.blur()` 함수를 활용할 수도 있다.

```python
img_blurred = cv2.blur(img, (3,3))
```

## (2) 가우시안 필터 (Gaussian Filter)

가우시안 필터는 **정규 분포(가우시안 커브)**를 적용하여 블러링을 수행하며, 평균값 필터보다 자연스러운 효과를 제공한다.

```python
img_gaussian = cv2.GaussianBlur(img, (5,5), 2)
```

---

# 4. 샤프닝 (Sharpening)

샤프닝(Sharpening)은 영상을 더 선명하게 만들며, 특히 경계를 강조하는 데 효과적이다.

## (1) 언샤프 마스킹 (Unsharp Masking)

샤프닝을 위해 먼저 블러링을 수행한 후, 블러된 영상을 원본 영상에서 빼는 방식이다.

\[
  h(x) = f(x) + (f(x) - f'(x))
\]

```python
img_blur = cv2.GaussianBlur(img, (5,5), 5)
img_sharpened = np.clip(2.0 * img - img_blur, 0, 255).astype('uint8')
```

---

# 5. 잡음 제거 필터링

잡음(Noise)은 원본 신호에 추가된 불필요한 신호로, 대표적인 잡음 제거 필터로는 **양방향 필터(Bilateral Filter)**, **미디언 필터(Median Filter)** 등이 있다.

## (1) 양방향 필터 (Bilateral Filter)

양방향 필터는 경계선을 유지하면서 노이즈만 제거하는 필터링 방식이다.

```python
img_denoised = cv2.bilateralFilter(img, 9, 100, 100)
```

## (2) 미디언 필터 (Median Filter)

미디언 필터는 주변 픽셀 값의 **중앙값**을 사용하여 잡음을 제거하는 방식으로, **소금과 후추 잡음(Salt & Pepper Noise)** 제거에 효과적이다.

```python
img_median = cv2.medianBlur(img, 3)
```

---