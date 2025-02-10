---
title: "[영상처리 기초] OpenCV와 영상 밝기 및 명암 조절"
last_modified_at: 2025-02-10
categories:
  - 영상처리 기초
  - 컴퓨터비전

tags:
  - Computer Vision
  - Image Processing
  - OpenCV
  - 영상처리

excerpt: "OpenCV를 활용한 영상의 밝기 조절, 명암비 조정 및 히스토그램 분석 방법을 설명합니다."
use_math: true
classes: wide
math_engine: katex
---

# 1. 영상의 밝기 조절

픽셀 값이 255에 가까울수록 밝고, 0에 가까울수록 어두워진다. 이 점을 활용하여 픽셀 값에 일정한 값을 더하거나 빼면 영상의 밝기를 조절할 수 있다.

## (1) 기본적인 밝기 조절

```python
import cv2
import numpy as np

img = cv2.imread('image.jpg')
img_brighter = img + 20  # 영상 밝게 처리
img_darker = img - 20  # 영상 어둡게 처리
```

이 방식은 단순하지만 **픽셀 값이 0보다 작거나 255보다 클 경우 문제가 발생**할 수 있다.

## (2) 포화 연산 (Saturation Operation)

포화 연산은 픽셀 값이 255를 초과하면 255로, 0 미만이면 0으로 제한하는 연산이다. OpenCV에서는 `np.clip()`을 사용하여 구현할 수 있다.

```python
arr = np.array([-12, 213, 307])
arr_clipped = np.clip(arr, 0, 255)
print(arr_clipped)  # 결과: [  0 213 255]
```

영상 처리에서 포화 연산을 적용하는 방법:

```python
img = cv2.imread('image.jpg')
img_adjusted = np.clip(img + 100, 0, 255)  # 밝기 조절 후 포화 연산 적용
```

# 2. 명암비(대비) 조절

명암비(Contrast)는 **밝은 부분과 어두운 부분 간의 밝기 차이**를 의미한다. 명암비를 조절하면 이미지의 윤곽이 뚜렷해지고, 세부 정보가 강조된다.

## (1) 기본적인 명암비 조절

영상의 픽셀 값에 특정 계수를 곱하면 명암비가 조절된다.

```python
def adjust_contrast(image, alpha):
    img = image.astype('int32')
    img = np.clip(img * alpha, 0, 255)
    return img.astype('uint8')

img = cv2.imread('image.jpg')
img_high_contrast = adjust_contrast(img, 2)  # 대비 증가
img_low_contrast = adjust_contrast(img, 0.5)  # 대비 감소
```

## (2) 명암비 조절 공식

밝은 부분을 더 밝게, 어두운 부분을 더 어둡게 만들기 위해 **128을 중심으로 대비를 조절**하는 방식을 사용한다.

\[
 dst(x, y) = src(x, y) + (src(x, y) - 128) * \alpha
\]

- \(\alpha > 0\) : 대비 증가 (더 밝고 어두운 부분이 강조됨)
- \(\alpha < 0\) : 대비 감소 (밝기 차이 축소됨)

```python
def contrast_stretching(image, alpha):
    img = image.astype('int32')
    img = np.clip(img + (img - 128) * alpha, 0, 255)
    return img.astype('uint8')

img_high_contrast = contrast_stretching(img, 2)
img_low_contrast = contrast_stretching(img, -0.5)
```

# 3. 히스토그램 분석

히스토그램은 영상의 **픽셀 값 분포**를 나타내는 그래프이며, 영상의 밝기 및 명암비를 조절하는 데 활용된다.

## (1) 히스토그램 생성

OpenCV의 `cv2.calcHist()`를 이용하여 영상의 히스토그램을 생성할 수 있다.

```python
import cv2
import matplotlib.pyplot as plt

img = cv2.imread('image.jpg', 0)  # 그레이스케일로 읽기
hist = cv2.calcHist([img], [0], None, [256], [0, 256])

plt.plot(hist)
plt.xlim([0, 255])
plt.show()
```

## (2) 히스토그램 스트레칭 (Histogram Stretching)

히스토그램이 특정 영역에 집중되어 있을 경우 **밝기 범위를 확장하여 대비를 개선**할 수 있다.

```python
def histogram_stretching(image):
    f_min = image.min()
    f_max = image.max()
    img_stretched = np.clip((image - f_min) / (f_max - f_min) * 255, 0, 255)
    return img_stretched.astype('uint8')

img_stretched = histogram_stretching(img)
```

## (3) 히스토그램 평활화 (Histogram Equalization)

히스토그램 평활화는 **픽셀 분포를 균등하게 만들어 대비를 개선**하는 방법이다.

```python
img_equalized = cv2.equalizeHist(img)
```

평활화된 영상의 히스토그램을 확인할 수 있다.

```python
hist_eq = cv2.calcHist([img_equalized], [0], None, [256], [0, 256])
plt.plot(hist_eq)
plt.show()
```

---