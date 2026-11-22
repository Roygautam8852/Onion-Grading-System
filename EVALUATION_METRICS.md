# Experimental Evaluation & Benchmark Metrics

## 📊 1. Overview of Evaluation Framework

As specified in Section 12 of the Capstone Project Synopsis, **OnionGrade AI** must be evaluated at two distinct levels:
1. **Machine Learning / Vision Level**: Assessing detection, segmentation, and defect classification accuracy.
2. **System & Application Level**: Assessing size measurement errors, inference speed, and application responsiveness.

---

## 🎯 2. Computer Vision & Deep Learning Metrics

### 2.1 Intersection over Union (IoU)
Measures the spatial overlap between predicted segmentation mask $M_{pred}$ and ground truth mask $M_{gt}$:
$$\text{IoU} = \frac{|M_{pred} \cap M_{gt}|}{|M_{pred} \cup M_{gt}|}$$

### 2.2 Precision, Recall & F1-Score
* **Precision**: Proportion of predicted defects that were truly defects:
  $$\text{Precision} = \frac{\text{TP}}{\text{TP} + \text{FP}}$$
* **Recall**: Proportion of true defects that the system detected:
  $$\text{Recall} = \frac{\text{TP}}{\text{TP} + \text{FN}}$$
* **F1-Score**: Harmonic mean of Precision and Recall:
  $$\text{F1} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$

### 2.3 Mean Average Precision (mAP)
Evaluated across all 4 classes (`Healthy`, `Damaged`, `Rotten`, `Sprouted`):
* **mAP@50**: Mean Average Precision computed at an IoU threshold of 0.50.
* **mAP@50:95**: Primary benchmark metric computed across IoU thresholds from 0.50 to 0.95 (in increments of 0.05).
* **Target Benchmark**: $\text{mAP@50} \ge 85.0\%$ on the held-out test split.

### 2.4 Multi-Class Confusion Matrix
A $4 \times 4$ matrix tracking classification distributions across:
1. Actual Healthy vs Predicted Classes
2. Actual Damaged vs Predicted Classes
3. Actual Rotten vs Predicted Classes
4. Actual Sprouted vs Predicted Classes

---

## 📏 3. Physical Size Estimation Error Metrics

Evaluated on 20 physically measured test onions against a digital Vernier Caliper ($D_{actual}$):

### 3.1 Mean Absolute Error (MAE)
$$\text{MAE} = \frac{1}{N} \sum_{i=1}^N |D_{pred, i} - D_{actual, i}|$$
* **Target Benchmark**: $\text{MAE} \le 2.50\text{ mm}$.

### 3.2 Root Mean Squared Error (RMSE)
Penalizes larger outliers more heavily:
$$\text{RMSE} = \sqrt{\frac{1}{N} \sum_{i=1}^N (D_{pred, i} - D_{actual, i})^2}$$
* **Target Benchmark**: $\text{RMSE} \le 3.50\text{ mm}$.

### 3.3 Mean Absolute Percentage Error (MAPE)
$$\text{MAPE} = \frac{1}{N} \sum_{i=1}^N \left| \frac{D_{actual, i} - D_{pred, i}}{D_{actual, i}} \right| \times 100\%$$
* **Target Benchmark**: $\text{MAPE} \le 5.0\%$.

---

## ⚡ 4. System & Inference Performance Metrics

| Metric | Target Benchmark | Measurement Method |
| :--- | :--- | :--- |
| **Model Inference Time (GPU)** | $\le 45\text{ ms / frame}$ | `torch.cuda.Event` elapsed time |
| **Model Inference Time (CPU)** | $\le 120\text{ ms / frame}$ | Python `time.perf_counter()` on 8-core CPU |
| **Size Geometry Computation** | $\le 15\text{ ms / bulb}$ | OpenCV contour & ellipse fitting latency |
| **PDF Report Generation** | $\le 1.5\text{ seconds}$ | ReportLab file export timer |
| **End-to-End Pipeline Latency** | $\le 1.0\text{ second}$ | Total elapsed time from snapshot capture to UI render |
| **Model Footprint** | $\le 25\text{ MB}$ (FP16/INT8) | `.pt` file size on disk for lightweight edge deployment |
