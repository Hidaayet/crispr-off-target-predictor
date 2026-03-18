# CRISPR Off-Target Predictor

A Transformer-based deep learning model that predicts where CRISPR-Cas9
might accidentally cut in the genome — a critical safety problem in
gene therapy and genome editing research.

>  Project status: In progress — Phase 1 (data exploration)

---

## What it does

Given a 20-nucleotide guide RNA sequence and a candidate DNA site in the
genome, the model predicts whether Cas9 will accidentally cleave at that
location. An attention mechanism learns which nucleotide positions matter
most for off-target activity — directly interpretable as biological insight.

---

## Why this matters

Off-target cuts by CRISPR-Cas9 can cause:
- Unintended mutations in healthy genes
- Activation of oncogenes (cancer risk)
- Failure of gene therapy treatments

Predicting these sites computationally — before any experiment — saves
months of lab work and makes gene editing safer.

---

## Biological background

CRISPR-Cas9 uses a 20-nucleotide guide RNA to find its target. It tolerates
mismatches between the guide and the DNA, especially near the 5' end. The
3' end (PAM-proximal region, positions 17-20) is highly sensitive — even
one mismatch here usually prevents cutting.

The model learns this position-dependent sensitivity automatically from data.

---

## Tech stack

| Layer | Technology |
|---|---|
| Dataset | GUIDE-seq experimental data |
| Sequence encoding | One-hot + mismatch features |
| Model | Transformer encoder with attention |
| Deep learning | PyTorch |
| Visualization | Attention weight heatmaps |

---

## Project structure
```
crispr-off-target-predictor/
├── data/
│   └── (download instructions in docs/SPEC.md)
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_model_training.ipynb
│   └── 04_evaluation.ipynb
├── src/
│   ├── dataset.py
│   ├── model.py
│   └── train.py
├── docs/
│   └── SPEC.md
└── README.md
```

---

## Progress log

- [x] Project defined and documented
- [ ] Data exploration
- [ ] Feature engineering
- [ ] Transformer model training
- [ ] Evaluation and attention visualization

---

## Author

**Hidayet Allah Yaakoubi**
BME student — Tunisia
