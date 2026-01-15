# Project Scope

## 1. Project Title
**Epigenetic Drug Resistance: A Multi-Omics and Explainable ML Framework**

## 2. Background and Motivation
Resistance to chemotherapy remains one of the major obstacles in cancer treatment, particularly in aggressive and heterogeneous tumors such as sarcomas. Increasing evidence suggests that epigenetic regulation plays a central role in shaping drug response phenotypes, influencing transcriptional programs, cellular plasticity, and adaptive resistance mechanisms.

Publicly available multi-omics datasets now enable integrative analyses combining gene expression, DNA methylation, genomic alterations, and drug response profiles. However, many existing studies focus on predictive performance without sufficient interpretability, limiting biological insight and translational value.

This project aims to bridge this gap by developing a reproducible, explainable machine learning framework to identify epigenetic signatures associated with drug resistance and to explore drug repurposing strategies based on these signatures.

## 3. Core Research Question
Can epigenetic and transcriptomic features be integrated to identify robust, interpretable molecular signatures of drug resistance, and can these signatures be leveraged to propose candidate compounds capable of reversing resistant phenotypes?

## 4. Objectives

### Primary Objective
- Identify and characterize epigenetic and transcriptomic signatures associated with resistance to chemotherapeutic agents using multi-omics data and explainable machine learning models.

### Secondary Objectives
- Evaluate the stability and biological coherence of identified resistance signatures across datasets and validation splits.
- Apply explainable AI (XAI) techniques to interpret feature importance and model decisions.
- Perform in silico drug repurposing analyses using perturbational databases (e.g., LINCS/CMap) to identify compounds that may reverse resistance-associated signatures.
- Establish a clear computational-to-experimental bridge for future in vitro validation in sarcoma models.

## 5. Scope and Boundaries

### In Scope
- Use of publicly available multi-omics datasets (e.g., gene expression, DNA methylation, mutations).
- Analysis at the level of cell lines and/or patient-derived samples, depending on data availability.
- Binary or continuous modeling of drug response (sensitive vs resistant, IC50/AUC).
- Classical and interpretable machine learning models (e.g., regularized regression, tree-based models).
- Explainability methods such as SHAP values, permutation importance, and coefficient analysis.
- In silico drug repurposing based on transcriptomic reversal signatures.

### Out of Scope
- Development of novel deep learning architectures.
- Clinical trial prediction or patient outcome modeling.
- Wet-lab experiments within this repository (experimental validation will be documented conceptually but not executed here).
- Real-time or streaming data integration.

## 6. Expected Outputs
- A reproducible data processing and modeling pipeline implemented in Python.
- Well-documented notebooks and scripts covering data ingestion, feature engineering, modeling, and interpretation.
- A set of candidate resistance-associated molecular signatures.
- Ranked lists of candidate compounds for potential drug repurposing.
- Figures and tables suitable for inclusion in a preprint or scientific report.

## 7. Reproducibility and Design Principles
- Clear separation between raw, interim, and processed data.
- Deterministic pipelines where possible, with controlled random seeds.
- Modular code organization to facilitate reuse and extension.
- Emphasis on interpretability and biological plausibility over purely predictive performance.

## 8. Future Extensions
- Integration of additional omics layers (e.g., chromatin accessibility).
- Expansion to specific cancer subtypes, including rhabdomyosarcoma and osteosarcoma.
- Experimental validation of selected targets and compounds in vitro.
- Preparation of a preprint or manuscript based on the computational findings.
