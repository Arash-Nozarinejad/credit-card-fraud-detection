## Step 1  Problem Definition & Business Understanding

The first step in any data-science project is to define the problem precisely and map its real-world impact. For fraud detection, that means:

1. Identify the problem type  
2. Quantify the business impact  
3. Map the stakeholders  
4. Document the current state  
5. List constraints and assumptions  

### 1.1  Problem Type

Credit-card fraud detection is a **binary classification** problem (fraud = 1, legitimate = 0) tackled with **supervised learning**. Two extra nuances are worth noting:

* **Severe class imbalance** – fraudulent transactions are typically < 1 % of the data. Standard accuracy is therefore misleading; we’ll rely on metrics such as precision, recall, F₁, AUROC, cost-weighted loss, or expected monetary value.  
* **Near-real-time inference** – although the historical model is trained offline, production scoring must be fast enough to approve or decline a transaction within milliseconds.

### 1.2  Business Impact

Fraud losses run into billions of dollars annually; for a mid-size card issuer the direct savings can reach millions per year. Indirect benefits include higher customer trust and lower charge-back fees. In a real engagement I would:

* Estimate current annual fraud loss (baseline)  
* Model expected savings at target recall / precision levels  
* Add operational costs (manual review, infrastructure) to compute ROI and payback period  

### 1.3  Stakeholder Analysis

Key parties are:

| Stakeholder                | Role / Concern                              |
|----------------------------|---------------------------------------------|
| Fraud operations team      | Review flagged transactions, tune threshold |
| Risk / compliance officers | Regulatory adherence, false-positive rates  |
| Engineering / DevOps       | Deploy and monitor the model                |
| Data science               | Build, retrain, and measure models          |
| Executives / finance       | Cost savings, ROI, reputation risk          |

Because you’re reading this on GitHub, I’ll keep the language technical and skip introductory explanations.

### 1.4  Current State Analysis

A thorough discovery covers:

* Existing rule-based or vendor solutions and their KPIs  
* Available datasets (transaction logs, device fingerprints, geolocation, historical fraud labels)  
* Data quality issues (latency, missing values, label delay)  
* Current model-monitoring and alerting practices  

### 1.5  Constraint Identification

Typical constraints include:

* **Data access** – PCI-DSS compliance, PII masking, and legal agreements  
* **Latency budget** – e.g., < 300 ms round-trip for online scoring  
* **Compute / budget** – cloud costs for training and real-time serving  
* **Timeline** – proof-of-concept vs. production hand-off dates  

In this public repository we’re constrained mainly by the availability of open fraud datasets and reasonable training time on a standard laptop; deployment will be demonstrated but not run in production.

### Deliverables

Produce two artefacts:

1. **Problem-statement document** – the full write-up of sections 1.1 – 1.5.  
2. **Tailored executive summary** – a one-pager with quantified impact, timeline, and resource requirements, written in business language for quick approval.

Both documents should be validated with stakeholders before modelling begins.

