# System Architecture & Technical Specifications

## 🏛️ High-Level System Architecture

The **OnionGrade AI** system is architected as a decoupled, modern client-server quality assessment platform. It adheres to strict separation of concerns:
1. **Separation of Presentation from Computation**: A reactive **React (Vite)** single-page application handles operator workflows and user interactions, communicating asynchronously with a **FastAPI** backend.
2. **Separation of Perception from Policy**: Deep learning models observe visual attributes and segment boundaries, while an isolated, versioned business rules engine enforces regulatory grading criteria.
3. **Hybrid Storage Strategy**: High-resolution imagery and binary assets are stored in a dedicated, cryptographically hashed media storage layer, while metadata, measurements, and audit trails reside in a relational database (SQLite/PostgreSQL via SQLAlchemy).

```
┌────────────────────────────────────────────────────────────────────────┐
│                   CLIENT PRESENTATION LAYER (REACT SPA)                │
│                                                                        │
│   ┌───────────────────────────┐      ┌───────────────────────────────┐ │
│   │   Mode 1: Batch Uploader  │      │    Mode 2: Live Webcam Input  │ │
│   │   (HTML5 Drag & Drop)     │      │    (WebRTC Canvas Snapshot)   │ │
│   └─────────────┬─────────────┘      └───────────────┬───────────────┘ │
│                 │                                    │                 │
│                 └─────────────────┬──────────────────┘                 │
│                                   ▼                                    │
│             HTTP REST / Multipart Upload (JSON + Binary Image)         │
└───────────────────────────────────┼────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                   FASTAPI ASYNCHRONOUS BACKEND SERVICE                 │
│                                                                        │
│   ┌────────────────────────────────────────────────────────────────┐   │
│   │ REST API Routing Layer (CORS, Request Validation, Static Media)│   │
│   │ Endpoints: /api/inspect, /api/inspections, /api/reports, etc.  │   │
│   └───────────────────────────────┬────────────────────────────────┘   │
│                                   │                                    │
│   ┌───────────────────────────────┴────────────────────────────────┐   │
│   │ COMPUTER VISION & PERCEPTION LAYER                             │   │
│   │                                                                │   │
│   │   ┌──────────────────────────┐                                 │   │
│   │   │ Pre-Processing & Resize  │ ──> Resolution / EXIF checks    │   │
│   │   └─────────────┬────────────┘                                 │   │
│   │                 │                                              │   │
│   │         ┌───────┴───────┐                                      │   │
│   │         ▼               ▼                                      │   │
│   │   ┌───────────────┐ ┌────────────────────────────────────────┐ │   │
│   │   │ Calibration   │ │ AI Instance Segmentation               │ │   │
│   │   │ Engine        │ │ (YOLOv8-Seg / YOLOv11-Seg)             │ │   │
│   │   │ (ArUco / Coin)│ │                                        │ │   │
│   │   │ Output:       │ │ - Individual Bulb Masks                │ │   │
│   │   │ pixels_per_mm │ │ - Classes: Healthy, Damaged,           │ │   │
│   │   │               │ │            Rotten, Sprouted            │ │   │
│   │   └───────┬───────┘ └───────────────────┬────────────────────┘ │   │
│   │           │                             │                      │   │
│   │           └─────────────┬───────────────┘                      │   │
│   │                         ▼                                      │   │
│   │   ┌──────────────────────────────────────────────────────────┐ │   │
│   │   │ Size Estimation Engine (Contour & Ellipse Geometry)      │ │   │
│   │   │ - Computes Equatorial Diameter in Millimeters (mm)       │ │   │
│   │   └─────────────────────┬────────────────────────────────────┘ │   │
│   └─────────────────────────┼──────────────────────────────────────┘   │
│                             │ Per-Bulb: {id, mask, defect, conf, size_mm}
│                             ▼                                          │
│   ┌────────────────────────────────────────────────────────────────┐   │
│   │ BUSINESS RULES & GRADING ENGINE (grading_rules.json)           │   │
│   │ - Size Thresholds (e.g. Min Diameter: 45 mm for Grade A)       │   │
│   │ - Defect Classification (Defect == Healthy -> Grade A; else URS│   │
│   │ - Lot Acceptance: Accept if URS <= 10%, Reject if URS > 20%    │   │
│   └─────────────────────────┬──────────────────────────────────────┘   │
└─────────────────────────────┼──────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌───────────────────────────┐   ┌────────────────────────────────────────┐
│ HUMAN-IN-THE-LOOP REVIEW  │   │ HYBRID STORAGE & REPORTING LAYER       │
│                           │   │                                        │
│ - Low Confidence (< 70%)  │   │ ┌────────────────────────────────────┐ │
│ - Inspector Override API  │   │ │ Relational DB (SQLAlchemy)         │ │
│ - Dynamic Re-grading      │   │ │ SQLite (Local) / Postgres (Cloud)  │ │
│                           │   │ └────────────────────────────────────┘ │
│                           │   │ ┌────────────────────────────────────┐ │
│                           │   │ │ Dedicated Image & Media Store      │ │
│                           │   │ │ (Disk / S3 + SHA-256 Checksums)    │ │
│                           │   │ └────────────────────────────────────┘ │
│                           │   │ ┌────────────────────────────────────┐ │
│                           │   │ │ PDF Report Generator (ReportLab)   │ │
│                           │   │ │ (Certified Audit PDF Certificates) │ │
│                           │   │ └────────────────────────────────────┘ │
└───────────────────────────┘   └────────────────────────────────────────┘
```

---

## 🧩 Architectural Components

### 1. Client Presentation Layer (React + Vite)
Located in `frontend/`:
* **Dual-Input Mode**:
  - **Batch File Uploader**: Supports single and multiple image selection (`.jpg`, `.jpeg`, `.png`) with drag-and-drop, thumbnail previews, and batch metadata configuration (lot number, mandi name, inspector name).
  - **Live Laptop/External Webcam Mode**: Utilizes browser WebRTC (`navigator.mediaDevices.getUserMedia`) to render a smooth video stream. The operator positions the onion lot with the reference marker and captures high-resolution canvas snapshots directly transmitted to the backend.
* **Interactive Defect Overlay**: Renders SVG/Canvas segmentation masks, bounding boxes, and color-coded status badges directly on top of the inspection image.
* **Human-in-the-Loop Review Interface**: Displays cards for predictions flagged with confidence $< 70\%$, providing cropped bulb previews, defect override dropdowns, and dynamic re-grading.
* **Analytics & Historical View**: Paginated data grid with search, date filters, lot verdict tags, and one-click PDF certificate downloads.

### 2. Asynchronous API Layer (FastAPI)
Located in `src/api/` and `src/main.py`:
* **High Performance**: Built on ASGI (Starlette + Uvicorn) for non-blocking I/O operations during heavy inference and file operations.
* **REST Endpoints**:
  - `POST /api/inspect/upload`: Accepts multipart image upload and lot metadata; executes vision and grading pipeline.
  - `POST /api/inspect/webcam`: Accepts base64 or multipart webcam snapshots.
  - `GET /api/inspections`: Returns paginated inspection history and summary statistics.
  - `GET /api/inspections/{id}`: Retrieves full lot details, individual bulb metrics, and image URLs.
  - `POST /api/inspections/{id}/override`: Submits inspector correction, logs audit trail, and recalculates lot grading.
  - `GET /api/inspections/{id}/report`: Dynamically compiles and streams certified ReportLab PDF certificate.
  - `GET /api/rules`: Returns active grading thresholds (Grade A, Grade B, URS).
  - `PUT /api/rules`: Updates grading parameters without service restart.
* **Static Media Mount**: Mounts `/media/` to serve raw uploaded photos, rendered overlay images, bulb crops, and generated PDF files.

---

## 💾 Suitable Database & Image Storage Architecture

### Why a Hybrid Storage Strategy?
In computer vision inspection systems, storing multi-megabyte image binaries directly as BLOBs inside relational database tables is an anti-pattern:
* **Database Bloat**: Table sizes expand drastically, exhausting memory and degrading index performance.
* **Sluggish Backups & Migrations**: Creating database backups or syncing tables across nodes becomes slow.
* **Poor Caching**: Relational buffer pools are wasted on large image payloads instead of query metadata.

### The Hybrid Storage Solution:
1. **Relational Database (SQLAlchemy ORM - SQLite / PostgreSQL)**:
   - Stores structured metadata, dimensions, defect counts, grading verdicts, and audit logs.
   - **Deployment Portability**:
     - **Local Edge / Mandi Deployment**: Embedded **SQLite** in WAL (Write-Ahead Logging) mode requiring zero setup or administration.
     - **Centralized Cloud Warehouse**: Switchable to **PostgreSQL** by altering `DATABASE_URL` in environment variables with zero code changes.
   - **Schema Tables**:
     - `inspections`: Lot ID, mandi name, inspector, total count, Grade A/B/URS counts, defect breakdown, lot verdict, average diameter, timestamps.
     - `bulb_records`: Per-bulb index, condition class, confidence score, equatorial diameter ($mm$), grade, bounding box JSON, polygon mask JSON, review flags.
     - `image_assets`: Foreign key link to inspection, asset type (`original`, `annotated`, `crop`, `report_pdf`), file path, public URL, file size, and **SHA-256 hash**.
     - `audit_logs`: Timestamped inspector overrides, original class vs corrected class, reason text.
     - `grading_configs`: Versioned grading thresholds for historical compliance traceability.

2. **Dedicated Image & Media Storage Service (`src/storage/image_store.py`)**:
   - Manages physical file assets organized hierarchically:
     ```
     data/storage/
     ├── images/
     │   ├── uploads/          # Raw images submitted by operators
     │   ├── annotated/        # Processed images with bounding boxes & segmentation masks
     │   └── crops/            # Individual flagged bulb crops for human review
     └── reports/              # Generated audit PDF inspection certificates
     ```
   - **Cryptographic Tamper-Proofing**: Calculates a SHA-256 checksum upon storing each image, safeguarding against post-inspection manipulation in accordance with government procurement audits.
   - **Cloud-Ready Interface**: Implemented behind an abstract storage adapter, allowing effortless switching from local filesystem to AWS S3, Cloudflare R2, or MinIO.

---

### 3. Reference Marker Calibration Engine (`src/calibration/calibrator.py`)
Computes the metric scale factor ($S = \text{pixels per mm}$) to translate image coordinates into real-world dimensions:
* **Primary Target (ArUco Marker)**: Scans the frame for a predefined 4x4 ArUco dictionary marker with known physical side length (e.g., $40.0\text{ mm}$).
* **Secondary Target (Standard ₹5 / ₹10 Coin)**: Fallback circular contour detection for a $25.0\text{ mm}$ coin.
* **Scale Factor Formula**:
  $$S = \frac{\text{Measured Marker Width in Pixels}}{\text{Known Marker Width in mm}}$$

### 4. AI Vision Layer (`src/ai/yolo_detector.py`)
Performs simultaneous instance localization, polygon segmentation, and multi-class condition classification:
* **Architecture**: Fine-tuned YOLOv8-Seg / YOLOv11-Seg instance segmentation network.
* **Classes**:
  1. `Healthy`: Uniform coloration, dry outer skin, free from rot or mechanical injury.
  2. `Damaged`: Mechanical gouges, skin abrasions, cracked or crushed bulbs.
  3. `Rotten`: Fungal decay, bacterial soft rot, black mold spores.
  4. `Sprouted`: Visible vegetative green shoot emergence at the neck.
* **Outputs**: Array of detections containing bounding boxes $[x_1, y_1, x_2, y_2]$, polygon mask arrays, class indices, and confidence scores $C \in [0.0, 1.0]$.

### 5. Size Estimation Engine (`src/calibration/size_estimator.py`)
Extracts physical dimensions from segmented onion mask polygons:
* Computes the convex hull and contour of the binary mask.
* Fits a minimum bounding ellipse (`cv2.fitEllipse`) to identify major and minor axes.
* Derives the **Equatorial Diameter** ($D_{eq}$):
  $$D_{eq} = \frac{\text{Major\_Axis\_Pixels} + \text{Minor\_Axis\_Pixels}}{2 \times S}$$
* Outputs diameter in millimeters ($mm$) for each detected bulb.

### 6. Configurable Grading Engine (`src/grading/grading_engine.py`)
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

### 7. Human-in-the-Loop Review System
Prevents automated errors from affecting procurement decisions:
* Isolates predictions where AI confidence $C < 0.70$.
* Dispatches a review card to the React client displaying a cropped view of the uncertain bulb.
* Permits authorized inspectors to override the defect classification or confirm the AI prediction.
* Instantly triggers dynamic re-grading of the entire batch upon operator submission via `POST /api/inspections/{id}/override`.

### 8. PDF Report Generation Engine (`src/reports/pdf_generator.py`)
Uses `reportlab.platypus` to generate official, unalterable digital certificates containing:
* Inspection ID, Date, Operator Name, Procurement Center ID.
* High-level summary scorecard (Total sample count, Grade A %, URS %, Lot Acceptance status).
* High-resolution embedded defect distribution chart.
* Comprehensive tabular listing of all detected bulbs with measurements and confidence scores.
* Model version hash, image SHA-256 checksums, and grading rule version for complete audit traceability.
