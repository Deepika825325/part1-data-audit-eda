# Data Quality Report

## Project

D2C Customer Churn Intelligence – Part 1: Data Audit & Exploratory Data Analysis

---

# 1. Overview

A data quality assessment was performed across six datasets:

* customers
* orders
* support_tickets
* web_events_snapshot
* intervention_history
* churn_labels

The objective was to identify issues that could affect business analysis, churn modeling, or retention strategy design.

---

# 2. Missing Value Assessment

The majority of fields are complete; however, several columns contain missing values.

| Dataset   | Column       | Missing Count | Missing % |
| --------- | ------------ | ------------: | --------: |
| customers | loyalty_tier |          1386 |    57.75% |
| customers | skin_type    |           401 |    16.71% |
| orders    | rating       |            80 |     0.80% |

### Observations

* The large proportion of missing values in `loyalty_tier` likely represents customers who are not enrolled in the loyalty program rather than a data collection issue.
* Missing `skin_type` values may reduce the usefulness of customer segmentation analyses.
* Missing ratings are limited and unlikely to materially impact analysis.

### Recommendation

* Treat missing loyalty tiers as a separate category ("Not Enrolled").
* Retain missing skin type values as "Unknown" if used for modeling.
* Leave rating values missing or impute cautiously depending on modeling requirements.

---

# 3. Duplicate and Duplicate-Like Records

A duplicate assessment was performed across transactional data.

### Findings

* No exact duplicate records were identified.
* 12 duplicate-like order records were detected using order identifiers containing the `_DUP` suffix.

### Impact

Duplicate transactional records can inflate revenue, purchase frequency, and customer activity metrics.

### Recommendation

Duplicate-like records should be reviewed with business stakeholders before inclusion in production models.

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

No major data integrity violations were detected.

### Recommendation

Continue enforcing these validation rules during future data ingestion processes.

---

# 5. Outlier Assessment

Order value analysis identified unusually large transactions.

### Findings

* Average order value: ₹743.90
* Median order value: ₹597.06
* Maximum order value: ₹24,789.38
* 536 orders were identified as statistical outliers using the IQR method.

### Impact

Outliers may disproportionately influence averages and customer value calculations.

### Recommendation

Retain outliers for business analysis because they likely represent genuine high-value purchases, but consider robust scaling or transformation during modeling.

---

# 6. Join Key Validation

Customer identifiers were validated across all datasets.

| Dataset   | Records | Orphan Records |
| --------- | ------: | -------------: |
| orders    |   10009 |              0 |
| tickets   |    1921 |              0 |
| web       |    2400 |              0 |
| campaigns |    2400 |              0 |
| churn     |    2400 |              0 |

### Observation

All datasets successfully join through `customer_id`.

### Impact

No customer records are lost during customer-level aggregation.

---

# 7. Date Consistency Assessment

Snapshot date for churn prediction is:

**2025-09-30**

### Findings

* 1,872 order records occur after the snapshot date.
* These records represent future information relative to the prediction target.

### Impact

Using post-snapshot transactions would introduce target leakage and artificially improve model performance.

### Recommendation

Future modeling should exclude all records occurring after the snapshot date.

---

# 8. Potential Data Leakage Risks

The following fields require careful treatment during model development:

| Feature                      | Risk           |
| ---------------------------- | -------------- |
| post-snapshot orders         | Target leakage |
| future support interactions  | Target leakage |
| future campaign interactions | Target leakage |

### Recommendation

All features must be constructed using information available on or before the snapshot date.

---

# 9. Overall Assessment

The datasets demonstrate generally good data quality and are suitable for exploratory analysis and churn modeling.

### Key Risks

1. Missing loyalty tier values.
2. Duplicate-like order records.
3. High-value transaction outliers.
4. Potential leakage from post-snapshot activity.

### Key Strengths

1. Strong join integrity across datasets.
2. Minimal missing values outside customer profile fields.
3. No major invalid-value issues.
4. Well-structured customer-level identifiers.

Overall data quality is sufficient for customer churn analysis after applying the recommended controls and validation procedures.
