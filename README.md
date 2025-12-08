# 🚗 DriveSafe: AI-Based Driver Drowsiness Detection System

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![YOLOv11](https://img.shields.io/badge/YOLOv11-Ultralytics-00ADD8.svg)](https://github.com/ultralytics/ultralytics)
[![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-Compatible-C51A4A.svg)](https://www.raspberrypi.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Real-time drowsiness monitoring system • Built with computer vision**

An intelligent, cost-effective driver drowsiness detection system designed to enhance road safety by monitoring driver alertness in real-time and providing instant warnings when signs of fatigue are detected.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [System Architecture](#-system-architecture)
- [Hardware Requirements](#-hardware-requirements)
- [Software Requirements](#-software-requirements)
- [Installation](#-installation)
- [Usage](#-usage)
- [Model Performance](#-model-performance)
- [Project Structure](#-project-structure)
- [How It Works](#-how-it-works)
- [Dataset](#-dataset)
- [Results](#-results)
- [Future Improvements](#-future-improvements)
- [Contributing](#-contributing)
- [Authors](#-authors)
- [Acknowledgments](#-acknowledgments)
- [License](#-license)

---

## 🎯 Overview

Driver drowsiness is a major cause of road accidents worldwide, especially during long trips or late-night driving. While advanced drowsiness detection systems exist in luxury vehicles, most cars lack this critical safety feature.

**DriveSafe** provides an affordable, adaptable solution that can be installed in various types of vehicles, including personal cars, buses, and trucks, making intelligent safety technology accessible to everyone.

### Motivation

- **Safety for All**: Every driver deserves to reach their destination safely
- **Save Lives**: Prevent accidents caused by driver fatigue
- **Raise Awareness**: Highlight the risks of drowsy driving through intelligent monitoring

---

## ✨ Features

- **Real-Time Detection**: Monitors driver state at 30 FPS using a front-facing camera
- **Three-State Classification**: Detects Awake, Drowsy, and Absent (face not visible) states
- **Intelligent Alert System**: 
  - Visual indicators (Green/Red LEDs)
  - Audio alarm for drowsiness warnings
  - Temporal tracking to avoid false positives
- **High Accuracy**: 99.06% classification accuracy using YOLOv11
- **Robust Performance**: Works under various lighting conditions (day/night/infrared)
- **Low-Cost Hardware**: Built using Raspberry Pi and affordable components
- **Desktop Application**: PyQt5-based GUI for testing and demonstration

---

## 🏗 System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    INPUT STAGE                          │
│  • Real-time video from dashboard camera (30 FPS)       │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│                 PROCESSING STAGE                        │
│  1. Frame Acquisition & Preprocessing                   │
│  2. YOLOv11 Classification (Awake/Drowsy/Absent)       │
│  3. Temporal Tracking (30 frames window)                │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│              DECISION-MAKING STAGE                      │
│  • Green LED: Face detected & awake                     │
│  • Red LED: Face not detected (absent)                  │
│  • Buzzer: Drowsiness threshold exceeded                │
└─────────────────────────────────────────────────────────┘
```

---

## 🔧 Hardware Requirements

| Component | Specification | Purpose |
|-----------|--------------|---------|
| **Processing Unit** | Raspberry Pi 4 (4GB RAM recommended) | Main computation unit |
| **Camera** | USB/Pi Camera Module (640×480, 30 FPS) | Captures driver's face |
| **Storage** | microSD Card (16GB+) | Stores OS, code, and model |
| **LEDs** | Green & Red LEDs | Visual status indicators |
| **Buzzer** | Active buzzer module | Audio alert system |
| **Power Supply** | 5V/3A adapter | Powers entire system |
| **GPIO Pins** | - | GPIO 17 (Green), 27 (Red), 22 (Buzzer) |

### Wiring Diagram

```
Raspberry Pi GPIO:
├── GPIO 17 → Green LED (+) → GND
├── GPIO 27 → Red LED (+) → GND
└── GPIO 22 → Buzzer (+) → GND
```

---

## 💻 Software Requirements

### For Raspberry Pi Deployment

```
Python 3.8+
OpenCV (cv2) 4.5+
Raspberry Pi OS
RPi.GPIO
Ultralytics YOLOv11
NumPy
```

### For Desktop Application

```
Python 3.8+
PyQt5
OpenCV (cv2)
Pygame
Ultralytics YOLOv11
```

---

## 📦 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/drivesafe-drowsiness-detection.git
cd drivesafe-drowsiness-detection
```

### 2. Install Dependencies

#### For Raspberry Pi:

```bash
pip install -r requirements_rpi.txt
```

#### For Desktop Application:

```bash
pip install -r requirements_desktop.txt
```

### 3. Download the Model

Place the trained YOLOv11 model file (`best.pt`) in the project root directory.

```bash
# Model file should be placed at:
./best.pt
```

### 4. Prepare Audio Files

Ensure the alarm sound file is in the correct location:

```bash
./alarm.mp3
```

---

## 🚀 Usage

### Running on Raspberry Pi

```bash
python drowsiness_detection_rpi.py
```

**Controls:**
- The system starts automatically upon running
- Press `q` in the video window to stop
- Use `Ctrl+C` for emergency stop

### Running Desktop Application

```bash
python drowsiness_detection_desktop.py
```

**GUI Controls:**
- Click **"▶ Start Detection"** to begin monitoring
- Click **"◼ Stop Detection"** to stop
- Press the info button (ℹ️) for system information

### Creating Executable (Windows)

```bash
pip install pyinstaller
pyinstaller --onefile --windowed --add-data "best.pt;." --add-data "alarm.mp3;." --add-data "images;images" drowsiness_detection_desktop.py
```

---

## 📊 Model Performance

The YOLOv11 model was trained on a comprehensive dataset of 24,246 images with the following results:

| Metric | Value |
|--------|-------|
| **Accuracy** | 99.06% |
| **Precision** | 99.06% |
| **Recall** | 99.07% |
| **F1-Score** | 99.06% |

### Confusion Matrix Results

| Predicted → | Absent | Awake | Drowsy |
|-------------|--------|-------|--------|
| **Absent**  | 2013   | 0     | 0      |
| **Awake**   | 0      | 2082  | 34     |
| **Drowsy**  | 0      | 24    | 2000   |

### Class-wise Performance

- **Absent**: 100% accuracy (perfect classification)
- **Awake**: 99% accuracy
- **Drowsy**: 98% accuracy

---

## 📁 Project Structure

```
drivesafe-drowsiness-detection/
│
├── drowsiness_detection_rpi.py      # Raspberry Pi implementation
├── drowsiness_detection_desktop.py  # Desktop GUI application
├── best.pt                           # Trained YOLOv11 model
├── alarm.mp3                         # Alert sound file
│
├── images/                           # GUI assets
│   ├── logo.jpeg
│   └── info_icon.png
│
├── requirements_rpi.txt              # Pi dependencies
├── requirements_desktop.txt          # Desktop dependencies
│
├── docs/                             # Documentation
│   ├── thesis.pdf
│   └── presentation.pdf
│
├── dataset/                          # Dataset information
│   ├── data_preparation.md
│   └── dataset_statistics.md
│
├── tests/                            # Test scripts
│   └── model_evaluation.py
│
└── README.md                         # This file
```

---

## 🔍 How It Works

### 1. Frame Acquisition
- Camera captures video at 30 FPS
- Each frame is extracted and preprocessed (grayscale conversion)

### 2. Classification
- YOLOv11 model analyzes facial features
- Detects eye closure, yawning, and head position
- Classifies into: **Awake**, **Drowsy**, or **Absent**

### 3. Temporal Tracking
- System tracks consecutive frames (30-frame window ≈ 1 second)
- Implements smoothing to reduce false positives
- Only triggers alarm after sustained drowsiness

### 4. Alert System
| Condition | Green LED | Red LED | Buzzer |
|-----------|-----------|---------|--------|
| Face detected & Awake | ✅ ON | ❌ OFF | ❌ OFF |
| Face detected & Drowsy (< 30 frames) | ✅ ON | ❌ OFF | ❌ OFF |
| Face detected & Drowsy (≥ 30 frames) | ✅ ON | ❌ OFF | ✅ ON |
| Face not detected (Absent) | ❌ OFF | ✅ ON | ❌ OFF |

---

## 📚 Dataset

The model was trained on a comprehensive dataset combining multiple sources:

### Primary Dataset
- **NTHU-DDD** (National Tsing Hua University Drowsy Driver Detection Dataset)
  - 36 subjects with multiple recording scenarios
  - Includes day/night conditions with IR illumination
  - 640×480 resolution at 15-30 FPS

### Supplementary Datasets
- **YawDD**: Yawning Detection Dataset
- **CEW**: Closed Eyes in the Wild
- **Driver Behavior Image Dataset**
- **Kaggle Datasets**:
  - UTA Real-Life Drowsiness Dataset
  - Drowsiness Prediction Dataset
  - NITYMED Dataset
  - Driver Inattention Detection Dataset

### Dataset Composition

| Class | Training | Testing | Total |
|-------|----------|---------|-------|
| Awake | 6,082 | 2,106 | 8,188 |
| Drowsy | 6,011 | 2,034 | 8,045 |
| Absent | 6,000 | 2,013 | 8,013 |
| **Total** | **18,093** | **6,153** | **24,246** |

**Gender Balance**: 7,997 male / 8,236 female samples

---

## 📈 Results

### Training Characteristics
- **Model**: YOLOv11n (Nano) for classification
- **Training Platform**: Google Colab Pro with NVIDIA A100 GPU
- **Epochs**: 100
- **Batch Size**: 32
- **Image Size**: 640×640 pixels
- **Data Augmentation**: Automatic (RandAugment)

### Key Achievements
✅ 99.06% overall accuracy  
✅ Perfect detection of absent state (100%)  
✅ Minimal false positives between awake/drowsy  
✅ Real-time performance (30 FPS)  
✅ Robust across lighting conditions  
✅ Gender-balanced training  

---

## 🔮 Future Improvements

- [ ] Add support for multiple driver profiles
- [ ] Implement cloud-based logging and analytics
- [ ] Integration with vehicle CAN bus
- [ ] Mobile app for remote monitoring
- [ ] Enhanced night vision support
- [ ] Multi-language alert system
- [ ] Driver fatigue level estimation (percentage)
- [ ] GPS integration for location-based alerts

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/YourFeature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to the branch (`git push origin feature/YourFeature`)
5. Open a Pull Request

---

## 👥 Authors

**Aseel Jayousi** & **Hanan Abuzainab**

Computer Systems Engineering  
Palestine Technical University - Khadoorie  
Supervised by: **Anas Melhm, Ph.D.**

---

## 🙏 Acknowledgments

- National Tsing Hua University for the NTHU-DDD dataset
- Ultralytics team for the YOLOv11 framework
- All dataset contributors and open-source communities
- Palestine Technical University - Khadoorie for project support

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 📞 Contact

For questions, suggestions, or collaboration:

- **Email**: [your.email@example.com](mailto:your.email@example.com)
- **LinkedIn**: [Your LinkedIn](https://linkedin.com/in/yourprofile)
- **Project Issues**: [GitHub Issues](https://github.com/yourusername/drivesafe-drowsiness-detection/issues)

---

## ⚠️ Disclaimer

This system is designed as a driver assistance tool and should not be relied upon as the sole means of preventing drowsy driving. Drivers should always:
- Get adequate rest before driving
- Take regular breaks during long trips
- Never drive when feeling fatigued
- Follow all traffic safety regulations

---

<div align="center">

**Drive Safe • Stay Alert • Save Lives**

Made with ❤️ in Palestine 🇵🇸

</div>
