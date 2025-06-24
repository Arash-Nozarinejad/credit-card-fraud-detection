# Step 3 — Technical Assumptions & Planning

Time to write down what I *think* will hold true before touching any code. If reality contradicts me later, I’ll update the plan.

---

## 3.1  Data Assumptions

| Question | My best guess (pre-EDA) |
|----------|-------------------------|
| **Source** | The well-known Kaggle “Credit Card Fraud Detection” CSV. |
| **Volume** | ~285 k rows, ~30 columns — not “big data” but big enough to train decent models on a laptop. |
| **Refresh cadence** | Static; no live feed. |
| **Quality** | Labels are reliable (charge-backs confirmed). Missing values should be minimal because the dataset is already cleaned. |
| **Class balance** | Fraud ≈ 0.17 % of rows — *extremely* skewed. |

---

## 3.2  Feature Assumptions

* The 28 anonymized **`V` features** are numeric PCA components, plus `Amount`, `Time`, and the `Class` label.  
* No text, no categorical columns.  
* Expect *some* feature engineering: log-scale `Amount`, maybe a rolling window on `Time`.

---

## 3.3  Algorithm Assumptions

* Start with traditional ML (logistic regression, XGBoost, LightGBM).  
* Interpretability isn’t top-priority, but tree-based **SHAP values** (think: “per-feature blame scores”) are nice to have.  
* Might add an ensemble later if single models plateau.

---

## 3.4  Cross-Validation Plan

* **Stratified k-fold** (prob k = 7) so each fold keeps the same fraud ratio.  
* No time-series split for now — the timestamps aren’t truly chronological spending sessions.  
* Nested CV only if I hyper-tune a fleet of models later.

---

## 3.5  Class-Imbalance Handling

| Tool | What it is | When I’ll try it |
|------|------------|------------------|
| **SMOTE** | “Synthetic Minority Over-sampling Technique”: creates fake fraud rows by interpolating between real ones instead of naïve duplication. | If baseline recall is ugly. |
| **Class weighting** | Penalize mistakes on fraud heavier than legit in the loss function. | Usually the first knob I turn. |
| Cost-sensitive loss, threshold tuning | Push decision boundary where cost (fraud) >> cost (false alarm). | Maybe later. |

---

## 3.6  Feature-Engineering To-Dos

| Technique | Status |
|-----------|--------|
| Scaling / normalization | Maybe (tree models don’t need it; LR does). |
| Categorical encoding | Not applicable. |
| Text or image processing | N/A. |
| Dimensionality reduction | Already baked in via PCA. |
| Feature selection | Revisit after initial model + SHAP. |

---

## 3.7  Compute & Ops

* Laptop-friendly: < 2 GB RAM for training, sub-second inference.  
* No GPUs or distributed tricks needed.

---

## 3.8  Crucial Reminders

1. **Guard against data leakage** — no peeking at future labels.  
2. Keep the first pass *simple*; fancy tweaks come later.  
3. I’m skipping model versioning and monitoring in this repo, but I’ll note where they belong.