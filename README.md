# 🚨 Fraud Detection Project

A **Data Science project** focused on analyzing transaction data to detect fraudulent activity. The notebook demonstrates **data exploration, feature engineering, model training, and interpretability** to guide effective fraud prevention strategies.

---

## 📂 Project Structure

* 📒 Notebook: `Fraud_Detection_Project (2).ipynb`
* 📊 Data: `Fraud.csv`

---

## 📑 Dataset Overview

Each row represents a transaction with the following features:

* **step**: Time step (simulation hours)
* **type**: Transaction type *(TRANSFER, CASH\_OUT, PAYMENT, DEBIT, etc.)*
* **amount**: Transaction amount
* **nameOrig**: Origin account ID *(anonymized)*
* **oldbalanceOrg / newbalanceOrg**: Origin account balances *(before/after)*
* **nameDest**: Destination account ID *(anonymized)*
* **oldbalanceDest / newbalanceDest**: Destination account balances *(before/after)*
* **isFraud**: Target label *(1 = fraud, 0 = non-fraud)*
* **isFlaggedFraud**: Rule-based flag *(1 = flagged, 0 = not flagged)*

🔎 **Notes**:

* No missing values detected ✅
* Outliers retained (important for fraud detection)

---

## 🎯 Problem Statement

Detect fraudulent transactions and identify **key risk drivers** to support prevention and investigation workflows.

---

## ⚙️ Data Preprocessing

* ✅ Missing values check → None found
* 📌 Outliers → Retained (domain-relevant)
* 🔄 Multicollinearity → Assessed using VIF; balance-related features showed expected correlation
* 🔠 Categorical handling → One-hot encoding of `type`
* 🎯 Targets → `isFraud` for modeling; `isFlaggedFraud` for reference

---

## 🛠️ Feature Engineering

**Time Features (Cyclical):**

* ⏰ `hour_sin`, `hour_cos`
* 📅 `day_of_week_sin`, `day_of_week_cos`

**Balance Change Features:**

* `orig_balance_change`, `dest_balance_change`
* `abs_orig_balance_change`, `abs_dest_balance_change`

**Interaction Features:**

* `amount_x_orig_balance_change`
* `amount_x_dest_balance_change`

✨ *Rationale:* Fraud often involves large outflows from the origin and suspicious inflows at the destination, especially during certain times and transaction types.

---

## 🤖 Modeling

* **Encoding:** One-hot for categorical `type`
* **Data Split:** 75/25 with stratification

**Trained Models:**

* 🔹 Logistic Regression *(with balanced class weights)*
* 🌲 Random Forest *(n\_estimators=50, balanced class weights)*
* ⚡ XGBoost *(scale\_pos\_weight, learning rate=0.1, max\_depth=5, n\_estimators=100)*

**Class Imbalance Handling:**

* Logistic Regression & Random Forest → `class_weight='balanced'`
* XGBoost → `scale_pos_weight = neg/pos`

---

## 📈 Evaluation

* Metrics: **Accuracy, Precision, Recall, F1, AUC, Confusion Matrix**

**XGBoost (Best Performing Model):**

* ✅ Accuracy: **99.31%**
* 🎯 Precision: **15.7%**
* 🔍 Recall: **99.7%**
* ⚖️ F1 Score: **27.1%**
* 🚀 AUC: **0.9998**

📊 Confusion Matrix:

```
[[1577626, 10976],
 [6, 2047]]
```

---

## 🔑 Key Findings

* 💸 Large negative **orig\_balance\_change** & high **amount** strongly signal fraud
* 🔄 **TRANSFER** & **CASH\_OUT** → higher fraud risk
* 🛡️ **PAYMENT** & **DEBIT** → lower fraud risk
* ⏰ Time-based patterns help detect suspicious activity
* 📌 Destination balance changes reveal fraudulent fund receivers

---

## 🛡️ Fraud Prevention Insights

**Rule-Based Suggestions:**

* 🚩 Flag high-value TRANSFER/CASH\_OUT with sharp balance drops
* 🚩 Prioritize alerts on repeat suspicious destinations
* ⏰ Use time-based rules for fraud-prone hours/days

**Model-Assisted Suggestions:**

* ⚖️ Adjust thresholds to balance false positives & detection rate
* 🔄 Regular re-training to handle evolving fraud tactics

---

## 💻 How to Run

1. **Environment:** Python 3.9+

2. **Install dependencies:**

   ```bash
   pip install jupyter pandas numpy scikit-learn matplotlib seaborn imbalanced-learn xgboost shap
   ```

3. **Launch Notebook:**

   ```bash
   jupyter notebook
   ```

   Open `Fraud_Detection_Project (2).ipynb`

4. **Dataset:** Place `Fraud.csv` in the project folder

---

## 📌 Next Steps

* Confirm model hyperparameters & evaluation
* Fine-tune thresholds against fraud risk capacity
* Package pipeline for production deployment

---

✨ *This project highlights the practical application of Data Science & Machine Learning in real-world fraud detection and prevention.*
