# Project Specification — CRISPR Off-Target Predictor

**Author:** Hidayet Allah Yaakoubi
**Date:** March 2026
**Status:** In progress

---

## 1. Project summary

A Transformer-based model that predicts CRISPR-Cas9 off-target cleavage
from DNA sequence features. Given a guide RNA and a candidate genomic site,
the model outputs a cleavage probability and attention weights showing which
nucleotide positions drive the prediction.

---

## 2. Background

CRISPR-Cas9 requires:
- A 20-nucleotide guide RNA (gRNA)
- A PAM sequence (NGG) adjacent to the target site

Cas9 tolerates mismatches between gRNA and DNA, particularly at the 5' end.
The PAM-proximal region (positions 17-20) is highly intolerant of mismatches.
Off-target sites typically have 1-6 mismatches with the on-target sequence.

---

## 3. Dataset — GUIDE-seq

| Property | Value |
|---|---|
| Source | Published GUIDE-seq experimental data |
| Reference | Tsai et al., Nature Biotechnology 2015 |
| Format | Guide RNA + DNA site pairs with cleavage labels |
| Task | Binary classification — cleaves / does not cleave |
| Features | 20-nt guide RNA, 20-nt DNA site, mismatch profile |

**GUIDE-seq** (Genome-wide Unbiased Identification of DSBs Evaluated by
Sequencing) is the gold standard experimental method for measuring
CRISPR off-target activity. It captures actual cleavage events in living
cells — not computational predictions.

---

## 4. Input representation

Each sample is a pair of aligned sequences:
```
Guide RNA:  5'- A T C G A T C G A T C G A T C G A T C G -3'
DNA site:   5'- A T C G A T C G A T C G A T C C A T C G -3'
                                              ↑   ↑
                                         mismatches
```

**Encoding:**
- One-hot encode each position: A=[1,0,0,0], T=[0,1,0,0],
  G=[0,0,1,0], C=[0,0,0,1]
- Encode the pair as a 20×8 matrix (guide + DNA concatenated per position)
- Add mismatch indicator: 1 if bases differ, 0 if same

Final input shape: (20, 9) per sample — 20 positions × 9 features

---

## 5. Model — Transformer encoder
```
Input: (batch, 20, 9) — sequence of 20 positions
    ↓
Linear projection → (batch, 20, 64)
    ↓
Positional encoding — tells model about position order
    ↓
Transformer encoder (4 layers, 8 heads)
    ↓
[CLS] token representation → (batch, 64)
    ↓
Classifier head → sigmoid → cleavage probability
```

**Why Transformer:**
- Self-attention captures long-range dependencies between positions
- Attention weights are directly interpretable as position importance
- Position encoding captures the known 5'-to-3' importance gradient

---

## 6. Evaluation metrics

| Metric | Description |
|---|---|
| ROC-AUC | Primary metric |
| Precision-Recall AUC | Important due to class imbalance |
| Accuracy | Overall correctness |
| Attention visualization | Which positions drive predictions |

---

## 7. Development phases

| Phase | Description |
|---|---|
| 1 | Data exploration and understanding |
| 2 | Feature engineering — sequence encoding |
| 3 | Transformer model training |
| 4 | Evaluation and attention visualization |

---

## 8. Dataset download

The GUIDE-seq dataset will be downloaded programmatically in the
data exploration notebook. No manual download required.
