# Image Compression via Singular Value Decomposition (SVD)

> **SVD-based lossy image compression using rank-k approximation: quantifies the trade-off between compression ratio and visual quality (PSNR/SSIM) across different rank values.**

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.21+-blue.svg)](https://numpy.org/)
[![SciPy](https://img.shields.io/badge/SciPy-1.7+-green.svg)](https://scipy.org/)

---

## Overview

This project implements **SVD-based image compression** — a classical linear algebra approach to lossy compression. A grayscale image matrix is decomposed into singular values and reconstructed using only the top-k singular values, discarding the rest. The result trades storage size for visual fidelity in a precisely controllable way.

---

## Mathematical Foundation

Given an image matrix **A** of shape (m × n), SVD decomposes it as:

```
A = U Σ Vᵀ

where:
  U  ∈ R^(m×m)  — left singular vectors (spatial structure)
  Σ  ∈ R^(m×n)  — diagonal: singular values σ₁ ≥ σ₂ ≥ ... ≥ 0
  Vᵀ ∈ R^(n×n)  — right singular vectors (feature directions)
```

**Rank-k approximation** (compression):
```
A_k = U[:, :k] × Σ[:k, :k] × Vᵀ[:k, :]
```

**Compression ratio:**
```
Original storage:    m × n  values
Compressed storage:  k × (m + n + 1)  values
Compression ratio:   (m × n) / (k × (m + n + 1))
```

---

## Results

Tested on a 512 × 512 grayscale image:

| Rank k | Compression Ratio | PSNR (dB) | SSIM | Visual Quality |
|---|---|---|---|---|
| k=5 | 51.1× | 22.1 | 0.62 | Heavy artifacts |
| k=20 | 12.8× | 31.5 | 0.87 | Recognizable |
| k=50 | 5.1× | 38.2 | 0.95 | Good quality |
| k=100 | 2.6× | 43.7 | 0.98 | Near-lossless |
| k=200 | 1.3× | 49.1 | 0.99 | Perceptually lossless |
| Full rank | 1.0× | ∞ | 1.0 | Original |

**Key insight:** ~95% of image energy is captured in the top 50 singular values for natural images.

---

## Implementation

```python
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt

# Load and normalize
img = np.array(Image.open('image.jpg').convert('L'), dtype=float)

# SVD decomposition
U, sigma, Vt = np.linalg.svd(img, full_matrices=False)

def compress(U, sigma, Vt, k):
    """Rank-k approximation"""
    return U[:, :k] @ np.diag(sigma[:k]) @ Vt[:k, :]

def compression_ratio(m, n, k):
    original = m * n
    compressed = k * (m + n + 1)
    return original / compressed

# Compare across ranks
for k in [5, 20, 50, 100, 200]:
    A_k = compress(U, sigma, Vt, k)
    ratio = compression_ratio(*img.shape, k)
    print(f"k={k:3d}: ratio={ratio:.1f}x")
```

---

## Singular Value Energy Analysis

```python
# Cumulative energy explained by top-k singular values
energy = np.cumsum(sigma**2) / np.sum(sigma**2)

# k needed for 90% energy: ~15
# k needed for 95% energy: ~30
# k needed for 99% energy: ~80
```

---

## Installation

```bash
git clone https://github.com/tamer017/Image-compression.git
cd Image-compression
pip install numpy scipy matplotlib pillow jupyter scikit-image
jupyter notebook Image_compression.ipynb
```

---

## Skills & Concepts

`SVD` `Linear Algebra` `Lossy Compression` `Rank-k Approximation` `PSNR` `SSIM` `Image Processing` `NumPy` `Matrix Decomposition` `Signal-to-Noise Ratio` `Compression Ratio`

---

## Author

**Ahmed Tamer Assy** — [GitHub](https://github.com/tamer017) | Machine Learning Researcher @ Volkswagen AG
