# behavioral-fraud-detection
An end-to-end fraud detection system built from fragmented transaction data using behavioral feature engineering, anomaly detection, velocity-based rules, and supervised machine learning models.
# Fraud Detection System (End-to-End)

This project builds a complete fraud detection system from fragmented real-world data.  
The workflow combines *behavioral feature engineering, unsupervised anomaly detection, velocity-based fraud logic, and supervised machine learning models.

The objective is not only to predict fraud, but to *explain why a transaction is flagged*.



##  Data Sources

The dataset was originally split into two independent files:

cc_info.csv
  Card-level and customer information (location, credit limit)

- transactions.csv  
  Transaction-level data (amount, time, coordinates)

Both datasets were merged using a shared credit card identifier.



##  Pipeline Overview

### 1. Data Loading & Merge
- Load raw CSV files
- Inspect schema, shape, and missing values
- Merge transaction and card information into a single table

📁 notebooks/01_data_loading_and_merge.ipynb



### 2. Time-Based & Behavioral Feature Engineering
- Convert transaction date to datetime
- Extract:
  - transaction hour
  - weekend indicator
- Log-transform transaction amount to reduce skew

📁 notebooks/02_time_and_behavioral_features.ipynb



### 3. Unsupervised Anomaly Detection
- Scale numerical features
- Train an *Isolation Forest*
- Flag anomalous transactions as suspicious, not fraud

📁 notebooks/03_anomaly_and_velocity_detection.ipynb



### 4. Velocity Fraud Detection
- Sort transactions per card and time
- Compute:
  - time between transactions
  - geographic distance (Haversine)
  - travel speed (km/h)
- Flag *physically impossible movement* (> 900 km/h)

📁 notebooks/03_anomaly_and_velocity_detection.ipynb



### 5. Fraud Label Engineering & Explainability
- Combine:
  - ML anomaly signal
  - velocity fraud signal
- Create a unified fraud label
- Attach human-readable fraud reasons:
  - ML anomaly
  - Impossible travel
  - ML anomaly + impossible travel

📁 notebooks/04_label_engineering_and_explainability.ipynb



### 6. Supervised Learning
Two supervised models were trained:

#### Logistic Regression (Baseline)
- Balanced class weights
- Probability-based threshold tuning
- Precision–recall tradeoff analysis

#### Random Forest Classifier
- 200 trees
- Regularized splits and leaf size
- Class imbalance handling
- ROC–AUC ≈ *0.88*
- Fraud recall ≈ *82%*

## Evaluation 

Fraud detection is not accuracy-driven.

Metrics emphasized:
- Recall (fraud detection rate)
- Precision (false alert control)
- ROC–AUC (separation power)

Thresholds were tuned from a *business-risk perspective*, not blindly fixed.

## Key Takeaways

- Fraud detection requires *behavior modeling*, not raw prediction
- Time, velocity, and anomaly signals are critical
- Explainability is essential for trust and deployment
- Ensemble models improve non-linear pattern capture



## Future Improvements
- Gradient Boosting / XGBoost
- Precision–Recall curves
- Feature importance analysis
- Cost-sensitive optimization
- Model monitoring and drift detection

requirements.txt

pandas
numpy
scikit-learn
matplotlib
seaborn
