# Project Phases and Teammate Allocation Matrix

## 📋 Overview
To ensure a structured, auditable, and collaborative development lifecycle, the **OnionGrade AI** capstone project is partitioned into **12 Sequential Phases**. Each team member is designated ownership of **exactly 2 sequential phases**, establishing clear accountabilities, seamless input-output handoffs, and traceable contributions for academic review and viva evaluation.

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

## 👥 Team Allocation Summary Table

| Teammate | Student Name | Assigned Phases | Core Focus Area | Primary Deliverables |
| :--- | :--- | :--- | :--- | :--- |
| **Teammate 1** | **Ajay** | **Phase 1 & Phase 2** | Foundation, Architecture & Dataset Planning | Project setup, specs, collection protocol, label standards |
| **Teammate 2** | **Ameg** | **Phase 3 & Phase 4** | Data Annotation & Deep Learning Training | Annotated YOLO dataset, trained weights (`best.pt`), mAP report |
| **Teammate 3** | **Bablu** | **Phase 5 & Phase 6** | Computer Vision Calibration & Size Estimation | `calibrator.py`, `size_estimator.py`, MAE error validation |
| **Teammate 4** | **Chethan** | **Phase 7 & Phase 8** | Business Grading Logic & PDF Reporting | `grading_engine.py`, `pdf_generator.py`, official PDF output |
| **Teammate 5** | **Gautam** | **Phase 9 & Phase 10** | React Frontend & Interactive UI Lead | React SPA (`frontend/`), Dual-Input (Upload + WebRTC Webcam), Human Review UI |
| **Teammate 6** | **Rithvik Teja** | **Phase 11 & Phase 12** | FastAPI Backend, Database & QA Lead | FastAPI REST API (`src/api/`), Hybrid DB & Image Store, E2E testing |

---

## 🔄 Sequential Dependency & Workflow Graph

```
[Phase 1 & 2: Ajay]
  Architecture & Data Collection Protocol
         │
         ▼
[Phase 3 & 4: Ameg]
  Annotation & YOLO Segmentation Training (best.pt)
         │
         ▼
[Phase 5 & 6: Bablu]
  ArUco/Coin Calibration & Metric Diameter (mm)
         │
         ▼
[Phase 7 & 8: Chethan]
  Grading Engine (Grade A vs URS) & PDF Generator
         │
         ▼
[Phase 9 & 10: Gautam]
  React Web UI + WebRTC Webcam + Human-in-the-Loop Review
         │
         ▼
[Phase 11 & 12: Rithvik Teja]
  FastAPI Backend, Hybrid DB & Media Store, End-to-End QA
```

---

## 📑 Detailed Breakdown of All 12 Phases

---

### 👤 TEAMMATE 1: Ajay (Project Foundation & Dataset Strategy)
**Owner**: **Ajay**

#### 🔹 Phase 1: System Specs, Environment Setup & Base Architecture
* **Objective**: Establish the Python backend environment, React frontend scaffolding, directory structure, baseline configurations, and architectural standards.
* **Prerequisites**: None (Initial phase).
* **Step-by-Step Tasks**:
  1. Initialize Python virtual environment and lock backend dependencies in `requirements.txt` (Ultralytics, OpenCV, FastAPI, Uvicorn, ReportLab, SQLAlchemy, etc.).
  2. Setup frontend workspace in `frontend/` with React, Vite, and package dependencies.
  3. Create standard folder skeleton (`data/`, `models/`, `src/`, `frontend/`, `tests/`, `docs/`).
  4. Formulate the comprehensive system architecture documentation in `ARCHITECTURE.md`.
  5. Define clean API contracts between React frontend and FastAPI backend.
* **Deliverables**:
  - Validated `requirements.txt` & `frontend/package.json`.
  - Initialized repository structure with `.gitkeep` placeholders.
  - Complete `ARCHITECTURE.md`.
* **Completion Checklist**:
  - [ ] Virtual environment installs all Python packages without conflict.
  - [ ] Node environment runs Vite build cleanly.
  - [ ] Directory layout matches project blueprint.
  - [ ] Base architecture specification completed.

#### 🔹 Phase 2: Dataset Collection Protocol & Labeling Standard
* **Objective**: Define rigorous data collection guidelines and annotation conventions for consistent dataset creation.
* **Prerequisites**: Phase 1 completion.
* **Step-by-Step Tasks**:
  1. Define hardware capture parameters: minimum resolution (1080p), illumination (diffused white light, no hard shadows), top-down angle ($90^\circ \pm 5^\circ$), and uniform background.
  2. Define taxonomy and visual criteria for the 4 classes:
     - `Healthy`: Intact papery tunic, no blemishes, firm neck.
     - `Damaged`: Cuts, mechanical bruises, peeled outer layer exposing flesh.
     - `Rotten`: Black mold (*Aspergillus niger*), bacterial soft rot, watery mushy lesions.
     - `Sprouted`: Green shoots breaking out of the apical neck.
  3. Specify polygon annotation rules (tight polygonal boundary contours vs loose bounding boxes).
  4. Author `DATASET_GUIDELINES.md` documenting the formal collection and annotation protocols.
* **Deliverables**:
  - Master dataset guide: `DATASET_GUIDELINES.md`.
* **Completion Checklist**:
  - [ ] All 4 defect classes clearly documented with boundary standards.
  - [ ] Dataset partitioning ratio locked (70% Train, 15% Val, 15% Test).

---

### 👤 TEAMMATE 2: Ameg (Data Engineering & Deep Learning Vision)
**Owner**: **Ameg**

#### 🔹 Phase 3: Dataset Annotation, Augmentation & Partitioning
* **Objective**: Collect, label, augment, and prepare the dataset into standard YOLO polygon segmentation format.
* **Prerequisites**: Phase 2 collection protocol.
* **Step-by-Step Tasks**:
  1. Organize collected raw images inside `data/raw/`.
  2. Perform polygon segmentation annotation using tools like Roboflow or CVAT for all 4 classes.
  3. Apply domain-specific augmentations:
     - Geometric: Horizontal/Vertical flip, Random rotation ($\pm 180^\circ$).
     - Photometric: Brightness adjustment ($\pm 15\%$), Contrast variation, Gaussian blur.
  4. Export dataset in YOLOv8 Segmentation format (`data.yaml`, `images/`, `labels/`) into `data/processed/`.
* **Deliverables**:
  - Fully annotated YOLO segmentation dataset in `data/processed/`.
  - Dataset configuration file: `data/processed/data.yaml`.
* **Completion Checklist**:
  - [ ] Balanced representation across Healthy, Damaged, Rotten, and Sprouted classes.
  - [ ] Label polygons verified for precision without background bleed.

#### 🔹 Phase 4: YOLO Instance Segmentation Training & Weights (`best.pt`)
* **Objective**: Train a state-of-the-art YOLO segmentation model, evaluate convergence, and export production model weights.
* **Prerequisites**: Phase 3 processed dataset.
* **Step-by-Step Tasks**:
  1. Configure YOLOv8-Seg / YOLOv11-Seg training hyperparameters (Image size: 640x640, Epochs: 50–100, Batch size: 16, Optimizer: AdamW/SGD).
  2. Execute model training and monitor loss curves (Box loss, Seg loss, Class loss).
  3. Validate against the test set and record performance metrics:
     - Mask $mAP_{50}$ and $mAP_{50-95}$.
     - Per-class Precision, Recall, and Confusion Matrix.
  4. Save optimal weights to `models/weights/best.pt`.
  5. Implement `src/ai/yolo_detector.py` to encapsulate model loading, inference, and polygon output formatting.
* **Deliverables**:
  - Trained model weights: `models/weights/best.pt`.
  - Inference module: `src/ai/yolo_detector.py`.
  - Training log report and confusion matrix graphic in `docs/`.
* **Completion Checklist**:
  - [ ] Model achieves $mAP_{50} \ge 0.85$ on validation set.
  - [ ] Inference speed $< 50\text{ ms}$ per frame on GPU / $< 200\text{ ms}$ on CPU.

---

### 👤 TEAMMATE 3: Bablu (Computer Vision Calibration & Metrology)
**Owner**: **Bablu**

#### 🔹 Phase 5: Reference Marker Detection & Pixel-to-mm Calibration
* **Objective**: Implement an automated computer vision calibration module to establish metric scale factors ($S = \text{pixels/mm}$).
* **Prerequisites**: Phase 1 directory layout; OpenCV library.
* **Step-by-Step Tasks**:
  1. Author `CALIBRATION_GUIDE.md` detailing marker requirements, placement rules, and scale calculation mathematics.
  2. Implement ArUco marker detection in `src/calibration/calibrator.py`:
     - Detect 4x4 ArUco dictionary markers (`cv2.aruco`).
     - Calculate Euclidean distance between corner points in pixels.
     - Derive metric scale: $S = \text{pixels} / \text{known\_size\_mm}$.
  3. Implement secondary fallback calibration using a standard Indian ₹5 or ₹10 coin ($25.0\text{ mm}$ diameter) via Hough Circle Transform / contour fitting.
  4. Provide unit tests for calibration accuracy across varied distances.
* **Deliverables**:
  - Guide document: `CALIBRATION_GUIDE.md`.
  - Calibration module: `src/calibration/calibrator.py`.
* **Completion Checklist**:
  - [ ] Robust detection of ArUco marker under slight perspective tilt ($< 15^\circ$).
  - [ ] Scale factor error $< 1.5\%$ when validated against vernier caliper measurements.

#### 🔹 Phase 6: Onion Contour Geometry & Physical Diameter Estimation
* **Objective**: Translate raw segmentation mask polygons into accurate real-world equatorial diameter ($D_{eq}$) measurements in millimeters.
* **Prerequisites**: Phase 4 masks; Phase 5 calibration engine.
* **Step-by-Step Tasks**:
  1. Implement `src/calibration/size_estimator.py`.
  2. Extract contour coordinates from binary segmentation mask polygons.
  3. Fit a Minimum Bounding Ellipse (`cv2.fitEllipse`) to obtain Major Axis ($A_{major}$) and Minor Axis ($A_{minor}$) in pixels.
  4. Compute Equatorial Diameter ($D_{eq}$):
     $$D_{eq} = \frac{A_{major} + A_{minor}}{2 \times S}$$
  5. Validate measured diameters against ground-truth manual vernier caliper readings on 30 test onions.
* **Deliverables**:
  - Python module: `src/calibration/size_estimator.py`.
  - Size estimation error benchmark report (MAE $\le 2.0\text{ mm}$).
* **Completion Checklist**:
  - [ ] Correct handling of irregular contours and slight occlusions.
  - [ ] Size estimation error $\le 2\text{ mm}$ across test batch.

---

### 👤 TEAMMATE 4: Chethan (Business Grading Logic & PDF Reporting)
**Owner**: **Chethan**

#### 🔹 Phase 7: Configurable Rule-Based Grading Engine (`grading_rules.json`)
* **Objective**: Implement a decoupled, parameter-driven grading engine enforcing regulatory procurement standards.
* **Prerequisites**: Phase 6 size calculation; Phase 4 defect classes.
* **Step-by-Step Tasks**:
  1. Author `GRADING_RULES.md` documenting Ministry of Consumer Affairs, Food & Public Distribution and ICAR-DOGR onion grading norms.
  2. Create structured configuration file `src/grading/grading_rules.json`:
     - Define Grade A thresholds (e.g., Condition = Healthy, Diameter $\ge 45\text{ mm}$).
     - Define Grade B thresholds (e.g., Condition = Healthy, $35\text{ mm} \le \text{Diameter} < 45\text{ mm}$).
     - Define URS (Under-sized / Rejected / Sub-standard) thresholds (Defect $\in$ {Damaged, Rotten, Sprouted} OR Diameter $< 35\text{ mm}$).
     - Define lot acceptance limits (Accept: URS $\le 10\%$, Conditional: $10\% < \text{URS} \le 20\%$, Reject: URS $> 20\%$).
  3. Implement `src/grading/grading_engine.py` to evaluate individual onions and aggregate lot-level verdicts.
* **Deliverables**:
  - Specification guide: `GRADING_RULES.md`.
  - Rules file: `src/grading/grading_rules.json`.
  - Python engine: `src/grading/grading_engine.py`.
* **Completion Checklist**:
  - [ ] Zero hard-coded thresholds in Python code; all values loaded from JSON.
  - [ ] Engine correctly grades individual bulbs and calculates aggregate lot status.

#### 🔹 Phase 8: Automated Digital PDF Quality Report Generator
* **Objective**: Generate official, tamper-evident digital inspection PDF certificates for procurement audits.
* **Prerequisites**: Phase 7 grading outputs; ReportLab library.
* **Step-by-Step Tasks**:
  1. Implement `src/reports/pdf_generator.py` using ReportLab Platypus:
     - **Header**: Mandi/Center Name, Lot Number, Date/Time, Inspector Name.
     - **Executive Scorecard**: Overall Lot Verdict (ACCEPTED / REJECTED), Total Sample Count, Grade A %, URS %.
     - **Defect Breakdown Visuals**: Embedded Matplotlib pie/bar chart showing Healthy vs Rotten vs Damaged vs Sprouted percentages.
     - **Bulb-by-Bulb Table**: Individual bulb index, diameter ($mm$), condition class, confidence, and assigned grade.
     - **Audit Footer**: Model Version, Image SHA-256 Checksums, Grading Rule Version, Timestamp, and Digital Signature line.
  2. Implement temporary image cleanup and PDF export to `data/storage/reports/`.
* **Deliverables**:
  - Python module: `src/reports/pdf_generator.py`.
  - Sample generated PDF report: `data/storage/reports/sample_inspection_report.pdf`.
* **Completion Checklist**:
  - [ ] PDF generates in $< 1.5\text{ seconds}$.
  - [ ] Styling adheres to professional government certification standards.

---

### 👤 TEAMMATE 5: Gautam (React Frontend & Interactive UI Lead)
**Owner**: **Gautam**

#### 🔹 Phase 9: React Web Application with Dual-Input (Batch Upload + Live WebRTC Webcam)
* **Objective**: Build the user-facing Single Page Application (SPA) in modern React (Vite) with seamless file upload and live camera capture capabilities.
* **Prerequisites**: Phase 1 structure; Phase 11 FastAPI endpoints.
* **Step-by-Step Tasks**:
  1. Initialize React + Vite application in `frontend/` with modern UI components and styling.
  2. Construct layout with header controls: Mandi selector, Lot ID input, Inspector Name, and Live Status indicator.
  3. Implement **Dual-Input Selector Component**:
     - **Mode 1 (Batch File Upload)**: HTML5 drag-and-drop supporting single and batch image selection (`.jpg`, `.jpeg`, `.png`) with instant client-side thumbnail previews.
     - **Mode 2 (Live WebRTC Webcam)**: Direct browser webcam integration via `navigator.mediaDevices.getUserMedia` and Canvas rendering, enabling inspectors to place onion lots under the camera and capture instant high-resolution frames.
  4. Wire file/snapshot payloads to FastAPI REST endpoint (`POST /api/inspect/upload` or `/api/inspect/webcam`).
  5. Render detection visual overlay: annotated bounding boxes, segmentation masks, labels, and color-coded status badges (Green = Grade A, Red = URS).
* **Deliverables**:
  - React application shell in `frontend/src/`.
  - `ImageUploader.jsx` & `WebcamCapture.jsx` components.
  - Interactive SVG/Canvas defect overlay rendering.
* **Completion Checklist**:
  - [ ] Drag-and-drop batch upload previews and dispatches images seamlessly.
  - [ ] WebRTC webcam feed streams smoothly and captures crisp snapshot frames.
  - [ ] Visual overlays cleanly render YOLO masks and millimeter size badges.

#### 🔹 Phase 10: Human-in-the-Loop Review System & Real-Time Analytics UI (React)
* **Objective**: Provide an interactive manual review workflow for low-confidence predictions, real-time analytics charts, and report triggering.
* **Prerequisites**: Phase 9 React UI; Phase 7 grading engine; Phase 11 override API.
* **Step-by-Step Tasks**:
  1. In React, filter all detections where $\text{Confidence} < 0.70$ and display an interactive "Human-in-the-Loop Review" section.
  2. Render dedicated inspection cards showing cropped views of uncertain bulbs alongside AI predictions.
  3. Provide interactive controls allowing the inspector to manually override the defect class (e.g., change "Rotten" to "Damaged") with mandatory audit notes.
  4. Implement dynamic re-grading: calling `POST /api/inspections/{id}/override` updates lot percentages and verdict in real time without reloading.
  5. Add interactive dashboard charts: Defect distribution donut chart, Size distribution histogram with Grade A cutoff threshold line.
  6. Add "Download Official PDF Report" button wired to the backend streaming endpoint `GET /api/inspections/{id}/report`.
* **Deliverables**:
  - `HumanReviewModal.jsx` and `AnalyticsDashboard.jsx` components in `frontend/src/components/`.
  - Dynamic client-side state management for live re-grading.
* **Completion Checklist**:
  - [ ] Low-confidence cases clearly highlighted with manual override controls.
  - [ ] Real-time updates reflect inspector overrides across all charts and summary cards.
  - [ ] One-click PDF report download functional from the UI.

---

### 👤 TEAMMATE 6: Rithvik Teja (FastAPI Backend, Database & System QA Lead)
**Owner**: **Rithvik Teja**

#### 🔹 Phase 11: FastAPI REST API, Database & Media Storage Architecture
* **Objective**: Architect and integrate the asynchronous FastAPI backend, relational database layer, and auditable media storage service.
* **Prerequisites**: Phase 1 structure; SQLAlchemy ORM; FastAPI framework.
* **Step-by-Step Tasks**:
  1. Implement FastAPI application in `src/main.py` with CORS middleware, static file mounts for `/media/`, and structured router mounts.
  2. Design relational database schema in `src/database/models.py`:
     - `Inspection`: `id`, `lot_number`, `mandi_name`, `inspector_name`, `total_bulbs`, `grade_a_count`, `urs_count`, `lot_verdict`, `urs_percentage`, `avg_diameter_mm`, `created_at`.
     - `BulbRecord`: `id`, `inspection_id`, `bulb_index`, `condition_class`, `confidence`, `diameter_mm`, `grade`, `is_flagged_for_review`, `inspector_override`, `bbox_json`, `mask_polygon_json`.
     - `ImageAsset`: `id`, `inspection_id`, `image_type` (`original`, `annotated`, `crop`, `report_pdf`), `file_path`, `file_url`, `file_size_bytes`, `sha256_hash`.
     - `AuditLog`: `id`, `inspection_id`, `bulb_id`, `inspector_name`, `action`, `previous_value`, `new_value`, `reason`, `timestamp`.
  3. Implement image storage service in `src/storage/image_store.py`:
     - Secure file-system storage for raw uploads, annotated overlays, and PDF certificates in `data/storage/`.
     - SHA-256 cryptographic hashing on all stored assets to prevent tampering.
  4. Implement database service in `src/database/db_service.py` with CRUD queries for inspection sessions, bulb metrics, and history.
  5. Implement FastAPI REST routes in `src/api/`:
     - `POST /api/inspect/upload` & `POST /api/inspect/webcam`: Orchestrates vision, calibration, grading, media storage, and DB persistence.
     - `GET /api/inspections`: Returns paginated history for React dashboard.
     - `POST /api/inspections/{id}/override`: Records audit log and recalculates lot verdict.
     - `GET /api/inspections/{id}/report`: Compiles and streams official PDF report.
* **Deliverables**:
  - FastAPI server: `src/main.py` and `src/api/`.
  - Database schema & connection: `src/database/models.py`, `src/database/connection.py`, `src/database/db_service.py`.
  - Media storage manager: `src/storage/image_store.py`.
* **Completion Checklist**:
  - [ ] All inspection sessions, bulb measurements, and images automatically indexed with SHA-256 hashes.
  - [ ] API endpoints respond reliably with complete OpenAPI Swagger documentation at `/docs`.
  - [ ] Zero database bloat by keeping heavy image binaries in dedicated storage rather than SQL BLOBs.

#### 🔹 Phase 12: End-to-End Pipeline Integration, Testing & Final Evaluation
* **Objective**: Wire all individual teammate modules into a seamless decoupled application, conduct benchmark evaluations, and prepare presentation artifacts.
* **Prerequisites**: Completion of Phases 1 through 11.
* **Step-by-Step Tasks**:
  1. Author `EVALUATION_METRICS.md` documenting experimental validation methodology.
  2. Write end-to-end integration tests in `tests/test_pipeline.py` testing FastAPI endpoints, vision pipeline, DB transactions, and PDF generation.
  3. Benchmark overall system performance:
     - End-to-end Latency (Image Upload ➔ Inference ➔ Calibration ➔ Grading ➔ Storage ➔ JSON Response).
     - Model Performance: Mask $mAP_{50}$, Precision, Recall, F1-Score, Confusion Matrix.
     - Size Estimation Accuracy: Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE) against manual caliper measurements.
  4. Conduct stress testing with edge cases (corrupted images, extreme lighting, heavy occlusion, missing reference marker).
  5. Prepare project presentation slide deck outline and viva demonstration runbook.
* **Deliverables**:
  - `EVALUATION_METRICS.md`.
  - Comprehensive test suite in `tests/test_pipeline.py`.
  - Final Performance Benchmarking Report.
  - Viva Demonstration Runbook.
* **Completion Checklist**:
  - [ ] All unit and integration tests passing (`pytest tests/`).
  - [ ] Decoupled React frontend and FastAPI backend operate seamlessly without CORS or latency issues.
  - [ ] Comprehensive performance metrics compiled for capstone report and defense.
