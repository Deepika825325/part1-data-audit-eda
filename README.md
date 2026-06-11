# D2C Customer Churn Intelligence

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Pandas](https://img.shields.io/badge/Pandas-2.2.3-blue)
![Project](https://img.shields.io/badge/Status-Completed-success)

## Part 1: Data Audit, Exploratory Data Analysis & Business Understanding

---

# Project Overview

Customer churn is a critical challenge for Direct-to-Consumer (D2C) businesses. Before designing retention campaigns or developing predictive models, it is essential to understand customer behaviour, validate data quality, and identify factors associated with customer attrition.

This project performs a comprehensive data audit and exploratory analysis across customer, transaction, support, engagement, campaign, and churn datasets to uncover business insights, identify churn-risk patterns, and develop evidence-based retention recommendations.

---

# Business Objective

The company wants to understand:

* Why customers churn
* Which behavioural signals indicate churn risk
* Which customer segments require retention attention
* What business actions should be prioritized before model development

The findings from this analysis support future retention strategies, customer segmentation, and churn prediction modelling.

---

# Prediction Target

**Target Variable:** `churn_next_60d`

**Business Question:**

> Will a customer churn within the next 60 days?

**Snapshot Date:** `2025-09-30`

All analytical decisions and feature engineering activities are evaluated relative to this snapshot date.

---

# Datasets Used

| Dataset              | Description                                   |
| -------------------- | --------------------------------------------- |
| customers            | Customer demographics and profile information |
| orders               | Transaction and purchasing history            |
| support_tickets      | Customer support interactions                 |
| web_events_snapshot  | Customer engagement activity                  |
| intervention_history | Marketing campaign history                    |
| churn_labels         | Churn outcome labels                          |

---

# Repository Structure

```text
part1-data-audit-eda/
│
├── data/
│
├── notebooks/
│   └── eda_audit.ipynb
│
├── reports/
│   ├── data_quality_report.md
│   └── business_memo.md
│
├── outputs/
│   ├── charts/
│   └── tables/
│
├── requirements.txt
└── README.md
```

---

# Methodology

The analysis followed the workflow below:

1. Data Loading & Initial Inspection
2. Schema Understanding
3. Data Quality Assessment
4. Join Integrity Validation
5. Date Consistency & Leakage Checks
6. Customer Master Dataset Construction
7. Exploratory Data Analysis
8. Advanced Business Analysis
9. Churn Hypothesis Evaluation
10. Priority Retention Cohort Identification
11. Executive Findings & Recommendations
12. Final Business Conclusions

---

# Technical Design Choices

Several implementation decisions were made to maximize interpretability, reproducibility, and business relevance.

| Decision                            | Rationale                                       |
| ----------------------------------- | ----------------------------------------------- |
| Pandas-based aggregation            | Efficient customer-level feature engineering    |
| Seaborn & Matplotlib visualizations | Reproducible business-friendly charts           |
| Customer-level analytical dataset   | Unified customer view across business domains   |
| LEFT JOIN integration strategy      | Prevents customer loss during dataset merging   |
| Pre-snapshot filtering              | Prevents target leakage                         |
| Business-driven hypothesis testing  | Connects analysis directly to retention actions |

The project prioritizes business understanding and actionable insights over modelling complexity because the objective of Part 1 is exploratory analysis rather than predictive modelling.

---

# Data Quality Summary

The following quality dimensions were assessed:

* Missing values
* Duplicate records
* Invalid values
* Outlier detection
* Join integrity
* Date consistency
* Leakage risk assessment

### Key Findings

| Finding              | Result                   |
| -------------------- | ------------------------ |
| Exact Duplicates     | None Detected            |
| Orphan Records       | None Detected            |
| Invalid Values       | None Detected            |
| Missing Loyalty Tier | 57.75%                   |
| Missing Skin Type    | 16.71%                   |
| Post-Snapshot Orders | 1,872 Records            |
| Leakage Risk         | Present in Future Orders |

All identified quality issues, business impacts, and treatment recommendations are documented in `reports/data_quality_report.md`.

---

# Results Snapshot

| Metric                     |               Value |
| -------------------------- | ------------------: |
| Customers Analysed         |               2,400 |
| Orders Analysed            |              10,009 |
| Support Tickets            |               1,921 |
| Overall Churn Rate         |              46.96% |
| Revenue Analysed           |       ₹6.12 Million |
| Revenue Exposure           |              40.93% |
| Churn Hypotheses Evaluated |                   6 |
| Strongest Churn Driver     | Customer Inactivity |

### Interpretation

The observed churn rate of **46.96%** indicates that nearly one in every two customers is expected to churn within the next 60 days. This highlights a significant retention challenge and suggests that customer attrition represents a major business risk.

Additionally, approximately **₹2.50 million (40.93%)** of analysed revenue is associated with customers who eventually churn, representing a substantial opportunity for revenue preservation through targeted retention initiatives.

---

# Key Findings

## Customer Inactivity Is the Strongest Churn Signal

| Activity Segment    | Churn Rate |
| ------------------- | ---------: |
| Recently Active     |     18.71% |
| Moderately Inactive |     50.00% |
| Highly Inactive     |     88.36% |

Customers exhibiting prolonged inactivity are substantially more likely to churn.

---

## Loyalty Participation Influences Retention

| Loyalty Tier | Churn Rate |
| ------------ | ---------: |
| Platinum     |     37.14% |
| Gold         |     40.75% |
| Not Enrolled |     48.34% |
| Silver       |     48.81% |

Gold and Platinum members exhibit stronger retention outcomes. Silver-tier performance suggests that entry-level loyalty benefits may not provide sufficient value to influence retention behaviour.

---

## Campaign Engagement Supports Retention

| Segment     | Churn Rate |
| ----------- | ---------: |
| Engaged     |     42.32% |
| Not Engaged |     50.14% |

Customers who interact with campaigns are less likely to churn than customers who do not engage.

---

## Revenue Exposure

Approximately **₹2.50 Million (40.93%)** of analysed revenue is associated with customers who eventually churn.

This highlights the financial importance of proactive customer retention initiatives.

---

# Priority Retention Cohorts

| Priority | Retention Cohort                  | Customer Count |
| -------- | --------------------------------- | -------------: |
| 1        | High Inactivity Customers         |            481 |
| 2        | Low Campaign Engagement Customers |          1,424 |
| 3        | Low Purchase Frequency Customers  |            696 |
| 4        | High Return Rate Customers        |            269 |

These customer groups represent the most actionable retention opportunities identified during the analysis and are prioritized according to churn risk and business impact.

---

# Deliverables

| File                           | Purpose                                         |
| ------------------------------ | ----------------------------------------------- |
| notebooks/eda_audit.ipynb      | Complete exploratory analysis notebook          |
| reports/data_quality_report.md | Data quality findings and recommendations       |
| reports/business_memo.md       | Business insights and retention recommendations |
| outputs/charts/                | Generated visualizations                        |
| outputs/tables/                | Supporting analytical outputs                   |

---

# Reproducibility

### Environment

* Python 3.11
* Jupyter Notebook

### Core Libraries

* Pandas
* NumPy
* Matplotlib
* Seaborn

### Dependency Management

Dependencies are listed in `requirements.txt`.

All package versions are pinned using exact (`==`) version specifications to ensure reproducible execution across environments.

### Data Assumptions

All analyses are performed relative to the customer snapshot date:

```text
2025-09-30
```

Post-snapshot records were excluded from analytical feature creation where necessary to prevent target leakage.

---

# How to Run

### Clone Repository

```bash
git clone https://github.com/Deepika825325/part1-data-audit-eda.git
cd part1-data-audit-eda
```

### Create Virtual Environment

```bash
python -m venv .venv
```

### Activate Environment

**Windows**

```bash
.venv\Scripts\activate
```

**Linux / macOS**

```bash
source .venv/bin/activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Launch Notebook

```bash
jupyter notebook
```

Open:

```text
notebooks/eda_audit.ipynb
```

Run all cells from top to bottom.

---

# Technologies Used

| Technology       | Purpose                                                      |
| ---------------- | ------------------------------------------------------------ |
| Python           | Data analysis and reporting                                  |
| Pandas           | Data loading, cleaning, aggregation, and feature engineering |
| NumPy            | Numerical computations                                       |
| Matplotlib       | Static visualization generation                              |
| Seaborn          | Statistical and business-oriented visualizations             |
| Jupyter Notebook | Interactive exploratory analysis                             |

---

# Limitations

Several limitations should be considered when interpreting the findings.

### Snapshot-Based Analysis

The analysis is based on a single customer snapshot and may not fully capture long-term behavioural changes.

### Correlation vs Causation

The identified churn-risk patterns represent statistical associations rather than proven causal relationships.

### Missing Customer Attributes

Certain attributes, including loyalty membership information and skin type, contain missing values that may influence segmentation quality.

### Unavailable Business Factors

External influences such as competitor activity, pricing changes, seasonality, customer satisfaction surveys, and marketing creative effectiveness were not available in the dataset.

---

# Conclusion

The analysis demonstrates that customer churn is primarily associated with declining engagement, increasing inactivity, lower campaign interaction, and weaker loyalty participation.

Customer inactivity emerged as the strongest churn indicator, while approximately **41% of total analysed revenue** was linked to customers who eventually churn.

These findings provide a strong foundation for retention strategy development, customer segmentation, churn prediction modelling, and subsequent phases of the D2C Customer Churn Intelligence project.
