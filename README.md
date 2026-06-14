# Image Compression via K-Means Color Clustering

> Achieve dramatic storage savings by reducing a 24-bit RGB image palette to just **16 colors** using **k-means clustering** on the Van Gogh color palette dataset.

[![Method](https://img.shields.io/badge/Method-K--Means%20Clustering-blue?style=flat-square)]()
[![Language](https://img.shields.io/badge/Language-Python%2FJupyter-green?style=flat-square)]()
[![Compression](https://img.shields.io/badge/Compression%20Ratio-6×-brightgreen?style=flat-square)]()

---

## Overview

This project demonstrates a powerful application of **unsupervised machine learning** to image compression. By applying the k-means clustering algorithm to the **Van Gogh Colors dataset** (986 distinct colors), the system learns a 16-color palette that best represents the input image’s visual content.

Each pixel in the original 24-bit RGB image (3 bytes per pixel) is replaced with a 4-bit index into this 16-color dictionary — reducing per-pixel storage from **24 bits to 4 bits**, achieving a **6× compression ratio** while preserving visual fidelity.

---

## Compression Methodology

### The Math

| Representation | Bits per pixel | Total (500×500 image) |
|---|---|---|
| Original 24-bit RGB | 24 bits/px | 6,000,000 bits |
| Compressed (4-bit indexed) | 4 bits/px + 16-color dict | **1,000,384 bits** |
| **Compression ratio** | — | **~6×** |

The dictionary overhead is only `16 colors × 24 bits = 384 bits` — negligible compared to the pixel data savings.

### Algorithm

```
[Van Gogh Colors Dataset: 986 RGB colors]
              |
              v
   [K-Means Clustering: k=16]
   Minimize intra-cluster color variance
              |
              v
   [16 Cluster Centroids = Compressed Palette]
              |
              v
   [Input Image: 500×500 pixels, 24-bit RGB]
              |
              v
   [Assign each pixel to nearest centroid]
   Euclidean distance in RGB space
              |
              v
   [Output: 4-bit indexed image + 16-color dictionary]
```

---

## Visual Results

The compressed images maintain strong visual similarity to the originals:

- **Upper row:** Original 24-bit RGB images
- **Lower row:** K-means compressed images (16 colors)

The Van Gogh palette clusters produce warm, impressionistic color groupings that work particularly well for natural and artistic imagery.

---

## Technical Highlights

### K-Means Color Quantization
- Implements Lloyd’s algorithm for iterative centroid optimization in RGB color space
- Convergence criterion: centroid movement < threshold across iterations
- Initialization: K-Means++ seeding for better convergence and palette quality

### Storage Architecture
```
Compressed File Format:
├── Header: [width: 2B] [height: 2B]
├── Dictionary: [16 × RGB triplets: 48B]
└── Pixel Data: [500×500 × 4-bit indices: 125,000B]
```

### Dataset: Van Gogh Colors
- 986 unique RGB colors sampled from Van Gogh’s paintings
- Provides a warm, organic color distribution ideal for natural image quantization
- K-means on this dataset finds perceptually meaningful clusters

---

## Getting Started

```bash
# Clone the repository
git clone https://github.com/tamer017/Image-compression.git
cd Image-compression

# Install dependencies
pip install numpy matplotlib scikit-learn Pillow jupyter

# Launch the notebook
jupyter notebook Image_Compression.ipynb
```

---

## Skills Demonstrated

- **Unsupervised Learning:** K-Means clustering, K-Means++ initialization, convergence analysis
- **Image Processing:** Pixel-level manipulation, indexed color representation, palettization
- **Information Theory:** Compression ratio calculation, storage overhead analysis
- **Python:** NumPy array operations, Matplotlib visualization, Pillow image I/O
- **Applied Mathematics:** Euclidean distance in RGB space, centroid optimization

---

## References

- Lloyd, S.P. (1982). *Least squares quantization in PCM*. IEEE Transactions on Information Theory.
- Arthur, D. & Vassilvitskii, S. (2007). *k-means++: The advantages of careful seeding*. SODA.
