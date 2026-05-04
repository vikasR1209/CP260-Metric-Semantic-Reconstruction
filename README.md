# Metric-Semantic 3D Reconstruction for a Desktop Scene

## Overview

This project develops a complete metric-semantic reconstruction pipeline for estimating **3D Oriented Bounding Boxes (OBBs)** of small components present in a desktop scene using multi-view RGB images.

Rather than depending on large deep learning-based detection models, this work uses a **geometry-first method**. The pipeline mainly applies multi-view triangulation and classical computer vision techniques to obtain accurate 3D localization of the target objects.

---

## Problem Statement

Input provided:

* 16 RGB images with known camera poses and 2560 × 1440 resolution
* Camera intrinsic parameters and camera-to-world pose information

Objective:

* Estimate 3D OBBs for:

  * `ethernet_socket`
  * `power_socket`
* Validate the reconstruction pipeline using:

  * `vga_socket` with provided ground truth

---

## Approach

The complete pipeline is carried out through the following steps:

1. **Manual 2D Annotation**

   * Bounding boxes are manually marked on two selected image frames

2. **Multi-View Triangulation**

   * Grid-based correspondences are generated within the annotated bounding boxes
   * DLT (Direct Linear Transform) is applied to reconstruct 3D points

3. **Outlier Removal**

   * Median Absolute Deviation (MAD) filtering is used to remove noisy points

4. **OBB Fitting**

   * PCA-based orientation estimation is used for fitting the oriented bounding boxes
   * A depth prior is applied because the connector regions are nearly planar

5. **Output Generation**

   * Final results are exported in the required `answers.json` format

---

## Key Features

* Does not depend on object detection models such as GroundingDINO or SAM
* Lightweight and fast pipeline that can run completely on Google Colab
* Modular implementation in Python
* Works effectively for small-sized object localization
* Achieves **< 5 cm center error** during validation

---

## Repository Structure

```text
project/
├── src/
│   ├── config.py
│   ├── data_loader.py
│   ├── semantic.py
│   ├── pose_estimation.py
│   ├── reconstruction.py
│   ├── utils.py
│   └── run_pipeline.py
│
├── notebook/
│   └── final_pipeline.ipynb
│
├── outputs/
│   ├── answers.json
│   └── transforms.json
│
├── report/
│   └── report.pdf
│
├── requirements.txt
└── README.md
```

---

## How to Run

### Option 1: Google Colab (Recommended)

Open the notebook file:

```text
notebook/final_pipeline.ipynb
```

Run all cells one by one in sequence.

---

### Option 2: Local Execution

```bash
pip install -r requirements.txt
python src/run_pipeline.py
```

---

## Results

* Successfully reconstructs the connector ports in 3D
* VGA socket ground truth is used for validating the pipeline
* Sub-centimeter level accuracy is achieved

### Output Format

```json
{
  "entity": "ethernet_socket",
  "obb": {
    "center": [cx, cy, cz],
    "extent": [ex, ey, ez],
    "rotation": [
      [r00, r01, r02],
      [r10, r11, r12],
      [r20, r21, r22]
    ]
  }
}
```

---

## Dependencies

* open3d==0.19.0
* numpy
* opencv-python
* scipy
* matplotlib
* tqdm

---

## Notes

* The dataset is not included because of size limitations
* Camera poses are provided in `poses.json`
* A 6 mm depth prior is used for all connector types

---

## Future Improvements

* Automate the 2D annotation process using segmentation models
* Improve robustness by using triangulation from multiple frames
* Extend the pipeline toward complete scene reconstruction

---

## Author

Vikas Rajpoot (27442), Karlosh Yadav (27506)  
CP260 — Robotic Perception (2026)
