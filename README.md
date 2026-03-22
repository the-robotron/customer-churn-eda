# Customer Churn EDA

[![Python](https://img.shields.io/badge/Python-3.10-blue)](https://python.org) [![Pandas](https://img.shields.io/badge/Pandas-2.0-green)](https://pandas.pydata.org) [![Seaborn](https://img.shields.io/badge/Seaborn-0.12-orange)](https://seaborn.pydata.org) [![Status](https://img.shields.io/badge/Status-Completed-brightgreen)](.) [![Model](https://img.shields.io/badge/Model-Logistic%20Regression-purple)](notebooks/) [![AUC](https://img.shields.io/badge/AUC--ROC-0.843-red)](reports/churn_metrics_summary.md)

## Overview

This project performs an in-depth **Exploratory Data Analysis (EDA)** on a telecom company's customer dataset to understand the factors driving customer churn. Using Python and data visualisation libraries, the notebook uncovers patterns and relationships that help predict which customers are likely to leave.

## Key Metrics at a Glance

| Metric | Value |
|--------|-------|
| **Overall Churn Rate** | 26.5% (industry avg: 15-20%) |
| **Month-to-Month Contract Churn** | ~42% |
| **Fiber Optic Churn Rate** | ~41.9% |
| **Avg Tenure of Churned Customers** | ~17.9 months vs ~37.6 months (retained) |
| **Avg Monthly Charges (Churned)** | ~$74.44 vs ~$61.27 (retained) |
| **Model Accuracy** | 80.4% |
| **AUC-ROC Score** | 0.843 |
| **Estimated Revenue at Risk** | ~$26,000/month from preventable churn |

## Dataset

- **Source**: IBM Telco Customer Churn Dataset (Kaggle)
- **Rows**: 7,043 customers
- **Features**: 21 columns including demographics, account info, and service subscriptions
- **Target**: `Churn` (Yes / No)

| Feature | Description |
|---|---|
| `tenure` | Number of months the customer has been with the company |
| `MonthlyCharges` | Monthly billing amount |
| `TotalCharges` | Total amount charged |
| `Contract` | Month-to-month, One year, Two year |
| `InternetService` | DSL, Fiber optic, No |
| `PaymentMethod` | Electronic check, Mailed check, etc. |

## EDA Steps

### 1. Data Loading & Inspection
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('WA_Fn-UseC_-Telco-Customer-Churn.csv')
print(df.shape)       # (7043, 21)
df.info()
df.isnull().sum()
```

### 2. Churn Distribution
```python
df['Churn'].value_counts(normalize=True).plot(kind='pie', autopct='%1.1f%%')
plt.title('Customer Churn Distribution')
```

### 3. Numerical Feature Analysis
```python
fig, axes = plt.subplots(1, 3, figsize=(15, 4))
for ax, col in zip(axes, ['tenure', 'MonthlyCharges', 'TotalCharges']):
    df.groupby('Churn')[col].hist(ax=ax, alpha=0.6, bins=30)
    ax.set_title(f'{col} by Churn')
```

### 4. Categorical Feature Analysis
```python
categorical_cols = ['Contract', 'InternetService', 'PaymentMethod',
                    'OnlineSecurity', 'TechSupport', 'SeniorCitizen']
for col in categorical_cols:
    churn_rate = df.groupby(col)['Churn'].apply(lambda x: (x=='Yes').mean() * 100)
    churn_rate.plot(kind='bar')
```

### 5. Correlation Heatmap
```python
numeric_df = df[['tenure', 'MonthlyCharges', 'TotalCharges', 'SeniorCitizen']].copy()
numeric_df['Churn'] = (df['Churn'] == 'Yes').astype(int)
sns.heatmap(numeric_df.corr(), annot=True, cmap='coolwarm')
```

## Key Findings

- **Contract type** is the strongest churn predictor: month-to-month customers churn at 42% vs 11% for two-year contracts
- **Fiber optic internet** customers have higher churn (~42%) despite paying more — potential service quality issue
- **Tenure** is strongly negatively correlated with churn: customers who stay 3+ years rarely leave
- **Electronic check** payment method associated with highest churn (~45%) — may indicate uncommitted customers
- **Senior citizens** churn at nearly double the rate of non-seniors (42% vs 24%)
- **No tech support + no online security** customers are at significantly higher churn risk
- Top 20% of high-risk customers could be identified and retained with targeted interventions

## Improvement Opportunities

| Area | Current Churn | Target | Action |
|------|--------------|--------|--------|
| Contract Migration | 42% (M2M) | <25% | Discount for annual plan upgrade |
| Fiber Optic Service | 41.9% | <30% | Service quality audit + loyalty credits |
| Tech Support Adoption | 41.6% | <25% | Bundle free trial at signup |
| Electronic Check | 45.3% | <30% | Incentivise auto-pay migration |
| Senior Segment | 41.7% | <30% | Dedicated customer success outreach |
| Early Tenure (0-12 mo) | Highest risk band | -20% | Structured onboarding program |

> Full metrics report: [reports/churn_metrics_summary.md](reports/churn_metrics_summary.md)

## Model Results (Logistic Regression)

| Metric | Score |
|--------|-------|
| Accuracy | 80.4% |
| Precision (Churn) | 65.8% |
| Recall (Churn) | 54.9% |
| F1-Score (Churn) | 59.9% |
| AUC-ROC | **0.843** |

**Top churn predictors:** Contract type > Tenure > Internet Service > Payment Method > Tech Support

## Tools & Libraries

- **Python 3.10** with Jupyter Notebook
- **Pandas 2.0** — data manipulation
- **Matplotlib / Seaborn** — visualisation
- **Scikit-learn** — Logistic Regression, metrics
- **NumPy** — numerical operations

## Project Structure

```
customer-churn-eda/
├── data/
│   └── telco_churn.csv
├── notebooks/
│   └── customer_churn_eda.ipynb
├── reports/
│   └── churn_metrics_summary.md    ← NEW: Full metrics + improvement table
├── requirements.txt
└── README.md
```

## How to Run

```bash
git clone https://github.com/the-robotron/customer-churn-eda.git
cd customer-churn-eda
pip install -r requirements.txt
jupyter notebook notebooks/customer_churn_eda.ipynb
```

## Author

**Shivam Singh** — [GitHub](https://github.com/the-robotron) | [LinkedIn](https://linkedin.com/in/shivam-singh)
