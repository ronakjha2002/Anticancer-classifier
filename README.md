# Anticancer Phytochemical Classifier

This was one of my main projects during my M.Sc. Bioinformatics program. The idea was pretty straightforward — can we use machine learning to predict whether a plant-derived compound (phytochemical) has anticancer activity or not? Turns out, yes, and it works really well.

---

## What I built

A binary classifier that takes a chemical compound (as a SMILES string) and predicts whether it's likely to be anticancer or not. I trained and compared three models — Random Forest, SVM, and KNN — and picked the best-performing one from 10 repeated runs.

The final SVM model hit **96.6% test accuracy** with an AUC of **0.9899**, which I was pretty happy with.

---

## Dataset

- ~4823 anticancer phytochemicals (positive class) — sourced from published databases
- ~4823 decoy/non-anticancer compounds (negative class)
- Total: ~9646 compounds, balanced dataset

Each compound was represented as a SMILES string and then converted into numerical features using RDKit.

---

## Feature Engineering

This was the most interesting part honestly. I used two types of molecular features:

- **RDKit molecular descriptors** — things like molecular weight, logP, number of H-bond donors/acceptors, ring counts etc. (~200 features)
- **ECFP fingerprints** (Morgan fingerprints, radius=2, 2048 bits) — captures the circular neighborhood around each atom

I concatenated both and then applied PCA for dimensionality reduction before training.

---

## Models & Results

I ran each model 10 times with different random seeds and saved the best-performing run for each.

| Model | Test Accuracy | Test AUC | F1-Score | MCC |
|-------|--------------|----------|----------|-----|
| Random Forest | 95.85% | 0.9884 | 0.9588 | 0.9172 |
| **SVM** | **96.63%** | **0.9899** | — | — |
| KNN | 93.94% | — | — | — |

SVM came out on top, so that's the one I'd use for any actual prediction task.

Hyperparameter tuning was done with GridSearchCV:
- RF: `max_depth=25`, `n_estimators=400`
- SVM: `C=100`, `gamma='auto'`
- KNN: `metric='euclidean'`

---


## Files in this repo

```
├── anticancer_classifier.ipynb       # main notebook
└── README.md
```

---

## Tech stack

Python, scikit-learn, RDKit, Pandas, NumPy, Matplotlib, Joblib

---

## Why I made this

During my internship at ACTREC (Tata Memorial Centre), I was working on cancer-related compound data and wanted to see if ML could actually distinguish anticancer phytochemicals from random drug-like compounds. This project came out of that curiosity. The results show that molecular descriptors + fingerprints together give a pretty strong signal, which makes sense biologically — anticancer compounds tend to have specific structural patterns.
