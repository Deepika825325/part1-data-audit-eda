# D2C Customer Churn Intelligence

## Part 1: Data Audit, Exploratory Data Analysis & Business Understanding

---

# Project Overview

Customer churn is one of the most important challenges for Direct-to-Consumer (D2C) businesses. Before building predictive models or retention strategies, it is essential to understand customer behavior, validate data quality, and identify the factors most strongly associated with customer attrition.

This project performs a comprehensive data audit and exploratory analysis across customer, transaction, support, engagement, campaign, and churn datasets to uncover business insights and generate evidence-based churn hypotheses.

### Objective

Identify behavioral patterns associated with customer churn and provide actionable recommendations for future retention initiatives.

### Prediction Target

Predict whether a customer will churn within the next **60 days**.

### Snapshot Date

**2025-09-30**

---

# Project Highlights

* Audited six business datasets containing customer, transaction, support, engagement, campaign, and churn information.
* Created a unified customer-level analytical dataset.
* Performed data quality validation, leakage assessment, and join integrity checks.
* Evaluated eight churn-risk hypotheses using customer-level evidence.
* Identified customer inactivity and declining engagement as the strongest churn indicators.
* Produced business recommendations to support retention planning.

---

# Business Problem

The company experiences significant customer attrition but lacks visibility into:

* Why customers stop purchasing
* Which engagement signals indicate churn risk
* Which customer segments require intervention
* What actions should be prioritized before model development

This analysis addresses these questions using customer-level behavioral and operational data.

---

# Results Snapshot

| Metric                         |               Value |
| ------------------------------ | ------------------: |
| Customers Analyzed             |               2,400 |
| Orders Analyzed                |              10,009 |
| Support Tickets                |               1,921 |
| Overall Churn Rate             |              46.96% |
| Supported Hypotheses           |                   5 |
| Partially Supported Hypotheses |                   1 |
| Unsupported Hypotheses         |                   2 |
| Strongest Churn Driver         | Customer Inactivity |

---

# Executive Summary

### Customers Who Churn

* Average sessions decreased from **6.73** to **4.02**
* Average order count decreased from **5.18** to **3.04**
* Average days since last visit increased from **9.77** to **26.55**
* Campaign engagement was lower
* Return rates were slightly higher

### Customers Who Stay

* Visit more frequently
* Purchase more often
* Engage with marketing campaigns
* Maintain consistent platform activity

The strongest predictor of churn observed during the analysis was customer inactivity.

---

# How to Run

### 1. Clone Repository

```bash
git clone https://github.com/Deepika825325/part1-data-audit-eda.git
cd part1-data-audit-eda
```

### 2. Create Virtual Environment

```bash
python -m venv .venv
```

### 3. Activate Environment

Windows:

```bash
.venv\Scripts\activate
```

Linux / Mac:

```bash
source .venv/bin/activate
```

### 4. Install Dependencies

```bash
pip install -r requirements.txt
```

### 5. Launch Notebook

```bash
jupyter notebook
```

Open:

```text
notebooks/eda_audit.ipynb
```

and run all cells from top to bottom.

---

# Generated Outputs

## Charts

```text
outputs/charts/
```

Contains all visualizations generated during the analysis.

## Tables

```text
outputs/tables/
```

Key outputs include:

* dataset_overview.csv
* schema_summary.csv
* missing_values_summary.csv
* churn_distribution.csv
* churn_hypothesis_summary.csv
* key_business_findings.csv
* customer_master_dataset.csv

---

# Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Jupyter Notebook

---

# Conclusion

The analysis indicates that customer churn is primarily driven by declining engagement and increasing inactivity rather than customer support interactions.

Customers who stop visiting the platform, reduce purchasing frequency, and disengage from marketing campaigns represent the highest-risk churn segment.

The findings establish a strong foundation for future churn prediction models, retention monitoring systems, and customer intervention strategies.
