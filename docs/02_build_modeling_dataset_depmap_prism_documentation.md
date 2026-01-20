# Build modeling dataset — DepMap expression & PRISM response

## Objective

This notebook builds the **final integrated modeling dataset** combining:

* DepMap gene expression profiles  
* PRISM drug response (AUC) measurements  

after the structural validation performed in the sanity-check stage.

The goal of this step is to construct clean, aligned, and reproducible input tables suitable for:

* Machine learning modeling  
* Drug sensitivity prediction  
* Downstream biological analysis  

This notebook produces the **canonical processed datasets** used by all subsequent modeling pipelines.

---

## Input datasets

| Dataset | Description |
|--------|-------------|
| DepMap expression | Full gene expression matrix (cell lines × genes) |
| PRISM response | Dose–response table with AUC values |
| Cell line metadata | DepMap cell line annotations |

All identifiers and integrity constraints were validated previously in the sanity-check notebook.

---

## Identifier alignment and mapping

### Join key definition

A standardized identifier `join_id` is used to align datasets:

* DepMap source column: `ModelID`  
* PRISM source column: `depmap_id`  

String normalization and stripping were applied to ensure exact matching.

### Mapping results

| Metric | Value |
|-------|-------|
| DepMap unique cell lines | **1,699** |
| PRISM unique cell lines | **737** |
| Shared cell lines | **727** |

Interpretation:

* Almost the entire PRISM dataset maps to DepMap expression data.
* DepMap contains additional unscreened cell lines, which are discarded.
* The mapping is biologically and structurally consistent.

---

## PRISM filtering and drug eligibility

### Coverage filtering

Drugs were filtered based on minimum cell-line coverage:

* Minimum required cell lines per drug: `MIN_CELL_LINES_PER_DRUG`
* Only drugs meeting this threshold were retained.

### Eligible drug statistics

| Metric | Value |
|--------|-------|
| Eligible drugs | **1,528** |
| Min cell lines per drug | **224** |
| Max cell lines per drug | **719** |

This ensures:

* Sufficient statistical support per compound  
* Stable estimation of drug response distributions  

---

## Drug response aggregation (drug index)

For each eligible drug, summary statistics were computed:

* Mean AUC  
* Standard deviation  
* Minimum and maximum AUC  
* Number of screened cell lines  

This produces a compact **drug index table** used for:

* Drug-level stratification  
* Benchmarking  
* Exploratory ranking  

---

## DepMap expression alignment

After filtering to matched cell lines:

| Metric | Value |
|--------|-------|
| Original DepMap rows | **1,754** |
| Matched DepMap rows | **751** |
| Unique matched cell lines | **727** |
| Final expression matrix | **751 × 19,220** |

Interpretation:

* Each row corresponds to one screened cell-line condition  
* Only expression profiles with valid PRISM response data are retained  

This guarantees **perfect alignment** between predictors and targets.

---

## Final outputs

The following processed datasets are generated and written to `data/processed/`:

| File | Description |
|------|-------------|
| `depmap_expression_matched.parquet` | Expression matrix restricted to matched cell lines |
| `prism_auc_filtered.parquet` | Filtered PRISM response table |
| `drug_index.parquet` | Drug-level summary statistics |
| `cell_line_metadata.parquet` | Metadata for matched cell lines |

Additionally, reproducibility metadata files are generated:

| File | Description |
|------|-------------|
| `02_build_dataset_summary.json` | Dataset statistics and configuration summary |
| `02_file_fingerprints.json` | SHA256 fingerprints of all outputs |

---

## Reproducibility and integrity guarantees

This step enforces:

* Deterministic filtering rules  
* Explicit join keys  
* Coverage-based drug selection  
* File fingerprinting for all outputs  

All downstream notebooks assume:

* The exact schema produced here  
* Stable row and column ordering  
* Identical join alignment  

Any modification in upstream data or parameters will propagate through new fingerprints and summaries.

---

## Conclusions

This notebook constructs a **clean, reproducible, and modeling-ready dataset** with:

* 727 aligned cell lines  
* 1,528 well-covered drugs  
* ~19k gene expression features  
* Fully documented filtering and aggregation steps  

The pipeline may now proceed to:

* Exploratory modeling  
* Feature engineering  
* Drug sensitivity prediction  
* Multi-omics integration  

with high confidence in data integrity and alignment.

---

## Notes

This stage deliberately separates:

* Structural validation (Notebook 01)  
* Dataset construction (this notebook)  
* Modeling (subsequent notebooks)  

to enforce modularity, traceability, and reproducibility across the full pipeline.
