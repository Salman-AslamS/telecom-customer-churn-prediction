# Telecom Customer Churn Prediction

Predicting customer churn for a telecom operator using end-to-end 
machine learning pipelines in both scikit-learn and Apache Spark (PySpark).

## Results

| Model | Framework | AUC | Accuracy |
|---|---|---|---|
| Random Forest (tuned) | scikit-learn | 0.855 | 78.6% |
| Random Forest (cross-validated) | PySpark MLlib | 0.860 | 81.4% |

## Problem Statement

Customer churn is a major revenue risk for telecom companies — 
acquiring new customers costs significantly more than retaining 
existing ones. This project identifies customers at high risk of 
churning and quantifies the key drivers, to support proactive 
retention strategies.

## Dataset

IBM Telco Customer Churn dataset (7,043 customers, 21 features).  
Source: [Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

## Approach

### scikit-learn pipeline (`notebooks/churn_sklearn.ipynb`)
- Exploratory data analysis and correlation analysis
- Feature engineering and preprocessing
- Random Forest with GridSearchCV hyperparameter tuning
- Class imbalance handled via undersampling
- Model serialised with joblib

### PySpark pipeline (`notebooks/churn_pyspark_pipeline.ipynb`)
- Distributed data loading and cleaning with Spark DataFrames
- ML Pipeline with StringIndexer, OneHotEncoder, VectorAssembler, StandardScaler
- Logistic Regression baseline vs Random Forest with CrossValidator
- Designed to scale to datasets far larger than 7k rows

## Key Findings

- **Contract type** is the strongest churn predictor — month-to-month 
  customers churn at significantly higher rates than annual subscribers
- **Tenure** is highly predictive — churn risk is highest in the first 
  0–7 months and declines sharply after that
- **Monthly charges** combined with low tenure indicate the highest-risk 
  customer segment

## Business Recommendations

- Target month-to-month customers with contract upgrade incentives
- Deploy retention interventions within the first 3 months of service
- Prioritise high-charge, low-tenure customers for outreach campaigns
- Integrate model scores into a monthly monitoring pipeline
- Measure campaign effectiveness via A/B testing

## Tools

- Python, pandas, scikit-learn, joblib
- Apache Spark (PySpark MLlib)
- Matplotlib, seaborn
- Google Colab
