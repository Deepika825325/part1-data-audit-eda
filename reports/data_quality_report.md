# Data Quality Report

## Executive Summary

A comprehensive data quality assessment was conducted across all datasets used in the customer churn analysis, including customer profiles, orders, support tickets, web activity, campaign history, and churn labels.

The audit evaluated missing values, duplicate records, invalid values, outliers, join integrity, date consistency, and potential data leakage risks. Overall data quality was found to be strong, with no critical integrity issues preventing analysis. However, several findings require consideration before developing predictive models or retention strategies.

---

# 1. Missing Value Assessment

## Summary

| Dataset   | Column       | Missing Values | Missing % |
| --------- | ------------ | -------------: | --------: |
| customers | loyalty_tier |          1,386 |    57.75% |
| customers | skin_type    |            401 |    16.71% |
| orders    | rating       |             80 |     0.80% |

## Missing Value Visualization

A missing-value summary chart was generated during the exploratory analysis and saved in:

```text
outputs/charts/missing_values_percentage.png
```

The visualization confirms that missingness is concentrated primarily in `loyalty_tier` and `skin_type`, while all remaining variables exhibit minimal or no missing values.

## Business Impact

* Missing loyalty tier values indicate that a large proportion of customers are either not enrolled in the loyalty program or membership information is unavailable.
* This should not be treated solely as a data quality issue. During the analysis, customers without meaningful loyalty participation exhibited higher churn rates than Gold and Platinum members, making loyalty enrollment itself a valuable business signal.
* Missing skin type values reduce the effectiveness of personalization, product recommendation, and customer segmentation initiatives.
* Additionally, missing skin type information may limit the ability to identify churn-risk patterns associated with product preferences or customer demographics if skin type is correlated with purchasing behaviour.
* Missing ratings are minimal and unlikely to materially affect analytical outcomes.

## Recommended Treatment

| Column       | Recommendation                                   |
| ------------ | ------------------------------------------------ |
| loyalty_tier | Create a separate "Not Enrolled" category        |
| skin_type    | Create an "Unknown" category                     |
| rating       | Leave missing or impute using median if required |

---

# 2. Duplicate Record Assessment

## Exact Duplicates

| Dataset         | Duplicate Rows |
| --------------- | -------------: |
| customers       |              0 |
| orders          |              0 |
| support_tickets |              0 |
| web_events      |              0 |
| campaigns       |              0 |
| churn_labels    |              0 |

## Duplicate-Like Records

| Dataset         | Duplicate-Like Records |
| --------------- | ---------------------: |
| orders          |                     26 |
| support_tickets |                      0 |

## Business Impact

No exact duplicate records were identified across any dataset.

The 26 duplicate-like order records were reviewed and appear to represent legitimate customer behaviour rather than accidental duplication. These records typically involve repeat purchases by the same customer occurring on the same date but with different transaction attributes, product categories, quantities, or order values.

As a result, these records are likely valid business events and should be retained for downstream analysis.

## Recommended Treatment

Retain duplicate-like records unless additional business rules indicate transactional duplication.

---

# 3. Invalid Value Validation

## Validation Results

| Validation Rule | Invalid Records |
| --------------- | --------------: |
| Quantity <= 0   |               0 |
| Discount < 0    |               0 |
| Discount > 1    |               0 |
| Rating < 1      |               0 |
| Rating > 5      |               0 |
| Sentiment < -1  |               0 |
| Sentiment > 1   |               0 |

## Business Impact

No invalid values were detected across the validated business-critical fields.

## Recommended Treatment

Continue enforcing validation rules during future data ingestion processes.

---

# 4. Outlier Assessment

## Order Value Distribution

| Metric  |      Value |
| ------- | ---------: |
| Minimum |    ₹149.00 |
| Average |    ₹743.90 |
| Median  |    ₹597.06 |
| Maximum | ₹24,789.38 |

## IQR Analysis

| Metric             |     Value |
| ------------------ | --------: |
| Q1                 |   ₹432.85 |
| Q3                 |   ₹907.43 |
| IQR                |   ₹474.58 |
| Upper Bound        | ₹1,619.30 |
| Outlier Orders     |       536 |
| Outlier Percentage |     5.36% |

## Business Impact

A small proportion of orders exhibit unusually high transaction values. These orders likely represent premium purchases, bulk purchases, or promotional buying activity rather than data quality errors.

Removing these records would distort revenue analysis and customer value assessments.

## Recommended Treatment

Retain outliers for business analysis. Consider winsorization or robust scaling only if future machine learning models prove sensitive to extreme values.

---

# 5. Join Integrity Validation

## Orphan Record Check

| Dataset         | Records | Unique Customer IDs | Orphan Customer IDs |
| --------------- | ------: | ------------------: | ------------------: |
| orders          |  10,009 |               2,400 |                   0 |
| support_tickets |   1,921 |               1,247 |                   0 |
| web_events      |   2,400 |               2,400 |                   0 |
| campaigns       |   2,400 |               2,400 |                   0 |
| churn_labels    |   2,400 |               2,400 |                   0 |

## Customer Coverage

| Dataset         | Customer Coverage % |
| --------------- | ------------------: |
| orders          |              100.00 |
| support_tickets |               51.96 |
| web_events      |              100.00 |
| campaigns       |              100.00 |
| churn_labels    |              100.00 |

## Business Impact

All datasets can be successfully linked using `customer_id` without data loss. No orphan records were identified.

Support-ticket coverage is notably lower than other datasets, with only 51.96% of customers appearing in support interactions. Consequently, the absence of support tickets should not automatically be interpreted as positive customer sentiment or customer satisfaction.

## Recommended Treatment

Use `customer_id` as the primary integration key and perform LEFT JOIN operations when constructing customer-level analytical datasets.

---

# 6. Date Consistency Assessment

## Snapshot Validation

All snapshot-based datasets use a consistent snapshot date.

| Dataset      | Snapshot Date |
| ------------ | ------------- |
| web_events   | 2025-09-30    |
| campaigns    | 2025-09-30    |
| churn_labels | 2025-09-30    |

## Post-Snapshot Records

| Dataset         | Records After Snapshot |
| --------------- | ---------------------: |
| orders          |                  1,872 |
| support_tickets |                      0 |

Post-snapshot records were identified by comparing transaction dates (`order_date`) against the analytical snapshot date (`2025-09-30`). Any order where:

```text
order_date > snapshot_date
```

was classified as future information unavailable at prediction time.

## Business Impact

A substantial number of order records occur after the analytical snapshot date and therefore represent future information that would not have been available at prediction time.

## Recommended Treatment

Exclude all post-snapshot records during feature engineering and predictive modelling activities.

---

# 7. Leakage Risk Assessment

## Leakage Risk Summary

| Feature Source   | Leakage Risk | Reason                              |
| ---------------- | ------------ | ----------------------------------- |
| Orders           | High         | Contains post-snapshot transactions |
| Support Tickets  | Low          | No post-snapshot records detected   |
| Campaign History | Low          | Aligned to snapshot date            |
| Web Events       | Low          | Aligned to snapshot date            |

## Business Impact

Using future information would artificially inflate model performance and produce unrealistic churn predictions.

## Recommended Treatment

Restrict all modelling features to information available on or before the customer snapshot date.

---

# 8. Data Quality Implications for Analysis

The identified data quality findings influenced downstream analytical decisions.

* Loyalty-tier missingness was treated as a meaningful customer segment rather than a simple data-quality issue.
* High-value order outliers were retained because they represent genuine purchasing behaviour and revenue concentration.
* Post-snapshot orders were excluded from feature engineering to prevent target leakage.
* Support-ticket coverage of 51.96% indicates that ticket absence should not automatically be interpreted as positive customer sentiment.
* Care should be taken when creating support-related features because imputing missing ticket activity as zero may introduce systematic bias if support engagement is associated with churn behaviour.

These considerations were incorporated into the customer master dataset construction and exploratory analysis workflow.

---

# Overall Assessment

The datasets demonstrate strong overall quality, with no critical issues related to duplication, invalid values, or referential integrity.

The primary analytical risks are:

1. Missing customer attributes, particularly loyalty enrollment information.
2. The presence of post-snapshot transactions that could introduce target leakage.
3. Uneven support-ticket coverage across the customer base.

With appropriate handling of missing values and strict exclusion of post-snapshot information, the data is suitable for exploratory analysis, customer segmentation, churn modelling, and retention strategy development.
