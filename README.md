```markdown
# YOLOv8 + DeepSORT: Custom Object Detection & Multi-Object Tracking

End-to-end computer vision pipeline for **real-time object detection** and **multi-object tracking** using **YOLOv8-Large** and **DeepSORT**, trained on a custom 10-class vehicle dataset.

![Python](https://img.shields.io/badge/Python-3.8-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-1.13-orange)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Large-red)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📌 Overview

This project implements a complete pipeline for detecting and tracking multiple objects (vehicles) in video streams. It combines:

- **YOLOv8-Large** for high-accuracy object detection
- **DeepSORT** for persistent multi-object tracking across frames
- **Kalman Filtering + Hungarian Algorithm** for motion prediction and track association
- **ResNet-based deep appearance features** for re-identification

---

## 🎯 Key Results

| Metric | Value |
|--------|-------|
| **mAP@0.5** | **76.6%** |
| **mAP@0.5:0.95** | **41.7%** |
| **Precision** | 81.6% |
| **Recall** | 67.2% |
| **Inference Speed** | ~16.4 ms/image (Tesla T4) |
| **Training Time** | ~3.1 hours (50 epochs) |

### Per-Class Performance (Validation Set)

| Class | Precision | Recall | mAP@0.5 |
|-------|-----------|--------|---------|
| car | 0.881 | 0.917 | **0.910** |
| motorcycle | 0.876 | 0.678 | 0.842 |
| pickup | 0.773 | 0.660 | 0.723 |
| van | 0.832 | 0.658 | 0.701 |
| truck | 0.674 | 0.560 | 0.627 |
| camping car | 0.866 | 0.667 | 0.735 |
| tractor | 1.000 | 0.481 | 0.737 |
| boat | 0.472 | 1.000 | 0.995 |
| plane | 0.891 | 1.000 | 0.995 |
| other | 0.321 | 0.250 | 0.373 |

---

## 🛠️ Tech Stack

- **Detection Model:** YOLOv8-Large (43.6M params, 165.4 GFLOPs)
- **Tracking:** DeepSORT (Kalman filter + deep appearance features)
- **Framework:** PyTorch, Ultralytics
- **Hardware:** NVIDIA Tesla T4 GPU
- **Dataset:** 4,051 training / 619 validation images (10 classes)
- **Dataset Source:** Custom dataset hosted on [Roboflow](https://roboflow.com)

---

## 📂 Dataset

- **Classes (10):** `boat`, `camping car`, `car`, `motorcycle`, `other`, `pickup`, `plane`, `tractor`, `truck`, `van`
- **Split:** ~4,051 train / 619 valid images
- **Format:** YOLOv5 (YOLOv8-compatible) format
- **Source:** Roboflow — workspace `drone-dataset-mvh8i`, project `detection-bzujh`

---

## 🚀 Pipeline

### 1. Environment Setup
- Enable **GPU runtime** (CUDA)
- Clone the `YOLOv8-DeepSORT-Object-Tracking` repository
- Install dependencies: `pip install -e '.[dev]'`

### 2. Dataset Acquisition
- Pull the custom dataset from Roboflow using the `roboflow` Python SDK
- Data auto-formatted for YOLOv8 training

### 3. DeepSORT Setup
- Download and extract DeepSORT files, including:
  - `ckpt.t7` — pretrained feature extractor
  - Kalman filter & Hungarian assignment modules
  - `deep_sort.yaml` config

### 4. Model Training

**Training Configuration:**

```text
Model       : yolov8l.pt (pretrained on COCO)
Epochs      : 50
Image Size  : 640 x 640
Batch Size  : 16
Optimizer   : SGD (lr = 0.01)
Device      : NVIDIA Tesla T4 (CUDA)
