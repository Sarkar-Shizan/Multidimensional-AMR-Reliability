# Trustworthy and Explainable Artificial Intelligence for Antimicrobial Resistance Prediction

### A Multi-Cohort Evaluation Across Genomic, Clinical, and Epidemiological Data

<p align="center">

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Multi--Model-orange)
![Explainable AI](https://img.shields.io/badge/XAI-SHAP%20%7C%20LIME-purple)
![Evaluation](https://img.shields.io/badge/Evaluation-Multi--Cohort-green)
![Reproducibility](https://img.shields.io/badge/Reproducibility-Seed%2042-brightgreen)

</p>

## 1. Overview

Antimicrobial resistance (AMR) is a major global public-health challenge that requires reliable approaches for predicting antibiotic resistance and interpreting machine-learning outputs.

This project presents a multi-cohort computational investigation of trustworthy and explainable artificial intelligence for AMR-related prediction. It examines predictive discrimination, probability calibration, conformal uncertainty, selective prediction, explainability, statistical uncertainty, and computational integrity across three heterogeneous datasets.

The study integrates three distinct prediction settings:

1. **MTB:** Genomic drug-resistance prediction using single-nucleotide polymorphism (SNP) features in *Mycobacterium tuberculosis*.
2. **DYRAD:** Demographic-only antibiotic susceptibility prediction using age and gender.
3. **Dataverse:** Species-specific carbapenem-resistant (CARB) versus extended-spectrum β-lactamase (ESBL) phenotype categorisation using epidemiological information, with and without multilocus sequence typing (MLST).

The project evaluates **11 frozen independent-test classifier configurations** through a shared reporting approach while preserving cohort-specific prediction targets, data partitions, and methodological assumptions.

> **Important scope distinction:** This is a heterogeneous multi-cohort reliability assessment, not external validation of one common predictive model. The Dataverse CARB-versus-ESBL task is phenotype-category classification, not antibiotic-specific susceptibility prediction.

## 2. Research Objectives

The principal objectives are:

- Evaluate predictive discrimination across different AMR-related prediction settings.
- Examine whether probability calibration provides information beyond discrimination.
- Investigate marginal and class-specific conformal prediction-set behaviour.
- Assess selective prediction and errors among accepted predictions.
- Examine local and global explanation behaviour using SHAP and LIME.
- Evaluate explanation stability and cross-method agreement.
- Quantify statistical uncertainty through bootstrap confidence intervals.
- Assess MLST-associated performance differences using paired statistical comparisons and multiplicity correction.
- Verify consistency between archived model predictions, calculated metrics, and retained computational artifacts.

## 3. Project Architecture

The study uses three separate cohorts with different feature representations and prediction targets.

| Cohort | Data modality | Prediction task | Evaluation unit |
|---|---|---|---|
| MTB | Genomic SNP features | Ciprofloxacin and isoniazid resistance | Isolate |
| DYRAD | Age and gender | Gentamicin, levofloxacin and ciprofloxacin resistance | Patient |
| Dataverse | Epidemiological features and optional MLST | Within-species CARB versus ESBL | Frozen split group |

### 3.1 MTB — Genomic Drug-Resistance Prediction

The MTB component investigates antibiotic-specific resistance prediction using genomic SNP representations.

**Characteristics:**

- Organism: *Mycobacterium tuberculosis*.
- Prediction targets: Ciprofloxacin and isoniazid resistance.
- Feature representation: SNP-level genomic features.
- Feature selection: Chi-square-based feature selection.
- Selected feature budget: 10,000 SNPs per drug.
- Data partitioning: Isolate-disjoint 80:20 split.
- Development evaluation: Five-fold cross-validation.
- Final models:
  - CatBoost for ciprofloxacin.
  - LightGBM for isoniazid.
- Classification threshold: 0.50.

The genomic analysis includes feature-quality assessment, class-imbalance analysis, model benchmarking, feature selection, prediction verification, calibration, conformal diagnostics, selective prediction, and explainability.

### 3.2 DYRAD — Demographic-Only Clinical Susceptibility Prediction

The DYRAD component evaluates the predictive limitations of demographic-only information.

**Characteristics:**

- Predictors: Age and gender.
- Outcomes: Antibiotic-specific resistance.
- Antibiotics:
  - Gentamicin.
  - Levofloxacin.
  - Ciprofloxacin.
- Data partitioning: Patient-disjoint approximately 70:15:15.
- Training cap: 12,000 observations per task.
- Model selection: Validation-based evaluation.
- Threshold selection: Validation-based Matthews correlation coefficient (MCC).

Final configurations:

| Antibiotic | Model |
|---|---|
| Gentamicin | XGBoost |
| Levofloxacin | CatBoost |
| Ciprofloxacin | Random Forest |

The restricted predictor set is intentional. The analysis does not assume that richer clinical variables are available at the time of prediction.

### 3.3 Dataverse — Epidemiological and MLST-Based Phenotype Classification

The Dataverse component investigates whether adding MLST information changes within-species CARB-versus-ESBL classification performance.

**Species:**

- *Escherichia coli*
- *Klebsiella pneumoniae*
- *Enterobacter hormaechei*

**Feature configurations:**

1. Epidemiological predictors only (EPI).
2. Epidemiological predictors combined with MLST (EPI + MLST).

**Characteristics:**

- Species-specific classification.
- Group-disjoint training, validation and test partitions.
- Eight benchmarked model families.
- Up to three predefined parameter variants.
- Validation-based MCC threshold selection.
- Frozen independent-test evaluation.

The Dataverse task should not be interpreted as direct antibiotic-specific susceptibility prediction or as a complete genomic resistance-prediction framework.

## 4. Common Reliability Assessment

The project examines five complementary reliability dimensions.

| Dimension | Evaluation methods |
|---|---|
| Discrimination and classification | AUROC, average precision (AP), MCC, sensitivity and specificity |
| Probability calibration | Brier score, expected calibration error (ECE), sigmoid and isotonic recalibration |
| Prediction sets and abstention | Conformal prediction sets, empirical coverage, set sizes and selective risk |
| Explainability | SHAP, LIME, permutation importance, explanation stability and agreement |
| Statistical uncertainty | Bootstrap confidence intervals, paired comparisons and Holm correction |

These dimensions are complementary rather than interchangeable. Strong discrimination does not independently establish calibrated probabilities, reliable prediction sets, explanation consistency, or clinical utility.

### Cohort-specific evaluation

- **MTB:** Includes genomic SHAP and LIME analyses, conformal diagnostics, selective prediction, calibration, and explanation-stability assessments.
- **DYRAD:** Includes probability calibration, exploratory conformal diagnostics, bootstrap uncertainty, and permutation importance.
- **Dataverse:** Includes calibration, exploratory conformal evaluation, permutation importance, and paired MLST-associated comparisons.

Not every diagnostic was performed under identical conditions across all cohorts. In particular, some DYRAD and Dataverse calibration and conformal analyses are exploratory because of validation-data reuse during model or threshold selection.

## 5. Independent-Test Results

The final audit reports 11 frozen classifier configurations.

| Cohort | Prediction task | Model | Test N | Positive N | AUROC | AP | MCC | Brier |
|---|---|---|---:|---:|---:|---:|---:|---:|
| MTB | Ciprofloxacin | CatBoost | 89 | 7 | 0.9338 | 0.8065 | 0.8350 | 0.0220 |
| MTB | Isoniazid | LightGBM | 1,547 | 689 | 0.9679 | 0.9694 | 0.8415 | 0.0620 |
| DYRAD | Gentamicin | XGBoost | 11,676 | 1,537 | 0.6060 | 0.2005 | 0.1636 | 0.1115 |
| DYRAD | Levofloxacin | CatBoost | 10,390 | 2,645 | 0.5751 | 0.3246 | 0.0986 | 0.1864 |
| DYRAD | Ciprofloxacin | Random Forest | 10,144 | 2,504 | 0.5717 | 0.3020 | 0.1059 | 0.2469 |
| Dataverse | *E. coli* (EPI) | XGBoost | 121 | 5 | 0.6336 | 0.1508 | -0.0190 | 0.0404 |
| Dataverse | *E. coli* (EPI + MLST) | XGBoost | 121 | 5 | 0.7698 | 0.3538 | 0.3351 | 0.0378 |
| Dataverse | *K. pneumoniae* (EPI) | LightGBM | 61 | 9 | 0.4936 | 0.1763 | 0.1122 | 0.1710 |
| Dataverse | *K. pneumoniae* (EPI + MLST) | LightGBM | 61 | 9 | 0.6453 | 0.2405 | 0.0267 | 0.1607 |
| Dataverse | *E. hormaechei* (EPI) | Extra Trees | 18 | 15 | 0.5333 | 0.8849 | -0.0555 | 0.2596 |
| Dataverse | *E. hormaechei* (EPI + MLST) | Extra Trees | 18 | 15 | 0.5889 | 0.9134 | 0.0000 | 0.2541 |

**Interpretation cautions:**

- MTB ciprofloxacin results are based on only seven resistant test isolates.
- DYRAD demonstrates weak discrimination using age and gender alone.
- Dataverse results have very small test-set minority classes.
- AP depends on positive-class prevalence and should not be compared without considering class distributions.
- The *E. hormaechei* test set contains 15 CARB observations out of 18; its high AP values do not independently demonstrate strong discrimination.

The three cohorts must not be combined into one overall performance score.

## 6. Statistical Uncertainty and Paired Inference

### 6.1 MTB Bootstrap Confidence Intervals

The final notebook reports 95% isolate-bootstrap percentile confidence intervals.

| Drug | Metric | Estimate | 95% CI |
|---|---|---:|---|
| Ciprofloxacin | AUROC | 0.9338 | [0.8011, 1.0000] |
| Ciprofloxacin | AP | 0.8065 | [0.4667, 1.0000] |
| Ciprofloxacin | MCC | 0.8350 | [0.5639, 1.0000] |
| Ciprofloxacin | Brier | 0.0220 | [0.0005, 0.0540] |
| Isoniazid | AUROC | 0.9679 | [0.9590, 0.9761] |
| Isoniazid | AP | 0.9694 | [0.9611, 0.9768] |
| Isoniazid | MCC | 0.8415 | [0.8134, 0.8678] |
| Isoniazid | Brier | 0.0620 | [0.0539, 0.0710] |

The notebook attempted 2,000 unstratified bootstrap replicates per drug:

- Ciprofloxacin: 1,996 valid replicates.
- Isoniazid: 2,000 valid replicates.

The ciprofloxacin confidence intervals are comparatively wide because of the small resistant-class denominator.

### 6.2 Dataverse Paired Comparisons

Six prespecified paired comparisons evaluate EPI versus EPI + MLST:

- AUROC for three species.
- AP for three species.

The final analysis uses group-preserving paired randomisation results and Holm multiplicity correction.

**Result: 0 of 6 comparisons remained statistically significant after Holm correction.**

Observed performance differences should therefore be interpreted as descriptive or exploratory, not as confirmed MLST superiority.

### 6.3 Conformal Evaluation

Conformal prediction is examined through prediction-set composition, empirical coverage, set sizes and selective behaviour.

The project distinguishes empirical observations from formal coverage guarantees. Due to model-selection and validation-data dependencies, the reported analyses do not establish formal post-selection conformal coverage guarantees.

The final integration did not reverify MTB class-conditional conformal coverage.

## 7. Explainability and Interpretation

### 7.1 MTB: SHAP and LIME

The MTB explainability analysis includes:

- Global SHAP feature importance.
- Local LIME explanations.
- SHAP–LIME rank agreement.
- SHAP bootstrap stability.
- Corrected LIME feature-set stability.
- Exploratory SNP co-occurrence hypergraphs.

Recorded mean local SHAP–LIME rank correlations:

| Drug | Mean Spearman correlation |
|---|---:|
| Ciprofloxacin | Approximately -0.646 |
| Isoniazid | Approximately -0.373 |

The selected-case analysis indicates that locally faithful explanations may still disagree across attribution methods.

The corrected ciprofloxacin LIME top-10 pairwise Jaccard stability was approximately 0.129 for selected cases.

These findings are exploratory and do not establish population-wide explanation inconsistency.

### 7.2 DYRAD and Dataverse

Permutation importance is used to examine feature contributions in the demographic and epidemiological prediction settings.

SHAP, LIME and permutation importance represent different explanation approaches and should not be interpreted as directly interchangeable numerical measures.

### 7.3 Biological Interpretation Boundaries

The exploratory SNP co-occurrence hypergraphs do not establish biological causality or molecular interactions.

**Numerical HyperSHAP was not executed.** Standard SHAP analysis and SNP co-occurrence analysis should not be presented as substitutes for a numerical HPO-coalition HyperSHAP experiment.

## 8. Computational Workflow

The Jupyter notebook documents the project workflow, including data preparation, modelling, reliability evaluation and final integration.

| Stage | Main activities |
|---|---|
| Environment setup | Reproducibility configuration, hardware and package audit |
| Genomic preparation | SNP quality verification, phenotype alignment and feature preparation |
| MTB model development | Antibiotic-specific modelling, feature selection and model benchmarking |
| MTB reliability | Calibration, conformal prediction, selective prediction and robustness |
| DYRAD preparation | Patient-level data audit and prediction-time feature eligibility |
| DYRAD modelling | Demographic-only model benchmarking and selection |
| DYRAD reliability | Calibration, conformal diagnostics and feature importance |
| Dataverse preparation | Phenotype, species and split-group verification |
| Dataverse modelling | Species-specific EPI and EPI + MLST comparisons |
| Integration | Three-cohort metric and explainability reconciliation |
| Integrity audit | Prediction verification, artifact inventories and SHA-256 checks |
| Statistical finalization | Bootstrap confidence intervals and paired statistical tests |
| Publication export | Publication tables, figures, reports and final project archive |

### Final integration stages

- **Cell 62:** Three-cohort integration and prediction-metric verification.
- **Cell 63:** Explainability integration and compatibility checks.
- **Cell 64:** Computational integrity audit.
- **Cell 65:** Statistical uncertainty and paired-test finalization.
- **Cell 66:** Publication table, figure and project-package generation.

The notebook's recorded finalization includes:

- 201 integrity checks passed.
- 0 recorded failures in that audit.
- 500 source artifacts SHA-256 hashed.
- 44 metric confidence-interval records.
- Six prespecified Dataverse paired tests.
- 19 publication CSV tables.
- An Excel publication workbook.
- 14 newly generated publication figure pairs.

These results document the retained computational outputs; they do not independently establish that every upstream modelling decision was free from leakage or that the entire pipeline can be reproduced without its original inputs.

## 9. Repository Structure

The final project archive contains 1,785 entries.

```text
AMR_TRUSTWORTHY_AI/
│
├── configs/
│
├── data/
│   ├── processed/
│   │   ├── mtb/
│   │   ├── dyrad/
│   │   └── dataverse/
│   └── split_manifests/
│
├── models/
│   ├── MTB/
│   ├── DYRAD/
│   └── Dataverse/
│
├── results/
│   ├── metrics/
│   ├── predictions/
│   ├── explainability/
│   ├── calibration/
│   ├── conformal/
│   ├── selective_prediction/
│   ├── robustness/
│   ├── dyrad/
│   ├── dataverse/
│   ├── integration/
│   └── finalization/
│
├── figures/
│   ├── CALIBRATION/
│   ├── CONFORMAL/
│   ├── SELECTIVE_PREDICTION/
│   ├── ROBUSTNESS/
│   ├── integration/
│   └── finalization/
│
├── publication_package/
│   ├── tables/
│   │   ├── CSV/
│   │   └── AMR_Three_Cohort_Publication_Tables.xlsx
│   ├── figures/
│   │   ├── PNG_600DPI/
│   │   └── PDF_Vector/
│   ├── reports/
│   └── manifests/
│
└── README.md
```

### Main publication outputs

Selected publication-ready files include:

| File | Description |
|---|---|
| `Table01_Independent_Test_Performance.csv` | Independent-test results for 11 configurations |
| `Table02_Bootstrap_95CI.csv` | Bootstrap confidence intervals |
| `Table03_MTB_Classification_Diagnostics.csv` | MTB sensitivity, specificity and interval estimates |
| `Table04_Dataverse_Holm_Tests.csv` | Paired Dataverse comparisons and Holm correction |
| `Table07_Explainability_Coverage.csv` | Cohort-specific explainability coverage |
| `Table09_Cohort_Outcome_Compatibility.csv` | Outcome compatibility across cohorts |
| `final_three_cohort_summary.json` | Final research audit summary |

The complete publication outputs are retained in the `publication_package/` directory.

## 10. Computational Environment

The recorded research environment uses Python 3.13.15 and a Kaggle execution environment.

### Recorded software versions

| Package | Version |
|---|---|
| Python | 3.13.15 |
| NumPy | 2.1.3 |
| Pandas | 2.3.3 |
| SciPy | 1.16.3 |
| Scikit-learn | 1.6.1 |
| XGBoost | 3.4.1 |
| LightGBM | 4.6.0 |
| CatBoost | 1.2.10 |
| SHAP | 0.52.0 |
| Matplotlib | 3.10.0 |
| Seaborn | 0.13.2 |
| Imbalanced-learn | 0.14.2 |
| Optuna | 5.0.0 |
| PyTorch | 2.11.0+cu128 |

LIME was used in the project; its exact package version was unavailable in the recorded environment report.

### Hardware

- GPU: NVIDIA Tesla T4.
- CUDA: 12.8.
- Execution platform: Kaggle.
- Random seed: 42.

### Reproducibility configuration

The project uses a fixed random seed of 42 for the recorded experimental workflow. Exact reproducibility may additionally depend on hardware, library versions, data availability, and the original split and preprocessing artifacts.

## 11. Running the Notebook

The main notebook is:

`amr-trustworthy-ai-1.0.ipynb`

It contains 123 cells:

- 116 code cells.
- 7 Markdown cells.

### Recommended execution environment

Kaggle Notebooks with Python 3 is the intended execution environment.

### Basic setup

1. Download or clone the GitHub repository.
2. Open the notebook in Kaggle or a compatible Jupyter environment.
3. Attach the original datasets through their permitted sources.
4. Configure the required input and working directories.
5. Install or verify the recorded package dependencies.
6. Review the original cohort-specific data and split requirements.
7. Execute the appropriate data-preparation and modelling stages.
8. Run reliability and explainability analyses.
9. Execute the integration and finalization stages.
10. Inspect generated metrics, reports, figures and integrity manifests.

### Important reproduction note

The final project archive preserves processed data, models, predictions, results and computational artifacts. However, the original raw Kaggle input datasets are not included.

The notebook also contains Kaggle-specific paths. A local execution environment will require path adaptation and access to the original datasets.

Running only Cells 62–66 performs final integration and verification; it does not recreate all earlier model training and preprocessing stages from scratch.

The recorded finalization output should not be interpreted as proof of complete end-to-end reproducibility.

## 12. Project Archive

The final three-cohort project archive contains:

| Item | Recorded value |
|---|---:|
| Project entries | 1,785 |
| Uncompressed size | 622.92 MB |
| ZIP size | Approximately 144.57 MiB |
| PNG files | 114 |
| PDF files | 104 |
| Publication CSV tables | 19 |
| Excel workbook | Included |
| ZIP integrity test | Passed |

The separately supplied Jupyter notebook must be retained alongside the archive. The generated ZIP itself does not contain the standalone notebook.

## 13. Scientific Limitations

The findings should be interpreted within the following limitations:

1. **Heterogeneous prediction tasks:** The three cohorts have different prediction targets and feature representations.
2. **Limited external validation:** No outcome-compatible common-model external validation was conducted.
3. **Small minority classes:** MTB ciprofloxacin and Dataverse species-specific evaluations contain limited minority-class observations.
4. **Restricted clinical predictors:** DYRAD uses age and gender rather than a comprehensive clinical feature set.
5. **Restricted genomic information:** Dataverse MLST is not equivalent to a complete SNP or gene-presence representation.
6. **Non-significant paired comparisons:** None of the six Dataverse comparisons remained significant after Holm correction.
7. **Explanation instability:** Selected MTB cases showed negative SHAP–LIME agreement and limited LIME stability.
8. **Conformal limitations:** Validation and model-selection dependencies prevent formal post-selection coverage claims.
9. **Upstream provenance:** Saved-output integrity checks do not establish that every upstream modelling and preprocessing decision was leakage-free.
10. **No clinical deployment validation:** The project does not establish clinical readiness or prospective utility.
11. **HyperSHAP limitation:** Numerical HyperSHAP was not executed.

These limitations define the scope of the evidence and should be considered when interpreting the results.

## 14. Research Contributions

The principal contributions of this project are:

1. A common reliability-oriented assessment approach across three heterogeneous AMR-related prediction settings.
2. Independent-test prediction and metric verification across 11 frozen classifier configurations.
3. Evaluation of calibration, conformal uncertainty and selective prediction under cohort-specific constraints.
4. Investigation of explanation fidelity, stability and cross-method agreement.
5. Statistical uncertainty assessment through bootstrap confidence intervals and paired multiplicity-adjusted comparisons.
6. Computational integrity documentation through artifact inventories, checksums and publication-ready outputs.
7. Explicit differentiation between empirical reliability findings, theoretical guarantees and clinical validation requirements.

The project contributes a task-aware computational evaluation framework rather than a universal AMR prediction algorithm.

## 15. Data Availability

The final archive contains processed project artifacts, model outputs, prediction files, split manifests and publication materials.

The original raw input datasets are not included in the final ZIP. Access to the original data must be obtained from their respective providers under applicable licensing, access and data-use conditions.

Users must independently verify the permissions governing redistribution or reuse of processed data, genomic information, patient-related records, model artifacts and identifiers before accessing or redistributing these materials.

No restricted dataset should be redistributed through this repository without appropriate authorization.

## 16. Code Availability

The principal computational workflow is provided through the accompanying Jupyter notebook and project archive.

The notebook documents the modelling workflow, reliability assessments, statistical analyses and final integration procedures.

The archived computational outputs support inspection of the recorded experiments. Full reconstruction of the original training pipeline requires the corresponding upstream datasets, dependencies, split manifests and execution stages.

## 17. Ethical and Responsible Use

This project is intended for academic research and methodological investigation of machine-learning reliability in AMR-related prediction.

The reported models have not been established as clinically validated decision-support systems.

Predictions should not be used independently to determine antibiotic treatment, replace antimicrobial susceptibility testing, or guide patient management.

Responsible interpretation requires attention to class imbalance, data provenance, calibration, uncertainty, population differences and external validation.

## 18. Citation

If you use this project in academic research, please cite the associated manuscript:

**Trustworthy and Explainable Artificial Intelligence for Antimicrobial Resistance Prediction: A Multi-Cohort Evaluation Across Genomic, Clinical, and Epidemiological Data.**

Full bibliographic information should be added once the manuscript has a confirmed publication or preprint record.

## 19. License

The repository license is to be determined.

Until data redistribution rights, software ownership and third-party licensing conditions have been reviewed, no blanket permission to reuse or redistribute the complete project should be inferred.

## 20. Acknowledgment

This project brings together computational modelling, genomic data analysis, clinical microbiology, epidemiological information, explainable AI, uncertainty estimation and statistical verification.

The work aims to encourage more transparent, reproducible and scientifically cautious evaluation of machine-learning systems in antimicrobial resistance research.

---

**Project:** AMR_TRUSTWORTHY_AI  
**Research area:** Antimicrobial Resistance | Machine Learning | Explainable AI | Trustworthy AI  
**Environment:** Kaggle / Python  
**Reproducibility seed:** 42  
**Project version:** 1.0

