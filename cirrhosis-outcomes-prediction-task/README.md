# Cirrhosis Outcomes Prediction Task (AOAI Task 4)

Predict the outcome of patients with liver cirrhosis from clinical measurements.

- **Kaggle:** https://www.kaggle.com/competitions/cirrhosis-outcomes-prediction-task (invitation only)
- **Type:** tabular multi-class classification
- **Metric:** multi-class log loss (lower is better)

| Class | Meaning |
|---|---|
| `C` | Censored: alive at the end of the study (67%) |
| `CL` | Censored due to a liver transplant (3%) |
| `D` | Deceased (30%) |

## Data

| File | Rows | Content |
|---|---|---|
| `train.csv` | 15,000 | `id`, 18 clinical features, `Status` (target) |
| `test.csv` | 10,000 | `id` + the same features |
| `sample_submission.csv` | 10,000 | `id,Status_C,Status_CL,Status_D` |

Features: `N_Days`, `Drug`, `Age` (days), `Sex`, `Ascites`, `Hepatomegaly`, `Spiders`, `Edema`,
`Bilirubin`, `Cholesterol`, `Albumin`, `Copper`, `Alk_Phos`, `SGOT`, `Tryglicerides`, `Platelets`,
`Prothrombin`, `Stage`. About 44% of rows are missing the same block of columns.

## Submission

One probability per class; each row should sum to 1.

```csv
id,Status_C,Status_CL,Status_D
15000,0.62,0.03,0.35
```

## Files

| File | Audience | Content |
|---|---|---|
| `baseline.ipynb` | Students | EDA, preprocessing, feature engineering, LightGBM with stratified 5-fold CV, OOF analysis, submission |
| `teacher.ipynb` | Organizers only | Reference solution (LightGBM + XGBoost), experiments and grading guide |

## Running

```bash
kaggle competitions download cirrhosis-outcomes-prediction-task -p cirrhosis-outcomes-prediction-task/data
unzip -o cirrhosis-outcomes-prediction-task/data/*.zip -d cirrhosis-outcomes-prediction-task/data
cd cirrhosis-outcomes-prediction-task && jupyter notebook baseline.ipynb
```

## Teaching points

1. **Log loss scores probabilities.** `class_weight='balanced'` inflates the rare `CL` class and
   miscalibrates every prediction. Removing it is the biggest improvement.
2. **Don't invent values.** The same block of columns is missing for ~44% of rows. Median imputation
   fills half the rows with one fake number. Let the trees handle `NaN` natively instead.
   The outcome mix is almost the same with or without the block, so the flag itself adds little.

## Changes from the original Kaggle baseline

- Removed pipeline steps the notebook claimed but did not do ("merge original PBC dataset",
  "calibrate probabilities").
- **Fixed leakage-prone imputation:** medians are now computed on train and applied to test
  (before, each set used its own medians). Removed the unused `fit_encoders` argument and `LabelEncoder`.
- Numbered sections, intro with metric and submission format, ideas-to-improve section.
- 10 folds → 5 folds; data path works on Kaggle and locally.
- `class_weight='balanced'` is **kept on purpose** as the trap students should find (see the teacher notebook).
