# Credit Card Fraud Detection as a Risk Prioritization System

## Overview
This project reframes credit card fraud detection from a binary classification task into a **risk prioritization and decision support system**.  
Rather than optimizing accuracy, the objective is to **minimize expected financial loss** while accounting for operational review capacity and customer friction.

Using a highly imbalanced real-world dataset (0.172% fraud rate), the system:
- produces calibrated fraud risk scores,
- converts scores into **Allow / Flag / Block** decisions,
- and evaluates policies using **cost-sensitive expected loss**, not accuracy.

---

## Dataset
**Source:** European cardholders, September 2013  
**Size:** 284,807 transactions  
**Fraud cases:** 492 (0.172%)

### Features
- `V1`–`V28`: PCA-transformed latent behavioral components  
- `Time`: seconds elapsed since the first transaction  
- `Amount`: transaction value (used for cost-sensitive learning)  
- `Class`: target label (1 = fraud, 0 = non-fraud)

> Due to confidentiality constraints, original features are not available.  
> Interpretability is therefore limited to *latent components* rather than human-readable behaviors.

---

## Project Structure
├── Fraud1.ipynb # EDA & decision framing

├── Fraud2.ipynb # Baseline logistic regression

├── Fraud3.ipynb # XGBoost risk model

├── Fraud4.ipynb # Threshold optimization & decision policy

├── Fraud5.ipynb # Explainability (SHAP) & governance

├── xgb_test_scoring.csv

├── decision_policy_output.csv

└── creditcard.csv


---

## Methodology Summary

### 1. Exploratory Analysis
- Quantified extreme class imbalance
- Analyzed transaction amounts and time dynamics
- Framed fraud detection as a **decision optimization problem**

### 2. Baseline Model
- Logistic regression with class weighting
- Established probability baseline
- Evaluated using **PR-AUC (AUPRC)**

### 3. Tree-Based Risk Model
- XGBoost with `scale_pos_weight`
- Optimized for PR-AUC
- Outputs probabilistic fraud risk scores

### 4. Decision Policy Optimization
- Explicit cost model:
  - False Negative cost ≈ missed fraud amount
  - False Positive cost ≈ operational & customer friction
- Thresholds selected to **minimize expected loss**
- Tiered actions:
  - **Allow**: low risk
  - **Flag**: review required
  - **Block**: extreme risk tail

### 5. Explainability & Governance
- SHAP used for global feature influence
- Interpretability constrained by PCA features
- Monitoring framework proposed for production use

---

## Key Insights
- Accuracy and confusion matrices are misleading under extreme imbalance
- Optimal thresholds are **business decisions**, not model outputs
- Cost-sensitive evaluation materially changes model deployment behavior
- Fraud detection systems should be designed as **risk pipelines**, not classifiers

---

## Tools & Libraries
- Python, Pandas, NumPy
- scikit-learn
- XGBoost
- SHAP
- Matplotlib

---

## Future Work
- Capacity-constrained optimization
- Probability calibration (Platt / Isotonic)
- Time-aware backtesting
- Drift detection and retraining triggers
- Human-in-the-loop feedback loops

---

## Disclaimer
This project is for educational and analytical purposes only.  
It does not constitute a production fraud detection sys
