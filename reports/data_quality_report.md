# Data Quality Report

## Project

D2C Customer Churn Intelligence – Part 1: Data Audit & Exploratory Data Analysis

---

# 1. Overview

A data quality assessment was conducted across six datasets:

* customers
* orders
* support_tickets
* web_events_snapshot
* intervention_history
* churn_labels

The objective was to identify issues that could impact exploratory analysis, customer-level feature engineering, churn modeling, and business decision-making.

---

# 2. Missing Value Assessment

The majority of fields are complete; however, several columns contain missing values.

| Dataset   | Column       | Missing Count | Missing % |
| --------- | ------------ | ------------: | --------: |
| customers | loyalty_tier |         1,386 |    57.75% |
| customers | skin_type    |           401 |    16.71% |
| orders    | rating       |            80 |     0.80% |

### Observations

* Missing values are concentrated in a small number of fields.
* The high proportion of missing values in `loyalty_tier` likely reflects customers who are not enrolled in the loyalty program rather than a system defect.
* Missing `skin_type` values may reduce segmentation effectiveness.
* Missing ratings are minimal and unlikely to materially affect analysis.

### Recommendation

* Create a dedicated **"Not Enrolled"** category for missing loyalty tiers.
* Retain missing skin type values as **"Unknown"** where appropriate.
* Leave ratings missing or apply business-approved imputation strategies during modeling.

---

# 3. Duplicate and Duplicate-Like Records

A duplicate assessment was performed across transactional data.

### Findings

* No exact duplicate records were identified.
* Twelve duplicate-like order records were detected using order identifiers containing the `_DUP` suffix.

### Impact

Duplicate transaction records can inflate:

* Revenue calculations
* Purchase frequency metrics
* Customer lifetime value estimates

### Recommendation

Validate duplicate-like transactions with business stakeholders before model development and production reporting.

---

# 4. Invalid Value Assessment

Validation checks were performed across key numerical fields.

| Validation Rule        | Invalid Records |
| ---------------------- | --------------: |
| Quantity < 1           |               0 |
| Discount < 0%          |               0 |
| Discount > 100%        |               0 |
| Negative Delivery Days |               0 |
| Rating Outside 1–5     |               0 |
| Sentiment < -1         |               0 |
| Sentiment > 1          |               0 |

### Observation

No business-rule violations were detected.

### Recommendation

Maintain these validation checks as part of future data ingestion and monitoring processes.

---

# 5. Outlier Assessment

Order value analysis identified a subset of unusually large transactions.

### Findings

| Metric                            |      Value |
| --------------------------------- | ---------: |
| Average Order Value               |    ₹743.90 |
| Median Order Value                |    ₹597.06 |
| Maximum Order Value               | ₹24,789.38 |
| Statistical Outliers (IQR Method) |        536 |

### Observation

The difference between average and median order value indicates the presence of a right-skewed spending distribution.

### Impact

High-value transactions can disproportionately influence:

* Average revenue metrics
* Customer value calculations
* Model coefficients

### Recommendation

Retain outliers for business analysis because they likely represent genuine purchases. Consider robust scaling, log transformation, or winsorization during model development.

---

# 6. Join Key Validation

Customer identifiers were validated across all datasets.

| Dataset   | Records | Orphan Records |
| --------- | ------: | -------------: |
| orders    |  10,009 |              0 |
| tickets   |   1,921 |              0 |
| web       |   2,400 |              0 |
| campaigns |   2,400 |              0 |
| churn     |   2,400 |              0 |

### Observation

No orphan records were identified.

### Impact

All datasets can be successfully integrated into a unified customer-level analytical dataset without record loss.

---

# 7. Date Consistency Assessment

Reference snapshot date:

**2025-09-30**

### Findings

* 1,872 order records occur after the snapshot date.
* These records contain information that would not have been available at prediction time.

### Impact

Including post-snapshot activity would introduce target leakage and produce unrealistically optimistic model performance.

### Recommendation

Restrict all feature engineering activities to information available on or before the snapshot date.

---

# 8. Potential Data Leakage Risks

The following information sources require special handling during model development:

| Feature Source           | Risk           |
| ------------------------ | -------------- |
| Post-snapshot orders     | Target leakage |
| Future support activity  | Target leakage |
| Future campaign activity | Target leakage |

### Observation

Target leakage represents the most significant modeling risk identified during this audit.

### Recommendation

Implement strict temporal filtering and validate that all engineered features are generated using only information available at the snapshot date.

---

# 9. Overall Assessment

### Key Risks

1. Missing loyalty program information.
2. Duplicate-like transaction records.
3. High-value spending outliers.
4. Potential leakage from future activity.

### Key Strengths

1. Strong join integrity across all datasets.
2. Minimal missing data outside customer profile attributes.
3. No invalid-value violations.
4. Consistent customer identifiers across sources.
5. Suitable structure for customer-level aggregation and churn modeling.

### Data Quality Verdict

The datasets demonstrate good overall quality and are suitable for exploratory analysis and churn modeling.

The primary area requiring attention is temporal leakage prevention during feature engineering. Apart from this risk, the data is well-structured, internally consistent, and capable of supporting reliable customer churn analysis.
