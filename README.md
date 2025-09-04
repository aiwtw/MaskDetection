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

![ui](https://github.com/aiwtw/MaskDetection/blob/main/demo/ui.png?raw=true)

### 2. Detection Results

![photo](https://github.com/aiwtw/MaskDetection/blob/main/demo/photo.gif?raw=true)
![folder]()
![video]()
![realtime]()


### 3. Training Metrics

![results](https://github.com/aiwtw/MaskDetection/blob/main/demo/results.jpg?raw=true)

---


