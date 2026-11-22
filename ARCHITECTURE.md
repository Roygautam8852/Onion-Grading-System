# System Architecture & Technical Specifications

## 🏛️ High-Level System Architecture

The **OnionGrade AI** system is architected as an end-to-end, modular, pure-Python quality assessment platform. It adheres to the principle of **separation of perception from policy**: deep learning models observe and classify visual properties, while a separate, versioned business rules engine applies regulatory grading criteria.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        PRESENTATION LAYER (STREAMLIT)                  │
│                                                                        │
│   ┌───────────────────────────┐      ┌───────────────────────────────┐ │
│   │   Mode 1: File Uploader   │      │    Mode 2: Live Webcam Input  │ │
│   │   (Batch JPG / PNG)       │      │    (st.camera_input Snapshot) │ │
│   └─────────────┬─────────────┘      └───────────────┬───────────────┘ │
│                 │                                    │                 │
│                 └─────────────────┬──────────────────┘                 │
│                                   ▼                                    │
│                     Input Image Frame (RGB Array)                      │
└───────────────────────────────────┼────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                     COMPUTER VISION & PERCEPTION LAYER                 │
│                                                                        │
│   ┌───────────────────────────────────┐                                │
│   │  Pre-Processing & Validation      │ ──> Validates resolution, size │
│   └─────────────────┬─────────────────┘                                │
│                     │                                                  │
│         ┌───────────┴───────────┐                                      │
│         ▼                       ▼                                      │
│   ┌───────────────┐      ┌───────────────────────────────────────────┐ │
│   │ Calibration   │      │ AI Instance Segmentation & Classification │ │
│   │ Engine        │      │ (YOLOv8-Seg / YOLOv11-Seg)                │ │
│   │ (ArUco / Coin)│      │                                           │ │
│   │               │      │ - Individual Bulb Localization            │ │
│   │ Output:       │      │ - Defect Classes: Healthy, Damaged,       │ │
│   │ pixels_per_mm │      │                   Rotten, Sprouted        │ │
│   └───────┬───────┘      │ - Mask Polygons & Confidence Scores       │ │
│           │              └─────────────────────┬─────────────────────┘ │
│           │                                    │                       │
│           └─────────────────┬──────────────────┘                       │
│                             ▼                                          │
│   ┌──────────────────────────────────────────────────────────────────┐ │
│   │ Size Estimation Engine (Contour Geometry)                        │ │
│   │ - Minimum Bounding Ellipse & Feret Diameter                      │ │
│   │ - Computes Equatorial Diameter in Millimeters (mm)               │ │
│   └─────────────────────────┬────────────────────────────────────────┘ │
└─────────────────────────────┼──────────────────────────────────────────┘
                              │ Per-Bulb: {id, mask, defect, conf, size_mm}
                              ▼
┌────────────────────────────────────────────────────────────────────────┐
│                     BUSINESS RULES & GRADING LAYER                     │
│                                                                        │
│   ┌────────────────────────────────────────────────────────────────┐   │
│   │ Configurable Grading Engine (grading_rules.json)               │   │
│   │ - Size Thresholds (e.g., Min Diameter: 45 mm for Grade A)      │   │
│   │ - Quality Rules (Defect == Healthy -> Grade A; else URS)       │   │
│   │ - Lot Acceptance: Accept if URS <= 10%, Reject if URS > 20%    │   │
│   └─────────────────────────┬──────────────────────────────────────┘   │
└─────────────────────────────┼──────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌───────────────────────────┐   ┌────────────────────────────────────────┐
│ HUMAN-IN-THE-LOOP LAYER   │   │ PERSISTENCE & REPORTING LAYER          │
│                           │   │                                        │
│ - Filters Conf < 70%      │   │ ┌────────────────────────────────────┐ │
│ - Inspector override card │   │ │ SQLite Traceability DB             │ │
│ - Dynamic re-grading      │   │ │ (Inspections, Bulbs, Audit Logs)   │ │
│                           │   │ └────────────────────────────────────┘ │
│                           │   │ ┌────────────────────────────────────┐ │
│                           │   │ │ PDF Report Generator (ReportLab)   │ │
│                           │   │ │ (Instant Certified PDF Download)   │ │
│                           │   │ └────────────────────────────────────┘ │
└───────────────────────────┘   └────────────────────────────────────────┘
```

---

## 🧩 Architectural Components

### 1. Dual-Input Presentation Layer (`src/ui/app.py`)
Provides a web-based, pure-Python dashboard implemented using Streamlit:
* **Batch Upload Mode**: Operators can upload single or multi-image batches captured in the field or inspection table.
* **Live Laptop Webcam Mode**: Uses Streamlit's `st.camera_input` component to activate the operator's laptop/external webcam directly in the browser. The operator holds an onion bulb and reference marker in the frame, clicks "Capture Snapshot", and the image is piped directly into the inference pipeline without saving intermediate temporary files.
* **Annotated Visual Overlay**: Draws clean segmentation masks, bounding boxes, defect labels, and color-coded status badges directly onto the input frame.

### 2. Reference Marker Calibration Engine (`src/calibration/calibrator.py`)
Computes the metric scale factor ($S = \text{pixels per mm}$) to translate image coordinates into real-world dimensions:
* **Primary Target (ArUco Marker)**: Scans the frame for a predefined 4x4 ArUco dictionary marker with known physical side length (e.g., $40.0\text{ mm}$).
* **Secondary Target (Standard ₹5 / ₹10 Coin)**: Fallback circular contour detection for a $25.0\text{ mm}$ coin.
* **Scale Factor Formula**:
  $$S = \frac{\text{Measured Marker Width in Pixels}}{\text{Known Marker Width in mm}}$$

### 3. AI Vision Layer (`src/ai/yolo_detector.py`)
Performs simultaneous instance localization, polygon segmentation, and multi-class condition classification:
* **Architecture**: Fine-tuned YOLOv8-Seg / YOLOv11-Seg instance segmentation network.
* **Classes**:
  1. `Healthy`: Uniform coloration, dry outer skin, free from rot or mechanical injury.
  2. `Damaged`: Mechanical gouges, skin abrasions, cracked or crushed bulbs.
  3. `Rotten`: Fungal decay, bacterial soft rot, black mold spores.
  4. `Sprouted`: Visible vegetative green shoot emergence at the neck.
* **Outputs**: Array of detections containing bounding boxes $[x_1, y_1, x_2, y_2]$, polygon mask arrays, class indices, and confidence scores $C \in [0.0, 1.0]$.

### 4. Size Estimation Engine (`src/calibration/size_estimator.py`)
Extracts physical dimensions from segmented onion mask polygons:
* Computes the convex hull and contour of the binary mask.
* Fits a minimum bounding ellipse (`cv2.fitEllipse`) to identify major and minor axes.
* Derives the **Equatorial Diameter** ($D_{eq}$):
  $$D_{eq} = \frac{\text{Major\_Axis\_Pixels} + \text{Minor\_Axis\_Pixels}}{2 \times S}$$
* Outputs diameter in millimeters ($mm$) for each detected bulb.

### 5. Configurable Grading Engine (`src/grading/grading_engine.py`)
A decoupled business rules engine reading from an external `grading_rules.json` file:
* **Grade A Criteria**:
  - Condition == `Healthy`
  - Equatorial Diameter $D_{eq} \ge 45.0\text{ mm}$
* **Grade B Criteria** (Optional medium tier):
  - Condition == `Healthy`
  - $35.0\text{ mm} \le D_{eq} < 45.0\text{ mm}$
* **URS Criteria (Under-sized / Rejected / Sub-standard)**:
  - Condition $\in$ {`Damaged`, `Rotten`, `Sprouted`} OR $D_{eq} < 35.0\text{ mm}$
* **Lot-Level Acceptance Rule**:
  - $\text{URS} \le 10\%$: **ACCEPTED**
  - $10\% < \text{URS} \le 20\%$: **CONDITIONAL ACCEPTANCE**
  - $\text{URS} > 20\%$: **REJECTED**

### 6. Human-in-the-Loop Review System (`src/ui/app.py`)
Prevents automated errors from affecting procurement decisions:
* Isolates predictions where AI confidence $C < 0.70$.
* Renders a dedicated verification card displaying a cropped view of the uncertain bulb.
* Permits authorized inspectors to override the defect classification or confirm the AI prediction.
* Instantly triggers dynamic re-grading of the entire batch upon operator submission.

### 7. PDF Report Generation Engine (`src/reports/pdf_generator.py`)
Uses `reportlab.platypus` to generate official, unalterable digital certificates containing:
* Inspection ID, Date, Operator Name, Procurement Center ID.
* High-level summary scorecard (Total sample count, Grade A %, URS %, Lot Acceptance status).
* High-resolution embedded defect distribution chart.
* Comprehensive tabular listing of all detected bulbs with measurements and confidence scores.
* Model version hash and grading rule version for complete audit traceability.

### 8. Relational Persistence Layer (`src/database/`)
SQLite database managed via SQLAlchemy:
* `inspections` table: Records lot-level metrics, timestamp, operator, and final verdict.
* `onion_records` table: Records individual bulb dimensions, classifications, confidence scores, and manual review audit flags.
* Enables historical trend analysis and retrospective procurement audits.
