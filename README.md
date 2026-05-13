# Spaceship titanic | Kaggle competition

![Platform](https://img.shields.io/badge/platform-Kaggle-20BEFF)
![Accuracy](https://img.shields.io/badge/accuracy-80%2B%25-brightgreen)
![Model](https://img.shields.io/badge/model-SVM-purple)
![Status](https://img.shields.io/badge/status-completed-brightgreen)

## 🔗 [Kaggle Notebook](https://www.kaggle.com/code/aisultanzhakupbaev/spaceship-2?scriptVersionId=302604723) <---- ссылка


# Spaceship titanic — Kaggle сlassification сompetition

![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)
![CatBoost](https://img.shields.io/badge/Model-CatBoost-yellow.svg)
![Kaggle](https://img.shields.io/badge/Kaggle-Competition-20BEFF.svg)
![Accuracy](https://img.shields.io/badge/Kaggle%20Score-~80%25-brightgreen.svg)
![Status](https://img.shields.io/badge/Status-Submitted-brightgreen.svg)

Binary classification task: predict which passengers were transported to an alternate dimension aboard the Spaceship Titanic.

[Competition on Kaggle](https://www.kaggle.com/competitions/spaceship-titanic)

---

## 📌 Project overview

The dataset contains passenger records from a fictional interstellar ship. The goal is to predict the `Transported` column (True/False) based on passenger metadata, cabin info, and onboard spending habits.

---

## Pipeline

```
train.csv + test.csv
        ↓
  EDA
  (class balance, nulls, crosstabs by CryoSleep / HomePlanet / VIP / Deck)
        ↓
  Feature engineering
  (12 new features — see table below)
        ↓
  Preprocessing
  (categorical NaN → "Missing", numeric NaN → train median)
        ↓
  CatBoost — 5-Fold stratified CV
        ↓
  Final model trained on full train set
        ↓
  submission.csv
```

---

## Feature Engineering

| Feature | Description |
|---|---|
| `Deck` | Extracted from `Cabin` (A / B / C …) |
| `CabinNum` | Cabin number, converted to numeric |
| `Side` | Port or Starboard side |
| `CabinGroup` | `Deck_Side` interaction |
| `CabinRegion` | CabinNum binned into 6 quantile groups |
| `GroupSize` | Number of passengers sharing a group ID |
| `IsAlone` | 1 if GroupSize == 1 |
| `TotalSpend` | Sum of all 5 spending columns |
| `NoSpend` | 1 if TotalSpend == 0 |
| `SpendPerPerson` | TotalSpend / GroupSize |
| `AgeGroup` | Age binned: Child / Teen / YoungAdult / Adult / Senior |
| `VIP_CryoSleep` | Interaction feature |
| `PlanetDestination` | HomePlanet + Destination interaction |
| `log_*` | Log1p transforms for all spending features |

---

## 📊 Results

### 5-Fold Cross-validation (train set)

| Fold | Accuracy |
|---|---|
| Fold 1 | 0.826|
| Fold 2 | 0.813|
| Fold 3 | 0.821|
| Fold 4 | 0.830|
| Fold 5 | 0.808|
| **Mean** | 0.819|

### Validation Set (80/20 split)

| Metric | Score |
|---|---|
| Accuracy | 0.82|
| F1 Score (macro) | 0.82|

### Kaggle public leaderboard

| Metric | Score |
|---|---|
| Accuracy | ~80% |

---

## Model

**CatBoostClassifier**

| Parameter | Value |
|---|---|
| `iterations` | 2000 |
| `learning_rate` | 0.03 |
| `depth` | 8 |
| `loss_function` | Logloss |
| `eval_metric` | Accuracy |
| CV strategy | StratifiedKFold, 5 folds |

CatBoost handles categorical features natively - no manual encoding needed. Categorical NaNs are filled with `"Missing"`, numeric NaNs with train median.

---

## How to Run

This notebook was built on Kaggle. To run it locally:

### 1. Download the data

```bash
kaggle competitions download -c spaceship-titanic
```

Or download manually from the [competition page](https://www.kaggle.com/competitions/spaceship-titanic/data).

### 2. Install dependencies

```bash
pip install pandas numpy catboost scikit-learn
```

### 3. Run the notebook

```bash
jupyter notebook spaceship-2.ipynb
```

---

## 📁 Project Structure

```
├── spaceship-2.ipynb    # Full pipeline: EDA → features → model → submission
├── submission.csv       # Generated predictions
└── README.md
```
