# 🧪 Microplate Detection using YOLOv5

This project performs **object detection on ELISA microplates** using the **YOLOv5 deep learning model**. It detects and classifies microplate wells into three categories:
- ✅ Positive
- ❌ Negative
- ⚪ Buffer

The project includes **custom dataset preparation**, **YOLO training**, **inference**, and **model export (TFLite)** for Android deployment.

---

## 🚀 Features

- 📦 Custom dataset conversion from Pascal VOC XML → YOLOv5 format
- 🎯 Object detection with 3 custom microplate classes
- 🧠 Training using YOLOv5s backbone (lightweight)
- 🖼️ Visual test inference results on custom dataset
- 📤 Model export to `.tflite` for mobile deployment
- 📊 Training metrics available via YOLO `runs/`

---

## 📁 File Structure

Microplate-detection-using-YOLOv5/
├── YoloV5_Microplate.ipynb # Full training + inference pipeline in Colab
├── Readme.md # This documentation file
├── Report.docx # Report describing the project results
├── yolov5/ # YOLOv5 source (cloned from Ultralytics)
│ ├── train.py # YOLO training script
│ ├── detect.py # Run inference on images
│ ├── export.py # Export model (TFLite, TorchScript, etc.)
│ └── ... # Other internal YOLOv5 components
├── dataset_YOLO/ # Generated YOLO-ready dataset (after conversion)
│ ├── Train/
│ ├── Val/
│ ├── Test/
│ └── data.yaml # YOLO dataset config file
└── best-fp16.tflite # ✅ Trained YOLOv5 model ready for Android

yaml
Copy
Edit

---

## 📥 Dataset

To train or retrain the model, download the dataset here:

🔗 [Eliza Microplate Dataset (Google Drive)](https://drive.google.com/drive/folders/18U0gQetGk9y2s-UF7RaHBKSA-EdTk3Zr?usp=drive_link)

Expected folder structure:

Eliza Microplates DATASET/
├── images/
│ ├── positive/
│ ├── negative/
│ └── buffer/
└── vocxml/
├── positive/
├── negative/
└── buffer/

yaml
Copy
Edit

---

## 🛠 How to Run

> ⚙️ The entire workflow is provided in the notebook: `YoloV5_Microplate.ipynb`

### 1. 🐍 Setup

If you're running outside Google Colab, install required libraries:

```bash
pip install -r yolov5/requirements.txt
pip install opencv-python numpy pandas scikit-learn PyYAML
2. 💽 Convert Dataset (already done in notebook)
Extract annotations from XML

Convert to YOLO format using provided code

Saves output to dataset_YOLO/Train, Val, and Test

3. 🏋️‍♀️ Train the Model
From inside yolov5/ directory:

bash
Copy
Edit
python train.py --img 736 --batch 16 --epochs 35 \
--data ../dataset_YOLO/data.yaml --weights yolov5s.pt --cache
Training artifacts will be saved to:

swift
Copy
Edit
yolov5/runs/train/exp/
4. 🔍 Run Inference
bash
Copy
Edit
python detect.py --weights runs/train/exp/weights/best.pt \
--img 1080 --conf 0.1 --source ../dataset_YOLO/Test/images
Resulting images with bounding boxes will be saved in:

swift
Copy
Edit
yolov5/runs/detect/exp/
5. 📤 Export to TFLite (for Android)
bash
Copy
Edit
python export.py --weights runs/train/exp/weights/best.pt --include tflite
Output:

swift
Copy
Edit
runs/train/exp/weights/best-fp16.tflite
📸 Output Example
✅ YOLO detects bounding boxes over wells

🔁 Classifies as "positive", "negative", or "buffer"

📂 Results saved as images inside runs/detect/exp/

🧪 Can be deployed to Android for real-time inference

📲 Deployment (Android Studio)
You can use the exported best-fp16.tflite model in a mobile app using TensorFlow Lite and CameraX.

📊 Model Performance
Training summary available in Report.docx

Evaluation metrics include mAP, precision, recall

Inference is fast and lightweight using YOLOv5s
