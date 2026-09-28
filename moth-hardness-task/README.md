# Moth Hardness Task (AOAI Task 3)

Predict the **Mohs hardness** of a mineral (1 = talc … 10 = diamond) from averaged physical and chemical
properties of its elements.

- **Kaggle:** https://www.kaggle.com/competitions/moth-hardness-task (invitation only)
- **Type:** tabular regression
- **Metric:** Median Absolute Error (lower is better)

## Data

| File | Rows | Content |
|---|---|---|
| `train.csv` | 15,000 | `id`, `Hardness` (target), 11 numeric features |
| `test.csv` | 10,000 | `id` + the same 11 features |
| `sample_submission.csv` | 10,000 | `id,Hardness` |

Features: `allelectrons_Total`, `density_Total`, `allelectrons_Average`, `val_e_Average`,
`atomicweight_Average`, `ionenergy_Average`, `el_neg_chi_Average`, `R_vdw_element_Average`,
`R_cov_element_Average`, `zaratio_Average`, `density_Average`.

## Submission

```csv
id,Hardness
15000,4.5
15001,6.0
```

## Files

| File | Audience | Content |
|---|---|---|
| `baseline.ipynb` | Students | Starter notebook: EDA, feature engineering, LightGBM (L2) with 5-fold CV, submission |
| `teacher.ipynb` | Organizers only | Reference solution, experiments and grading guide |

## Running

```bash
# from the repo root, with the venv active
kaggle competitions download moth-hardness-task -p moth-hardness-task/data
unzip -o moth-hardness-task/data/moth-hardness-task.zip -d moth-hardness-task/data
cd moth-hardness-task && jupyter notebook baseline.ipynb
```

On Kaggle, attach the competition data and the notebooks find it automatically.

## Scores (5-fold OOF, local run)

| Notebook | Approach | OOF Median AE |
|---|---|---|
| — | Predict the median everywhere | 1.20 |
| `baseline.ipynb` | LightGBM, squared-error loss | ~0.70 |
| `teacher.ipynb` | LightGBM, **L1 loss** | ~0.57 |
| `teacher.ipynb` | L1 loss + **snapping to the 10 most frequent hardness values** | **0.50** |

Best public leaderboard score during the competition: 0.532.

## Teaching points

1. **Match the loss to the metric.** Squared error predicts the mean, absolute error predicts the median.
   The metric is a median, so an L1 objective is the biggest win.
2. **Look at the target before modelling.** Hardness is nearly discrete (6.0, 5.5, 2.5, …). Snapping
   close predictions to those values makes many of them exactly right, which pulls the median error down.

## Changes from the original Kaggle baseline

- Added a proper introduction (task, metric, submission format) and numbered sections; fixed the
  truncated title and the "Load & Merge" heading (nothing was merged).
- The notebook now reports **Median AE**, the actual competition metric, instead of only MAE.
- Removed the incorrect "best params found via Optuna" claim; 10 folds → 5 folds to run faster.
- Data path works on Kaggle and locally (`./data`).
