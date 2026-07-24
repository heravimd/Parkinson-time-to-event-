# Radiomics + Clinical Survival Analysis

This notebook predicts **time-to-event** using three feature sets, compared side by side:
- **Radiomics only**
- **Clinical only**
- **Radiomics + Clinical combined**

For each feature set, four survival models are trained and evaluated:
- **Random Survival Forest (RSF)**
- **Gradient Boosting Survival Analysis (GBSA)**
- **Survival Support Vector Machine (Survival SVM)**
- **Penalized Cox Model** (elastic-net regularized Cox regression, `CoxnetSurvivalAnalysis`)

Evaluation uses **stratified 5-fold cross-validation on a training set** plus a single
**held-out test set** (same patients held out for every feature set, for a fair
comparison), with the **concordance index (C-index)** and **log-rank test** as metrics.
A summary CSV comparing all feature-set/model combinations is saved at the end.

**Expected inputs**

1. `radiomics_features.csv` — one row per scan (first PET scan of each patient):
   `image_id, feature_1, feature_2, ..., feature_n`

2. `clinical_events.csv` — one row per patient (time-to-event labels):
   `image_id, patient_id, time, event`
   - `time`: time to event or censoring (ideally > 0; non-positive values are auto-corrected)
   - `event`: 1 = event occurred, 0 = censored

3. `clinical_data.csv` — one row per patient (clinical variables used as predictors), e.g.:
   `patient_id, age, sex, stage, smoking_status, ...`
   - Can contain a mix of numeric (age, lab values) and categorical (sex, stage) columns —
     both are handled automatically (see Section 5).

Edit the file paths and column names in the **Config** cell below, then run all cells.
