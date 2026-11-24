# OnionGrade AI: AI-Based Onion Quality Assessment, Defect Detection and Automated Grading

[![Python Version](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Frontend](https://img.shields.io/badge/Frontend-React%20%2B%20Vite-61DAFB.svg)](https://react.dev/)
[![Backend](https://img.shields.io/badge/Backend-FastAPI-009688.svg)](https://fastapi.tiangolo.com/)
[![Database](https://img.shields.io/badge/Database-SQLAlchemy%20%7C%20SQLite%20%2F%20PostgreSQL-003B57.svg)](https://www.sqlalchemy.org/)
[![Deep Learning](https://img.shields.io/badge/Vision-YOLOv8%20%2F%20v11--Seg-green.svg)](https://ultralytics.com/)
[![License](https://img.shields.io/badge/Status-Capstone%20Project-orange.svg)](#)

---

## 📌 Project Overview
**OnionGrade AI** is a decoupled full-stack automated computer vision and quality inspection system engineered for onion procurement centers, sorting facilities, and agricultural mandis (APMCs). Based on **Smart India Hackathon (SIH) Problem Statement ID 26031 (Ministry of Consumer Affairs, Food & Public Distribution)**, this system replaces subjective and manual visual inspection with standardized, calibrated, and audit-traceable quality grading.

The architecture decouples the user experience from the computational backend:
- **Frontend**: High-performance **React (Vite)** single-page application for responsive webcam capture, batch file uploads, real-time defect overlay visualization, interactive inspector review, and analytics.
- **Backend**: High-throughput asynchronous **FastAPI** REST API orchestrating YOLO segmentation inference, metric calibration, configurable grading policy, and certified PDF report generation.
- **Storage Layer**: **Hybrid Storage Architecture** combining a relational metadata database (SQLite for local edge mandi installations, PostgreSQL for central cloud warehouses) with an auditable media storage service for raw/annotated images with SHA-256 cryptographic verification.

### Key Capabilities:
1. **Dual-Input Mode (React UI)**: 
   - Batch image upload from local storage (`.jpg`, `.jpeg`, `.png`) with drag-and-drop preview.
   - Live Laptop/External Webcam feed with browser-native WebRTC snapshot capture and live grading.
2. **Instance Segmentation & Multi-Defect Detection**:
   - Locates individual onion bulbs even in slight clusters/batches.
   - Classifies condition: **Healthy**, **Damaged**, **Rotten**, **Sprouted**.
3. **Calibrated Physical Size Estimation**:
   - Converts pixels to metric units (**millimeters - mm**) using reference marker calibration (ArUco / Coin).
   - Computes equatorial diameter ($D_{eq}$) via contour and minimum bounding ellipse geometry.
4. **Configurable Grading Engine**:
   - Applies versioned procurement criteria (**Grade A** vs **URS - Under-sized/Rejected/Sub-standard**).
   - Decoupled from the AI vision model (rules can be updated via REST API without retraining neural networks).
5. **Human-in-the-Loop Review**:
   - Automatically flags predictions with confidence $< 70\%$ for manual inspector verification/correction in a dedicated React review modal.
6. **Digital PDF Audit Report**:
   - Instantly generates downloadable, digitally verifiable inspection PDF reports including lot metadata, defect pie charts, sample statistics, and model version.
7. **Traceability & Image Storage**:
   - Retains full cryptographic audit trails, linking relational lot records to stored raw images, annotated masks, and generated PDF reports.

---

## 🗂️ Project Documentation Index
Every aspect of the project is documented in dedicated markdown guides:

* 📋 [PROJECT_PHASES_AND_ALLOCATION.md](file:///d:/Onion%20Quality%20Grading%20System/PROJECT_PHASES_AND_ALLOCATION.md) - **12 Sequential Phases divided among 6 Teammates** with deliverables and checklists.
* 🏗️ [ARCHITECTURE.md](file:///d:/Onion%20Quality%20Grading%20System/ARCHITECTURE.md) - Decoupled React + FastAPI system architecture, hybrid image storage model, and API contracts.
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
├── ARCHITECTURE.md                    # Decoupled system architecture & API design
├── DATASET_GUIDELINES.md              # Image collection & annotation standards
├── CALIBRATION_GUIDE.md               # Calibration & size estimation math
├── GRADING_RULES.md                   # Grading thresholds & JSON specifications
├── EVALUATION_METRICS.md              # ML & System evaluation formulas
├── requirements.txt                   # Backend Python dependencies
│
├── frontend/                          # React + Vite Single Page Application
│   ├── package.json                   # Frontend dependencies (React, Lucide, Axios)
│   ├── vite.config.js                 # Vite build & proxy configuration
│   ├── index.html                     # HTML root template
│   └── src/
│       ├── components/                # WebcamCapture, ImageUploader, DefectOverlay, ReviewModal
│       ├── pages/                     # Dashboard, InspectionView, History, Settings
│       ├── services/                  # API client (Axios) connecting to FastAPI
│       └── App.jsx                    # Core React application shell
│
├── src/                               # FastAPI Backend Application
│   ├── api/                           # REST API routes (inspect, history, reports, rules, stats)
│   ├── ai/                            # YOLO inference & segmentation logic
│   ├── calibration/                   # ArUco / Coin detector & pixel-to-mm logic
│   ├── grading/                       # Configurable grading engine (JSON-based)
│   ├── reports/                       # ReportLab PDF report generation
│   ├── database/                      # SQLAlchemy models, connection & CRUD services
│   ├── storage/                       # Image storage service (disk/S3 + SHA-256 tracking)
│   └── main.py                        # FastAPI application entrypoint & static mount
│
├── data/
│   ├── storage/                       # Managed image & report asset storage
│   │   ├── images/
│   │   │   ├── uploads/               # Raw uploaded inspection images
│   │   │   ├── annotated/             # Rendered segmentation overlays
│   │   │   └── crops/                 # Flagged individual bulb crops for review
│   │   └── reports/                   # Generated audit PDF certificates
│   ├── raw/                           # Raw collected onion images
│   ├── processed/                     # Annotated YOLO dataset (train/val/test)
│   └── sample_inputs/                 # Sample test images for demo
│
├── models/
│   └── weights/                       # Trained YOLO weights (best.pt)
│
├── tests/                             # Unit & integration tests (API, DB, Size, Grading)
└── docs/                              # Flowcharts, presentation assets, screenshots
```

---

## 🚀 Quick Start Setup

### 1. Backend Setup (FastAPI & Vision Core)
```bash
# Navigate to the project root directory
cd "d:\Onion Quality Grading System"

# Create and activate a Python virtual environment
python -m venv venv
venv\Scripts\activate

# Install all backend dependencies
pip install -r requirements.txt

# Start the FastAPI REST server
uvicorn src.main:app --reload --host 0.0.0.0 --port 8000
```
Interactive API Swagger documentation is available at `http://localhost:8000/docs`.

### 2. Frontend Setup (React + Vite)
```bash
# In a new terminal, navigate to the frontend directory
cd "d:\Onion Quality Grading System\frontend"

# Install Node dependencies
npm install

# Start the Vite development server
npm run dev
```
Open your browser at `http://localhost:5173` to access the React Inspection Dashboard.

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
* 🔹 **Gautam**: **Phase 9 & Phase 10** (React Web Application, Dual-Input Camera & Human Review UI)
* 🔹 **Rithvik Teja**: **Phase 11 & Phase 12** (FastAPI Backend, Database & Media Storage, End-to-End QA)

For individual tasks and deliverables, see [STUDENT_PHASE_ROSTER.md](file:///d:/Onion%20Quality%20Grading%20System/STUDENT_PHASE_ROSTER.md) and [PROJECT_PHASES_AND_ALLOCATION.md](file:///d:/Onion%20Quality%20Grading%20System/PROJECT_PHASES_AND_ALLOCATION.md).
