# Dataset Collection, Annotation & Augmentation Guidelines

## 📸 1. Image Collection Protocol

To ensure the AI model generalizes robustly to procurement mandis, testing labs, and live webcam scenarios, all images must adhere to standardized collection criteria.

### 1.1 Hardware & Environment Setup
* **Camera Angle**: Top-down planar view ($90^\circ \pm 5^\circ$ perpendicular to the sorting surface).
* **Distance**: $30\text{ cm} - 60\text{ cm}$ vertical distance between camera lens and table surface.
* **Illumination**: Uniform, diffused white lighting ($5000\text{K} - 6500\text{K}$). Avoid strong directional spotlights that cast hard shadows.
* **Background**: Clean, non-textured, matte surface (preferably light gray, white, or matte black sorting trays) to maximize contrast with onion skins.
* **Reference Marker**: Every image frame **must** include either:
  1. A standardized **ArUco Marker** (4x4 dictionary, $40\text{ mm} \times 40\text{ mm}$ square card).
  2. A standard **₹5 or ₹10 Coin** ($25.0\text{ mm}$ diameter) placed on the same focal plane as the onions.

### 1.2 Sampling Variability
* Include onions of varying sizes (small $< 35\text{ mm}$, medium $35 - 55\text{ mm}$, large $> 55\text{ mm}$).
* Include single-bulb images and multi-bulb cluster images (2 to 10 onions per frame with slight contact or overlap to train instance segmentation).
* Capture images under varying natural ambient light (morning, afternoon, indoor fluorescent).

---

## 🏷️ 2. Defect Class Ontology & Visual Criteria

The dataset is structured around **4 distinct condition classes**:

| Class ID | Class Name | Visual Criteria | Example Conditions |
| :---: | :--- | :--- | :--- |
| `0` | **Healthy** | Intact, dry papery outer tunic; uniform red/pink/white coloration; firm neck; no signs of microbial decay or mechanical tears. | Normal harvest onions meeting Grade A appearance standards. |
| `1` | **Damaged** | Broken or peeled outer layers exposing fleshy scale; puncture cuts; knife lacerations; crushed or flattened bulb sections. | Mechanical injury during harvesting, grading, or transport. |
| `2` | **Rotten** | Black powdery Aspergillus mold spores; bacterial soft rot (watery, slimy, brownish sunken areas); fungal decay at neck or basal plate. | Microbial post-harvest rot causing total loss of consumption value. |
| `3` | **Sprouted** | Visible green vegetative shoot/leaf emerging from the neck of the bulb; softening of the bulb core. | Premature dormancy break due to improper storage humidity or temperature. |

---

## ✏️ 3. Annotation Conventions (YOLOv8-Seg Polygon Format)

* **Annotation Tool**: Use **Roboflow** (recommended for collaboration) or **CVAT**.
* **Annotation Type**: **Instance Segmentation Polygons** (NOT plain rectangular bounding boxes).
  - Tight boundaries: The polygon vertices must trace the exact physical contour of the onion bulb.
  - Exclude shadows: Do not include cast shadows in the segmentation polygon.
  - Multi-bulb separation: When onions touch, draw distinct, non-overlapping polygons for each individual bulb.
  - Sprout handling: For `Sprouted` onions, annotate the entire bulb including the base of the emergent green shoot.

### YOLOv8-Seg Export Format
Each image has a corresponding `.txt` label file formatted as:
```
<class_id> <x1> <y1> <x2> <y2> <x3> <y3> ... <xn> <yn>
```
Where all coordinates $(x_i, y_i)$ are normalized to $[0.0, 1.0]$ relative to image width and height.

---

## 🔄 4. Data Augmentation Pipeline

To prevent overfitting on limited initial datasets, apply the following offline or online augmentations:

| Augmentation Type | Parameter Range | Rationale |
| :--- | :--- | :--- |
| **Rotation** | Between $-180^\circ$ and $+180^\circ$ | Onions are rotationally invariant on the inspection table. |
| **Horizontal Flip** | 50% probability | Doubles sample diversity without distorting geometry. |
| **Vertical Flip** | 50% probability | Ensures orientation-agnostic detection. |
| **Brightness** | $\pm 15\%$ | Simulates varied mandi lighting and cloudy/sunny conditions. |
| **Contrast** | $\pm 12\%$ | Simulates variable camera sensor dynamic ranges. |
| **Gaussian Blur** | Max kernel 3x3 | Simulates slight camera motion or webcam defocus. |

> [!CAUTION]
> **Do NOT apply shear, affine skew, or non-uniform aspect-ratio scaling!** Distorting the aspect ratio will corrupt the circularity of the bulbs and lead to erroneous size calibration.

---

## 📊 5. Dataset Partitioning Standard

The dataset must be partitioned strictly to prevent data leakage:
* **Train Split (70%)**: Used for model weight convergence and gradient descent.
* **Validation Split (20%)**: Used for hyperparameter tuning and early stopping.
* **Test Split (10%)**: Strictly held out for final benchmark evaluation and report generation.

### Example `data.yaml` Structure
```yaml
path: ../data/processed
train: images/train
val: images/val
test: images/test

names:
  0: Healthy
  1: Damaged
  2: Rotten
  3: Sprouted
```
