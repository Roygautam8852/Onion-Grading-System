# OnionGrade AI: AI-Based Onion Quality Assessment, Defect Detection and Automated Grading

[![Python Version](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/UI-Streamlit-red.svg)](https://streamlit.io/)
[![Deep Learning](https://img.shields.io/badge/Vision-YOLOv8%20%2F%20v11--Seg-green.svg)](https://ultralytics.com/)
[![License](https://img.shields.io/badge/Status-Capstone%20Project-orange.svg)](#)

---

## 📌 Project Overview
**OnionGrade AI** is a fully Python-based automated computer vision and quality inspection system designed for onion procurement centers, sorting facilities, and agricultural mandis. Based on **Smart India Hackathon (SIH) Problem Statement ID 26031 (Ministry of Consumer Affairs, Food & Public Distribution)**, this system replaces subjective and manual visual inspection with standardized, calibrated, and audit-traceable quality grading.

### Key Capabilities:
1. **Dual-Input Mode**: 
   - Batch image upload from local disk (`.jpg`, `.jpeg`, `.png`).
   - Live Laptop/External Webcam feed with real-time snapshot and grading.
2. **Instance Segmentation & Multi-Defect Detection**:
   - Locates individual onion bulbs even in slight clusters/batches.
   - Classifies condition: **Healthy**, **Damaged**, **Rotten**, **Sprouted**.
3. **Calibrated Physical Size Estimation**:
   - Converts pixels to metric units (**millimeters - mm**) using reference marker calibration (ArUco / Coin).
   - Computes equatorial diameter ($D_{eq}$) via contour and minimum bounding ellipse geometry.
4. **Configurable Grading Engine**:
   - Applies versioned procurement criteria (**Grade A** vs **URS - Under-sized/Rejected/Sub-standard**).
   - Decoupled from the AI vision model (rules can be updated without retraining the neural network).
5. **Human-in-the-Loop Review**:
   - Automatically flags predictions with confidence $< 70\%$ for manual inspector verification/correction.
6. **Digital PDF Audit Report**:
   - Instantly generates downloadable, digitally verifiable inspection PDF reports including lot metadata, defect pie charts, sample statistics, and model version.
7. **Traceability Database**:
   - Stores all inspection records, individual bulb measurements, and audit logs in an SQLite database.

---

## 🗂️ Project Documentation Index
Every aspect of the project is documented in dedicated markdown guides:

* 📋 [PROJECT_PHASES_AND_ALLOCATION.md](file:///d:/Onion%20Quality%20Grading%20System/PROJECT_PHASES_AND_ALLOCATION.md) - **12 Sequential Phases divided among 6 Teammates** with deliverables and checklists.
* 🏗️ [ARCHITECTURE.md](file:///d:/Onion%20Quality%20Grading%20System/ARCHITECTURE.md) - System architecture, end-to-end data pipeline, dual-input mechanism, and component interactions.
* 📸 [DATASET_GUIDELINES.md](file:///d:/Onion%20Quality%20Grading%20System/DATASET_GUIDELINES.md) - Image collection protocols, 4 defect class standards, polygon annotation rules, augmentation, and dataset splits.
* 📏 [CALIBRATION_GUIDE.md](file:///d:/Onion%20Quality%20Grading%20System/CALIBRATION_GUIDE.md) - Pixel-to-mm mathematical formulas, ArUco / Coin reference marker setup, and camera angle guidelines.
* ⚖️ [GRADING_RULES.md](file:///d:/Onion%20Quality%20Grading%20System/GRADING_RULES.md) - Ministry of Consumer Affairs & ICAR-DOGR grading specifications, Grade A vs URS rules, and JSON schema.
* 📊 [EVALUATION_METRICS.md](file:///d:/Onion%20Quality%20Grading%20System/EVALUATION_METRICS.md) - Precision, Recall, mAP@50, Confusion Matrix, Size MAE/RMSE, and Inference latency formulas.

---

## 📁 Repository Structure
```
d:/Onion Quality Grading System/
│
├── README.md                          # Master documentation & quick-start guide
├── PROJECT_PHASES_AND_ALLOCATION.md   # 12 Phases & 6 Teammates allocation
├── ARCHITECTURE.md                    # System architecture & data flow
├── DATASET_GUIDELINES.md              # Image collection & annotation standards
├── CALIBRATION_GUIDE.md               # Calibration & size estimation math
├── GRADING_RULES.md                   # Grading thresholds & JSON specifications
├── EVALUATION_METRICS.md              # ML & System evaluation formulas
├── requirements.txt                   # Complete Python dependencies
│
├── data/
│   ├── raw/                           # Raw collected onion images
│   ├── processed/                     # Annotated YOLO dataset (train/val/test)
│   └── sample_inputs/                 # Sample test images for demo
│
├── models/
│   └── weights/                       # Trained YOLO weights (best.pt)
│
├── src/
│   ├── ai/                            # YOLO inference & segmentation logic
│   ├── calibration/                   # ArUco / Coin detector & pixel-to-mm logic
│   ├── grading/                       # Configurable grading engine (JSON-based)
│   ├── reports/                       # ReportLab PDF report generation
│   ├── database/                      # SQLite models, engine & queries
│   └── ui/                            # Streamlit web dashboard & camera inputs
│
├── tests/                             # Unit tests for size, grading, and DB
└── docs/                              # Flowcharts, presentation assets, screenshots
```

---

## 🚀 Quick Start Setup (100% Python)

### 1. Environment Setup
```bash
# Clone or navigate to the directory
cd "d:\Onion Quality Grading System"

# Create a virtual environment (recommended)
python -m venv venv
venv\Scripts\activate

# Install all dependencies
pip install -r requirements.txt
```

### 2. Run the Application
```bash
streamlit run src/ui/app.py
```
Open your browser at `http://localhost:8501`.

---

## 👥 Team & Academic Context
* **Capstone Project ID**: K4C0611
* **Research Area**: Machine Learning and Artificial Intelligence (Computer Vision & Deep Learning)
* **Problem Statement ID**: SIH 26031 (Ministry of Consumer Affairs, Food & Public Distribution)
* **Academic Year**: 2026–2027

### 🧑‍💻 Project Team & Phase Roster
* 🔹 **Ajay**: **Phase 1 & Phase 2** (Project Foundation, Base Architecture & Dataset Protocol)
* 🔹 **Ameg**: **Phase 3 & Phase 4** (Data Annotation, Augmentation & YOLOv8-Seg Model Training)
* 🔹 **Bablu**: **Phase 5 & Phase 6** (OpenCV Calibration & Metric Size Estimation Engine)
* 🔹 **Chethan**: **Phase 7 & Phase 8** (Grading Business Logic & Automated PDF Report Generator)
* 🔹 **Gautam**: **Phase 9 & Phase 10** (Streamlit Web App, Dual-Input Camera & Human Review UI)
* 🔹 **Rithvik Teja**: **Phase 11 & Phase 12** (SQLite Database, End-to-End Integration & Final QA)

For individual tasks and deliverables, see [STUDENT_PHASE_ROSTER.md](file:///d:/Onion%20Quality%20Grading%20System/STUDENT_PHASE_ROSTER.md) and [PROJECT_PHASES_AND_ALLOCATION.md](file:///d:/Onion%20Quality%20Grading%20System/PROJECT_PHASES_AND_ALLOCATION.md).
