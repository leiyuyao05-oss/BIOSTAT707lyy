# ICU data analysis checkpoint1
This checkpoint analyzes PhysioNet 2012 ICU dataset, focusing on developing and evaluating machine learning models to predict in-hospital mortality among intensive care unit (ICU) patients using the PhysioNet/CinC Challenge 2012 dataset.

The analysis uses the Set A patient records and corresponding outcome information. Patient measurements recorded during the first 48 hours of the ICU stay are used to construct predictors. The primary objective is to compare the discrimination performance of statistical and machine learning models with established clinical severity scores.

## Objectives
The main objectives are to:
- Construct a patient-level analytical dataset from longitudinal ICU measurements.
- Preprocess clinical predictors while avoiding information leakage during cross-validation.
- Develop and compare LASSO logistic regression, spline-based logistic regression, and Random Forest models.
- Evaluate model discrimination using five-fold stratified cross-validation.
- Compare the predictive performance of the fitted models with SAPS-I and SOFA benchmark scores.
- Examine model coefficients, feature-selection stability, and conditional predicted-probability patterns where applicable.

## Dataset
The analysis uses Set A of the PhysioNet/CinC Challenge 2012 dataset.

The raw data contain time-stamped clinical measurements and patient-level outcome information. The measurements are processed into one row per ICU stay, and outcomes are linked using RecordID.

The primary outcome is in-hospital death, represented by In-hospital_death.

Predictors are constructed from measurements available during the first 48 hours of the ICU stay. Longitudinal measurements are summarized using features such as the last observed value, minimum, maximum, mean, and observation count, as applicable to each variable.

The following variables are not used as predictors:
- RecordID, which serves as an identifier.
- Length_of_stay and Survival, which contain outcome-related information.
- SAPS-I and SOFA scores, which are used as benchmark predictors rather than input features in the primary models.

The ICU type is treated as a categorical variable.

**Data access**: The raw dataset can be obtained from the official PhysioNet/CinC Challenge 2012 data source, subject to the applicable access and usage requirements. Place the files in the directory structure expected by the notebook.

## Data Processing
The analysis includes the following steps:
- Load the Set A clinical measurements and outcome records.
- Process time-stamped measurements and retain information from the first 48 hours.
- Construct summary features at the ICU-stay level.
- Merge the features with the outcome data using RecordID.
- Inspect missingness and verify the patient-level structure.
- Apply model-specific preprocessing, including imputation, scaling, categorical encoding, and feature selection where appropriate.

Preprocessing operations that estimate quantities from the data are fitted within the training portion of each cross-validation split to reduce information leakage.

## Models
The following models are evaluated:
- LASSO: Logistic regression with L1 regularization
- SplineLogistic: Logistic regression with spline-transformed continuous predictors to capture nonlinear relationships
- RandomForest	Tree-based ensemble model
- SAPS-I: Clinical severity score used as a benchmark
- SOFA: Clinical severity score used as a benchmark

The three primary predictive models are compared using the same five-fold stratified cross-validation splits. Hyperparameter tuning, where applicable, is performed using the training data within the cross-validation procedure.

## Evaluation
The primary evaluation metric is the area under the receiver operating characteristic curve (AUROC).

Mean AUROC and standard deviation across the five evaluated folds are used to summarize discrimination performance and fold-to-fold variability.

## Results
The current five-fold cross-validation results are summarized below.	
- SplineLogistic: 0.8451 (Mean AUROC), 0.0246 (SD of AUROC)
- LASSO: 0.8430 (Mean AUROC), 0.0292 (SD of AUROC)
- RandomForest: 0.8428 (Mean AUROC), 0.0168 (SD of AUROC)
- SAPS-I: 0.6379 (Mean AUROC), 0.0299 (SD of AUROC)
- SOFA: 0.6262 (Mean AUROC), 0.0222 (SD of AUROC)

## Structure
- checkpoint2.ipynb
- README.md
- pixi.toml
- pixi.lock
- output/
  - checkpoint2.html
  - cp2_oof_predictions.csv
  - cp2_final_model.pkl
- data/
  - Outcomes-a.txt
  - set-a/

## How to run
- Install Pixi following the official installation instructions.
- Clone or download this repository.
- Place the course-provided Set A data in the expected location.
- Open a terminal in the project root directory.
- Install or synchronize the project environment as needed.
- Execute the notebook from a clean kernel or run the configured task: pixi run --locked checkpoint2

The notebook should be run from top to bottom. If the command fails because of a Jupyter kernel configuration, verify that the kernel points to the Python interpreter in the current project's Pixi environment.

## Reproducibility
- The cross-validation procedure uses five stratified folds.
- A fixed random seed is used where specified in the notebook.
- Model comparisons use consistent cross-validation splits.
- Data-dependent preprocessing is performed within the training folds.
- The environment is managed through Pixi.

## Limitations
This analysis is based on Set A and uses internal cross-validation to evaluate model discrimination. Performance on independent data has not been established by these results alone.

AUROC does not assess calibration or clinical decision-making benefit. Additional analyses, such as external validation, calibration assessment, subgroup evaluation, and decision-curve analysis, would be needed before considering clinical application.
