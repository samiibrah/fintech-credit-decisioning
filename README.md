# AI-Driven Credit Decisioning & Pricing Engine

## Overview

This project builds an **end-to-end AI-assisted credit decisioning system** that mirrors how modern FinTech lenders evaluate consumer loan applications.

The system estimates **probability of default (PD)** using interpretable machine learning, translates predictions into **risk tiers and pricing decisions**, and evaluates tradeoffs between **approval rate, expected loss, and risk-adjusted return** — with explicit attention to **explainability and governance**.

The goal is to **support defensible lending decisions**.

---

## Business Problem

Consumer lenders face a constant tradeoff:

> Approve more customers → grow revenue
> Tighten credit → reduce losses

This project answers:

* Who should be approved or declined?
* At what interest rate should credit be offered?
* How do we explain each decision to regulators and customers?
* What is the portfolio-level impact of different credit strategies?

---

## Dataset

**Source:** LendingClub public loan data (2007–2018)

**Target Variable**

* `default_flag = 1` if loan status ∈ {Charged Off, Default}
* `default_flag = 0` if loan status = Fully Paid

This framing mirrors real-world **probability of default (PD)** modeling.

---

## Feature Selection

Features were intentionally limited to variables commonly used in regulated consumer lending.

### Credit & Financial Profile

* `loan_amnt`
* `term`
* `int_rate`
* `fico_range_low`
* `annual_inc`
* `dti`
* `revol_util`
* `open_acc`
* `delinq_2yrs`
* `inq_last_6mths`

### Stability & Verification

* `emp_length`
* `home_ownership`
* `verification_status`

### Loan Context

* `purpose`
* `application_type`
* `grade / sub_grade`

**Explicit exclusions**

* No protected attributes (race, gender, age)
* No post-origination variables (to prevent data leakage)
* No ZIP-level geographic features

---

## Modeling Approach

### 1. Baseline Model 

* Logistic Regression
* Bucketed and monotonic-friendly features
* Serves as an interpretable reference model

### 2. Machine Learning Model

* Gradient Boosting (XGBoost / LightGBM)
* Optimized for discrimination and calibration
* Same feature set as baseline (no feature leakage)

### 3. Model Evaluation

* ROC-AUC
* Precision-Recall (class imbalance aware)
* Calibration curves
* Stability checks

---

## Explainability

To support transparency and governance:

* **Global explanations:** Feature importance and SHAP summary plots
* **Local explanations:** Individual loan-level SHAP breakdowns
* Clear articulation of *why* a borrower was approved or declined

---

## Decisioning & Pricing Engine

Model outputs are converted into **actionable credit decisions**.

### Risk Tiering

| PD Range | Risk Tier | Decision    | APR    |
| -------- | --------- | ----------- | ------ |
| < 3%     | A         | Approve     | 11–13% |
| 3–6%     | B         | Approve     | 15–18% |
| 6–9%     | C         | Conditional | 20–24% |
| > 9%     | D         | Decline     | —      |

### Portfolio Simulation

For each strategy, the system simulates:

* Approval rate
* Expected credit loss
* Risk-adjusted return
* Distribution of accepted risk

This allows comparison of conservative vs growth-oriented lending policies.

---

## Fairness & Governance Considerations

While the dataset does not include protected attributes, this project explicitly:

* Avoids proxy features likely to introduce bias
* Evaluates tradeoffs between approval rate and risk concentration
* Documents modeling assumptions and limitations

The intent is to demonstrate **responsible AI usage** in financial decision-making.

---

## Repository Structure

```
fintech-credit-decisioning/
│
├── data/
│   └── lending_club_clean.csv
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_baseline_model.ipynb
│   ├── 03_ml_model.ipynb
│   ├── 04_explainability.ipynb
│   └── 05_decision_simulation.ipynb
├── src/
│   └── decision_engine.py
├── reports/
│   └── credit_committee_memo.pdf
└── README.md
```

---

## Key Takeaways

* Interpretable ML can outperform traditional models **without sacrificing governance**
* Decisioning layers matter as much as prediction accuracy
* Small changes in approval thresholds can significantly shift portfolio risk

---


## Author

Built as a portfolio project focused on **FinTech credit risk, AI explainability, and decision science**.

---
