# ICU data analysis checkpoint3
This checkpoint analyzes PhysioNet 2012 ICU dataset, focusing on test sets and assessment. More details on the Markdown of checkpoint3.html

## Structure
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

## How to run
pixi run --locked checkpoint3

## output
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
