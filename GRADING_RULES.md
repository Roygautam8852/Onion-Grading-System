# Regulatory Grading Rules & Business Logic Engine

## 🏛️ 1. Procurement Standards & Context

In India, onion procurement quality standards are governed by guidelines from:
1. **Ministry of Consumer Affairs, Food & Public Distribution**.
2. **ICAR-Directorate of Onion and Garlic Research (DOGR)**.
3. **NAFED / NCCF Buffer Stock Procurement Specifications**.

These specifications classify onions based on two primary dimensions:
* **Physical Size (Equatorial Diameter in mm)**.
* **External Visible Quality (Soundness & Defect Freedom)**.

---

## ⚖️ 2. Onion Quality Grading Tiers

### Grade A (Premium Procurement Grade)
* **Visual Condition**: Must be completely **Healthy**. Free from cuts, rot, microbial black mold, and sprouting.
* **Equatorial Diameter**: $D_{eq} \ge 45.0\text{ mm}$ (Good commercial size for storage and retail).
* **Skin Integrity**: Dry, tight, well-cured outer tunic.

### Grade B (Fair Average Quality / Medium Grade - Optional Tier)
* **Visual Condition**: **Healthy**.
* **Equatorial Diameter**: $35.0\text{ mm} \le D_{eq} < 45.0\text{ mm}$.
* **Use Case**: Acceptable for immediate consumption, but lower price tier compared to Grade A.

### URS (Under-Sized / Rejected / Sub-Standard)
Any bulb exhibiting at least one of the following disqualifying characteristics:
1. **Under-Sized**: $D_{eq} < 35.0\text{ mm}$ (Regardless of health condition).
2. **Mechanically Damaged**: Punctures, deep knife cuts, or split bulbs.
3. **Rotten / Diseased**: Fungal decay, watery soft rot, or Aspergillus niger black mold.
4. **Sprouted**: Active vegetative green shoot emerging from the bulb neck (breaks storage dormancy and renders the bulb unviable for buffer stockpiling).

---

## 📦 3. Lot-Level Acceptance & Rejection Protocol

When an inspection batch of $N$ sample onions is evaluated, the batch quality statistics determine procurement acceptance:

$$\text{Grade A Percentage} = \left(\frac{N_{\text{Grade A}}}{N}\right) \times 100\%$$
$$\text{URS Percentage} = \left(\frac{N_{\text{URS}}}{N}\right) \times 100\%$$

### Procurement Verdict Criteria:
* **ACCEPTED (PASS)**:
  $$\text{URS Percentage} \le 10.0\%$$
  The batch qualifies for standard government procurement price.
* **CONDITIONAL ACCEPTANCE (REVIEW / DISCOUNT)**:
  $$10.0\% < \text{URS Percentage} \le 20.0\%$$
  Batch subject to price deduction or secondary manual sorting.
* **REJECTED (FAIL)**:
  $$\text{URS Percentage} > 20.0\%$$
  Batch rejected from procurement center due to high defect/undersize proportion.

---

## ⚙️ 4. Configurable JSON Schema (`src/grading/grading_rules.json`)

To prevent hardcoding business logic inside Python code, the rules are maintained in a version-controlled JSON configuration:

```json
{
  "rule_version": "v1.2.0-2026",
  "standard_authority": "ICAR-DOGR / Ministry of Consumer Affairs",
  "size_thresholds_mm": {
    "grade_a_min_diameter": 45.0,
    "grade_b_min_diameter": 35.0,
    "undersized_max_diameter": 35.0
  },
  "tolerances": {
    "max_allowed_urs_pct_for_acceptance": 10.0,
    "max_allowed_urs_pct_for_conditional": 20.0
  },
  "defect_severity_mapping": {
    "Healthy": "PASS",
    "Damaged": "REJECT",
    "Rotten": "REJECT",
    "Sprouted": "REJECT"
  }
}
```

This architecture ensures that if government procurement policy changes size thresholds (e.g., from $45\text{ mm}$ to $50\text{ mm}$ in drought seasons), only the JSON configuration is updated without retraining the AI model.
