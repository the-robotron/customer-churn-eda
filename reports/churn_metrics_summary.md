# Customer Churn EDA — Metrics & Findings Summary

> This report summarises all key metrics, statistical findings, and improvement recommendations derived from the Customer Churn EDA notebook.

---

## Dataset Overview

| Attribute | Value |
|-----------|-------|
| **Source** | IBM Telco Customer Churn Dataset (Kaggle) |
| **Total Records** | 7,043 customers |
| **Features** | 21 columns |
| **Target Variable** | `Churn` (Yes / No) |
| **Churn Rate (Overall)** | ~26.5% |
| **Non-Churn Rate** | ~73.5% |
| **Class Imbalance Ratio** | ~1:2.8 (Churn:Non-Churn) |

---

## Key Churn Metrics

| Metric | Value | Insight |
|--------|-------|--------|
| **Overall Churn Rate** | 26.5% | High — industry avg is ~15-20% |
| **Month-to-Month Contract Churn** | ~42% | 3x higher than annual contract customers |
| **Fiber Optic Churn Rate** | ~41.9% | Despite premium service, higher churn |
| **DSL Churn Rate** | ~19.0% | More stable customer base |
| **No Internet Service Churn** | ~7.4% | Lowest churn segment |
| **Electronic Check Payment Churn** | ~45.3% | Highest among payment methods |
| **Credit Card Payment Churn** | ~15.2% | Lowest churn payment method |
| **No Tech Support Churn** | ~41.6% | Double the rate of those with tech support |
| **Senior Citizens Churn Rate** | ~41.7% | Nearly 2x the non-senior rate (~23.6%) |
| **Avg Tenure of Churned Customers** | ~17.9 months | vs ~37.6 months for retained customers |
| **Avg Monthly Charges (Churned)** | ~$74.44 | vs ~$61.27 for retained customers |

---

## Feature Correlation with Churn

| Feature | Correlation Direction | Strength | Notes |
|---------|----------------------|----------|-------|
| `tenure` | Negative | Strong | Longer tenure = lower churn |
| `MonthlyCharges` | Positive | Moderate | Higher charges = higher churn |
| `TotalCharges` | Negative | Moderate | (proxy for tenure * charges) |
| `Contract_Month-to-month` | Positive | Strong | Biggest churn driver |
| `InternetService_Fiber optic` | Positive | Moderate | Dissatisfied despite premium |
| `OnlineSecurity_No` | Positive | Moderate | No security = higher churn |
| `TechSupport_No` | Positive | Moderate | No support = higher churn |
| `PaymentMethod_Electronic check` | Positive | Moderate | Less committed customers |
| `SeniorCitizen` | Positive | Moderate | Demographics factor |
| `Partner` | Negative | Weak | Partnered customers more stable |

---

## Churn Risk Segments

### High Risk (Churn Probability > 60%)
- Month-to-month contract + Fiber optic + Electronic check payment + No tech support
- Senior citizens with no dependents on short tenures (<12 months)
- Customers with monthly charges > $80 and no add-on services

### Medium Risk (Churn Probability 30–60%)
- Month-to-month contract with any internet service
- Non-senior customers with no online security or backup
- Customers with tenure between 12–24 months

### Low Risk (Churn Probability < 30%)
- Annual or two-year contract holders
- Customers with tenure > 36 months
- Customers with multiple add-on services (Online Security + Tech Support + Streaming)

---

## Statistical Tests Performed

| Test | Variables | Result | p-value |
|------|-----------|--------|--------|
| Chi-Square | Contract vs Churn | Significant | < 0.001 |
| Chi-Square | InternetService vs Churn | Significant | < 0.001 |
| Chi-Square | PaymentMethod vs Churn | Significant | < 0.001 |
| T-Test | Tenure (Churn vs No-Churn) | Significant | < 0.001 |
| T-Test | MonthlyCharges (Churn vs No-Churn) | Significant | < 0.001 |
| Point-Biserial | TotalCharges vs Churn | Moderate negative correlation | -0.20 |

---

## Model Performance (Logistic Regression Baseline)

| Metric | Score |
|--------|-------|
| **Accuracy** | 80.4% |
| **Precision (Churn)** | 65.8% |
| **Recall (Churn)** | 54.9% |
| **F1-Score (Churn)** | 59.9% |
| **AUC-ROC** | 0.843 |
| **Class Imbalance Handling** | class_weight='balanced' |

### Top Feature Importances (by coefficient magnitude)
1. `Contract_Two year` (-1.92) — Strongest retention signal
2. `Contract_One year` (-1.11) — Strong retention signal
3. `tenure` (-0.043 per month) — Each extra month reduces churn risk
4. `InternetService_Fiber optic` (+0.89) — Risk factor
5. `PaymentMethod_Electronic check` (+0.65) — Risk factor
6. `OnlineSecurity_No` (+0.60) — Risk factor
7. `TechSupport_No` (+0.55) — Risk factor
8. `SeniorCitizen` (+0.41) — Moderate risk

---

## Improvement Opportunities

| Area | Current Metric | Target | Recommended Action |
|------|---------------|--------|-------------------|
| Contract Migration | 42% month-to-month churn | <25% | Offer 10-20% discount for annual plan upgrades |
| Fiber Optic Retention | 41.9% churn | <30% | Investigate service quality issues; offer loyalty credits |
| Tech Support Adoption | 41.6% churn (no support) | <25% | Bundle free 3-month tech support trial on signup |
| Electronic Check Users | 45.3% churn | <30% | Incentivise auto-pay/credit card with bill credit |
| Senior Citizen Segment | 41.7% churn | <30% | Dedicated senior customer success team |
| Early Tenure Intervention | High churn in months 1-12 | Reduce by 20% | Onboarding program for new customers (<6 months) |
| Add-on Cross-sell | Low multi-service adoption | Increase by 15% | Bundle Online Security + Tech Support at signup |

---

## Business Impact Estimate

Assuming average monthly charge of $65 per churned customer:

| Improvement | Customers Saved | Monthly Revenue Protected |
|-------------|-----------------|---------------------------|
| Contract migration (10% conversion) | ~187 customers | ~$12,155/month |
| Tech support bundling (15% retention boost) | ~140 customers | ~$9,100/month |
| Electronic check intervention (10% reduction) | ~75 customers | ~$4,875/month |
| **Total estimated impact** | **~400 customers** | **~$26,000/month** |

---

## Visualisations Produced in Notebook

1. Churn distribution pie chart
2. Tenure distribution by churn label (histogram)
3. Monthly charges box plot (churn vs retained)
4. Churn rate by contract type (bar chart)
5. Churn rate by internet service type
6. Correlation heatmap of numerical features
7. Churn rate by payment method
8. Churn rate by senior citizen status
9. Feature importance bar chart (Logistic Regression)
10. ROC Curve with AUC score

---

## Files in This Repository

```
customer-churn-eda/
├── data/
│   └── telco_churn.csv              # Sample dataset
├── notebooks/
│   └── customer_churn_eda.ipynb    # Main EDA notebook
├── reports/
│   └── churn_metrics_summary.md   # This file
├── requirements.txt
└── README.md
```
