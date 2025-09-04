# MaskDetection


This project aims to achieve intelligent recognition and real-time warning of whether people in public places are wearing masks through advanced object detection technology. The project uses the **YOLOv11n** model as the core of detection, combined with **PySide6** to develop a graphical interface, and builds a complete application system integrating **model training**, **validation**, **inference**, **display**, and **voice prompts**. 

Users can load different models and select input sources (images, videos, cameras) through a simple and intuitive interface to detect the faces and mask status of people in real-time in the picture. The system can accurately distinguish between people who have worn masks and those who have not. When it detects people who have not worn masks, it will automatically trigger a voice reminder.

---

## 📂 Project Details

This project consists of the following core modules:

- 🟢 **safeui/main.py**: The main entry for launching the **GUI interface**. Users can run detection on images, folders, videos, or real-time webcam streams.
- 🟢 **yolo_server/scripts/yolo_infer.py**: Inference script. Loads a trained YOLO model (e.g., `best.pt`) and performs detection directly from command line.
- 🟢 **yolo_server/scripts/yolo_train.py**: Model training script. Outputs model weights and training logs under `yolo_server/runs/detect/train/`.
- 🟢 **yolo_server/scripts/yolo_model_val.py**: Model evaluation script. Computes performance metrics (accuracy, mAP, etc.) and saves results in `yolo_server/runs/val/`.
- 🟢 **yolo_server/scripts/yolo_validate.py**: Dataset validation script. Checks dataset integrity and annotation consistency.
- 🟢 **yolo_server/scripts/yolo_trans.py**: Dataset processing and splitting script (train/val/test).
- 🟢 **yolo_server/utils/**: Utility functions supporting training, validation, and inference.
- 🟢 **init_project.py**: One-time initialization script for environment/project setup.

---

## 📊 Dataset

- **Annotation format**: PASCAL VOC  
- **Training samples**: 1280  
- **Validation samples**: 160  
- **Test samples**: 160  

👉 [Download Dataset](https://drive.google.com/file/d/1IozyuKmFLdiwvtuVxZeNj-SGGJ0E2Xpg/view?usp=sharing)  

---

## 🚀 How to Run


### 1. Setup Environment

```bash
conda create -n maskdetection python=3.9
conda activate maskdetection
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```


### 2. Prepare Input Data

* Place the images, videos, or folders you want to test inside the `test/` directory.
* Pre-trained weights (`best.pt`, `last.pt`) are already provided under:

  * `yolo_server/runs/train/weights/`
  * `yolo_server/runs/val/weights/`

---

### 3. Run Detection

#### Option A: Run GUI

```bash
python main.py
```

* The UI allows detection on:

  * Single image
  * Image folder
  * Video file
  * Real-time webcam stream

#### Option B: Run Inference (without GUI)

```bash
python yolo_infer.py --weights runs/train/weights/best.pt --source test/
```

* Results will be saved in:
  `runs/detect/predict/`

---



## 🖼️ Examples

### 1. GUI Interface

The graphical user interface (GUI) provides flexible configuration options for users.  
It allows selecting different detection models, choosing detection modes (image, folder, video, or real-time camera), and customizing confidence thresholds and IoU settings.  
Additionally, users can decide whether to save results and whether to enable voice feedback.  
The GUI is designed to be intuitive, user-friendly, and practical for real-world deployment.

![ui](https://github.com/aiwtw/MaskDetection/blob/main/demo/ui.png?raw=true)

---

### 2. Detection Results

Below are representative examples of the four supported detection modes:

- **Image Detection Example:**  
  ![photo](https://github.com/aiwtw/MaskDetection/blob/main/demo/photo.gif?raw=true)

- **Batch Folder Detection Example:**  
  ![folder](https://github.com/aiwtw/MaskDetection/blob/main/demo/folder.gif?raw=true)

- **Video Stream Detection Example:**  
  ![video](https://github.com/aiwtw/MaskDetection/blob/main/demo/video.gif?raw=true)

- **Real-Time Camera Detection Example:**  
  ![realtime](https://github.com/aiwtw/MaskDetection/blob/main/demo/realtime.jpg?raw=true)

---

### 3. Training and Evaluation Metrics

This section summarizes the model training configuration, validation results, and performance benchmarks.

#### 3.1 YOLOv11 Benchmark Comparison

| Model    | Parameters (M) | mAP@0.5 | Inference Latency (T4 GPU) | Efficiency (TOPS/W) |
|----------|----------------|---------|----------------------------|----------------------|
| YOLOv11n | 2.7            | 42.1    | 5.1ms                      | 58.3                 |
| YOLOv11s | 9.8            | 48.5    | 8.3ms                      | 47.6                 |
| YOLOv11m | 23.1           | 54.8    | 19.7ms                     | 36.2                 |

This project adopts **YOLOv11m** to balance detection accuracy and real-time performance.

---

#### 3.2 Training Configuration

| Parameter                | Value        | Description |
|---------------------------|-------------|-------------|
| Epochs                   | 300         | Total training rounds |
| Batch size               | 128         | Samples per iteration (GPU memory optimized) |
| Image size               | 640         | Resized for small mask detection |
| Device                   | NVIDIA A100 | Training accelerator |
| Dataset config            | `data.yaml` | Dataset definition |
| Mosaic augmentation      | 1.0         | 100% probability enabled |
| Horizontal flip (fliplr) | 0.5         | 50% chance |
| Warmup epochs            | 3.0         | Gradual LR warmup |
| Optimizer                | AdamW       | With weight decay |
| Early stopping           | 100 epochs  | No improvement triggers stop |
| Mixed precision training | Enabled     | Faster and memory efficient |
| Model saving             | Enabled     | Save best & checkpoint |
| Validation               | Enabled     | Every epoch evaluation |

---

#### 3.3 Validation Curves

The following figure shows the validation curves and confusion matrices:  
- **Precision (P) Curve**  
- **Recall (R) Curve**  
- **F1 Score Curve**  
- **PR Curve (Precision-Recall relationship)**  
- **Confusion Matrix & Normalized Confusion Matrix**

![results](https://github.com/aiwtw/MaskDetection/blob/main/demo/results.jpg?raw=true)

---

#### 3.4 Evaluation Results

We evaluated the trained YOLOv11m model on an independent validation/test set.  
Key performance metrics are as follows:

- **General Classification Metrics**  
  - Precision: **87.30%**  
  - Recall: **81.30%**  
  - F1 Score: **84.98%**

- **Object Detection Metrics**  
  - mAP@0.5: **86.45%**  
  - mAP@0.5:0.95: **47.61%**

- **Efficiency and Speed**  
  - End-to-end latency: **1.651 ms/frame**  
    - Preprocessing: 0.126 ms  
    - Inference: 0.497 ms  
    - Postprocessing: 1.029 ms  
  - GPU memory usage: 18 GB (NVIDIA A100)  
  - CPU usage: Peak 12% (Intel Xeon, 80 threads)

---

#### 3.5 Analysis

- The validation curves indicate **stable convergence** of the training process.  
- The **precision-recall tradeoff** suggests a good balance between false positives and false negatives.  
- The **confusion matrix** shows robust performance across mask/no-mask classes, with limited misclassification.  
- Compared to smaller YOLOv11 variants (n, s), the **YOLOv11m** achieves significantly higher accuracy while maintaining acceptable inference speed.  
- The results confirm that the model is suitable for **real-time mask detection in practical deployment scenarios**.


---


