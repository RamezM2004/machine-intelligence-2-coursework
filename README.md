# Machine Intelligence II: Computer Vision, Deep Learning, and Accepted PCB Defect Detection Paper
### ICFT-Accepted YOLOv11 PCB Inspection Study plus Computer Vision Coursework

**Course:** ME0572 - Machine Intelligence II  
**Instructor:** Dr. Ghaith Al-refai  
**Department:** Department of Mechatronics & Artificial Intelligence Engineering  
**Institution:** German Jordanian University (GJU)

---

## Overview

This repository archives the laboratory assignments, algorithms, and computational implementations developed for **ME0572: Machine Intelligence II**. It also links the course project that was developed into an accepted ICFT conference paper on YOLOv11-based PCB surface defect detection.

---

## Modules & Technical Implementations

### 0. Accepted ICFT Conference Paper: YOLOv11 PCB Defect Detection
- **Project:** Stage-wise evaluation of YOLOv11s for PCB surface defect detection.
- **Status:** Accepted at the ICFT conference.
- **Scope:** Evaluated baseline YOLOv11s against input-level, backbone-level, head-level, and inference-level modifications on the DsPCBSD+ PCB defect dataset.
- **Techniques:** YOLOv11, SAHI sliced inference, Coordinate Attention, CBAM, P2 small-object head experiments, rare-class oversampling, and high-resolution training.
- **Related repository:** `yolov11-pcb-defect-detection`.

### 1. Spatial Domain Image Filtering (`01-Image-Filtering/`)
- **Implementations:** Implements 2D spatial convolution from scratch and compares against OpenCV optimized filters.
- **Kernels Explored:**
  - Box and Gaussian Blur filters for high-frequency noise attenuation.
  - Sobel and Prewitt gradient operators ($\nabla I_x, \nabla I_y$) for directional edge extraction.
  - Laplacian of Gaussian (LoG) second-derivative zero-crossing operators for isotropic edge localization.
  - Unsharp masking and high-boost filtering for edge enhancement.
- **Artifacts:** Interactive Jupyter notebook ([`Image_filtering.ipynb`](01-Image-Filtering/Image_filtering.ipynb)) and documented report ([`Image_filtering.pdf`](01-Image-Filtering/Image_filtering.pdf)).

### 2. Feature Point Detection: Corners & Blobs (`02-Corner-and-Blob-Detection/`)
- **Harris Corner Detection:** Computes the second-moment structure tensor matrix $M$:

  $$
  M = \sum_{(x,y)} w(x,y) \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix}
  $$

  and evaluates the corner response function $R$:

  $$
  R = \det(M) - k \cdot (\operatorname{trace}(M))^2
  $$

  with non-maximum suppression (NMS) to isolate distinct rotationally invariant interest points.
- **Scale-Space Analysis:** Evaluates corner stability under image scale variations on `Schweinfurt.jpg` ($1.0\times$ vs. $1.5\times$ scale).
- **Blob Detection:** Employs multi-scale Laplacian of Gaussian (LoG) and Difference of Gaussians (DoG) scale-space pyramids to identify circular image features across scale space.
- **Artifacts:** Documented laboratory report ([`Corner_Blob_Detection.pdf`](02-Corner-and-Blob-Detection/Corner_Blob_Detection.pdf)) and interactive Jupyter notebook ([`HW2.ipynb`](02-Corner-and-Blob-Detection/HW2.ipynb)).

### 3. Multi-Image Panorama Stitching with RANSAC (`03-Panorama-Image-Stitching/`)
- **End-to-End Image Mosaic Pipeline:**
  1. Keypoint extraction and descriptor matching via **SIFT** and **ORB**.
  2. Ratio test (Lowe's criterion, $d_1 < 0.65 d_2$) to prune ambiguous descriptor matches.
  3. **RANSAC (Random Sample Consensus)** geometric estimation to filter outlier pairs and robustly compute the 8-DOF Projective Homography matrix $H$:
     $$\mathbf{x}' \sim H \mathbf{x}$$
  4. Perspective warping and Euclidean **distance transform feather blending** ($L_2$ alpha gradient) to eliminate photometric seams and exposure boundaries.
- **Pipeline Demonstration:** Stitches 3 sequential overlapping views of St. Michael's Church (Würzburg, Germany) into a seamless high-resolution composite:

| Input 1: Left (`inputs/pt2.jpg`) | Input 2: Middle (`inputs/pt1.jpg`) | Input 3: Right (`inputs/pt3.jpg`) | Final Blended Panorama (`final_stitched_panorama.jpg`) |
| :---: | :---: | :---: | :---: |
| <img src="03-Panorama-Image-Stitching/inputs/pt2.jpg" width="130" alt="Left View (pt2)"/> | <img src="03-Panorama-Image-Stitching/inputs/pt1.jpg" width="130" alt="Middle View (pt1)"/> | <img src="03-Panorama-Image-Stitching/inputs/pt3.jpg" width="130" alt="Right View (pt3)"/> | <img src="03-Panorama-Image-Stitching/final_stitched_panorama.jpg" width="280" alt="Final Stitched Panorama"/> |

- **Artifacts & Notebook Viewing:**
  - **Interactive Notebook:** [`Stitching.ipynb`](03-Panorama-Image-Stitching/Stitching.ipynb) (natively renders directly inside GitHub).
  - **Comprehensive Report:** [`Stitching.pdf`](03-Panorama-Image-Stitching/Stitching.pdf) (53 pages with SIFT vs. ORB comparisons and blending proofs).
  - **HTML Export:** [`Stitching.html`](03-Panorama-Image-Stitching/Stitching.html) (*Note:* GitHub treats large HTML files as raw source. To view interactively in your browser without cloning, use [HTMLPreview](https://htmlpreview.github.io/?https://github.com/RamezM2004/machine-intelligence-2-coursework/blob/main/03-Panorama-Image-Stitching/Stitching.html) or run locally).
  - **High-Resolution Composite:** [`final_stitched_panorama.jpg`](03-Panorama-Image-Stitching/final_stitched_panorama.jpg) ($1848 \times 4002\text{ px}$).

### 4. Artificial Neural Networks for Predictive Maintenance (`04-ANN-Predictive-Maintenance/`)
- **Industrial Telemetry Dataset:** Utilizes sensor features (air temperature, process temperature, rotational speed, torque, tool wear) from `machine_failure_cleaned.csv` to diagnose multiple failure modes (heat dissipation, power failure, tool wear failure, overstrain).
- **Deep Architecture:** Multilayer Perceptron (MLP) with Batch Normalization, Dropout regularization, and cross-entropy loss.
- **Performance:** Evaluated via ROC-AUC curves, confusion matrices, and precision-recall trade-offs to minimize false-negative unplanned factory downtime.
- **Artifacts:** Technical results deck ([`ANN_Machine_Failure_Results.pdf`](04-ANN-Predictive-Maintenance/ANN_Machine_Failure_Results.pdf)) and cleaned dataset ([`machine_failure_cleaned.csv`](04-ANN-Predictive-Maintenance/machine_failure_cleaned.csv)).

---

## Repository Structure

```text
├── 01-Image-Filtering/
│   ├── Image_filtering.ipynb              # 2D convolution & gradient filtering
│   └── Image_filtering.pdf                # Visualized convolution outputs
├── 02-Corner-and-Blob-Detection/
│   ├── HW2.ipynb                          # Harris corner & DoG blob detection notebook
│   ├── Corner_Blob_Detection.pdf          # Harris response & DoG blob report
│   ├── Schweinfurt.jpg                    # Landmark image for scale analysis
│   └── README.md                          # Detailed corner & blob detection docs
├── 03-Panorama-Image-Stitching/
│   ├── Stitching.ipynb                    # Interactive stitching notebook (natively viewable on GitHub)
│   ├── Stitching.pdf                      # Comprehensive 53-page homography & blending report
│   ├── Stitching.html                     # Jupyter HTML export (view via HTMLPreview or browser)
│   ├── final_stitched_panorama.jpg        # Output high-resolution stitched panorama (1848x4002 px)
│   ├── FINAL_ULTIMATE_PANORAMA.jpg        # Match notebook final output artifact
│   ├── step_by_step_final_panorama.jpg    # Verified composite matching final pipeline
│   ├── README.md                          # Detailed panorama stitching documentation
│   └── inputs/                            # Sequential exposures & reference campus photos
│       ├── pt1.jpg                        # Würzburg Center slice
│       ├── pt2.jpg                        # Würzburg Left slice
│       ├── pt3.jpg                        # Würzburg Right slice
│       ├── Schweinfurt.jpg                # Schweinfurt landmark
│       ├── Wurzburg.jpg                   # Full Würzburg view
│       ├── GJU.jpg                        # GJU campus photo
│       ├── Part1.jpg                      # GJU multi-angle photo 1
│       ├── Part2.jpg                      # GJU multi-angle photo 2
│       └── Part3.jpg                      # GJU multi-angle photo 3
├── 04-ANN-Predictive-Maintenance/
│   ├── ANN_Machine_Failure_Results.pdf    # ROC curves & confusion matrices
│   └── machine_failure_cleaned.csv        # Preprocessed sensor telemetry dataset
├── requirements.txt
└── README.md
```

---

## Quickstart & Running the Labs

All lab input images and datasets (`machine_failure_cleaned.csv`, `03-Panorama-Image-Stitching/inputs/`) are stored directly in this repository.

### Installation
```bash
git clone https://github.com/RamezM2004/machine-intelligence-2-coursework.git
cd machine-intelligence-2-coursework
pip install -r requirements.txt
```

### Running Jupyter Notebooks
```bash
# Lab 1: Image Filtering
jupyter notebook 01-Image-Filtering/Image_filtering.ipynb

# Lab 2: Corner & Blob Detection
jupyter notebook 02-Corner-and-Blob-Detection/HW2.ipynb

# Lab 3: Panorama Image Stitching
jupyter notebook 03-Panorama-Image-Stitching/Stitching.ipynb
```
