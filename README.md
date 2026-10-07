# Feature Selection – Course Exam Project

Exam notebook for **Feature Selection** course (https://github.com/sib-swiss/feature-selection-training/). 

The task: identify genes in TCGA breast cancer (BRCA) RNA-seq data that predict patient prognosis, from a very large number of features (~tens of thousands of genes) and a small number of samples.

## Steps

1. **Prior-knowledge filtering** – remove pseudogenes (non-coding) using the `gene_type` annotation.
2. **Univariate variance filtering** – log-transformed gene variance shows a bimodal distribution; the antimode is found automatically with **Otsu's threshold**, reducing the data to ~12k genes.
3. **Multivariate filtering (with cross-validation)** – Random Forest importance ranking to keep the top candidates, followed by the **Boruta** all-relevant feature selection algorithm.
4. **Evaluation** – `RepeatedStratifiedKFold` cross-validation (small sample size makes a single train/test split unreliable), with feature selection performed inside each fold to avoid data leakage; performance measured by AUC.
5. **Stability analysis** – genes selected in at least 5 of 10 folds are considered robust and interpreted biologically (e.g. PGK1, a glycolytic enzyme implicated in breast cancer, was selected in 9/10 folds), distinguishing genes linked to good vs. poor prognosis.


## Packages

Python, pandas, NumPy, scikit-learn, Boruta (`BorutaPy`), scikit-image, seaborn, matplotlib.

## Files

- `FEATS_Bruzzone.ipynb` – the full analysis.

## Data

The notebook expects these files in the working directory (not included in this repository):

- `TGCA_BRCA_expression_matrix.TPM.csv.gz` – TCGA BRCA expression matrix (TPM)
- `TGCA_BRCA_clinical_filtered.small.csv` – clinical data with the `poor_prognosis` label