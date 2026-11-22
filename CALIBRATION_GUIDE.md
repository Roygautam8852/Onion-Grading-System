# Camera Calibration & Size Estimation Technical Guide

## 📏 1. Principle of Image Calibration

In standard 2D digital imagery, distances are measured in discrete integer **pixels**, which do not correlate directly to physical units (such as millimeters) without an established conversion factor. The relationship between physical distance and pixel count depends on:
1. Camera sensor pixel pitch.
2. Lens focal length.
3. Vertical distance from the camera lens to the object plane.

To achieve accurate size estimation without requiring expensive laboratory camera calibration, **OnionGrade AI** employs **Reference Object Calibration**.

---

## 🎯 2. Reference Marker Specifications

The system supports two standardized reference objects placed on the inspection surface:

### Method A: Standardized ArUco Marker (Primary & Recommended)
* **Dictionary**: `cv2.aruco.DICT_4X4_50`
* **Marker ID**: `0`
* **Physical Dimension**: A precision square card of exactly $40.0\text{ mm} \times 40.0\text{ mm}$.
* **Detection Principle**:
  - `cv2.aruco.detectMarkers()` detects the 4 corner points $[P_0, P_1, P_2, P_3]$ with sub-pixel precision.
  - Side lengths in pixels:
    $$L_1 = \|P_1 - P_0\|_2, \quad L_2 = \|P_2 - P_1\|_2, \quad L_3 = \|P_3 - P_2\|_2, \quad L_4 = \|P_0 - P_3\|_2$$
  - Average side length in pixels:
    $$\bar{L}_{px} = \frac{L_1 + L_2 + L_3 + L_4}{4}$$
  - Metric Scale Factor:
    $$S = \frac{\bar{L}_{px}}{40.0} \quad \left(\frac{\text{pixels}}{\text{mm}}\right)$$

### Method B: Standard Circular Coin (Convenient Fallback for Demos)
* **Target**: Standard Indian ₹5 or ₹10 coin.
* **Physical Diameter**: Known standard diameter of $25.0\text{ mm}$.
* **Detection Principle**:
  - Detected using `cv2.HoughCircles` or circularity contour filtering:
    $$\text{Circularity} = \frac{4\pi \times \text{Area}}{\text{Perimeter}^2} \ge 0.88$$
  - Diameter in pixels: $D_{px} = 2 \times r_{px}$
  - Metric Scale Factor:
    $$S = \frac{D_{px}}{25.0} \quad \left(\frac{\text{pixels}}{\text{mm}}\right)$$

---

## 🧅 3. Onion Equatorial Diameter ($D_{eq}$) Calculation

Onion bulbs exhibit an oblate spheroid or prolate ellipsoid morphology. For agricultural grading, the official dimension is the **Equatorial Diameter** (the widest cross-sectional dimension perpendicular to the stem-to-root polar axis).

### Geometric Algorithm:
1. **Contour Extraction**: Convert the AI-predicted binary segmentation mask into external contour boundary points:
   ```python
   contours, _ = cv2.findContours(mask_binary, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
   ```
2. **Ellipse Fitting**: Fit a minimum enclosing ellipse to the largest contour using the direct least-squares method:
   ```python
   ellipse = cv2.fitEllipse(contour)
   (cx, cy), (axis_minor_px, axis_major_px), angle = ellipse
   ```
3. **Equatorial Diameter Formulation**:
   To minimize orientation bias when onions lie flat on their side:
   $$D_{px} = \frac{\text{axis\_major\_px} + \text{axis\_minor\_px}}{2}$$
   $$D_{eq} = \frac{D_{px}}{S} \quad (\text{mm})$$

---

## 💻 4. Live Laptop Webcam Demo Guidelines (For Teachers & Examiners)

When demonstrating size estimation to your teacher/examiner using a laptop webcam:
1. **Keep the Lens Stable**: Place the laptop flat on a table and angle the screen so the webcam points down at the desk surface or sorting sheet.
2. **Keep the Onion and Marker on the Same Plane**: The reference coin/marker must be placed flat on the table directly beside the onion bulb.
3. **Avoid Tilting**: Holding the onion in mid-air closer to the lens than the table will artificially magnify the onion relative to the marker. Always rest both on the table surface.
4. **Trigger Snapshot**: Press "Capture Snapshot" in Streamlit; the calibration engine extracts the scale and calculates the onion diameter in real time.

---

## 📊 5. Size Estimation Validation Metrics

To validate the calibration module, Teammate 3 measures 20 sample onions with a digital Vernier Caliper ($D_{actual}$) and compares them against the vision module ($D_{pred}$):
* **Mean Absolute Error (MAE)**:
  $$\text{MAE} = \frac{1}{N}\sum_{i=1}^N |D_{pred, i} - D_{actual, i}| \le 2.5\text{ mm}$$
* **Root Mean Squared Error (RMSE)**:
  $$\text{RMSE} = \sqrt{\frac{1}{N}\sum_{i=1}^N (D_{pred, i} - D_{actual, i})^2}$$
* **Mean Absolute Percentage Error (MAPE)**:
  $$\text{MAPE} = \frac{1}{N}\sum_{i=1}^N \left|\frac{D_{actual, i} - D_{pred, i}}{D_{actual, i}}\right| \times 100\% \le 5.0\%$$
