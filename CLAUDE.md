# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Python/Jupyter data science project for the [Kaggle LISH-MoA competition](https://www.kaggle.com/c/lish-moa): predict which of 206 biological mechanisms of action (MoA) a drug activates, given gene expression and cell viability measurements. It is a **multi-label binary classification** problem.

## Running the Notebook

```bash
cd src
jupyter lab
# or
jupyter notebook
```

No `requirements.txt` exists yet. Minimum dependencies: `pandas`, `numpy`, `scikit-learn`, `jupyter`.

## Data

All data lives in `data/`. Do not commit CSVs — they are large (up to 150 MB).

### Feature columns (`train_features.csv`, `test_features.csv`)
- `sig_id` — sample identifier
- `cp_type` — `trt_cp` (drug treatment) or `ctl_vehicle` (negative control; MoA labels are all 0 for controls)
- `cp_time` — treatment duration: `24`, `48`, or `72` hours
- `cp_dose` — dose: `D1` (low) or `D2` (high)
- `g-0` … `g-771` — 772 normalized gene expression features
- `c-0` … `c-99` — 100 normalized cell viability features

### Target columns
- `train_targets_scored.csv` — 206 binary MoA labels (the competition target)
- `train_targets_nonscored.csv` — 402 additional binary MoA labels usable for auxiliary/multi-task training

### Other files
- `train_drug.csv` — maps `sig_id` → anonymized `drug_id`; multiple sig_ids share a drug_id (different dose/time). Critical for avoiding data leakage in cross-validation (group by `drug_id`).
- `sample_submission.csv` — 3,982 test rows × 206 columns, initialized to 0.5

### Dataset sizes
- Train: 23,814 rows | Test: 3,982 rows
- 876 input columns total (4 metadata + 772 gene + 100 cell viability)

## Architecture

Work lives in `src/data_processing.ipynb`. The `src/trial/` directory is empty and available for experiments.

### Key modeling considerations
- **Control samples** (`cp_type == 'ctl_vehicle'`) always have all-zero targets — predict 0 for these directly rather than running them through a model.
- **Group-aware CV**: use `drug_id` as the group key when splitting folds to prevent the same drug from appearing in both train and validation.
- **Evaluation metric**: mean column-wise log loss across all 206 target columns.
