# Telecom Customer Churn Prediction
> End-to-end churn prediction pipeline built across two frameworks — 
> scikit-learn for rapid experimentation and Apache Spark (PySpark) for 
> scalable deployment. Mirrors the real-world challenge faced by mobile 
> operators managing millions of subscribers.

---

## Results

| Model | Framework | AUC | Accuracy | F1 |
|---|---|---|---|---|
| Logistic Regression | scikit-learn | 0.84 | 79.2% | 0.77 |
| Random Forest (tuned) | scikit-learn | 0.855 | 78.6% | 0.79 |
| Gradient Boosting | scikit-learn | 0.86 | 81.1% | 0.80 |
| Random Forest (cross-validated) | PySpark MLlib | 0.860 | 81.4% | 0.805 |

**Best model: Gradient Boosting / PySpark Random Forest (AUC 0.86)**  
At this performance level, a model deployed across a 5M+ subscriber base 
could identify tens of thousands of at-risk customers monthly — enabling 
targeted retention before churn occurs.

---

## Problem Statement

Telecom companies lose significant recurring revenue to churn, and acquiring 
a new customer costs 5–7x more than retaining an existing one. This project 
answers two questions:

1. Which customers are most likely to churn in the next period?
2. What are the strongest drivers — and what can the business do about them?

---

## Repository Structure
telecom-customer-churn-prediction/

├── notebooks/

│   ├── churn_sklearn.ipynb         # EDA, feature engineering, 3-model comparison

│   └── churn_pyspark_pipeline.ipynb  # Scalable Spark ML pipeline

├── models/

│   ├── gradient_boosting.pkl

│   ├── random_forest.pkl

│   ├── logistic_regression.pkl

│   └── imputer.pkl

└── README.md


---

## Approach

### 1. Exploratory Data Analysis
- Class distribution analysis (26.5% churn rate — imbalanced dataset)
- Correlation matrix across 21 features
- Churn rate segmented by contract type, tenure band, internet service, 
  and monthly charges
- Key insight: month-to-month customers churn at 3x the rate of 
  two-year contract holders

### 2. Feature Engineering & Preprocessing
- Converted TotalCharges from string to numeric (11 blank entries imputed)
- One-hot encoded categorical variables (contract, payment method, 
  internet service)
- StandardScaler applied to continuous features
- Class imbalance handled via `class_weight='balanced'` — preserves all 
  7,043 rows rather than discarding majority-class data

### 3. Model Training & Selection (scikit-learn)
Three models trained with stratified train/test split (70/30):
- **Logistic Regression** — interpretable baseline
- **Random Forest** — GridSearchCV hyperparameter tuning (5-fold CV)
- **Gradient Boosting** — best overall AUC and F1

### 4. Scalable Pipeline (PySpark MLlib)
Rebuilt the full pipeline in Apache Spark:
- Spark DataFrame ingestion and cleaning
- ML Pipeline: `StringIndexer → OneHotEncoder → VectorAssembler → 
  StandardScaler → Classifier`
- `CrossValidator` for hyperparameter tuning
- Designed to run unchanged on a cluster with 100M+ rows

---

## Key Findings

**Contract type is the single strongest predictor of churn.**  
Month-to-month customers churn at significantly higher rates than annual 
or two-year subscribers — contract migration is the highest-leverage 
retention action available.

**Churn is concentrated in the first 0–7 months.**  
Customers who survive past 12 months are far more likely to stay long-term. 
Early onboarding experience is critical.

**High monthly charges + low tenure = highest risk segment.**  
These customers are paying premium rates before they've built loyalty. 
They need proactive value reinforcement, not just reactive win-back.

**Fiber optic customers churn more than DSL customers.**  
Likely driven by price sensitivity or unmet service quality expectations — 
worth a dedicated root cause analysis.

---

## Business Recommendations

| Priority | Action | Target Segment |
|---|---|---|
| High | Offer contract upgrade incentives | Month-to-month, high churn probability |
| High | Proactive onboarding outreach | Tenure < 3 months, high monthly charges |
| Medium | Investigate Fiber Optic churn drivers | Fiber optic subscribers |
| Medium | Deploy monthly scoring pipeline | Full subscriber base |
| Low | A/B test retention campaigns | High-risk treatment vs control group |

---

## Limitations & Future Work

- **No behavioural data** — call centre interactions, complaints, and 
  network quality metrics would meaningfully improve accuracy
- **Static snapshot** — survival analysis would predict *when* churn 
  occurs, not just whether it will
- **Threshold not optimised** — production deployment should tune the 
  classification threshold against actual retention costs and CLV
- **No drift monitoring** — model performance should be tracked over time 
  as customer behaviour evolves

---

## Tools & Stack

| Category | Tools |
|---|---|
| Languages | Python |
| ML | scikit-learn, PySpark MLlib |
| Data | pandas, NumPy |
| Visualisation | Matplotlib, Seaborn |
| Scalability | Apache Spark 3.x |
| Environment | Google Colab |

---

## Dataset

IBM Telco Customer Churn — 7,043 customers, 21 features including 
demographics, services subscribed, contract type, and billing information.  
Source: [Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)
