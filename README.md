# ICU data analysis checkpoint3

## 1. Project Overview

This project evaluates the external validity and clinical usefulness of a LASSO logistic regression model for predicting in-hospital mortality among ICU patients using data from the PhysioNet Computing in Cardiology Challenge 2012.

The primary objective of Checkpoint 3 is to assess whether the predictive model developed in Checkpoint 2 can maintain its performance when applied to two independent evaluation datasets, set-b and set-c.

In Checkpoint 2, LASSO logistic regression was selected after comparing LASSO, spline logistic regression, and random forest using five-fold cross-validation on set-a. LASSO was retained because it achieved discrimination comparable to the other candidate models while providing a relatively sparse and interpretable model.

In Checkpoint 3, the saved model is applied directly to set-b and set-c without retraining, feature selection, or hyperparameter tuning. Its performance is evaluated in terms of discrimination, calibration, clinical utility, subgroup performance, model interpretation, and prediction errors.

## 2. Dataset

The project uses data from the **PhysioNet Computing in Cardiology Challenge 2012**.

Three datasets are involved:

| Dataset | Purpose |
|---|---|
| set-a | Model development and internal cross-validation (Checkpoint 2) |
| set-b | Independent external evaluation (Checkpoint 3) |
| set-c | Independent external evaluation (Checkpoint 3) |

The prediction window is the first 48 hours of each ICU stay.

The external evaluation datasets each contain 4,000 ICU stays.

The outcome of interest is in-hospital mortality.

## 3. Modeling Strategy

### 3.1 Previously Developed Model

The final model selected in Checkpoint 2 is LASSO logistic regression.

The model was developed using set-a, with five-fold cross-validation used to compare candidate models.

The mean cross-validation AUROC for LASSO was approximately 0.843 (SD = 0.029).

The saved model includes the fitted preprocessing pipeline and the original predictor columns.

### 3.2 External Validation

The saved LASSO pipeline is applied to set-b and set-c.

The original predictor columns are preserved to ensure consistency with model development.

- Missing predictor columns are added as missing values and handled by the saved imputation procedure.
- Extra predictor columns are excluded.
- The original preprocessing pipeline is reused.
- No preprocessing steps are refitted on the evaluation datasets.
- No model retraining or hyperparameter tuning is performed using set-b or set-c.

These procedures help prevent information leakage and ensure that external evaluation reflects the performance of the previously developed model.

## 4. Model Evaluation

The following evaluation components are included.

### 4.1 Discrimination

Discrimination measures the model's ability to distinguish patients who died in the hospital from those who survived.

Evaluation methods include:

- Receiver Operating Characteristic (ROC) curves.
- Area Under the ROC Curve (AUROC).
- Stratified bootstrap 95% confidence intervals.
- Comparison with SAPS-I and SOFA clinical scores.

### 4.2 Calibration

Calibration assesses the agreement between predicted mortality risks and observed mortality outcomes.

Evaluation methods include:

- Calibration plots.
- LOWESS-smoothed calibration curves.
- Calibration intercept and slope.
- Brier score.
- Scaled Brier score.
- Bootstrap confidence bands for calibration curves.

### 4.3 Clinical Utility

Clinical utility is assessed using Decision Curve Analysis (DCA).

The analysis examines net benefit across different probability thresholds and compares the model with alternative decision strategies.

A clinical threshold of 20% is also used to examine the sensitivity–positive predictive value trade-off and the potential clinical workload.

### 4.4 Subgroup Analysis

Subgroup evaluation examines whether predictive performance varies across different patient groups.

The purpose is to identify potential differences in model performance and assess whether the model may be less reliable in particular subgroups.

### 4.5 Model Interpretation

Model interpretation is used to investigate which predictors contribute most to individual predictions.

The analysis examines feature contributions and their patterns across individual patients.

These contributions describe model behavior and should not be interpreted as causal effects.

### 4.6 Most Confident Prediction Errors

The analysis also identifies highly confident incorrect predictions, including:

- False positives: patients predicted to have a high mortality risk who survived.
- False negatives: patients predicted to have a low mortality risk who died.

These cases are reviewed to identify potential data-quality issues, missing clinical information, and possible limitations of the predictor set.

## 5. Main Results

### 5.1 Discrimination

The final LASSO model demonstrated consistent discrimination across the two external evaluation datasets. It substantially outperformed both clinical scores in terms of discrimination in both evaluation cohorts. Both datasets showed reasonably good overall calibration, although some deviations from the ideal calibration curve remained.

However, the review of highly confident prediction errors identified:

- False-positive cases with predicted mortality risks ranging from 82.4% to 94.1%.
- False-negative cases with predicted mortality risks ranging from 1.1% to 1.6%.

Frequently contributing features included Glasgow Coma Scale (GCS), age, bilirubin, BUN, and laboratory measurement counts.

These cases highlight potential model failure modes and areas requiring further investigation.

## 6. Limitations and Considerations

Several limitations should be considered when interpreting the results:

1. **Prediction window:** The model uses information from the first 48 hours of ICU admission and may not capture subsequent changes in patient condition.
2. **Missing data:** Missingness and measurement frequency may reflect clinical practice and patient severity.
3. **Time-varying information:** Simplified feature construction may not fully capture the dynamic nature of physiological measurements.
4. **External validity:** Evaluation on set-b and set-c does not establish generalizability to all ICU populations or clinical settings.
5. **Clinical implementation:** Good discrimination alone does not establish clinical usefulness or readiness for deployment.
6. **Causal interpretation:** Predictors, treatment variables, and measurement counts should not be interpreted as causal effects.

Further validation in the intended clinical setting, selection of a defensible decision threshold, and prospective impact evaluation would be necessary before clinical implementation.

## 7. Requirements

The analysis is implemented in Python using the following main libraries:

- Python
- NumPy
- pandas
- scikit-learn
- SciPy
- statsmodels
- Matplotlib
- Seaborn
- joblib

Jupyter Notebook is used to organize and execute the analysis.

The exact package versions are not specified in this project documentation.

## 8. How to Run the Analysis

### Step 1. Prepare the Data

Download and organize the PhysioNet Challenge 2012 datasets.

The structure should be:
- checkpoint3.ipynb
- output/
- output2/
  - cp2_final_model.joblib
- data/
- test/
  - set-b/
  - set-c/
  - Outcomes-b.txt
  - Outcomes-c.txt

### Step 2. Run Checkpoint 2

Run the Checkpoint 2 analysis first, if its saved model artifact is not already available.

The following files are required:

- `output2/cp2_final_model.joblib`
- `output2/cp2_model_comparison.csv`

The saved model artifact contains the fitted LASSO pipeline and its original predictor columns.

### Step 3. Run Checkpoint 3

use "pixi run --locked checkpoint3"

The analysis will:

1. Load the saved LASSO model.
2. Load and preprocess set-b and set-c.
3. Align evaluation predictors with the original set-a predictor columns.
4. Generate mortality risk predictions.
5. Evaluate discrimination and calibration.
6. Assess clinical utility using DCA.
7. Conduct subgroup and model interpretation analyses.
8. Identify highly confident prediction errors.
9. Generate evaluation tables and visualizations.

Output:
- cp3_auroc_bootstrap.csv
- cp3_calibration_metrics.csv
- cp3_calibration_set-b.png
- cp3_calibration_set-c.png
- cp3_cohort_summary.csv
- cp3_confident_errors_set_b.csv
- cp3_cv_vs_external.csv
- cp3_dca_set-b.png
- cp3_dca_set-c.png
- cp3_decision_curve_data.csv
- cp3_final_summary.csv
- cp3_lasso_coefficients.csv
- cp3_lasso_coefficients.png
- cp3_precision_recall_set_b.png
- cp3_precision_recall_set_c.png
- cp3_roc_set-b.png
- cp3_roc_set-c.png
- cp3_subgroup_performance_set_b.csv
- cp3_threshold_metrics.csv
- checkpoint3.html

### Step 4. Review Results

The generated results and figures are saved in the `output/` directory.

Review the evaluation outputs and use them to interpret the model's external performance and limitations.

## 9. Reproducibility and Data Leakage Prevention

Several procedures are implemented to maintain reproducibility and prevent information leakage:

- The saved model from Checkpoint 2 is reused without retraining.
- The original predictor columns are maintained during external validation.
- Missing values are handled using the preprocessing steps learned during model development.
- No preprocessing parameters are estimated from the external evaluation datasets.
- No feature selection or hyperparameter tuning is conducted using set-b or set-c.
- Outcome-related variables and identifiers are excluded from model predictors.

The analysis uses a fixed random seed of 123 for reproducible resampling procedures.

## 10. Conclusion

Checkpoint 3 evaluates the generalizability of the LASSO logistic regression model developed in Checkpoint 2 using two independent ICU evaluation datasets.

The model demonstrated consistent discrimination across set-b and set-c and outperformed the SAPS-I and SOFA clinical scores in both cohorts. Calibration results also indicated reasonably good agreement between predicted and observed mortality risks.

However, predictive performance alone is insufficient to establish clinical readiness. Clinical usefulness, subgroup reliability, data quality, and potential implementation risks require further investigation.

Overall, this checkpoint provides an external evaluation of the previously developed mortality prediction model and identifies important considerations for future validation and clinical application.
This checkpoint analyzes PhysioNet 2012 ICU dataset, focusing on test sets and assessment. More details on the Markdown of checkpoint3.html



