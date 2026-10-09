# Feature Point Detection: Corners & Blobs

This module investigates keypoint localization algorithms in spatial scale space, focusing on **Harris Corner Detection** and multi-scale **Blob Detection** using the Laplacian of Gaussian (LoG) and Difference of Gaussians (DoG).

---

## Theoretical Formulation

### 1. Harris Corner Detection
Harris corner detection identifies image locations where the local image intensity changes significantly in all spatial directions. The second-moment structure tensor matrix $M$ is computed over a local window $w(x,y)$:

$$
M = \sum_{(x,y)} w(x,y) \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix}
$$

where $I_x$ and $I_y$ are the spatial image gradients computed via Sobel operators. The corner response measure $R$ evaluates the eigenvalues of $M$ without explicit eigenvalue decomposition:

$$
R = \det(M) - k \cdot (\operatorname{trace}(M))^2
$$

where $\det(M) = \lambda_1 \lambda_2$, $\operatorname{trace}(M) = \lambda_1 + \lambda_2$, and $k$ is an empirical parameter (typically $0.04 \le k \le 0.06$). Non-maximum suppression (NMS) is applied to retain local maxima corresponding to salient corners.

### 2. Multi-Scale Blob Detection
Blobs represent image regions that are either darker or brighter than their surrounding environment. The scale-normalized Laplacian of Gaussian:

$$
\nabla_{\text{norm}}^2 G(x,y;\sigma) = \sigma^2 \left( \frac{\partial^2 G}{\partial x^2} + \frac{\partial^2 G}{\partial y^2} \right)
$$

is approximated efficiently using Difference of Gaussians (DoG) scale-space pyramids:

$$
D(x,y;\sigma) = (G(x,y; k\sigma) - G(x,y; \sigma)) * I(x,y)
$$

---

## Experimental Findings

- **Scale Invariance Analysis:** Tested on the Schweinfurt Town Hall landmark (`Schweinfurt.jpg`) at $1.0\times$ scale versus $1.5\times$ scale. The experiment demonstrates that standard Harris corner detection is **rotationally invariant** but **scale variant** (scaling the image alters the effective window size relative to physical corner features, resulting in fewer detected corners at $1.5\times$ scale unless scale-space adaptation is applied).
- **Blob Extrema:** Demonstrates scale selection where the radius of circular features is accurately tracked by the characteristic scale $\sigma_{\text{max}} = r / \sqrt{2}$.

---

## Files in this Directory

| File | Description |
| :--- | :--- |
| [`HW2.ipynb`](HW2.ipynb) | **Interactive Jupyter Notebook** with Harris corner detection, scale comparisons, and blob extraction. |
| [`Corner_Blob_Detection.pdf`](Corner_Blob_Detection.pdf) | **Documented Laboratory Report** with response heatmaps and threshold analyses. |
| [`Schweinfurt.jpg`](Schweinfurt.jpg) | High-contrast landmark image used for corner and blob feature evaluation. |

---

## Running the Notebook

```bash
# From repository root
cd 02-Corner-and-Blob-Detection
jupyter notebook HW2.ipynb
```
