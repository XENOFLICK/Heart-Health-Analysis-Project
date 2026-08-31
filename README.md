Pulse of Prevention: Analyzing Heart Health for Better Outcomes ❤️

An exploratory data analysis (EDA) project conducted for HealthPulse Analytics, aimed at identifying the key clinical and demographic factors that drive heart disease risk, profiling high-risk patients, and delivering actionable recommendations for preventive care.

📋 Table of Contents
Project Overview
Business Problem
Dataset
Requirements & Tools
Project Structure
Methodology
Key Insights
Challenges Faced
Recommendations for Improvement
How to Run
Project Overview

Acting as a data analyst at HealthPulse Analytics, this project analyzes a cardiology dataset of 1,025 patients to uncover the demographic and clinical factors most strongly associated with heart disease. The analysis progresses from basic descriptive statistics through medium-level bivariate comparisons to advanced multivariate analysis and predictive modeling, culminating in a set of preventive-care recommendations for healthcare providers.

Business Problem

Primary Challenge: Identify the key factors contributing to heart disease and build a profile of high-risk patients, so HealthPulse Analytics can support earlier intervention and better treatment planning.

Stakeholders:

Internal	External
Management team	Patients
Healthcare providers	Cardiology research institute
Data analysts	Healthcare policymakers

Goals:

Understand which demographic, clinical, and lifestyle factors are most linked to heart disease.
Develop a profile of high-risk patients.
Recommend preventive measures and lifestyle interventions.
Surface areas warranting further medical research.
Dataset
Source: Heart Disease Dataset — Kaggle
Size: 1,025 rows × 14 columns
Target variable: target (1 = heart disease present, 0 = absent)
Column	Description
age	Age of the patient
sex	1 = male, 0 = female
cp	Chest pain type (0–3)
trestbps	Resting blood pressure (mm Hg)
chol	Serum cholesterol (mg/dl)
fbs	Fasting blood sugar > 120 mg/dl (1 = true, 0 = false)
restecg	Resting electrocardiographic results (0–2)
thalach	Maximum heart rate achieved
exang	Exercise-induced angina (1 = yes, 0 = no)
oldpeak	ST depression induced by exercise relative to rest
slope	Slope of the peak exercise ST segment (0–2)
ca	Number of major vessels (0–4) colored by fluoroscopy
thal	Thalassemia (1 = normal, 2 = fixed defect, 3 = reversible defect)
target	Diagnosis of heart disease (1 = yes, 0 = no)
Requirements & Tools

Language: Python 3

Core libraries:

pandas, numpy — data manipulation and numerical computation
matplotlib, seaborn — data visualization
scikit-learn — feature scaling (StandardScaler), train/test split, and LogisticRegression predictive modeling
scipy.stats — statistical hypothesis testing (independent t-test)

Environment: Jupyter Notebook (Heart_Health_Analysis.ipynb)

Project Structure
├── EDA03_-_Heart_Health_Analysis.docx   # Case study brief: problem statement, data dictionary, guided questions
├── Heart_Health_Analysis.ipynb          # Full analysis: cleaning, EDA, statistics, modeling
└── README.md                            # This file
Methodology

The notebook follows a structured, three-tier analytical flow:

1. Data Cleaning & Preprocessing

Checked for missing values (none found — the dataset had complete integrity).
Converted categorical columns to int64 and continuous columns to float64 to prevent type distortion.
Detected outliers in trestbps, chol, thalach, and oldpeak using the IQR (1.5×) method and capped them within statistical fences.
Normalized continuous clinical measurements using StandardScaler.

2. Basic-Level Analysis — descriptive statistics on demographics and individual clinical measures (age, gender split, blood pressure, cholesterol, chest pain types, fasting blood sugar, etc.).

3. Medium-Level Analysis — bivariate relationships and cross-tabulations (e.g., chest pain by age group, blood pressure by gender with a t-test, fasting blood sugar vs. heart disease, vessel count vs. heart disease).

4. Advanced-Level Analysis — multivariate exploration (pair plots across age/cholesterol/blood pressure by target), full correlation ranking against the target, and a logistic regression model to predict heart disease presence from all available features.

Key Insights

Patient profile

Average patient age: 54.4 years; cohort skews male (69.6% male vs. 30.4% female).
Average resting blood pressure: 131.3 mm Hg; average cholesterol: 245.0 mg/dl — both above commonly cited healthy thresholds.
33.7% of patients experience exercise-induced angina; 14.9% have fasting blood sugar > 120 mg/dl.

Strongest predictors of heart disease (from full correlation ranking against target):

Positive: chest pain type (cp, +0.43), maximum heart rate achieved (thalach, +0.42), ST slope (slope, +0.35)
Negative: ST depression (oldpeak, −0.44), exercise-induced angina (exang, −0.44), number of major vessels (ca, −0.38), thalassemia type (thal, −0.34)

Notable relationships

Patients without exercise-induced angina reach a meaningfully higher average max heart rate (155.4 bpm) than those with it (136.8 bpm).
Resting blood pressure differs significantly by gender (female avg. 133.0 vs. male avg. 130.5 mm Hg; independent t-test p = 0.038), though the gap is small in absolute terms.
Counter-intuitively, patients with 0 major vessels colored by fluoroscopy show the highest heart disease prevalence (71.8%) — this variable behaves as a strong risk marker in the opposite direction of a naive "more blockages = more risk" assumption, and prevalence drops sharply as vessel count rises.
The "Fixed Defect" thalassemia type is heavily overrepresented among heart disease patients (412 of 544 patients with that type were diagnosed positive), making it one of the most clinically informative categorical features in the dataset.
The single most common risk-factor combination among diagnosed patients is non-anginal chest pain + normal fasting blood sugar + no exercise-induced angina + fixed-defect thalassemia (133 patients).
Diagnosed patients, on average, are younger (52.4 vs. 56.6 years) with a higher max heart rate and lower oldpeak than non-diagnosed patients — a pattern worth flagging for clinical review, since it runs against the typical "older age = higher risk" intuition and may reflect how this particular cohort was sampled.

Predictive modeling

A logistic regression model trained on all features achieved 80.98% out-of-sample accuracy (recall of 91% for the heart-disease-positive class), with cp and slope among the largest positive feature weights.
Challenges Faced
Deceptive dataset cleanliness: With zero missing values and no structural errors, the dataset looked "too clean" for a real clinical source, which required extra scrutiny (e.g., validating the plausibility of category codes and outlier bounds) rather than assuming the data needed no preprocessing decisions at all.
Counter-intuitive clinical signals: Several relationships (e.g., 0 vessels colored ↔ highest disease prevalence, younger age ↔ higher disease rate) run against common assumptions about cardiovascular risk. Interpreting these required care to avoid drawing causal conclusions from what are ultimately correlational, cross-sectional patterns.
Encoded categorical variables: Several fields (cp, thal, restecg, slope, ca) are stored as numeric codes with non-obvious real-world meanings, so every cross-tabulation and chart needed a clear label mapping to stay clinically interpretable rather than just numerically correct.
Balancing outlier handling with data integrity: Clinical measurements like cholesterol and blood pressure can have legitimate extreme values in real patients, so outlier detection had to be applied carefully (flag/cap rather than blindly drop) to avoid discarding genuinely high-risk cases — the exact patients this analysis cares about most.
Model interpretability vs. performance trade-off: Logistic regression was chosen for interpretability (clear feature weights) over a potentially higher-accuracy black-box model, since the business need was explaining why a patient is high-risk, not just flagging that they are.
Recommendations for Improvement

For the analysis / modeling:

Validate the logistic regression model with cross-validation (not just a single train/test split) and compare against tree-based models (Random Forest, Gradient Boosting) to check for accuracy gains while retaining feature-importance interpretability.
Add multicollinearity checks (e.g., VIF) before finalizing the regression, since several clinical features (e.g., thalach, exang, oldpeak) are physiologically related.
Investigate the counter-intuitive ca and age findings with a subject-matter clinician — they may reflect this specific dataset's sampling rather than a generalizable clinical pattern, and should be flagged before being used in patient-facing recommendations.
Extend the analysis with a precision-recall / ROC-AUC evaluation, since in a healthcare screening context, missing a true positive (a sick patient) is typically costlier than a false alarm.

For the business / product:

Build a simple risk-scoring dashboard for clinicians using the top predictors (cp, thalach, oldpeak, exang, ca, thal) so frontline staff can flag high-risk patients without needing to interpret raw model coefficients.
Pilot age- and gender-specific preventive programs, since blood pressure and chest-pain patterns showed statistically meaningful demographic differences.
Expand the dataset with longitudinal / follow-up outcomes (e.g., time-to-event data) to move from a snapshot risk profile toward genuine early-warning capability.
Establish a data governance and privacy review process before any predictive model is used in real patient care, given the sensitivity of the underlying health data.
How to Run
Install dependencies:
bash
   pip install pandas numpy seaborn matplotlib scikit-learn scipy
Open Heart_Health_Analysis.ipynb in Jupyter Notebook / JupyterLab.
Update the dataset file path in the first cell if needed.
Run all cells sequentially — the notebook is organized in the same Basic → Medium → Advanced order as the case study brief.
