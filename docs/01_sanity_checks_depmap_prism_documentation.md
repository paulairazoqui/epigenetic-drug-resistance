# Sanity checks — DepMap expression & PRISM response

## Objective

This notebook performs structural and integrity sanity checks on the two core datasets used throughout the project:

* DepMap gene expression matrix
* PRISM dose–response (AUC) dataset

The goal of this step is to validate data integrity, identifier consistency, join feasibility, and response metric quality **before any downstream exploratory analysis, modeling, or biological interpretation**.

This step acts as a formal **GO / NO-GO gate** for the DepMap–PRISM integration pipeline.

---

## Input datasets

| Dataset             | Description                                                       |
| ------------------- | ----------------------------------------------------------------- |
| DepMap expression   | High‑dimensional gene expression matrix (cell lines × genes)      |
| PRISM dose–response | Drug screening response table with AUC as primary response metric |

Only minimal columns were initially loaded to validate identifiers and schema before loading the full matrices.

---

## Integrity verification (file fingerprints)

SHA256 fingerprints were computed for both input files prior to loading. This provides:

* File identity verification
* Reproducibility guarantees
* Protection against silent file replacement or corruption

Fingerprint presence was confirmed before proceeding.

---

## Structural inspection

Full datasets were loaded after schema validation.

### Dataset dimensions

| Dataset           | Shape              |
| ----------------- | ------------------ |
| DepMap expression | **1,754 × 19,222** |
| PRISM response    | **753,778 × 21**   |

The DepMap matrix contains ~19k gene features, requiring memory‑safe handling for diagnostics.

---

## Identifier mapping and overlap analysis

### Identifier normalization

DepMap and PRISM use the same biological identifiers but with different column names:

* DepMap: `ModelID`
* PRISM: `depmap_id`

A standardized column `join_id` was created in both datasets using stripped string normalization.

### Unique identifiers

| Metric                   | Value     |
| ------------------------ | --------- |
| DepMap unique cell lines | **1,699** |
| PRISM unique cell lines  | **738**   |

### Overlap statistics

| Metric                           | Value       |
| -------------------------------- | ----------- |
| Shared cell lines (intersection) | **727**     |
| Coverage vs DepMap               | **42.79 %** |
| Coverage vs PRISM                | **98.51 %** |

Interpretation:

* Almost the entire PRISM dataset maps onto DepMap expression data.
* DepMap contains additional cell lines not screened in PRISM, which is expected.
* The join key mapping is correct and biologically meaningful.

---

## Response metric validation (AUC)

The primary response variable is the area under the dose–response curve (`auc`).

### AUC diagnostics

| Metric                    | Value       |
| ------------------------- | ----------- |
| AUC column detected       | `auc`       |
| Non‑null AUC values       | **753,778** |
| Unique rounded AUC values | **741,939** |

Interpretation:

* AUC is present for all rows.
* The distribution is highly non‑degenerate.
* The response variable is suitable for modeling and stratified analyses.

---

## Memory‑safe numerical diagnostics

Due to the size of the expression matrix (≈34 million values), full matrix sampling was avoided.

Instead:

* A random subset of 200 gene columns was selected
* Values were flattened and summarized
* Missingness and distribution statistics were computed

This confirmed:

* Numerical stability
* Low missingness
* Coherent value ranges

without exceeding memory limits.

---

## GO / NO‑GO criteria

The following criteria were evaluated:

| Criterion                                  | Result |
| ------------------------------------------ | ------ |
| Both datasets load without errors          | ✅ Pass |
| Required identifiers exist                 | ✅ Pass |
| Meaningful cell‑line overlap               | ✅ Pass |
| Response metric present and non‑degenerate | ✅ Pass |

### Final decision

> **GO — Integration may safely proceed**

The DepMap expression matrix and PRISM response dataset are structurally compatible, correctly mapped, and suitable for downstream joint analysis and modeling.

---

## Conclusions

This sanity‑check stage confirms that:

* Input files are intact and reproducible
* Identifier mapping (`ModelID` ↔ `depmap_id`) is correct
* Dataset overlap is biologically and statistically meaningful
* The AUC response variable is valid and informative

The pipeline may now proceed to exploratory data analysis, feature engineering, and modeling steps with high confidence in data integrity and alignment.

---

## Notes

This validation step is intentionally isolated from downstream analysis to:

* Enforce reproducibility
* Detect schema or mapping issues early
* Prevent silent propagation of corrupted joins or degenerate targets

All subsequent notebooks assume the `join_id` mapping validated in this step.
