# 👥 Student Phase Allocation & Team Roster

**Capstone Project**: AI-Based Web Application for Onion Quality Assessment, Defect Detection and Automated Grading (OnionGrade AI)  
**Group ID**: K4C0611  
**Problem Statement**: SIH ID 26031 (Ministry of Consumer Affairs, Food & Public Distribution)  
**Academic Year**: 2026–2027  

---

## ⚡ Live Phase Status Tracker

> **Independent Execution**: Har teammate apne assigned phases ka sole owner hai. Jaise hi kisi teammate ka code/task complete hota hai, woh seedha apna status `[x] DONE` tick karega. Kisi doosre teammate ke approval ya agreement ki zaroorat nahi hai.

| Phase | Assigned Student | Phase Title | Status |
| :---: | :--- | :--- | :---: |
| **Phase 1** | **Ajay** | System Specs, Environment Setup & Base Architecture | `[ ] PENDING` |
| **Phase 2** | **Ajay** | Dataset Collection Protocol & Labeling Standard | `[ ] PENDING` |
| **Phase 3** | **Ameg** | Dataset Annotation, Augmentation & Partitioning | `[ ] PENDING` |
| **Phase 4** | **Ameg** | YOLO Instance Segmentation Training & Weights (`best.pt`) | `[ ] PENDING` |
| **Phase 5** | **Bablu** | Reference Marker Detection & Pixel-to-mm Calibration | `[ ] PENDING` |
| **Phase 6** | **Bablu** | Onion Contour Geometry & Physical Diameter Estimation | `[ ] PENDING` |
| **Phase 7** | **Chethan** | Configurable Rule-Based Grading Engine (`grading_rules.json`)| `[ ] PENDING` |
| **Phase 8** | **Chethan** | Automated Digital PDF Quality Report Generator | `[ ] PENDING` |
| **Phase 9** | **Gautam** | React Web Application with Dual-Input (Batch Upload + Live WebRTC Webcam) | `[ ] PENDING` |
| **Phase 10** | **Gautam** | Human-in-the-Loop Review System & Real-Time Analytics UI (React) | `[ ] PENDING` |
| **Phase 11** | **Rithvik Teja** | FastAPI REST API, Database & Media Storage Architecture (SQLAlchemy + Image Store) | `[ ] PENDING` |
| **Phase 12** | **Rithvik Teja** | End-to-End System Integration (React + FastAPI + Vision), Testing & Final Evaluation | `[ ] PENDING` |

---

## 📋 Quick Student-to-Phase Lookup

| # | Student Name | Allocated Phases | Role Title | Core Deliverables |
| :-: | :--- | :--- | :--- | :--- |
| **1** | **Ajay** | **Phase 1 & Phase 2** | Project Foundation & Dataset Strategy Lead | Base architecture, backend requirements, React scaffolding, collection protocol |
| **2** | **Ameg** | **Phase 3 & Phase 4** | Data Engineering & Deep Learning Lead | Annotated YOLO dataset, trained weights (`best.pt`), mAP evaluation curves |
| **3** | **Bablu** | **Phase 5 & Phase 6** | Computer Vision & Calibration Specialist | ArUco/Coin calibrator (`calibrator.py`), equatorial diameter estimator (`size_estimator.py`), MAE size error report |
| **4** | **Chethan** | **Phase 7 & Phase 8** | Business Logic & PDF Reporting Engineer | Configurable rules engine (`grading_engine.py` + `grading_rules.json`), automated PDF inspection certificate (`pdf_generator.py`) |
| **5** | **Gautam** | **Phase 9 & Phase 10** | React Frontend & Interactive UI Lead | React SPA (`frontend/`), Dual-Input (Upload + WebRTC Webcam), Human Review UI & Charts |
| **6** | **Rithvik Teja** | **Phase 11 & Phase 12** | FastAPI Backend, Database & QA Lead | FastAPI REST API (`src/api/`), Hybrid DB & Media Store (`image_store.py`), E2E testing |

---

## 🎯 Individual Student Detailed Assignments

---

### 1. Ajay
* **Allocated Phases**: **Phase 1 & Phase 2**
* **Primary Role**: Project Foundation, Architecture & Dataset Planning
* **Detailed Responsibilities**:
  1. Setup and maintain backend environment in `requirements.txt` and frontend scaffolding in `frontend/`.
  2. Maintain decoupled system architecture specs in `ARCHITECTURE.md`.
  3. Formulate data collection protocols (lighting, distance, angle, reference markers) in `DATASET_GUIDELINES.md`.
  4. Define standard visual taxonomy for the 4 classes (`Healthy`, `Damaged`, `Rotten`, `Sprouted`).
  5. Setup shared annotation project workspace (Roboflow / CVAT).
* **Expected Output**: Working environment, `ARCHITECTURE.md`, `DATASET_GUIDELINES.md`, configured Roboflow workspace.

---

### 2. Ameg
* **Allocated Phases**: **Phase 3 & Phase 4**
* **Primary Role**: Dataset Annotation, Augmentation & YOLO Segmentation Training
* **Detailed Responsibilities**:
  1. Label collected onion batch images with tight instance segmentation polygons in Roboflow/CVAT.
  2. Apply balanced geometric and photometric augmentations (rotation, flips, exposure).
  3. Partition data into Train (70%), Validation (20%), and Test (10%) splits with valid `data.yaml`.
  4. Train and fine-tune YOLOv8-Seg / YOLOv11-Seg instance segmentation model on custom onion classes.
  5. Evaluate loss curves and generate model weights checkpoint (`models/weights/best.pt`).
  6. Develop `src/ai/yolo_detector.py` inference wrapper class.
* **Expected Output**: Clean YOLO dataset in `data/processed/`, trained weights `best.pt`, `src/ai/yolo_detector.py`, mAP@50 report.

---

### 3. Bablu
* **Allocated Phases**: **Phase 5 & Phase 6**
* **Primary Role**: Computer Vision, Calibration & Size Estimation (OpenCV)
* **Detailed Responsibilities**:
  1. Develop OpenCV-based calibration module `src/calibration/calibrator.py` to detect ArUco markers ($40\text{ mm}$) and Indian coins ($25\text{ mm}$).
  2. Compute real-time metric scale factor ($S = \text{pixels per mm}$) from calibrated frames.
  3. Develop `src/calibration/size_estimator.py` to fit minimum enclosing ellipses on predicted onion mask contours.
  4. Calculate physical equatorial diameter ($D_{eq}$) in millimeters ($mm$).
  5. Validate size estimation error against digital Vernier Caliper readings to ensure $\text{MAE} \le 2.5\text{ mm}$.
* **Expected Output**: `src/calibration/calibrator.py`, `src/calibration/size_estimator.py`, size validation test report.

---

### 4. Chethan
* **Allocated Phases**: **Phase 7 & Phase 8**
* **Primary Role**: Regulatory Grading Logic & Automated PDF Reporting
* **Detailed Responsibilities**:
  1. Maintain regulatory grading standards in `GRADING_RULES.md` and `src/grading/grading_rules.json`.
  2. Develop `src/grading/grading_engine.py` to evaluate individual bulb size and defects into **Grade A**, **Grade B**, and **URS** (Under-Sized / Rejected).
  3. Compute lot-level percentages and batch acceptance verdicts (Accept if $\text{URS} \le 10\%$, Reject if $\text{URS} > 20\%$).
  4. Develop `src/reports/pdf_generator.py` using ReportLab to generate digital inspection certificates.
  5. Automatically embed inspection lot details, summary scorecards, defect pie charts, individual item tables, SHA-256 checksums, and audit timestamps.
* **Expected Output**: `src/grading/grading_rules.json`, `src/grading/grading_engine.py`, `src/reports/pdf_generator.py`, sample certified PDF report.

---

### 5. Gautam
* **Allocated Phases**: **Phase 9 & Phase 10**
* **Primary Role**: React Frontend, Dual-Input Handling & Human Review Experience Lead
* **Detailed Responsibilities**:
  1. Develop the React Single Page Application in `frontend/` using Vite.
  2. Implement **Dual-Input Mode**:
     - Batch File Upload mode with drag-and-drop and image thumbnail previews.
     - **Live WebRTC Webcam Mode** allowing examiners/inspectors to capture snapshots directly in the browser.
  3. Overlay detection masks, bounding boxes, size annotations, and color-coded status badges on inspected frames.
  4. Build the **Human-in-the-Loop Review System**: automatically flag predictions with confidence $< 70\%$, provide cropped bulb previews, and offer interactive override controls.
  5. Integrate dynamic re-grading and interactive analytics charts (defect distribution chart and size distribution histogram).
  6. Connect UI buttons to download official PDF certificates from the FastAPI backend.
* **Expected Output**: Complete React application in `frontend/` with dual-input camera, review interface, and chart dashboard.

---

### 6. Rithvik Teja
* **Allocated Phases**: **Phase 11 & Phase 12**
* **Primary Role**: FastAPI REST API, Database & Media Storage Architecture, System QA Lead
* **Detailed Responsibilities**:
  1. Build asynchronous FastAPI REST API application (`src/main.py` and `src/api/`).
  2. Design and maintain SQLAlchemy relational database schema supporting SQLite (local mandi) and PostgreSQL (cloud) (`src/database/models.py`).
  3. Implement dedicated media storage manager (`src/storage/image_store.py`) to store raw uploads, annotated overlays, and PDFs in `data/storage/` with SHA-256 cryptographic verification.
  4. Implement data access services (`src/database/db_service.py`) for inspection sessions, bulb measurements, and audit logs.
  5. Integrate all modules (React Frontend + FastAPI + Calibrator + YOLO AI + Grading + PDF + DB) into an end-to-end automated pipeline.
  6. Conduct integration testing (`pytest tests/`), measure end-to-end latency, compile final metrics in `EVALUATION_METRICS.md`, and write viva demo runbook.
* **Expected Output**: `src/main.py`, `src/api/`, `src/database/models.py`, `src/storage/image_store.py`, integration test suite in `tests/test_pipeline.py`, benchmark report, and viva demo script.

---

## 🤝 Collaboration & Handoff Guidelines

1. **Ajay ➔ Ameg**: Ajay finalizes data collection standards and sets up Roboflow; Ameg labels the data and trains the YOLO model.
2. **Ameg ➔ Bablu**: Ameg provides segmentation masks (`best.pt`); Bablu calculates millimeter sizes using his calibration engine.
3. **Bablu ➔ Chethan**: Bablu supplies sizes and Ameg supplies defect classes; Chethan applies grading rules and generates the PDF certificate.
4. **Chethan ➔ Gautam & Rithvik Teja**: Chethan's grading logic and PDF generator are exposed via FastAPI REST endpoints by Rithvik Teja and rendered in Gautam's React dashboard.
5. **Gautam ➔ Rithvik Teja**: Gautam's React UI sends inspections and human review overrides via REST API to Rithvik Teja's FastAPI backend, which handles DB persistence, SHA-256 image storage, and end-to-end QA.
