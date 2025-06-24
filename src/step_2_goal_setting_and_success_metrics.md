# Step 2 — Goal Setting & Success Metrics

The project now moves from “what is the problem?” to “how do we measure success?”

---

## 2.1  Primary Business Metric

Fraud detection is a class-imbalanced task where **false negatives** (missed fraud) hurt more than **false positives** (extra reviews).  
The stakeholders’ top requirement is therefore:

| Target | Rationale |
|--------|-----------|
| **Precision ≥ 95 % at Recall ≥ 85 %** | Each flagged transaction should be truly fraudulent ≥ 95 % of the time, while still catching at least 85 % of all fraud. |

> **Why not accuracy?** With fraud rates below 1 %, a naïve model that predicts “legitimate” for every transaction scores > 99 % accuracy yet saves no money. Precision/recall (or cost-weighted loss) aligns with business value.

---

## 2.2  Secondary Metrics

| Metric | Why we track it | Target / Note |
|--------|-----------------|---------------|
| **Recall** | Complements precision; controls fraud still slipping through. | ≥ 85 % (see above) |
| **F₁ or F<sub>β</sub>** | Single number balancing P/R; useful for model selection. | Monitor only |
| **AUROC / PR-AUC** | Compare candidate models across thresholds. | Higher is better |
| **Latency (p99)** | Live scoring must approve/decline in near-real-time. | < 300 ms end-to-end |
| **Model size / RAM** | Fits into low-cost serverless or edge endpoints. | < 100 MB |
| **Concept-drift monitor** | Detects degradation in data or label distribution. | Alert if P/R drops 2 pp week-over-week |

Fairness metrics (e.g., disparate impact) can be added later if demographic attributes enter the dataset.

---

## 2.3  Baseline Performance

Before modelling we log a “zero-effort” benchmark:

| Baseline | Precision | Recall | Notes |
|----------|-----------|--------|-------|
| Predict-all-legit | 0 % | 0 % | Saves no fraud, zero cost |
| Simple rule engine (e.g., amount > \$5 000 AND country not = billing) | TBD | TBD | Will compute once data is loaded |

These numbers give us a floor to beat and a sanity check on metric calculations.

---

## 2.4  Production Measurement Plan

Even though this repo won’t deploy a live service, a real project would:

* **Log predictions & outcomes** to a secure store.  
* **Compute P/R, latency, drift** daily and display on a Grafana / Superset dashboard.  
* **Trigger alerts** if primary metric falls below SLA for two consecutive days.

