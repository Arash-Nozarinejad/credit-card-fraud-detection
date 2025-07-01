# Credit Card Fraud Detection

## Prelude

While AI tools—especially large language models—have helped us enormously, over-reliance on them can erode expertise and encourage laziness. To counter that, I'm building a series of projects from scratch using only my own knowledge, and I hope it inspires you to do the same.

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