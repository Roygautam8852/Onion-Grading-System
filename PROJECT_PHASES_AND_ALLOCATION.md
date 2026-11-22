# Project Phases and Teammate Allocation Matrix

## 📋 Overview
To ensure a structured, auditable, and collaborative development lifecycle, the **OnionGrade AI** capstone project is partitioned into **12 Sequential Phases**. Each team member is designated ownership of **exactly 2 sequential phases**, establishing clear accountabilities, seamless input-output handoffs, and traceable contributions for academic review and viva evaluation.

---

## ⚡ Live Phase Status Tracker

> **Independent Execution**: Har teammate apne assigned phases ka sole owner hai. Jaise hi kisi teammate ka code/task complete hota hai, woh seedha apna status [x] DONE tick karega. Kisi doosre teammate ke approval ya agreement ki zaroorat nahi hai.

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
| **Phase 9** | **Gautam** | Streamlit Web UI with Dual-Input (Upload + Live Webcam) | `[ ] PENDING` |
| **Phase 10** | **Gautam** | Human-in-the-Loop Review System & Visual Analytics | `[ ] PENDING` |
| **Phase 11** | **Rithvik Teja** | Database Persistence, History & Traceability (SQLite) | `[ ] PENDING` |
| **Phase 12** | **Rithvik Teja** | End-to-End Pipeline Integration, Testing & Final Evaluation | `[ ] PENDING` |

---

## 👥 Team Allocation Summary Table

| Teammate | Student Name | Assigned Phases | Core Focus Area | Primary Deliverables |
| :--- | :--- | :--- | :--- | :--- |
| **Teammate 1** | **Ajay** | **Phase 1 & Phase 2** | Foundation, Architecture & Dataset Planning | Project setup, specs, collection protocol, label standards |
| **Teammate 2** | **Ameg** | **Phase 3 & Phase 4** | Data Annotation & Deep Learning Training | Annotated YOLO dataset, trained weights (`best.pt`), mAP report |
| **Teammate 3** | **Bablu** | **Phase 5 & Phase 6** | Computer Vision Calibration & Size Estimation | `calibration.py`, `size_estimator.py`, MAE error validation |
| **Teammate 4** | **Chethan** | **Phase 7 & Phase 8** | Business Grading Logic & PDF Reporting | `grading_engine.py`, `pdf_generator.py`, official PDF output |
| **Teammate 5** | **Gautam** | **Phase 9 & Phase 10** | Streamlit Web Dashboard & Dual-Input UI | `app.py`, File Upload + Live Webcam mode, Human Review UI |
| **Teammate 6** | **Rithvik Teja** | **Phase 11 & Phase 12** | Database Persistence, Integration & Evaluation | `database.py`, end-to-end integration, final testing & metrics |

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
  Streamlit Web UI + Webcam + Human-in-the-Loop Review
         │
         ▼
[Phase 11 & 12: Rithvik Teja]
  SQLite Persistence, System Testing & Evaluation Report
```

---

## 📑 Detailed Breakdown of All 12 Phases

---

### 👤 TEAMMATE 1: Ajay (Project Foundation & Dataset Strategy)
**Owner**: **Ajay**

#### 🔹 Phase 1: System Specs, Environment Setup & Base Architecture
* **Objective**: Establish the Python development environment, directory structure, baseline configurations, and architectural standards.
* **Prerequisites**: None (Initial phase).
* **Step-by-Step Tasks**:
  1. Initialize the Python virtual environment and lock required dependencies in `requirements.txt` (Ultralytics, OpenCV, Streamlit, ReportLab, SQLAlchemy, etc.).
  2. Create standard folder skeleton (`data/`, `models/`, `src/`, `tests/`, `docs/`).
  3. Formulate the comprehensive system architecture documentation in `ARCHITECTURE.md`.
  4. Write base dummy interfaces so subsequent modules can begin independent unit development without waiting for model weights.
* **Deliverables**:
  - Validated `requirements.txt`.
  - Initialized repository structure with `.gitkeep` placeholders.
  - Complete `ARCHITECTURE.md`.
* **Completion Checklist**:
  - [ ] Virtual environment installs all packages without conflict.
  - [ ] Directory layout matches project blueprint.
  - [ ] Base architecture specification completed.

#### 🔹 Phase 2: Dataset Collection Protocol & Labeling Standard
* **Objective**: Define rigorous data collection guidelines and annotation conventions for consistent dataset creation.
* **Prerequisites**: Phase 1 completion.
* **Step-by-Step Tasks**:
  1. Define hardware capture parameters: minimum resolution (1080p), illumination (diffused white light, no hard shadows), top-down angle ($90^\circ \pm 5^\circ$), and uniform background.
  2. Define taxonomy and visual criteria for the 4 classes:
     - `0: Healthy` (firm, dry outer skin, no cuts, no green shoots, uniform coloration).
     - `1: Damaged` (mechanical cuts, skin punctures, crushed surfaces).
     - `2: Rotten` (black mold, fungal discoloration, soft/watery rot, foul decay).
     - `3: Sprouted` (visible green vegetative shoot emerging from the neck).
  3. Create reference marker standards: ArUco Marker ID 0 (4x4) or standard ₹5 / ₹10 coin placed within the capture frame.
  4. Setup a shared annotation workspace (Roboflow / CVAT) and author `DATASET_GUIDELINES.md`.
* **Deliverables**:
  - Comprehensive `DATASET_GUIDELINES.md`.
  - Roboflow/CVAT project workspace configured with class ontology.
* **Completion Checklist**:
  - [ ] Visual identification guide for each defect class documented.
  - [ ] Reference marker dimensions and printing sheet prepared.
  - [ ] Team trained on annotation standards.

---

### 👤 TEAMMATE 2: Ameg (Dataset Annotation & Deep Learning Training)
**Owner**: **Ameg**

#### 🔹 Phase 3: Dataset Annotation, Augmentation & Partitioning
* **Objective**: Label raw onion images with precise segmentation polygons and generate an augmented, partitioned YOLO dataset.
* **Prerequisites**: Raw images collected as per Phase 2 guidelines.
* **Step-by-Step Tasks**:
  1. Import raw image batches into Roboflow/CVAT.
  2. Draw tight polygon segmentation masks around individual onion bulbs, labeling each with its respective defect class.
  3. Apply balanced data augmentations:
     - Rotation ($\pm 180^\circ$ for orientation invariance).
     - Brightness adjustment ($\pm 15\%$).
     - Scale/zoom ($\pm 10\%$).
     - Horizontal and vertical flips.
  4. Partition the dataset into strict splits: **70% Train, 20% Validation, 10% Test** (preventing image leakage across splits).
  5. Export dataset in YOLOv8-Seg format with `data.yaml` to `data/processed/`.
* **Deliverables**:
  - Clean, quality-audited dataset in `data/processed/`.
  - `data.yaml` pointing to train, val, and test paths.
  - Class distribution report showing balanced sample counts.
* **Completion Checklist**:
  - [ ] Zero missing labels or overlapping ambiguous polygons.
  - [ ] All 4 classes represented adequately in train, val, and test splits.
  - [ ] `data.yaml` verified with valid relative paths.

#### 🔹 Phase 4: YOLO Instance Segmentation Model Training & Fine-Tuning
* **Objective**: Fine-tune a pretrained YOLOv8-Seg / YOLOv11-Seg instance segmentation network on custom onion defect classes.
* **Prerequisites**: Phase 3 dataset export (`data.yaml`).
* **Step-by-Step Tasks**:
  1. Configure training pipeline using PyTorch and Ultralytics:
     - Pretrained backbone: `yolov8n-seg.pt` or `yolov8s-seg.pt`.
     - Image size: $640 \times 640$ pixels.
     - Epochs: 100 with early stopping patience of 15.
     - Batch size: 16 (or GPU memory limit).
  2. Monitor training loss convergence (box loss, segmentation loss, classification loss, DFL loss).
  3. Evaluate validation set: compute $mAP_{50}$ and $mAP_{50-95}$ for mask and bounding box heads.
  4. Save optimal weights to `models/weights/best.pt`.
  5. Develop `src/ai/yolo_detector.py` providing an inference wrapper class `OnionDetector` returning parsed contours, class labels, and confidence scores.
* **Deliverables**:
  - Trained model weights: `models/weights/best.pt`.
  - Training metrics plots (loss curves, confusion matrix, PR curves).
  - Python inference module: `src/ai/yolo_detector.py`.
* **Completion Checklist**:
  - [ ] Model reaches target $mAP_{50} \ge 85\%$ on test split.
  - [ ] `OnionDetector.predict(image)` returns bounding boxes, masks, classes, and confidence scores in $< 100\text{ ms}$.

---

### 👤 TEAMMATE 3: Bablu (Calibration & Metric Size Estimation)
**Owner**: **Bablu**

#### 🔹 Phase 5: Reference Marker Detection & Pixel-to-mm Calibration
* **Objective**: Implement an OpenCV-based calibration engine to compute physical metric scale factors from calibrated images.
* **Prerequisites**: Phase 1 folder structure; standard reference marker (ArUco / Coin).
* **Step-by-Step Tasks**:
  1. Author `CALIBRATION_GUIDE.md` detailing mathematical principles and marker placement.
  2. Implement `src/calibration/calibrator.py`:
     - Method 1: **ArUco Marker Detection** (`cv2.aruco`) - Detects 4 corner coordinates of a 40 mm ArUco marker, calculates pixel perimeter/side length, and derives:
       $$\text{scale} = \frac{\text{pixel\_width}}{\text{known\_width\_mm}} \quad (\text{pixels/mm})$$
     - Method 2: **Circular Coin Detection** (`cv2.HoughCircles` / threshold contours) - Detects ₹5 coin ($25.0\text{ mm}$ diameter) as fallback.
  3. Implement perspective rectification if camera angle is slightly tilted.
* **Deliverables**:
  - `CALIBRATION_GUIDE.md`.
  - Python module: `src/calibration/calibrator.py`.
  - Unit tests in `tests/test_calibration.py`.
* **Completion Checklist**:
  - [ ] Marker detector reliably extracts scale factor under variable lighting.
  - [ ] Graceful fallback/error message if no marker is found.

#### 🔹 Phase 6: Onion Contour Geometry & Physical Diameter Estimation
* **Objective**: Compute equatorial diameter ($D_{eq}$) in millimeters from segmented onion masks using calibrated scale factors.
* **Prerequisites**: Phase 4 segmentation masks + Phase 5 calibration engine.
* **Step-by-Step Tasks**:
  1. Implement `src/calibration/size_estimator.py`.
  2. For each onion mask, extract external boundary contour points.
  3. Fit geometric primitives:
     - Minimum Bounding Ellipse (`cv2.fitEllipse`) to obtain Major Axis ($A_{major}$) and Minor Axis ($A_{minor}$).
     - Minimum Enclosing Circle (`cv2.minEnclosingCircle`).
  4. Calculate Equatorial Diameter:
     $$D_{eq} = \frac{A_{major} + A_{minor}}{2 \times \text{scale}}$$
  5. Validate accuracy against physical Vernier Caliper measurements of 20 physical test onions to compute Mean Absolute Error (MAE):
     $$\text{MAE} = \frac{1}{N}\sum_{i=1}^N |D_{predicted} - D_{actual}| \le 2.5\text{ mm}$$
* **Deliverables**:
  - Python module: `src/calibration/size_estimator.py`.
  - Size validation report comparing caliper readings vs predicted mm.
* **Completion Checklist**:
  - [ ] Diameter calculation functional on arbitrary polygon shapes.
  - [ ] Size estimation error $\text{MAE} \le 2.5\text{ mm}$ on calibrated test set.

---

### 👤 TEAMMATE 4: Chethan (Grading Logic & PDF Reporting Engine)
**Owner**: **Chethan**

#### 🔹 Phase 7: Configurable Rule-Based Grading Engine
* **Objective**: Build a decoupled, configurable grading module that separates visual perception from procurement policy rules.
* **Prerequisites**: Phase 1 structure; ICAR-DOGR / Ministry grading standards.
* **Step-by-Step Tasks**:
  1. Author `GRADING_RULES.md` and define `src/grading/grading_rules.json`:
     - Grade A: Diameter $\ge 45\text{ mm}$ AND Condition == `Healthy`.
     - Grade B: Diameter between $35\text{ mm}$ and $44.9\text{ mm}$ AND Condition == `Healthy` (optional secondary tier).
     - URS (Under-sized / Rejected / Sub-standard): Diameter $< 35\text{ mm}$ OR Condition in [`Damaged`, `Rotten`, `Sprouted`].
  2. Develop `src/grading/grading_engine.py`:
     - Evaluates each bulb against configured thresholds.
     - Computes batch statistics: Total Count, Grade A count & %, URS count & %, Defect breakdown %.
     - Assigns Lot Acceptance Status: Accept if $\text{URS} \le 10\%$, Conditional Accept if $10\% < \text{URS} \le 20\%$, Reject if $\text{URS} > 20\%$.
* **Deliverables**:
  - `GRADING_RULES.md` and `src/grading/grading_rules.json`.
  - Python module: `src/grading/grading_engine.py`.
  - Unit tests in `tests/test_grading.py`.
* **Completion Checklist**:
  - [ ] Rule thresholds can be modified in JSON without altering Python code.
  - [ ] Correct percentage and lot verdict calculations verified via unit tests.

#### 🔹 Phase 8: Automated Digital PDF Quality Report Generator
* **Objective**: Generate an instant, downloadable, audit-ready PDF quality inspection certificate using ReportLab.
* **Prerequisites**: Phase 7 grading engine output.
* **Step-by-Step Tasks**:
  1. Implement `src/reports/pdf_generator.py` using `reportlab.platypus`:
     - **Header Block**: Organization logo, "National Onion Quality Inspection Certificate", Inspection ID, Date, Inspector Name, Procurement Center.
     - **Lot Summary Table**: Total Samples, Grade A Count & %, URS Count & %, Overall Batch Quality Verdict (PASS / CONDITIONAL / REJECT).
     - **Embedded Defect Pie Chart**: Matplotlib-generated graphic displaying percentage of Healthy, Damaged, Rotten, and Sprouted bulbs.
     - **Individual Item Breakdown Table**: Tabular list of every detected onion with Bulb ID, Diameter (mm), Detected Condition, AI Confidence Score, and Final Grade.
     - **Audit Footer**: Model Version, Grading Rule Version, Timestamp, and Digital Signature line.
  2. Implement temporary image cleanup and PDF export to `data/reports/`.
* **Deliverables**:
  - Python module: `src/reports/pdf_generator.py`.
  - Sample generated PDF report: `data/reports/sample_inspection_report.pdf`.
* **Completion Checklist**:
  - [ ] PDF generates in $< 1.5\text{ seconds}$.
  - [ ] Styling adheres to professional government certification standards.

---

### 👤 TEAMMATE 5: Gautam (Web Dashboard & Dual-Input Interface)
**Owner**: **Gautam**

#### 🔹 Phase 9: Streamlit Web UI with Dual-Input (Upload + Live Webcam)
* **Objective**: Build the user-facing web application with support for both static file uploads and live camera feeds.
* **Prerequisites**: Phase 1 structure; Phase 4 inference module; Phase 5 calibrator.
* **Step-by-Step Tasks**:
  1. Implement `src/ui/app.py` using Streamlit.
  2. Construct layout with sidebar controls (Lot ID input, Inspector Name, Confidence threshold slider, Grading rule profile selector).
  3. Implement **Dual-Input Selector**:
     - **Mode 1 (File Upload)**: `st.file_uploader` supporting single and batch image selection (`.jpg`, `.png`).
     - **Mode 2 (Live Webcam)**: `st.camera_input` allowing operators/professors to place an onion in front of their laptop camera and snap a live frame for immediate analysis.
  4. Render detection visual overlay: annotated bounding boxes, segmentation masks, labels, and color-coded status (Green = Grade A, Red = URS).
* **Deliverables**:
  - Python Streamlit dashboard: `src/ui/app.py`.
  - Dual-input handling functions.
* **Completion Checklist**:
  - [ ] Drag-and-drop upload processes images seamlessly.
  - [ ] Webcam snapshot triggers full AI pipeline on live-captured frames.
  - [ ] Visual overlays display clean masks and text labels.

#### 🔹 Phase 10: Human-in-the-Loop Review System & Visual Analytics
* **Objective**: Provide a manual review workflow for low-confidence predictions and interactive dashboard analytics.
* **Prerequisites**: Phase 9 web UI; Phase 7 grading engine.
* **Step-by-Step Tasks**:
  1. In `src/ui/app.py`, filter all detections where $\text{Confidence} < 0.70$.
  2. Display "Attention Needed: Low Confidence Predictions" section with cropped preview of uncertain onions.
  3. Provide interactive dropdowns allowing the inspector to manually override the AI defect class (e.g., change "Rotten" to "Damaged").
  4. Implement dynamic recalculation: clicking "Update & Re-grade" refreshes the batch grading percentages immediately.
  5. Add interactive Plotly charts: Defect distribution donut chart, Size distribution histogram with Grade A cutoff line.
  6. Add "Download Official PDF Report" button wired to Teammate 4's PDF engine.
* **Deliverables**:
  - Human review components integrated into `src/ui/app.py`.
  - Interactive Plotly analytics dashboards.
* **Completion Checklist**:
  - [ ] Low-confidence cases clearly highlighted with manual override controls.
  - [ ] Recalculate button updates summary statistics in real time.
  - [ ] One-click PDF report download functional.

---

### 👤 TEAMMATE 6: Rithvik Teja (Database, Integration & Final Evaluation)
**Owner**: **Rithvik Teja**

#### 🔹 Phase 11: Database Persistence, History & Traceability (SQLite)
* **Objective**: Architect and integrate the relational database layer for lot auditability and historical tracking.
* **Prerequisites**: Phase 1 structure; SQLAlchemy ORM.
* **Step-by-Step Tasks**:
  1. Implement `src/database/models.py` defining:
     - `Inspection`: `id`, `lot_id`, `inspector_name`, `timestamp`, `total_onions`, `grade_a_pct`, `urs_pct`, `verdict`, `model_version`, `rule_version`.
     - `OnionRecord`: `id`, `inspection_id`, `bulb_index`, `diameter_mm`, `defect_class`, `confidence`, `grade`, `was_manually_edited`.
  2. Implement `src/database/db_service.py` with CRUD functions: `save_inspection()`, `get_inspection_history()`, `export_history_csv()`.
  3. Add an "Inspection History & Audit Log" tab in `src/ui/app.py` allowing users to browse past lots, view timestamped results, and re-download past PDF reports.
* **Deliverables**:
  - Database schema: `src/database/models.py`.
  - Service functions: `src/database/db_service.py`.
  - UI inspection history viewer.
* **Completion Checklist**:
  - [ ] Every inspection automatically saved to SQLite database.
  - [ ] History tab displays searchable log with re-downloadable reports.

#### 🔹 Phase 12: End-to-End Pipeline Integration, Testing & Final Evaluation
* **Objective**: Wire all individual teammate modules into a seamless application, conduct benchmark evaluations, and prepare presentation artifacts.
* **Prerequisites**: Completion of Phases 1 through 11.
* **Step-by-Step Tasks**:
  1. Author `EVALUATION_METRICS.md` documenting experimental validation methodology.
  2. Write end-to-end integration tests in `tests/test_pipeline.py`.
  3. Benchmark overall system performance:
     - Inference Latency & FPS on CPU and GPU.
     - Final Model Metrics: $mAP_{50}$, Precision, Recall, F1-Score, Confusion Matrix.
     - Size Estimation Accuracy: Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE).
  4. Conduct end-to-end stress tests with edge cases (empty images, bad lighting, heavy occlusion, no reference marker).
  5. Prepare project presentation slide deck outline and viva demonstration runbook.
* **Deliverables**:
  - `EVALUATION_METRICS.md`.
  - Test suite in `tests/test_pipeline.py`.
  - Final Performance Benchmarking Report.
  - Viva Demonstration Runbook.
* **Completion Checklist**:
  - [ ] All unit and integration tests passing (`pytest tests/`).
  - [ ] System operates reliably on both uploaded images and live webcam inputs.
  - [ ] Comprehensive performance metrics compiled for capstone report and defense.
