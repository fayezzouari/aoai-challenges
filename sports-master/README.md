# Sports Master (SportSnap)

*Can your model guess sports from one input image?*

Classify photos into **100 sports**, from basketball and tennis to sepak takraw and disc golf.

- **Kaggle:** https://www.kaggle.com/competitions/sports-master (private)
- **Type:** image classification (transfer learning)
- **Metric:** categorization accuracy (top-1)

## Data

| File | Content |
|---|---|
| `train/` + `train.csv` | 13,992 images, `image_id,label` (about 140 per class) |
| `test/` + `test.csv` | 500 images, `image_id` |

## Submission

`id` is the test `image_id`. `label` is the **class index** (0–99) in the **alphabetically sorted**
class list, which is what both notebooks produce.

```csv
id,label
air hockey_1.jpg,0
```

> The practice dataset (`fayezzouari/sports-classification`) used class **names** in `image_id,label`.
> These notebooks follow your final competition baseline (`id` + index). Check this matches the
> competition's solution file before running the challenge again.

## Files

| File | Audience | Content |
|---|---|---|
| `baseline.ipynb` | Students | Frozen MobileNetV2 + new head, 4 epochs, curves, top-1/top-5, per-class accuracy, confusion analysis, submission |
| `teacher.ipynb` | Organizers only | EfficientNet-B0, head training then full fine-tuning at `lr=1e-5`, test-time augmentation, grading guide |

## Running

A GPU is required in practice. On Kaggle, attach the competition data and turn on *Accelerator → GPU*.
Locally:

```bash
kaggle competitions download sports-master -p sports-master/data
unzip -o sports-master/data/sports-master.zip -d sports-master/data
```

## Scores (validation split, 15% of train; saved notebook outputs come from a local Apple-GPU run)

| Notebook | Approach | Val top-1 | Val top-5 |
|---|---|---|---|
| `baseline.ipynb` | Frozen MobileNetV2, 4 epochs | 75.8% | 95.5% |
| `teacher.ipynb` | EfficientNet-B0, 10 head epochs + 9 fine-tune epochs | 90.7% | 98.9% |

## Organizer note: test labels leak through file names

Test files are named `air hockey_1.jpg`, `ampute football_1.jpg`, …, so the label can be read from the
file name. The practice dataset's `sample_submission.csv` even contained the correct labels. The 1.000 at
the top of the leaderboard very likely comes from this. Before reusing the challenge, rename test images
to random IDs and give out a sample submission with dummy labels.

## Changes from the original Kaggle notebooks

**Baseline** (from `baseline-master`)
- Text said **EfficientNet-B0** and **10 epochs**, but the code trains **MobileNetV2** for **4 epochs**.
  The text now matches the code, including the final results printout.
- Deprecated `pretrained=True` → `weights=...` API.
- Device picks CUDA, then Apple `mps`, then CPU; DataLoader workers only on CUDA (macOS can't spawn workers for notebook-defined datasets).
- Removed the "Install" step (nothing was installed), fixed the seed, simplified top-5 and per-class
  accuracy code, numbered sections, added a submission-format table and ideas-to-improve section.

**Teacher** (from `teacher-sports-classification`)
- Pointed at the competition data instead of the practice dataset.
- Submission now matches the competition format (`id` + class index).
- Added test-time augmentation (horizontal flip), the leakage note and a grading guide.
