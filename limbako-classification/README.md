# Limbako Classification

*Can you teach a machine to understand a language it has never seen before? Again?*

4,200 unlabeled statements in "Limbako", a language with no dictionary. Find the 6 hidden topics without
any labels, name them, and label 2,400 test statements.

- **Kaggle:** https://www.kaggle.com/competitions/limbako-classification (private)
- **Type:** unsupervised text clustering
- **Metric:** Macro F1

## Data

| File | Rows | Content |
|---|---|---|
| `train_unlabeled.csv` | 4,200 | `statement` (no id, no label) |
| `test.csv` | 2,400 | `id,statement` |

The six labels: `agriculture`, `education`, `finance`, `health`, `sports`, `technology`.

## Submission

`label` must be one of the six names. Cluster numbers score about 0.

```csv
id,label
0,sports
1,technology
```

## Files

| File | Audience | Content |
|---|---|---|
| `baseline.ipynb` | Students | Vocabulary exploration, CountVectorizer + LDA, manual topic naming, silhouette, submission |
| `teacher.ipynb` | Organizers only | TF-IDF + agglomerative clustering (best), LDA/NMF/K-Means comparison, automatic naming, grading guide |

## Running

```bash
kaggle competitions download limbako-classification -p limbako-classification/data
unzip -o limbako-classification/data/*.zip -d limbako-classification/data
cd limbako-classification && jupyter notebook baseline.ipynb
```

**Teacher evaluation (optional).** Section 8 of `teacher.ipynb` scores each approach when the private
labels are in `limbako-classification/labels/`:
- `solution_private.csv` with columns `id,label`
- `train.csv` with columns `statement,label`

These files are not on Kaggle's public API. Without them, the notebook skips evaluation and still
writes a submission. `labels/` is git-ignored.

## Expected scores (Macro F1)

| Approach | Macro F1 |
|---|---|
| K-Means on TF-IDF (the classic trap) | ~0.15–0.20 |
| NMF | ~0.40–0.45 |
| LDA (`baseline.ipynb`, correctly named) | ~0.50–0.65 |
| **TF-IDF + agglomerative (cosine, average) + centroid prediction** (`teacher.ipynb`) | **~0.87–0.90** |

Best public leaderboard score during the competition: 0.869.

## Organizer note: Limbako is Swahili

The cluster top words are real Swahili (*shamba* = farm, *daktari* = doctor, *simu* = phone,
*elimu* = education, *fedha* = money, *timu* = team, …). Students who know Swahili can name the clusters
by sight, or even label statements by hand. For a future edition, consider a synthetic vocabulary.

## Changes from the original Kaggle notebooks

**Baseline**
- **Fixed the submission bug:** the published notebook wrote cluster IDs (`0–5`) instead of label names.
- Added a check that each label is used exactly once in `TOPIC_TO_LABEL`.
- Removed `%pip install` (everything is preinstalled); added a metric/submission table; numbered sections.

**Teacher** (based on your earlier private teacher notebook)
- It read `../limbako_dataset/...` files that don't exist on Kaggle, so it crashed on the first cell.
  Evaluation is now optional (`labels/` folder).
- The submission used the cluster → label mapping learned from the private solution, which counts as
  label leakage. It now names clusters with a small Swahili keyword list, as a student could.
- Added a "named F1 vs. best possible (Hungarian) F1" comparison and a K-Means reference run.
