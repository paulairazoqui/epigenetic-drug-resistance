# Data Sources

## 1. Overview
This project relies exclusively on publicly available datasets to ensure transparency, reproducibility, and extensibility. The selected data sources support integrative analyses of molecular profiles and drug response, enabling the identification of resistance-associated signatures and the exploration of drug repurposing strategies.

Data sources are grouped into two main categories:
1. Multi-omics datasets with drug response information, used for modeling drug resistance.
2. Drug perturbation and annotation databases, used for in silico drug repurposing and biological interpretation.

At this stage, data sources are selected to support a minimal viable pipeline (MVP), with additional datasets explicitly documented as future extensions.

---

## 2. Multi-Omics and Drug Response Datasets

### 2.1 DepMap (Broad Institute)

**Description**  
The Dependency Map (DepMap) project provides high-quality molecular and pharmacological data for a large collection of human cancer cell lines. It integrates transcriptomic profiles with functional and drug sensitivity screens, making it a robust foundation for modeling drug response phenotypes.

**Data Modalities**
- Gene expression (RNA-seq)
- Genetic alterations (mutations, copy number variation)
- Drug sensitivity data (PRISM Repurposing dataset)

**Role in This Project**
DepMap gene expression data will be used as the primary feature space for modeling drug sensitivity and resistance. Drug response measurements from the PRISM dataset will serve as labels for supervised learning tasks.

**Rationale for Selection**
- Broad coverage of cancer cell lines
- High-quality, well-curated RNA-seq data
- Direct integration with large-scale drug sensitivity screens
- Strong adoption and validation in the scientific community

**Scope**
DepMap constitutes the primary data source for the MVP implementation.

#### 2.1.1 Files Used in This Project

**Dataset:** DepMap Public 25Q3 — RNA-seq Expression  
**File:** `OmicsExpressionTPMLogp1HumanProteinCodingGenes.csv`  
**Release:** DepMap Public 25Q3  
**Source:** https://depmap.org/portal/download/

**Description:**  
Log-transformed gene-level RNA-seq expression values (log2(TPM + 1)) for human protein-coding genes. Expression data were quantified using Salmon and aligned to Gencode v38 as part of the DepMap RNA-seq processing pipeline.

**Role in This Project:**  
Primary feature matrix used to model drug sensitivity and resistance across cancer cell lines.

**Notes:**  
Only protein-coding genes are included to reduce dimensionality and improve biological interpretability. Raw expression data are used without additional normalization beyond the provided log2(TPM + 1) transformation.

---

### 2.2 PRISM Repurposing Dataset

**Description**  
The PRISM Repurposing dataset provides quantitative drug response measurements across hundreds of cancer cell lines, generated using pooled screening technologies.

**Data Modalities**
- Drug response metrics (e.g., viability-based AUC)

**Role in This Project**
PRISM drug response data will be used to define sensitive and resistant phenotypes, either as continuous outcomes or binarized labels, depending on modeling strategy.

**Rationale for Selection**
- Direct compatibility with DepMap cell line identifiers
- Large number of screened compounds
- Suitable scale for machine learning applications

**Scope**
Included as part of the MVP through its integration with DepMap.

#### 2.2.1 Files Used in This Project

**Dataset:** PRISM Repurposing Secondary Screen  
**File:** `secondary_dose_response_curve_parameters.csv`  
**Release:** PRISM Repurposing 20Q2 (Secondary Screen)  
**Source:** https://depmap.org/portal/download/

**Description:**  
Dose–response curve parameters derived from pooled cell line chemical perturbation screens. The dataset includes drug response metrics such as AUC, IC50, and EC50, computed from fitted dose–response models across multiple compounds and cancer cell lines.

**Role in This Project:**  
Used as the primary source of drug response measurements. AUC values are employed as continuous outcomes to model drug sensitivity and resistance, with optional binarization for classification tasks.

**Notes:**  
This dataset is preprocessed and replicate-collapsed, enabling direct integration with DepMap gene expression data without additional curve fitting.

---

### 2.3 CCLE (Cancer Cell Line Encyclopedia) — Extension

**Description**  
The Cancer Cell Line Encyclopedia (CCLE) provides multi-omics characterization of cancer cell lines, including epigenetic layers not fully represented in DepMap.

**Data Modalities**
- DNA methylation
- Gene expression
- Genomic alterations

**Role in This Project**
CCLE methylation data may be incorporated in later stages to explicitly model epigenetic contributions to drug resistance.

**Scope**
Documented as a planned extension beyond the MVP.

---

## 3. Drug Perturbation and Repurposing Databases

### 3.1 LINCS / Connectivity Map (CMap)

**Description**  
The Library of Integrated Network-Based Cellular Signatures (LINCS) project provides transcriptomic responses of human cell lines to a wide range of chemical and genetic perturbations, primarily measured using the L1000 platform.

**Data Modalities**
- Gene expression signatures following compound treatment

**Role in This Project**
LINCS/CMap will be used to perform in silico drug repurposing by identifying compounds whose transcriptional signatures are negatively correlated with resistance-associated gene signatures.

**Rationale for Selection**
- Established framework for signature-based drug repurposing
- Direct compatibility with gene-level resistance signatures
- Extensive documentation and community use

**Scope**
Core component of the repurposing analysis for the MVP.

---

### 3.2 DrugBank

**Description**  
DrugBank is a curated database containing detailed information on approved and experimental drugs, including molecular targets, mechanisms of action, and pharmacological properties.

**Role in This Project**
DrugBank will be used to annotate and interpret candidate compounds emerging from the repurposing analysis.

**Scope**
Used for biological contextualization and interpretation.

---

### 3.3 ChEMBL — Optional Extension

**Description**  
ChEMBL is a large-scale bioactivity database containing information on drug-like molecules and their interactions with biological targets.

**Role in This Project**
ChEMBL may be used to complement DrugBank annotations and explore additional target–compound relationships.

**Scope**
Optional extension, not required for the MVP.

---

## 4. Dataset Selection Criteria

Datasets were selected based on the following criteria:
- Public availability and clear licensing terms
- Compatibility with reproducible computational workflows
- Sufficient sample size for machine learning applications
- Alignment with resistance modeling and drug repurposing objectives
- Community acceptance and prior use in peer-reviewed studies

---

## 5. Data Access and Reproducibility Considerations

- All datasets will be accessed through official distribution channels.
- Data versions and release dates will be documented at the time of download.
- Raw data will be preserved separately from processed data to ensure traceability.
- Scripts for data ingestion and preprocessing will be version-controlled and fully reproducible.

---

## 6. Summary

The initial implementation of this project is anchored on DepMap gene expression data and PRISM drug response measurements, complemented by LINCS/CMap for drug repurposing analyses. Additional omics layers and annotation resources are explicitly documented as extensions to maintain clarity of scope and avoid overcommitment at early stages.
