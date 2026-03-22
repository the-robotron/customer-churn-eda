# Customer Churn EDA

![Python](https://img.shields.io/badge/Python-3.10-blue) ![Pandas](https://img.shields.io/badge/Pandas-2.0-green) ![Seaborn](https://img.shields.io/badge/Seaborn-0.12-orange) ![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## Overview

This project performs an in-depth **Exploratory Data Analysis (EDA)** on a telecom company's customer dataset to understand the factors driving customer churn. Using Python and data visualisation libraries, the notebook uncovers patterns and relationships that help predict which customers are likely to leave.

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

### 2. Data Cleaning
```python
# Convert TotalCharges to numeric
df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce')
df.dropna(inplace=True)

# Encode binary target
df['Churn'] = df['Churn'].map({'Yes': 1, 'No': 0})
```

### 3. Univariate Analysis
```python
# Churn distribution
sns.countplot(x='Churn', data=df, palette='Set2')
plt.title('Churn Distribution')
plt.show()
# ~26.5% of customers churned
```

### 4. Bivariate Analysis
```python
# Churn rate by Contract type
sns.barplot(x='Contract', y='Churn', data=df, palette='muted')
plt.title('Churn Rate by Contract Type')
plt.show()
```

### 5. Correlation Heatmap
```python
numeric_df = df.select_dtypes(include='number')
sns.heatmap(numeric_df.corr(), annot=True, cmap='coolwarm', fmt='.2f')
plt.title('Correlation Matrix')
plt.show()
```

## Key Insights

| Insight | Finding |
|---|---|
| Contract type | Month-to-month customers churn at ~42% vs ~11% for annual contracts |
| Tenure | Customers with < 12 months tenure are 3x more likely to churn |
| Internet service | Fiber optic users have the highest churn rate (~41%) |
| Payment method | Electronic check users show the highest churn (~45%) |
| Senior citizens | Senior customers churn at ~41% vs ~26% overall |

## Visualisations

- Churn distribution bar chart
- Tenure vs Churn KDE plot
- Monthly charges distribution by churn
- Heatmap of feature correlations
- Churn rate by internet service type
- Stacked bar charts for categorical features

## Tools & Libraries

| Tool | Purpose |
|---|---|
| Python 3.10 | Core programming language |
| Pandas | Data manipulation and cleaning |
| Matplotlib | Base visualisation |
| Seaborn | Statistical visualisation |
| Jupyter Notebook | Interactive analysis environment |

## Project Structure

```
customer-churn-eda/
├── data/
│   └── telco_churn.csv
├── notebooks/
│   └── customer_churn_eda.ipynb
├── images/
│   ├── churn_distribution.png
│   ├── correlation_heatmap.png
│   └── tenure_vs_churn.png
└── README.md
```

## How to Run

```bash
git clone https://github.com/the-robotron/customer-churn-eda.git
cd customer-churn-eda
pip install pandas matplotlib seaborn jupyter
jupyter notebook notebooks/customer_churn_eda.ipynb
```

## Business Recommendations

1. **Offer long-term contract incentives** to convert month-to-month customers
2. **Target senior citizens** with loyalty rewards to improve retention
3. **Investigate fiber optic service quality** to reduce churn in that segment
4. **Promote auto-pay options** as alternatives to electronic checks

## Author

**Shivam Singh** — [GitHub](https://github.com/the-robotron) | [LinkedIn](https://linkedin.com/in/shivam-singh)
