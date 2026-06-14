```markdown
# 🫁 Pneumonia Detection System

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.6.0-orange.svg)](https://www.tensorflow.org/)
[![Flask](https://img.shields.io/badge/Flask-2.0.1-green.svg)](https://flask.palletsprojects.com/)
[![PyQt5](https://img.shields.io/badge/PyQt5-Desktop%20App-red.svg)](https://www.riverbankcomputing.com/software/pyqt/)

> An AI-powered system for detecting Pneumonia from Chest X-ray images using Deep Learning. Available as both a Web Application (Flask) and Desktop Application (PyQt5).

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [API Endpoints](#api-endpoints)
- [Database Schema](#database-schema)
- [Model Information](#model-information)
- [Disclaimer](#disclaimer)
- [License](#license)

---

## 🎯 Overview

This project leverages a **Convolutional Neural Network (CNN)** based on **VGG16 architecture** to classify Chest X-ray images into two categories:

- ✅ **Normal** – No signs of Pneumonia
- ⚠️ **Pneumonia** – Bacterial or Viral Pneumonia detected

The system provides **two interfaces**:
1. **Web Application** – Accessible via browser, stores prediction history
2. **Desktop Application** – PyQt5 based GUI with voice output support

---

## ✨ Features

### Common Features (Both Versions)
- 🔍 Real-time Pneumonia detection from X-ray images
- 📊 Confidence score for each prediction
- 🖼️ Image preview before analysis
- 📈 Prediction history tracking

### Web Application (Flask)
- 🌐 Browser-based interface
- 💾 SQLite database for persistent storage
- 📜 View last 10 predictions with timestamps
- 📱 Responsive design for mobile devices

### Desktop Application (PyQt5)
- 🎙️ **Voice output** – Reads the result aloud (Windows)
- 🖥️ Native desktop experience
- 🎨 Animated GIF background
- 🚀 Standalone execution

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| **Backend** | Flask 2.0.1, Python 3.7+ |
| **Frontend** | HTML5, CSS3, JavaScript (Vanilla) |
| **Deep Learning** | TensorFlow 2.6.0, Keras, VGG16 |
| **Database** | SQLite3 |
| **Desktop GUI** | PyQt5 |
| **Voice Engine** | Windows SAPI (pywin32) |
| **Image Processing** | NumPy, Pillow |

---

## 📁 Project Structure

```
pneumonia-detection-system/
│
├── app.py                      # Flask web application
├── chest_xray.py               # PyQt5 desktop application
├── chest_xray.h5               # Trained Keras model (required)
├── predictions.db              # SQLite database (auto-created)
├── requirements.txt            # Python dependencies
│
├── templates/
│   └── index.html              # Web UI template
│
├── uploads/                    # Uploaded images storage
│
├── picture.gif                 # Desktop app background (optional)
└── patient.png                 # Desktop app icon (optional)
```

---

## 🚀 Installation

### Prerequisites
- Python 3.7 or higher
- pip package manager

### Step 1: Clone or Download the Project
```bash
git clone https://github.com/yourusername/pneumonia-detection-system.git
cd pneumonia-detection-system
```

### Step 2: Install Dependencies
```bash
pip install -r requirements.txt
```

**requirements.txt content:**
```
flask==2.0.1
tensorflow==2.6.0
numpy==1.19.5
werkzeug==2.0.1
```

### Step 3: Prepare Model File
Place your trained Keras model file as `chest_xray.h5` in the root directory.

> **Note:** The model must be trained on Chest X-ray images with input size 224x224 pixels.

### Step 4: (Optional) Desktop App Additional Dependencies
```bash
# Windows only - for voice output feature
pip install pyqt5 pywin32
```

---

## 💻 Usage

### Running the Web Application

```bash
python app.py
```

Then open your browser and navigate to:
```
http://127.0.0.1:5000
```

**Web App Workflow:**
1. Click "Choose an image" to select a Chest X-ray
2. Preview the selected image
3. Click "Predict" button
4. View result (Normal/Pneumonia) with confidence percentage
5. Check "Recent Predictions" section for history

### Running the Desktop Application (Windows)

```bash
python chest_xray.py
```

**Desktop App Workflow:**
1. Click "Upload Image" to select an X-ray
2. Click "Prediction" to analyze
3. View popup result with voice announcement

---

## 🔌 API Endpoints (Web App)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Serves the main web interface |
| `/predict` | POST | Uploads image and returns prediction |
| `/history` | GET | Returns last 10 predictions as JSON |
| `/uploads/<filename>` | GET | Serves uploaded images |

### Prediction Response Example
```json
{
  "prediction": "Pneumonia",
  "confidence": 94.5,
  "filename": "patient_xray_001.jpg",
  "image_url": "/uploads/patient_xray_001.jpg"
}
```

### History Response Example
```json
[
  {
    "id": 1,
    "filename": "xray_001.jpg",
    "prediction": "Pneumonia",
    "confidence": 94.5,
    "timestamp": "2025-01-15 10:30:45",
    "image_url": "/uploads/xray_001.jpg"
  }
]
```

---

## 🗄️ Database Schema

**Table: `predictions`**

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER PRIMARY KEY | Auto-incrementing ID |
| filename | TEXT | Original uploaded filename |
| prediction | TEXT | 'Normal' or 'Pneumonia' |
| confidence | REAL | Confidence percentage (0-100) |
| timestamp | DATETIME | Auto-generated timestamp |

---

## 🧠 Model Information

| Parameter | Value |
|-----------|-------|
| **Architecture** | VGG16 (Transfer Learning) |
| **Input Size** | 224 x 224 pixels |
| **Output** | Binary classification |
| **Threshold** | >0.5 = Normal, ≤0.5 = Pneumonia |
| **Preprocessing** | VGG16 preprocessing (including mean subtraction) |

### Prediction Logic
```python
pred = model.predict(img_array)
confidence = float(pred[0][0])
result = "Normal" if confidence > 0.5 else "Pneumonia"
confidence_percent = (confidence if result == "Normal" else 1-confidence) * 100
```

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Model not found error | Ensure `chest_xray.h5` exists in root directory |
| TensorFlow warnings | Set `TF_CPP_MIN_LOG_LEVEL='2'` (already in code) |
| Upload file too large | Max file size is 16MB (configurable in app.py) |
| Database error | Check write permissions in project folder |
| Voice not working (Desktop) | Ensure Windows OS and `pywin32` installed |

---

## 🔮 Future Enhancements

- [ ] Multiple disease detection (COVID-19, Tuberculosis, Lung Cancer)
- [ ] User authentication & roles (Doctor/Patient)
- [ ] PDF report generation
- [ ] Batch prediction for multiple images
- [ ] Docker containerization
- [ ] Mobile app (React Native/Flutter)
- [ ] Cloud storage integration (AWS S3)
- [ ] Advanced visualization (Grad-CAM heatmaps)

---

## ⚠️ Disclaimer

**IMPORTANT: This system is for educational and research purposes only.**

- ❌ **NOT** intended for real clinical diagnosis
- ❌ **NOT** a substitute for professional medical advice
- ✅ Always consult qualified healthcare professionals
- ✅ Final diagnosis should be based on multiple factors including clinical examination, patient history, and laboratory tests

The developers assume **no liability** for any misuse or misinterpretation of results.

---

## 📄 License

This project is licensed under the **MIT License**.

```
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🙏 Acknowledgments

- TensorFlow/Keras community for deep learning frameworks
- PyQt5 for desktop GUI toolkit
- Flask for lightweight web framework
- Research papers on Pneumonia detection using CNNs

---

## ⭐ Show Your Support

If this project helped you, please give it a ⭐ on GitHub!

---

**Made with 🩺 for healthcare AI research**
```
