# YOLOv8 + DeepSORT Object Tracking

Real-time object detection and multi-object tracking on a custom vehicle dataset, powered by YOLOv8-Large and DeepSORT.

![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-1.13%2B-orange)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Large-red)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-success)

---

## Table of Contents

- [Overview](#overview)
- [Key Results](#key-results)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
  - [Training](#training)
  - [Tracking](#tracking)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Metrics](#metrics)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)
- [Contact](#contact)

---

## Overview

This repository provides an end-to-end computer vision pipeline for detecting and tracking multiple objects in video streams. It combines:

- **YOLOv8-Large** for high-accuracy object detection
- **DeepSORT** for persistent multi-object tracking across frames
- **Kalman filtering** and the **Hungarian algorithm** for motion prediction and track association
- **ResNet-based deep appearance features** for robust re-identification

The model was trained on a custom 7-class vehicle dataset to detect and track buses, cars, motorcycles, pickups, tricycles, trucks, and vans in real-world traffic footage.

## Sample Predictions
<img width="731" height="409" alt="image" src="https://github.com/user-attachments/assets/ccea6c37-95e2-4f4f-8501-be22c0abe81d" />

<img width="733" height="470" alt="image" src="https://github.com/user-attachments/assets/9a5ee91b-ad3a-4ba5-a098-21264296c1ee" />


## Key Results

| Metric | Value |
|---|---|
| mAP@0.5 | **65.0%** |
| mAP@0.5:0.95 | **55.3%** |
| Training Epochs | 200 |
| Model Size | YOLOv8-Large (43.6M parameters) |
| Dataset Size | 1,200 images |
| Classes | 7 |

## Getting Started

### Prerequisites

- Python 3.8 or higher
- CUDA-compatible GPU (tested on NVIDIA Tesla T4)
- Git
- pip

### Installation

1. Install the dependencies:

```bash
pip install -e '.[dev]'
```

2. Download the DeepSORT feature extractor (`ckpt.t7`) and place it in:

```text
ultralytics/yolo/v8/detect/deep_sort_pytorch/deep_sort/deep/checkpoint/
```

## Usage
### System Workflow
<img width="673" height="645" alt="image" src="https://github.com/user-attachments/assets/7c915fe0-e6c4-4ca7-8b77-3502e9632863" />

### Training

Train the YOLOv8-Large model on your dataset:

```bash
python train.py \
  model=yolov8l.pt \
  data=/path/to/data.yaml \
  epochs=200 \
  imgsz=640
```

Key training parameters used in this project:

| Parameter | Value |
|---|---|
| Model | `yolov8l.pt` |
| Epochs | 200 |
| Image Size | 640 |
| Batch Size | 16 |
| Optimizer | SGD (lr = 0.01) |

### Tracking

Run detection + tracking on a video source:

```bash
python predict.py \
  model=runs/detect/train/weights/best.pt \
  source=path/to/video.mp4 \
  show=True
```

## Dataset

| Attribute | Value |
|---|---|
| Classes | 7 (`bus`, `car`, `motorcycle`, `pickup`, `tricycle`, `truck`, `van`) |
| Total Images | 1,200 |
| Train / Test / Val Split | 70% / 20% / 10% |
| Format | YOLOv5 / YOLOv8 |
| Source | [Roboflow](https://roboflow.com) |

## Project Structure

```text
YOLOv8-DeepSORT-Object-Tracking/
├── ultralytics/
│   └── yolo/v8/detect/
│       ├── deep_sort_pytorch/
│       │   ├── configs/
│       │   ├── deep_sort/
│       │   └── utils/
│       ├── train.py
│       ├── predict.py
│       └── detection-1/
├── runs/detect/train/
│   ├── weights/
│   │   ├── best.pt
│   │   └── last.pt
│   ├── confusion_matrix.png
│   ├── results.png
│   ├── PR_curve.png
│   └── val_batch*_pred.jpg
├── notebooks/
│   └── YOLOv8_Detection_Tracking_CustomData_Complete.ipynb
└── README.md
```

## Metrics
### Confusion Matrix
<img width="824" height="618" alt="image" src="https://github.com/user-attachments/assets/45ca7eb5-2c30-4fd2-802b-820eb73881fb" />

Per-Class Detection & Tracking Performance

| Class | TP | FP | FN | Precision | Recall | F1-score | AP |
|---|---|---|---|---|---|---|---|
| Bus | 14 | 23 | 36 | 0.383 | 0.286 | 0.327 | 0.334 |
| Car | 90 | 25 | 11 | 0.782 | 0.891 | 0.833 | 0.836 |
| Motorcycle | 98 | 6 | 0 | 0.947 | 1.000 | 0.973 | 0.974 |
| Pickup | 10 | 7 | 0 | 0.588 | 1.000 | 0.741 | 0.821 |
| Tricycle | 58 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 |
| Truck | 19 | 7 | 0 | 0.723 | 1.000 | 0.839 | 0.861 |
| Van | 18 | 7 | 28 | 0.710 | 0.382 | 0.496 | 0.546 |

### Model prediction on Validation Batch
<img width="836" height="612" alt="image" src="https://github.com/user-attachments/assets/bbb10df8-4469-4006-ae12-f55fd809f4e9" />


## Contributing

Contributions are welcome. To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m "Add your feature"`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

## License

Distributed under the MIT License. See `LICENSE` for more information.

## Acknowledgments

- [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics)
- [DeepSORT](https://github.com/nwojke/deep_sort)
- [YOLOv8-DeepSORT-Object-Tracking](https://github.com/MuhammadMoinFaisal/YOLOv8-DeepSORT-Object-Tracking) by Muhammad Moin Faisal
- [Roboflow](https://roboflow.com) for dataset hosting and management

## Contact

Your Ayodele Steven Salami — [LinkedIn](linkedin.com/in/ayodele-salami-09b71a139) — ayostevsalam@gmail.com
