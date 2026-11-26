# Predicting Financial Distress in Vietnamese Companies with CatBoost

Code for my bachelor's thesis in Finance at the University of Economics Ho Chi Minh City (UEH), 2025:
*Applying the Categorical Boosting (CatBoost) Machine Learning Model to Predict Financial Distress in Vietnamese Companies*.

To the best of my knowledge, this was the first study in Vietnam to apply CatBoost to financial distress prediction.

## Question

1. Can CatBoost predict financial distress for Vietnamese listed companies 1, 2 and 3 years before it happens?
2. How does it compare with Logistic Regression, SVM, Random Forest and XGBoost?

## Data

- **1,455 companies** listed on HOSE, HNX and UPCoM, **2015–2023**
- **190 distressed** and **1,265 non-distressed** companies
- A company is classed as distressed if it meets at least one condition: net loss for three consecutive years, negative equity, or declared bankrupt by a court
- Financial statements collected with the open-source [vnstock](https://github.com/thinh-vu/vnstock) library
- Features: financial ratios for profitability (ROA, ROE, margins), liquidity, leverage, efficiency and growth, plus industry as a categorical variable

The raw data files are not included in this repository. See [`data/README.md`](data/README.md).

## Method

1. Compute financial ratios from balance sheet, income statement and cash flow data
2. Label each company and its event year; build three datasets for 1, 2 and 3 years before the event (t-1, t-2, t-3)
3. Cap outliers (IQR method), fill missing values with the median, drop highly correlated ratios
4. 70/30 train–test split; tune each model with Bayesian optimisation (`BayesSearchCV`, 3-fold CV, ROC-AUC)
5. Evaluate with Accuracy, Precision, Recall, F1-score (weighted averages) and AUC
6. Interpret CatBoost with Feature Importance and Partial Dependence Plots

## Results

**CatBoost performance**

| Horizon | Accuracy | Precision | Recall | F1-score | AUC |
|---|---|---|---|---|---|
| t-1 | 0.924 | 0.921 | 0.924 | 0.922 | 0.971 |
| t-2 | 0.952 | 0.952 | 0.952 | 0.952 | 0.985 |
| t-3 | 0.940 | 0.940 | 0.940 | 0.933 | 0.939 |

**AUC compared with other models**

| Horizon | CatBoost | Logistic Regression | SVM | Random Forest | XGBoost |
|---|---|---|---|---|---|
| t-1 | 0.971 | 0.973 | 0.968 | 0.975 | **0.977** |
| t-2 | 0.985 | 0.971 | 0.973 | **0.987** | 0.981 |
| t-3 | 0.939 | 0.923 | 0.916 | 0.929 | **0.947** |

**Main findings**

- CatBoost predicted distress well at all three horizons (all metrics above 0.92) and was most accurate two years before the event.
- It generally beat Logistic Regression and SVM, but Random Forest and XGBoost were slightly better at some horizons. Among tree-based ensembles, the differences were small.
- **ROA was the most important predictor at every horizon.** Net profit margin and ROE were also always in the top five. Further from the event, the quick ratio became more important, suggesting that liquidity matters for longer-term risk.

## Repository

```
financial_distress_catboost.ipynb   full pipeline: data preparation, EDA, models, evaluation
data/README.md                      expected input files and how to obtain them
requirements.txt                    Python packages
```

Comments and column names in the notebook are in Vietnamese, as in the original thesis.

## Run it

```bash
pip install -r requirements.txt
# add the two CSV files to data/ (see data/README.md)
jupyter notebook financial_distress_catboost.ipynb
```

## Author

**Nguyen Thi Bich Van** · Food technologist and Finance graduate, Ho Chi Minh City, Vietnam
