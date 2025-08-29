# Tooth Numbering Detection with YOLOv11  

## 📌 Project Overview  
This project implements **tooth numbering detection** using YOLOv11.  
A dataset of dental radiographs with annotated tooth numbers was provided. The goal is to train a YOLO model that can accurately detect and classify teeth into **32 classes** following the FDI numbering system.  

---

## 📂 Dataset  
- Dataset: **ToothNumber_TaskDataset**  
- Contains dental images and YOLO-format labels (`.txt` files).  
- Classes: 32 tooth numbers (0–31 in YOLO IDs).  
- The dataset was split into **train / val / test** sets before training.  

**Data structure:**  
```
ToothNumber_TaskDataset/
│── images/
│   ├── train/
│   ├── val/
│   ├── test/
│── labels/
│   ├── train/
│   ├── val/
│   ├── test/
│── tooth_data.yaml
```

---

## ⚙️ Setup in Google Colab  

```bash
# Clone ultralytics
!pip install ultralytics

# Verify YOLO installation
from ultralytics import YOLO
```

---

## 🚀 Training  

We trained YOLOv11m on the dataset:  

```bash
!yolo detect train   data=/content/tooth_data.yaml   model=yolo11m.pt   epochs=100   imgsz=640   batch=16   device=0
```

- **epochs**: 100  
- **image size**: 640  
- **batch size**: 16  
- **GPU used**: Tesla T4 (Colab)  

---

## 📊 Results  

### Validation Performance (val set)
- **mAP50:** ~0.91  
- **mAP50-95:** ~0.64  

### Test Performance (test set)
- **mAP50:** ~0.95  
- **mAP50-95:** ~0.71  

This shows the model generalizes well to unseen images.  

---

## 🖼️ Inference  

Run inference on new images:  

```bash
!yolo detect predict   model=runs/detect/train/weights/best.pt   source=/content/ToothNumber_TaskDataset/images/test   conf=0.25
```

Results will be saved in:  
```
runs/detect/predict/
```

---

## 📌 Post-Processing (Optional but Recommended)  
To enforce anatomical correctness in predictions:  
- Separate **upper vs lower arch** (Y-axis clustering).  
- Split **left vs right** (X-midline).  
- Sort teeth within quadrants.  
- Skip numbers if spacing indicates missing teeth.  

---

## 📁 Repository Structure  

```
├── tooth_project.ipynb    # Colab notebook
├── ToothNumber_TaskDataset/   # Dataset
├── runs/                  # Training logs and results
├── tooth_data.yaml        # Dataset config
├── README.md              # Documentation
```


