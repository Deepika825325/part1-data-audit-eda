# D2C Customer Churn Intelligence – Part 1: Data Audit, EDA & Business Understanding

## Project Overview

Customer churn is one of the most important business challenges for direct-to-consumer (D2C) companies. Before building predictive models or designing retention campaigns, it is essential to understand customer behavior, identify data-quality issues, and uncover patterns associated with churn.

This repository contains a complete data audit and exploratory analysis of customer, transaction, support, engagement, campaign, and churn datasets. The objective is to convert raw business data into actionable insights and evidence-based churn-risk hypotheses.

**Prediction Target:** Identify customers who are likely to churn within the next 60 days.

**Snapshot Date:** 2025-09-30

---

## Business Problem

The company experiences a significant level of customer attrition but lacks visibility into the behavioral and operational factors driving churn.

The primary goals of this analysis are:

* Assess overall data quality.
* Validate dataset integrity and join relationships.
* Understand customer behavior patterns.
* Identify potential churn-risk indicators.
* Generate business recommendations for future retention initiatives.

---

## Dataset Summary

The analysis was performed using six datasets.

| Dataset              | Description                                  | Records |
| -------------------- | -------------------------------------------- | ------: |
| customers            | Customer profile and demographic information |   2,400 |
| orders               | Historical order transactions                |  10,009 |
| support_tickets      | Customer support interactions                |   1,921 |
| web_events_snapshot  | Customer engagement metrics                  |   2,400 |
| intervention_history | Marketing and campaign history               |   2,400 |
| churn_labels         | Churn outcomes and dataset split information |   2,400 |

All datasets are connected through a common customer identifier (`customer_id`).

---

## Analysis Workflow

The project follows a structured analytical workflow:

### 1. Data Audit

* Missing value assessment
* Duplicate and duplicate-like record detection
* Invalid value validation
* Outlier detection
* Join integrity validation
* Date consistency checks
* Potential leakage identification

### 2. Exploratory Data Analysis

Analysis was performed across:

* Customer demographics
* Acquisition channels
* Loyalty program participation
* Order behavior
* Revenue and spending patterns
* Product returns
* Customer ratings
* Support interactions
* Web engagement activity
* Campaign exposure
* Churn distribution

### 3. Customer-Level Master Dataset

A consolidated customer-level analytical dataset was created by combining:

* Customer profile data
* Transaction history
* Support metrics
* Engagement metrics
* Campaign information
* Churn outcomes

This dataset was used to evaluate churn-risk hypotheses.

---

## Key Findings

### Strongest Churn Indicators

| Rank | Driver                | Strength    |
| ---- | --------------------- | ----------- |
| 1    | Days Since Last Visit | Very Strong |
| 2    | Session Activity      | Very Strong |
| 3    | Purchase Frequency    | Strong      |
| 4    | Campaign Engagement   | Strong      |
| 5    | Return Rate           | Moderate    |

### Supported Hypotheses

* Customers with fewer orders are more likely to churn.
* Customers with lower session activity are more likely to churn.
* Customers with longer inactivity periods are more likely to churn.
* Customers with higher return rates are more likely to churn.
* Customers with lower campaign engagement are more likely to churn.

### Partially Supported Hypotheses

* Higher loyalty tiers appear beneficial, but Silver-tier membership shows limited impact.

### Unsupported Hypotheses

* Support-ticket sentiment was not a strong churn differentiator.
* Ticket reopen rates showed minimal relationship with churn.

---

## Business Recommendations

### Priority 1 – Monitor Customer Inactivity

Customers who have not visited recently represent the highest-risk segment and should be targeted with re-engagement campaigns.

### Priority 2 – Focus on Low-Frequency Buyers

Declining purchase frequency is strongly associated with churn and should trigger retention interventions.

### Priority 3 – Improve Loyalty Program Effectiveness

Gold and Platinum members show improved retention compared to lower-tier customers.

### Priority 4 – Increase Campaign Engagement

Marketing engagement metrics can serve as early warning indicators of customer attrition.

---

## Repository Structure

```text
part1-data-audit-eda/
│
├── data/
│
├── notebooks/
│   └── eda_audit.ipynb
│
├── outputs/
│   ├── charts/
│   └── tables/
│
├── reports/
│   ├── data_quality_report.md
│   └── business_memo.md
│
├── README.md
└── requirements.txt
```

---

## Deliverables

### Notebook

* `notebooks/eda_audit.ipynb`

Contains:

* Data loading
* Data quality assessment
* Exploratory analysis
* Customer-level feature aggregation
* Churn analysis
* Eight churn-risk hypotheses
* Business interpretation and recommendations

### Reports

#### Data Quality Report

`reports/data_quality_report.md`

Documents:

* Missing values
* Duplicates
* Outliers
* Join validation
* Leakage risks
* Data-quality recommendations

#### Business Memo

`reports/business_memo.md`

Provides:

* Executive findings
* Retention priorities
* Recommended business actions

---

## Generated Outputs

### Charts

All visualizations generated during analysis are available in:

```text
outputs/charts/
```

### Tables

Supporting tables and summaries are available in:

```text
outputs/tables/
```

Important outputs include:

* dataset_overview.csv
* schema_summary.csv
* missing_values_summary.csv
* churn_distribution.csv
* churn_hypothesis_summary.csv
* key_business_findings.csv

---

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Jupyter Notebook

---

## Conclusion

The analysis indicates that customer churn is primarily driven by declining engagement and inactivity rather than support-related factors. Customers who reduce platform visits, purchase less frequently, and engage less with marketing campaigns represent the highest-risk segments.

The findings from this analysis provide a strong foundation for future churn prediction models and retention strategy development.
