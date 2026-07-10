# Customer-Churn-Prediction-Revenue-Impact-Dashboard
An end-to-end data analytics and machine learning project that predicts which telecom customers are likely to churn, quantifies the revenue at risk, and delivers an interactive Power BI dashboard that lets stakeholders simulate retention ROI in real time.

---

## 🧩 Problem Statement

A telecom company is losing customers silently — by the time they cancel, it's too late. The business needs to identify *who* is going to leave *before* they actually do, understand *why* they churn, and estimate *how much revenue* is at stake.

This project solves all three.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Python** | EDA, feature engineering, ML modelling |
| **XGBoost** | Churn classification model |
| **SHAP** | Model explainability — why each customer churns |
| **Pandas / Seaborn / Matplotlib** | Data manipulation and EDA visualisation |
| **Power BI** | Interactive business dashboard |
| **DAX** | Custom KPI measures and what-if analysis |

---

## 🔄 End-to-End Pipeline

```
Raw CSV → Data Cleaning → EDA → Feature Engineering → XGBoost Model
→ SHAP Explainability → Export Predictions → Power BI Dashboard
```

---

## 📊 Dataset

**Telco Customer Churn** — IBM Sample Dataset via Kaggle  
7,043 customers | 21 features | Binary target: Churn (Yes/No)  
[Download here](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

> Business questions, feature engineering, DAX measures, and insights are fully original.

---

## 🧹 Data Cleaning & Preprocessing

- Converted `TotalCharges` from string to numeric (contained whitespace values)
- Dropped `customerID` — unique identifier with no predictive value
- Mapped `Churn` from Yes/No to 1/0 for binary classification
- One-hot encoded all categorical features using `pd.get_dummies(drop_first=True)` to avoid the dummy variable trap
- Applied `StandardScaler` to normalise feature scales — fit on train only to prevent data leakage

---

## 🔍 EDA — Key Findings

| Finding | Insight |
|---|---|
| 26% churn rate | 1 in 4 customers leaves — significant business problem |
| Median tenure: churners = 10 months, loyal = 38 months | New customers are the highest risk group |
| Churners pay higher monthly charges | Expensive plans drive dissatisfaction |

---

## 🤖 ML Model — XGBoost Classifier

```python
XGBClassifier(
    n_estimators=200,
    max_depth=4,
    learning_rate=0.05,
    scale_pos_weight=3,  # handles class imbalance (3:1 ratio)
    eval_metric='logloss',
    random_state=42
)
```

### Results

| Metric | Score |
|---|---|
| ROC-AUC | **0.8381** |
| Recall (Churners) | **0.81** |
| Precision (Churners) | 0.49 |
| Overall Accuracy | 0.72 |

**Recall of 0.81 means the model catches 8 out of every 10 real churners** — the metric that matters most for a retention campaign, where missing a churner is far more costly than a false alarm.

---

## 🔬 SHAP — Model Explainability
<img width="1420" height="977" alt="shap_importance" src="https://github.com/user-attachments/assets/143523df-4f2d-435d-8af9-328794f90478" />

![SHAP Beeswarm Plot]()

SHAP (SHapley Additive exPlanations) reveals exactly which features drive churn predictions:

| Feature | Impact |
|---|---|
| Contract_Two year | Strongest churn protector — 2yr customers almost never leave |
| tenure | Short tenure = highest churn risk |
| InternetService_Fiber optic | Fiber customers churn significantly more |
| MonthlyCharges | Higher bills = higher churn probability |
| PaymentMethod_Electronic check | Electronic check users churn more than auto-pay customers |
| OnlineSecurity_Yes | Having security add-ons reduces churn |

**Business strategy derived from SHAP:** Push new fiber optic customers toward annual contracts and bundle security add-ons early — these are the two highest-impact retention levers.

---

## 📈 Power BI Dashboard

![Dashboard](<img width="1133" height="798" alt="image" src="https://github.com/user-attachments/assets/6dcce4a4-787c-4adf-8f40-588e25c6b820" />
)

### KPI Cards
- Total Customers | Predicted Churners | Churn Rate % | Revenue At Risk | Avg Churn Probability

### Visuals
- **Churn Rate by Contract Type** — Month-to-month at 74%, Two year at 1%
- **Predicted Churners by Internet Service** — Fiber optic: 441, DSL: 147, No internet: 32
- **Customer Churn Risk: Probability vs Monthly Spend** — scatter plot colored by risk tier (High/Medium/Low), sized by tenure
- **What-If Retention Slicer** — move the slider to any retention %, card updates projected revenue recovered in real time

### DAX Measures
```dax
Churn Rate % = DIVIDE([Churners Predicted], [Total Customers])

Revenue At Risk = SUM(churn_predictions[monthly_revenue_at_risk])

Revenue Recovered =
[Revenue At Risk] * DIVIDE('Retention Rate'[Retention Rate Value], 100)
```

---

## 💡 Key Business Insights

1. **Contract type is the single biggest churn driver** — month-to-month customers churn at 74% vs 1% for two-year contracts. Converting even 10% of month-to-month customers to annual plans dramatically reduces churn.

2. **Fiber optic is a retention problem, not a product problem** — 441 predicted churners use fiber optic vs 147 on DSL. The issue is likely pricing perception, not service quality.

3. **Revenue at risk is concentrated in new, high-paying customers** — the scatter plot shows high churn probability clusters around high monthly charges and low tenure. These customers generate the most revenue and are the most likely to leave.

4. **Retention ROI is quantifiable** — at 20% retention of high-risk customers, the business recovers ~$9K in monthly recurring revenue.

---

## 📁 Repository Structure

```
├── churn_prediction.ipynb     # Full Python notebook — EDA, modelling, SHAP
├── churn_predictions.csv      # Model output — predictions + risk tiers
├── churn_dashboard.pbix       # Power BI dashboard file
├── shap_importance.png        # SHAP beeswarm plot
├── dashboard.png              # Dashboard screenshot
└── README.md
```

---

## 🚀 How to Run

1. Download the dataset from Kaggle (link above)
2. Run `churn_prediction.ipynb` top to bottom — outputs `churn_predictions.csv`
3. Open `churn_dashboard.pbix` in Power BI Desktop
4. Update the data source path to your local `churn_predictions.csv`
5. Refresh — all visuals update automatically

---

## 👤 Author

**Nikhil Solanki**  

