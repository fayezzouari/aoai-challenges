# AOAI Kaggle Challenges

Baseline (student) and teacher (reference solution) notebooks for four African Olympiad of AI
Kaggle challenges. Each challenge has its own folder.

| Folder | Challenge | Type | Metric | Baseline → teacher |
|---|---|---|---|---|
| [`moth-hardness-task/`](moth-hardness-task/) | Moth Hardness (Task 3) | Tabular regression | Median AE ↓ | 0.70 → 0.50 |
| [`cirrhosis-outcomes-prediction-task/`](cirrhosis-outcomes-prediction-task/) | Cirrhosis Outcomes (Task 4) | Tabular multi-class | Log loss ↓ | 0.39 → 0.364 |
| [`limbako-classification/`](limbako-classification/) | Limbako Classification | Unsupervised text clustering | Macro F1 ↑ | ~0.55 → ~0.88 |
| [`sports-master/`](sports-master/) | Sports Master (SportSnap) | Image classification, 100 classes | Accuracy ↑ | 81.2% → 95.0% (test) |

Each folder contains:

```
<challenge>/
├── README.md        task, data, submission format, scores, teaching points, changes made
├── baseline.ipynb   starter notebook for students
├── teacher.ipynb    reference solution + grading guide ( organizers only)
└── data/            competition files (git-ignored, download with the Kaggle CLI)
```

> `teacher.ipynb` files contain full solutions. Keep this repository private, or remove the teacher
> notebooks before sharing it with students.

## Setup

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

Kaggle API credentials go in `~/.kaggle/kaggle.json` (Kaggle → Settings → API → Create New Token).
You must have joined a competition, or be its host, to download its data.

Download all the data:

```bash
for c in moth-hardness-task cirrhosis-outcomes-prediction-task limbako-classification sports-master; do
  kaggle competitions download "$c" -p "$c/data" && unzip -oq "$c/data/$c.zip" -d "$c/data" && rm "$c/data/$c.zip"
done
```

Every notebook finds its data automatically: on Kaggle under `/kaggle/input/...`, and locally in `./data`
(run Jupyter from inside the challenge folder).

## Uploading a notebook to Kaggle

```bash
cd moth-hardness-task
kaggle kernels init -p .          # creates kernel-metadata.json; set title, code_file, competition_sources
kaggle kernels push -p .
```
