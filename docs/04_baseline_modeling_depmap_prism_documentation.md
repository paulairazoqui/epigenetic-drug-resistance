# Baseline Modeling on DepMap + PRISM

## Objective

This notebook implements a first-pass **baseline modeling pipeline** to evaluate whether gene expression profiles from DepMap can predict drug response (PRISM AUC) at the **individual drug level**.

The goal is not to optimize performance yet, but to:

- Validate that the dataset built in Notebook 02 is usable for supervised learning.
- Establish simple reference models.
- Quantify expected signal-to-noise levels.
- Detect structural or modeling issues early.

This notebook operates on the **processed datasets** generated previously and focuses on a small subset of drugs for rapid iteration.

---

## Input Data

This notebook loads the following processed artifacts:

- `data/processed/prism_auc_filtered.parquet`  
  Filtered PRISM AUC measurements mapped to DepMap cell lines.

- `data/processed/depmap_expression_matched.parquet`  
  Expression matrix (genes × matched cell lines).

- `reports/03_selected_drugs_summary.json`  
  Output of Notebook 03 defining the drug subset selected for modeling.

These inputs are assumed to be static and reproducible.

---

## Modeling Scope

At this stage, modeling is performed on a **small test subset of drugs** to validate the pipeline:

- First 5 drugs from the selected drug list.
- Each drug is modeled independently.
- One regression model is trained per drug.

This design supports:

- Debugging feature/target alignment.
- Assessing per-drug predictability.
- Inspecting performance variability across drugs.

---

## Dataset Construction

For each drug (`broad_id`), a drug-specific modeling dataset is constructed using:

- DepMap expression features (gene expression per cell line).
- PRISM AUC values as the regression target.
- Only cell lines with both expression and AUC data are retained.

Key constraints:

- Non-numeric metadata columns (e.g., `join_id`) are excluded from features.
- Only numeric expression features are passed to the model.
- No feature selection or dimensionality reduction is applied at this stage.

---

## Train / Validation / Test Splitting

Each drug-specific dataset is split into:

- Training set  
- Validation set  
- Test set  

Using fixed random seeds for reproducibility.

This supports:

- Hyperparameter-free baseline comparison.
- Early detection of overfitting or data leakage.
- Consistent evaluation across models.

---

## Baseline Models

Three baseline regression models are evaluated:

- **Ridge Regression**
- **ElasticNet Regression**
- **Random Forest Regressor (fast configuration)**

Each model is trained using a standard scikit-learn pipeline:

