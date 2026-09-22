# Machine Intelligence II: Computer Vision & Deep Learning Coursework
### Spatial Filtering, Harris/Blob Detection, RANSAC Panorama Stitching & Predictive Maintenance ANNs

**Course:** ME0572 - Machine Intelligence II  
**Instructor:** Dr. Ghaith Al-refai  
**Department:** Department of Mechatronics & Artificial Intelligence Engineering  
**Institution:** German Jordanian University (GJU)

---

## Overview

This repository archives the laboratory assignments, algorithms, and computational implementations developed for **ME0572: Machine Intelligence II**. The coursework spans classical computer vision (spatial convolution, feature matching, homography estimation) through modern deep learning for industrial diagnostics.

---

## Modules & Technical Implementations

### 1. Spatial Domain Image Filtering (`01-Image-Filtering/`)
- **Implementations:** Implements 2D spatial convolution from scratch and compares against OpenCV optimized filters.
- **Kernels Explored:**
  - Box and Gaussian Blur filters for high-frequency noise attenuation.
  - Sobel and Prewitt gradient operators ($\nabla I_x, \nabla I_y$) for directional edge extraction.
  - Laplacian of Gaussian (LoG) second-derivative zero-crossing operators for isotropic edge localization.
  - Unsharp masking and high-boost filtering for edge enhancement.
- **Artifacts:** Interactive Jupyter notebook (`Image_filtering.ipynb`) and documented report (`Image_filtering.pdf`).

### 2. Feature Point Detection: Corners & Blobs (`02-Corner-and-Blob-Detection/`)
- **Harris Corner Detection:** Computes the second-moment structure matrix $M = \sum w(x,y) \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix}$ and evaluates the corner response function:
  $$R = \det(M) - k \cdot (\text{trace}(M))^2$$
  with non-maximum suppression (NMS) to isolate distinct rotational invariant interest points.
- **Blob Detection:** Employs multi-scale Laplacian of Gaussian (LoG) and Difference of Gaussians (DoG) pyramid approximations to identify circular image features across scale space.
- **Artifacts:** Analytical laboratory report with response heatmaps (`Corner_Blob_Detection.pdf`).

### 3. Multi-Image Panorama Stitching with RANSAC (`03-Panorama-Image-Stitching/`)
- **End-to-End Image Mosaic Pipeline:**
  1. Keypoint extraction and descriptor matching via **SIFT** and **ORB**.
  2. Ratio test (Lowe's criterion) to prune ambiguous descriptor matches.
  3. **RANSAC (Random Sample Consensus)** geometric estimation to filter outlier pairs and robustly compute the 8-DOF Projective Homography matrix $H$:
     $$\mathbf{x}' \sim H \mathbf{x}$$
  4. Perspective warping, cylindrical projection, and multi-band feathering/blending to eliminate photometric seam artifacts.
- **Evaluation:** Tested on multi-angle campus scenes (German Jordanian University) and German landmarks (Schweinfurt and Würzburg).
- **Artifacts:** Interactive HTML notebook (`Stitching.html`), complete technical report (`Stitching.pdf`), input sequence photos, and generated panorama output (`step_by_step_final_panorama.jpg`).

### 4. Artificial Neural Networks for Predictive Maintenance (`04-ANN-Predictive-Maintenance/`)
- **Industrial Telemetry Dataset:** Utilizes sensor features (air temperature, process temperature, rotational speed, torque, tool wear) from `machine_failure_cleaned.csv` to diagnose multiple failure modes (heat dissipation, power failure, tool wear failure, overstrain).
- **Deep Architecture:** Multilayer Perceptron (MLP) with Batch Normalization, Dropout regularization, and cross-entropy loss.
- **Performance:** Evaluated via ROC-AUC curves, confusion matrices, and precision-recall trade-offs to minimize false-negative unplanned factory downtime.
- **Artifacts:** Technical results deck (`ANN_Machine_Failure_Results.pdf`) and cleaned dataset.

### 5. MediaPipe Vision Trigger & UART Telemetry (`05-MediaPipe-Vision-Trigger/`)
- Real-time gesture and hand landmark tracking pipeline utilizing Google MediaPipe (`hand_landmarker.task`).
- Triggers deterministic microcontroller state changes over serial UART (`uart_wait_trigger.py`).

---

## Repository Structure

```text
├── 01-Image-Filtering/
│   ├── Image_filtering.ipynb              # 2D convolution & gradient filtering
│   └── Image_filtering.pdf                # Visualized convolution outputs
├── 02-Corner-and-Blob-Detection/
│   └── Corner_Blob_Detection.pdf          # Harris response & DoG blob detection
├── 03-Panorama-Image-Stitching/
│   ├── Stitching.html                     # Interactive stitching notebook export
│   ├── Stitching.pdf                      # Comprehensive homography report
│   ├── step_by_step_final_panorama.jpg    # Output stitched panorama
│   └── inputs/                            # Multi-view source photos (GJU, Schweinfurt, Würzburg)
├── 04-ANN-Predictive-Maintenance/
│   ├── ANN_Machine_Failure_Results.pdf    # ROC curves & confusion matrices
│   └── machine_failure_cleaned.csv        # Preprocessed sensor telemetry dataset
├── 05-MediaPipe-Vision-Trigger/
│   ├── uart_wait_trigger.py               # Serial triggering script
│   └── hand_landmarker.task               # MediaPipe neural asset
└── README.md
```
---

## Quickstart & Running the Labs

All lab input images and datasets (`machine_failure_cleaned.csv`, `03-Panorama-Image-Stitching/inputs/`, `hand_landmarker.task`) are stored directly in this repository.

### Installation
```bash
git clone https://github.com/RamezM2004/machine-intelligence-2-coursework.git
cd machine-intelligence-2-coursework
pip install -r requirements.txt
```

### Running Jupyter Notebooks
```bash
jupyter notebook 01-Image-Filtering/Image_filtering.ipynb
```
