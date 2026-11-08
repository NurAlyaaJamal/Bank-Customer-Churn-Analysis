# Bank Customer Churn Analytics

**Understanding why customers leave through data analytics, statistical testing and churn-risk modelling.**

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![SQLite](https://img.shields.io/badge/SQL-SQLite-003B57?logo=sqlite&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikitlearn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

> EDA · Segment churn analysis · Statistical testing · Driver analysis · Risk segmentation · Predictive modelling

---

## 📌 Project Overview

This project explores which bank customers leave, which profiles, product-holding patterns and behaviours are linked to churn, and which of those associations are statistically meaningful. It also sizes the balances at risk and builds a **leakage-safe model** that ranks customers by churn risk so retention effort can be prioritised.

**Notebook:** [`bank-customers-churn-analysis.ipynb`](bank-customers-churn-analysis.ipynb)
**Dataset:** [Bank Customer Churn (Kaggle)](https://www.kaggle.com/datasets/radheshyamkollipara/bank-customer-churn) — `Customer-Churn-Records.csv`, 10,000 customers × 18 columns

## 🔑 Headline Results

| Metric | Result |
|---|---|
| Customers analysed | **10,000** (no missing values, no duplicates) |
| Overall churn rate | **20.4%** (2,038 customers) |
| Balance held by leavers | **185.7M** — 24.3% of all balances |
| Risk score concentration | **74%** of churners sit in the top 35% of customers (High / Very high tiers) |
| Best model | Random Forest — test **ROC-AUC 0.87**, recall 69%, precision 60% (threshold 0.32) |
| Top 20% of customers by risk score | contain about **62%** of churners |

## 🔍 Key Findings

- **Age:** churn peaks at **46–55 (50.6%)** vs 7.5% at 18–25 — a hump, not a straight line.
- **Products:** two products is safest (**7.6%** churn) vs 27.7% for one product; customers with 3–4 products churn at 83–100% (only 326 customers).
- **Activity:** inactive members churn at **26.9%** vs 14.3% for active members.
- **Geography & gender:** Germany churns at **32.4%** (vs 16.2% lowest country); women at 25.1% vs 16.5% for men. Both persist after adjustment (odds ratios 2.66 and 1.67).
- **Balances:** higher balances are *not* protective — leavers are 20.4% of customers but hold 24.3% of balances.
- **Little signal:** card type, satisfaction score, credit-card ownership, credit score and estimated salary.
- **Highest-risk profile:** Germany · Inactive · 1 product · Aged 40–64 (**73%** churn).

## ⚠️ Target Leakage: `Complain`

`Complain` matches `Exited` for **99.9%** of customers (99.5% of complainers left vs 0.05% of others). Adding it lifts test ROC-AUC from **0.870 to 0.999** — not because the model is better, but because it reads the outcome from a column that records it. `Complain` and `Satisfaction Score` are therefore **excluded from modelling**.

## 🧭 Workflow

1. **Load & validate** — structure, types, missing values, duplicates, identifiers
2. **Customer profile** — demographics, products, balances, card types
3. **Measure churn** — overall rate and balance at risk
4. **Segment comparison** — churn *rate* (not count) with 95% Wilson confidence intervals
5. **Age & financial bands** — age, credit score, balance, salary
6. **Combined effects** — product × activity, geography × gender, the Germany pattern
7. **Complaints & satisfaction** — early-warning signal or leakage?
8. **Statistical tests** — Chi-square (Cramér's V), Mann–Whitney U (rank-biserial r), Benjamini–Hochberg FDR
9. **Independent drivers** — logistic regression odds ratios with 300-resample bootstrap CIs
10. **Risk segmentation** — explainable five-factor score and risk tiers
11. **Predictive modelling** — Logistic Regression vs Random Forest vs Gradient Boosting, 5-fold CV, threshold tuning on out-of-fold predictions, single held-out test evaluation
12. **Business insights** — findings, recommendations, limitations

## 🛠️ Tech Stack

- **SQL (SQLite, in-memory):** segment aggregation, conditional aggregation, window functions, CTEs, risk scoring, a `v_customers` view
- **Python:** pandas, NumPy, SciPy
- **Visualisation:** Matplotlib, Seaborn
- **Modelling:** scikit-learn (pipelines, cross-validation, permutation importance)

## 💡 Recommendations

| # | Action | Priority |
|---|---|---|
| 1 | Targeted retention for inactive customers aged 40–64 | High |
| 2 | Move one-product customers to a second product (test with a control group) | High |
| 3 | Investigate the 3–4 product bundles (pricing, fees, service) | High |
| 4 | Country-level review of Germany | Medium |
| 5 | Monitor activity and trigger outreach when it drops | Medium |
| 6 | Use the risk score to rank outreach and measure save rate with a holdout group | Medium |
| 7 | Confirm when `Complain` / `Satisfaction Score` are recorded before using them | Data task |

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/NurAlyaaJamal/<your-repo-name>.git
cd <your-repo-name>

# 2. Install dependencies (SQLite ships with Python)
pip install pandas numpy scipy matplotlib seaborn scikit-learn jupyter

# 3. Download Customer-Churn-Records.csv from Kaggle and place it in the project folder

# 4. Launch the notebook
jupyter notebook bank-customers-churn-analysis.ipynb
```

The notebook looks for the CSV at the Kaggle input path first, then falls back to local locations, so it also runs directly on Kaggle.

## 📝 Analysis Principles & Limitations

- Findings are **associations**, not proven causes. Validate with a controlled retention test (contact a random half of high-risk customers, hold back the rest).
- Churn is compared as a **rate within each segment**, and statistical significance is read together with **effect size**.
- The data is a **single snapshot with no dates**, so timing, trends and seasonality cannot be analysed.
- Provenance is undocumented and some columns behave like simulated data — results **demonstrate the method** and are not evidence about a real bank.
- Risk tiers were designed on the same data they describe (in-sample); model results use a held-out test set, which still carries sampling error.
- The threshold maximises F1, not profit. The real cost of a missed churner vs a wasted contact should set the production threshold.
- No currency is given, so amounts are shown as plain numbers.

## 👩‍💻 Author

**Nur Alyaa Jamal**
[LinkedIn](https://www.linkedin.com/in/nur-alyaa-jamal13/) · [GitHub](https://github.com/NurAlyaaJamal) · nralyaajamal@gmail.com
