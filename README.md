# breast-cancer-early-detection-ml
Machine learning models for early breast cancer detection using clinical diagnostic data
Breast Cancer Early Detection using Interpretable Machine Learning
Project Overview
This project develops and evaluates machine learning models for early breast cancer detection using the Wisconsin Diagnostic Breast Cancer (WDBC) dataset.
The primary objective is to maximize diagnostic performance while preserving interpretability, a critical requirement in clinical decision-support systems.

Rather than blindly optimizing for model complexity, the project follows a model comparison and validation strategy, starting from transparent linear models and progressing to ensemble and gradient-boosted methods.

Dataset
Source: Wisconsin Diagnostic Breast Cancer Dataset (scikit-learn)

Samples: 569

Features: 30 numerical features derived from cell nuclei measurements

Target:

0 → Malignant

1 → Benign

The dataset is moderately imbalanced, but not severely enough to justify aggressive resampling.

Methodology
1. Data Preparation
Stratified train–test split (80/20)

Feature scaling using StandardScaler

Scaling fitted only on training data to prevent leakage

2. Baseline Model — Logistic Regression (No SMOTE)
Logistic Regression was selected as the primary baseline due to:

High interpretability

Clinical transparency

Stable behavior on structured medical data

Performance (Test Set):

Metric	Value
Accuracy	0.982
Recall	0.986
F1 Score	0.986
AUC	0.996

Conclusion
The baseline model already achieves near-optimal performance, indicating strong intrinsic signal in the data.

3. Ensemble Model — Random Forest
A Random Forest classifier was trained to:

Capture potential nonlinear interactions

Validate whether increased complexity improves performance

Performance (Test Set):

Metric	Value
Accuracy	0.956
Recall	0.972
F1 Score	0.966
AUC	0.993

Despite its flexibility, Random Forest did not outperform Logistic Regression.

4. Hyperparameter Tuning — RandomizedSearchCV
Random Forest hyperparameters were optimized using RandomizedSearchCV to ensure a fair comparison.

Best Configuration (summary):

Shallow trees (max_depth = 5)

Large ensemble (n_estimators ≈ 600)

Feature subsampling (max_features = log2)

Result:
Performance remained nearly identical to the untuned Random Forest and still below Logistic Regression.

5. Gradient Boosting — XGBoost
An XGBoost classifier was trained to assess:

Boosted decision trees

Nonlinear interactions with regularization

Performance (Test Set):

Metric	Value
Accuracy	0.956
Recall	0.986
F1 Score	0.966
AUC	0.995

XGBoost matched Logistic Regression on recall and AUC, but did not provide a clear overall advantage.

Model Comparison Summary
Model	Accuracy	Recall	F1	AUC
Logistic Regression	0.982	0.986	0.986	0.996
Random Forest	0.956	0.972	0.966	0.993
Tuned Random Forest	0.956	0.972	0.966	0.993
XGBoost	0.956	0.986	0.966	0.995

Final Decision:
Logistic Regression was selected as the primary model due to:

Superior overall performance

Simplicity

Full interpretability

Clinical reliability

Interpretability
Logistic Regression Coefficients
Large negative coefficients (e.g., worst perimeter, worst area, concave points) strongly indicate malignancy

Positive coefficients contribute toward benign classification

Coefficient directions align with established oncological knowledge

SHAP Analysis (XGBoost)
SHAP summary plots confirm:

Tumor size and shape irregularity dominate predictions

Feature effects are directionally consistent with Logistic Regression

Complex models reinforce, rather than contradict, linear findings

Why SMOTE Was Not Used
SMOTE was evaluated but ultimately rejected because:

The dataset is only mildly imbalanced

Baseline recall was already extremely high

SMOTE slightly reduced overall performance

Artificial samples can distort medical feature distributions

Repository Structure
Copiar código
breast-cancer-early-detection-ml/
│
├── notebooks/
│   └── 01_eda_modeling_interpretability.ipynb
│
├── figures/
│   ├── confusion_matrix.png
│   ├── roc_curve.png
│   └── shap_summary.png
│
├── requirements.txt
└── README.md
Key Takeaways
Simple, interpretable models can outperform complex ensembles

Medical ML should prioritize clarity over complexity

Model comparison strengthens confidence, even when simpler models win

Interpretability tools validate, not replace, sound modeling choices

Future Work
External dataset validation

Cost-sensitive learning for clinical deployment

Model calibration and threshold optimization

Integration into clinical decision pipelines

Author
Nasibeh Rahimi
Clinical Laboratory Scientist | Medical Biotechnology
Machine Learning for Early Disease Detection
