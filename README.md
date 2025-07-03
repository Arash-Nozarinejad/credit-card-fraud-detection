# Credit Card Fraud Detection

## Prelude

While AI tools—especially large language models—have helped us enormously, over-reliance on them can erode expertise and encourage laziness. To counter that, I'm building a series of projects from scratch using only my own knowledge, and I hope it inspires you to do the same.

## Summary

I tackled credit card fraud detection as a complete end-to-end machine learning project, following the same systematic approach I use in real-world work. Starting with problem definition and business understanding, I identified this as a binary classification problem with severe class imbalance (fraud represents only 0.17% of transactions). I set specific business metrics - precision ≥ 95% at recall ≥ 85% - rather than relying on misleading accuracy scores.

After establishing technical assumptions and acquiring the Kaggle credit card fraud dataset, I performed thorough data quality assessment. The dataset turned out to be remarkably clean with no missing values or major issues, though I did investigate 1,081 duplicate rows and concluded they were legitimate business scenarios.

The exploratory analysis revealed key insights: the Amount feature emerged as a strong predictor with fraud rates varying dramatically across transaction ranges (especially $500-1000 showing 2.5x higher fraud rates), and the Time feature showed an unusual bimodal distribution. These findings guided my preprocessing decisions - mainly log transformation for the skewed Amount feature.

What surprised me most was the baseline model results. When I tested simple approaches to establish performance floors, the default Random Forest model not only worked well but actually exceeded my business targets right out of the box, achieving 94.1% precision and 81.6% recall. This dramatically simplified the project timeline.

I created a minimal preprocessing pipeline since the data was already high quality, split the data appropriately (70/15/15 train/validation/test), and conducted a final head-to-head comparison between Random Forest and XGBoost. Random Forest won with superior precision despite longer training times, which matters more for business operations since false positives carry operational costs.

The final model meets all business requirements and is ready for deployment. While this was meant to be an educational project showing complex modeling techniques, it turned out to demonstrate an equally important lesson: sometimes simple, well-executed approaches solve real problems effectively without over-engineering.

## Introduction

Among the many projects in data science and machine learning, credit card fraud detection is one of the most practical ways to sharpen your skills and tackle real-world challenges.

In this repository, I'll follow the same steps I use in my day-to-day work: reinforcing essential techniques, learning new ones, and showing readers how to approach a mission-critical task. I'll update this README as the project evolves, documenting each checklist item and milestone along the way.

## Project Outline

### Step 1 - Problem Definition & Business Understanding

**Link:** [`step_1_problem_definition_and_business_understanding.md`](./src/step_1_problem_definition_and_business_understanding.md)

Almost all machine learning and data science projects start with defining the problem and understanding the business impact.

In this file, I walk through my thought process and the steps necessary to complete this foundation.

This project deals with a binary classification problem using severely imbalanced datasets. I explore the business impact conceptually, but since this isn't a real project, concrete figures weren't needed. I identify common stakeholders you'd encounter in real-life projects, along with current state analysis and typical constraints. Finally, I outline the key deliverables that should be produced.

While some steps here are just acknowledged and explored, in actual projects each one would require significant time and detailed work.

---

### Step 2 - Goal Setting & Success Metrics

**Link:** [`step_2_goal_setting_and_success_metrics.md`](./src/step_2_goal_setting_and_success_metrics.md)

Once the problem is defined, the next critical step is establishing how to measure success.

In this file, I outline my approach to setting meaningful metrics for fraud detection projects. The primary focus is on precision and recall rather than accuracy, since accuracy is misleading with severely imbalanced datasets. I set specific targets: precision ≥ 95% at recall ≥ 85%, meaning we want to catch most fraud while ensuring flagged transactions are actually fraudulent.

I also identify important secondary metrics like F₁ scores, AUROC, latency requirements, and model size constraints that matter in production. Establishing baselines before any modeling begins is crucial for having benchmarks to beat.

While I outline a production measurement plan with dashboards and alerts, the actual implementation of monitoring systems would be a significant undertaking in real projects. Here, the focus is on understanding what metrics matter and why they align with business objectives.

---

### Step 3 - Technical Assumptions & Planning

**Link:** [`step_3_technical_assumptions_and_planning.md`](./src/step_3_technical_assumptions_and_planning.md)

Before diving into code, it's essential to document your technical assumptions and create a plan based on what you expect to find.

In this file, I lay out my predictions about the data, features, and modeling approach before any exploratory work begins. I make assumptions about the dataset size, quality, and the extreme class imbalance I expect to encounter. I also outline my initial algorithm choices, focusing on traditional machine learning approaches rather than complex solutions.

The planning covers key technical decisions like cross-validation strategy, class imbalance handling techniques, and feature engineering considerations. I also set realistic compute requirements to keep everything laptop-friendly.

The important mindset here is acknowledging these are just assumptions that will likely need updating as reality unfolds. Writing them down upfront helps track what changes and why, plus prevents getting overwhelmed by too many options when the actual work begins.

---

### Step 4 - Data Acquisition

**Link:** [`step_4_data_acquisition.ipynb`](./src/step_4_data_acquisition.ipynb)

With the foundation established, the next step is acquiring the actual data for analysis.

In this file, I outline the typical challenges of data acquisition in real-world projects, including handling various data sources like data lakes, databases, and file storage systems. Each presents unique challenges around authentication, file formats, and data governance.

For this project, I simplify the process by downloading the Kaggle Credit Card Fraud Detection dataset. I provide code to automate the download using Kaggle's API, along with setup instructions for the required authentication file. I also include a manual download option for those who prefer not to configure API access.

While real projects involve much more complex data acquisition workflows with multiple sources and extensive validation steps, this demonstrates the basic mechanics of programmatically fetching data and organizing it for analysis.

---

### Step 5 - Initial EDA

**Link:** [`step_5_initial_eda.ipynb`](./src/step_5_initial_eda.ipynb)

Once data is acquired, the next step is initial exploratory data analysis focused on identifying data quality issues that need cleaning.

In this file, I walk through my systematic approach to data quality assessment. I check for the most common problems: missing data, data type issues, obvious quality problems like impossible values and duplicates, structural inconsistencies, and target variable quality.

The analysis reveals this dataset is quite clean - no missing values, correct data types matching the documentation, and no impossible values. I do find 1,081 duplicate rows and zero-amount transactions, but after investigation conclude both are legitimate business scenarios rather than data quality issues.

I also check the target variable quality and find no labeling inconsistencies or problematic patterns. Since this is a curated Kaggle dataset, the positive results aren't surprising, but I always run these checks regardless of the data source to establish a baseline understanding before any cleaning or modeling work begins.

---

### Step 6 - Data Cleaning

**Link:** [`step_6_data_cleaning.md`](./src/step_6_data_cleaning.md)

After identifying data quality issues in the previous step, this phase focuses on actually fixing those problems.

In this file, I acknowledge that based on the initial EDA findings, no data cleaning steps are required for this particular dataset. The data is already in good condition and ready for the next phase.

I outline what would typically be addressed in data cleaning: missing data, incorrect data types, outliers, duplicates, structural problems, and target variable issues. In real projects, this step often involves significant work with graphs, formulas, and validation procedures.

Since no cleaning was needed here, I can move directly to the next step. This demonstrates that not every project requires extensive data cleaning, especially when working with well-curated datasets, though the assessment process should always be completed regardless.

---

### Step 7 - EDA

**Link:** [`step_7_eda.ipynb`](./src/step_7_eda.ipynb)

With clean data in hand, the next step is comprehensive exploratory data analysis to understand distributions, relationships, and patterns that will guide modeling decisions.

In this file, I systematically work through univariate and bivariate analysis approaches. For univariate analysis, I focus on the target variable's extreme imbalance (1:587 ratio), the Time feature's unusual bimodal distribution, and the Amount feature's heavy right skew with extensive outliers. I skip individual PCA component analysis since these transformed features lack direct business interpretation.

The bivariate analysis reveals key insights: Amount emerges as the strongest predictor with fraud rates varying dramatically across transaction ranges (especially the $500-1000 range showing 2.5x higher fraud rates). Time shows weak correlation but still provides value, and fraud patterns generally follow the same bimodal distribution as legitimate transactions.

These findings directly inform my next steps: log transformation for Amount due to skewness, class weighting for imbalance, and consideration of tree-based models like XGBoost. I also explain why other common EDA techniques like multivariate analysis, clustering, or time-series analysis aren't applicable to this particular dataset and problem.

---

### Step 8 - Baseline Model Establishment

**Link:** [`step_8_baseline_model_establishment.ipynb`](./src/step_8_baseline_model_establishment.ipynb)

Before building sophisticated models, it's crucial to establish baseline performance to understand if complex approaches are justified.

In this file, I test multiple baseline approaches to set performance floors: majority class prediction (completely useless for fraud detection), random predictions (poor precision), rule-based predictions using EDA insights (slightly better but still inadequate), and single-feature prediction using Amount alone (failed despite Amount being a strong predictor).

The real surprise comes from testing default machine learning models without any tuning. Random Forest achieves 94.1% precision and 81.6% recall, actually exceeding our business targets from Step 2. XGBoost performs well too at 86.7% precision and 79.6% recall, while Logistic Regression trails but still shows respectable performance.

This step fundamentally changed the project trajectory. Instead of needing complex feature engineering and extensive hyperparameter tuning, the baseline results show that well-implemented simple approaches can solve real business problems effectively. The PCA-transformed features contain strong fraud signals that tree-based models can leverage effectively.

---

### Step 9 - Preprocessing Pipeline

**Link:** [`step_9_preprocessing_pipeline.ipynb`](./src/step_9_preprocessing_pipeline.ipynb)

With baseline performance established, this step focuses on creating a robust preprocessing pipeline for the final model.

In this file, I create a minimal but effective preprocessing workflow. The main tasks include stratified data splitting (70/15/15 train/validation/test) to preserve class balance, log transformation for the Amount feature to handle its right-skewed distribution, and pipeline validation to ensure no data leakage.

Since the dataset was already high quality with PCA-transformed features, extensive preprocessing wasn't needed. I create and test the pipeline thoroughly, then save both the preprocessing pipeline and split datasets for consistent use in model training.

The systematic approach ensures reproducibility and prevents common pitfalls like data leakage. While real projects often require complex preprocessing workflows, this demonstrates that sometimes simple, well-executed preprocessing is sufficient when working with quality data.

---

### Step 10 - Model Comparison & Selection

**Link:** [`step_10_model_comparison.ipynb`](./src/step_10_model_comparison.ipynb)

Since Random Forest already exceeded business targets in baseline testing, this step conducts a practical head-to-head comparison to make the final model selection.

In this file, I compare Random Forest and XGBoost with proper class balancing techniques. Random Forest uses class_weight='balanced' while XGBoost uses scale_pos_weight. The evaluation reveals Random Forest achieving 98.1% precision and 70.3% recall on validation data, while XGBoost reaches 89.1% precision and 77.0% recall.

Despite XGBoost being significantly faster to train (3.57 vs 54.65 seconds), Random Forest wins due to superior precision, which is crucial for minimizing operational costs from false positives. Feature importance analysis shows V14 as the top predictor in both models, providing confidence in the results.

I train the final Random Forest model on combined training and validation data, save both the standalone model and complete pipeline, and verify everything works correctly. The final model is ready for deployment and meets all business requirements established in Step 2.