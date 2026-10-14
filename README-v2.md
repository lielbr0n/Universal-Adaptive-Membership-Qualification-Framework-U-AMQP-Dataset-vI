# Universal Adaptive Membership Qualification Framework (U-AMQP)

This repository contains the official benchmark dataset, mathematical framework specifications, evaluation scripts, and manuscript draft for the Universal Adaptive Membership Qualification Framework (U-AMQP).

U-AMQP is a formal, four-engine computational architecture designed to modernize Member-Consumer-Owner (MCO) onboarding, identity verification, competency diagnostics, qualification gating, and pre-energization risk management for electric cooperatives (calibrated against BUSECO parameters).

---

## Repository Layout

- `README.md`: Repository documentation & variable dictionary
- `LICENSE`: MIT Open Source License
- `.gitignore`: Git exclusion rules for Python & IDEs
- `u_amqp_thesis_framework.docx`: Complete Thesis Manuscript (Chapters 1, 2, and 3)
- `u_amqp_thesis_dataset.csv`: Benchmark Microdata (1,000 synthetic records, 34 features)
- `u_amqp_thesis_dataset.xlsx`: Interactive Dashboard & Multi-Tab Dataset
- `engine4_model_evaluation.py`: Model training & evaluation script for Engine 4
- `engine4_roc_curve.png`: Receiver Operating Characteristic (ROC) curve plot
- `engine4_confusion_matrix.png`: Delinquency classification confusion matrix
- `engine4_probability_distribution.png`: Predicted default probability density plot

---

## Dataset Overview

The benchmark dataset consists of 1,000 synthetic applicant records calibrated against operational demographics of Bukidnon II Electric Cooperative, Inc. (BUSECO) across its Manolo Fortich and Malaybalay City branches. Each record tracks a candidate through all 34 variables across the 4-engine pipeline.

### Model Performance Summary (Engine 4 Predictive Utility Analytics)

| Metric | Fitted Logistic Regression Model | Analytical Closed-Form Formula |
| :--- | :---: | :---: |
| ROC-AUC | 0.7535 | 0.7527 |
| Accuracy | 68.80% | 68.30% |
| Precision | 69.47% | 71.76% |
| Recall | 67.53% | 60.76% |
| F1-Score | 0.6848 | 0.6580 |
| Brier Score | 0.2016 | 0.2042 |

---

## Variable Dictionary (34 Columns)

### 1. Demographic & Spatial Features
- `Applicant_ID`: Unique applicant tracking identifier (`APP-2026-0001` to `APP-2026-1000`).
- `Full_Name`: Synthetic applicant full name.
- `DOB`: Date of birth (`YYYY-MM-DD`).
- `Branch`: Electric cooperative service branch (`Manolo Fortich` / `Malaybalay City`).
- `Barangay`: Local administrative division / neighborhood.
- `Address`: Full residential address string.
- `Geo_Tier`: Infrastructure remoteness tier (`1 = Urban/Poblacion`, `2 = Semi-rural`, `3 = Remote Highland/GIDA`).
- `Household_Size`: Occupancy headcount (`1` to `9+`).
- `Employment_Income_Type`: Income regularity status (`Salaried/Regular` vs `Informal/Seasonal`).

### 2. Engine 1: Verification Engine (Entity Resolution)
- `E1_Matched_Member_ID`: Matched existing member account ID or `N/A`.
- `E1_Sim_Name`: Jaro-Winkler string similarity score on full name (`0.0` to `1.0`).
- `E1_I_DOB`: Binary date-of-birth exact match indicator (`1.0` or `0.0`).
- `E1_Sim_Address`: Normalized Levenshtein address similarity score (`0.0` to `1.0`).
- `E1_Composite_Score`: Weighted linkage score (`0.50*Sim_Name + 0.30*I_DOB + 0.20*Sim_Address`).
- `E1_Duplicate_Flag`: Duplicate account hold trigger (`True` if Composite Score >= 0.85).
- `Identity_Confidence_Ic`: Identity validation score passed to Engine 3 (`1.0 - Composite_Score`).

### 3. Engine 2: Assessment Engine (Adaptive PMES & Micro-Remediation)
- `E2_S1_Principles`: Domain 1 score (Cooperative Principles & History).
- `E2_S2_Rights`: Domain 2 score (Member Rights & Responsibilities).
- `E2_S3_Standards`: Domain 3 score (Electric Service Standards & Safety).
- `E2_S4_Billing_Initial`: Domain 4 score (Power Billing & Tariffs - Pre-remediation).
- `E2_S5_Governance`: Domain 5 score (Cooperative Governance & Democratic Assemblies).
- `E2_Initial_Status`: Diagnostic outcome (`PASSED_DIRECTLY` vs `REMEDIATION_REQUIRED`).
- `E2_S4_Billing_PostRemediation`: Domain 4 score after targeted micro-remediation.
- `E2_Mean_Mastery_Pc`: Mean competency mastery across all 5 cognitive domains.

### 4. Engine 3: Analysis Engine (MAUT & State Control)
- `Doc_Completeness_Ac`: Document completeness ratio (`N_submitted / N_required`).
- `MAUT_Qualification_Score_QS`: Multi-Attribute Utility score (`0.40*Ic + 0.30*Pc + 0.30*Ac`).
- `FSM_Subgates_Passed`: Boolean check for sub-gates (`Ic >= 0.80` and `Pc >= 0.80` and `Ac = 1.00`).
- `FSM_Final_State`: Terminal state assigned by Deterministic FSM (`QUALIFIED` vs `MANUAL_REVIEW`).

### 5. Engine 4: Recommendation Engine (Predictive Analytics)
- `Engine4_X_Billing`: Normalized PMES Domain 4 tariff comprehension input.
- `Engine4_Logit_Z`: Log-odds default score (`z = 0.45 - 1.85*x_billing + 0.65*x_geo + 0.12*x_hh_size - 1.20*x_income`).
- `Default_Probability_Pd`: Predicted delinquency probability via Sigmoid activation `1 / (1 + exp(-z))`.
- `Risk_Classification`: Pre-energization risk classification (`LOW`, `MEDIUM`, `HIGH`).
- `Prescriptive_Strategy`: Recommended credit mitigation strategy (e.g., daily SMS alerts, flexible weekly billing).
- `Actual_12Mo_Delinquent`: Retrospective 12-month delinquency outcome (`1 = Delinquent`, `0 = Current`).

---

## Quickstart & Model Evaluation

### Requirements
Ensure Python 3.10+ and required packages are installed (pandas, numpy, scikit-learn, matplotlib, seaborn, openpyxl).

### Running Model Evaluation Script
To reproduce the ROC curve, confusion matrix, and performance metrics for Engine 4:
```bash
python engine4_model_evaluation.py
```

### Loading the Dataset in Python
```python
import pandas as pd

# Load dataset
df = pd.read_csv('u_amqp_thesis_dataset.csv')

# Inspect dataset shape and qualification distribution
print(f"Dataset shape: {df.shape}")
print(df['FSM_Final_State'].value_counts())
print(df['Risk_Classification'].value_counts())
```

---

## Synthetic Data & Privacy Statement
This dataset is 100% synthetically generated for research, benchmarking, and framework evaluation purposes. It contains zero authentic Personally Identifiable Information (PII) or confidential cooperative member data.

---

## License
This project is licensed under the MIT License - see the LICENSE file for details.
