# Thai Social Media Sentiment Analysis for SET Index Prediction
> วิเคราะห์ความรู้สึกของคนไทยบนโซเชียลมีเดียเพื่อพยากรณ์ทิศทางดัชนีหุ้น SET

---

## Overview

This project investigates the relationship between Thai social media sentiment and SET Index movement direction (UP/DOWN). By combining **Natural Language Processing (NLP) for Thai language** with **Technical Indicators**, the best model achieved **83.7% accuracy** and **AUC = 0.843** — a +18.9% improvement over using technical indicators alone.

**Tools:** KNIME Analytics Platform 5.10.0 · Python (PyThaiNLP, scikit-learn) · pandas · numpy
**Course:** 254486 Data Science · Naresuan University

---

## Research Questions

1. Can Thai social media sentiment predict SET Index direction (UP/DOWN)?
2. Does adding Sentiment Score improve prediction accuracy over Technical Indicators alone?
3. Which ML model performs best for this classification task?

---

## Datasets

### 1. Wisesight Sentiment Corpus
| Detail | Info |
|---|---|
| Source | [PyThaiNLP/wisesight-sentiment](https://github.com/PyThaiNLP/wisesight-sentiment) |
| Size | 25,946 Thai texts (after filtering) |
| Labels | pos (20%), neg (25.8%), neu (54%) |
| Platform | Twitter, Facebook, Pantip |

### 2. SET Index Historical Data
| Detail | Info |
|---|---|
| Source | [Investing.com](https://www.investing.com/indices/thailand-set-historical-data) |
| Period | Jan 2016 – Mar 2026 (10 years) |
| Size | 2,472 trading days |
| Target | Direction: UP (Change% > 0) / DOWN (Change% ≤ 0) |

---

## Workflow

![KNIME Workflow](https://raw.githubusercontent.com/pawitra-thongma/thai-sentiment-set-prediction/main/workflow.png)

The pipeline consists of 2 main branches:
- **Branch 1:** Thai Sentiment Analysis (NLP + MLP Classifier)
- **Branch 2:** SET Index Direction Prediction (Technical + Sentiment Features)

---

## Methodology

### NLP Preprocessing (PyThaiNLP)
- **Word Tokenization** using PyThaiNLP `newmm` engine
- **TF-IDF Vectorization** with 300 most important features
- **RProp MLP Learner** — Hidden layers=1, Neurons=10, Max iterations=100

### Feature Engineering
| Feature | Type | Description |
|---|---|---|
| MA_5, MA_20 | Technical | 5 and 20-day Moving Averages |
| Lag_1, Lag_2, Lag_3 | Technical | Price from 1, 2, 3 days ago |
| Volatility | Technical | 5-day Rolling Standard Deviation |
| sentiment_score | Sentiment | Daily score (-1 to 1) |
| pos_ratio, neg_ratio, neu_ratio | Sentiment | Sentiment proportion ratios |

### Model Comparison Setup
- **Group A** — Technical Indicators only
- **Group B** — Technical Indicators + Sentiment Score
- **Models tested:** Logistic Regression, Random Forest, Gradient Boosted Trees
- **Tuning:** 5-Fold Cross Validation on Random Forest (n_trees, max_depth)

---

## Results

### Model Performance Comparison

| Model | Group A (No Sentiment) | Group B (With Sentiment) | Improvement | AUC |
|---|---|---|---|---|
| **Logistic Regression** | 64.8% | **83.7%** | **+18.9%** | **0.843** |
| Random Forest | 100%* | 100%* | — | Overfit |
| Gradient Boosted Trees | 100%* | 100%* | — | Overfit |

*Overfitting detected — excluded from comparison

### Data Visualization — SET Index with Moving Averages & Sentiment Score vs Market Change
![Data Visualization](https://raw.githubusercontent.com/pawitra-thongma/thai-sentiment-set-prediction/main/data_visualization.png)

### ROC Curve (Logistic Regression — Group B)
![ROC Curve](https://raw.githubusercontent.com/pawitra-thongma/thai-sentiment-set-prediction/main/roc_curve.png)

---


## Special Techniques Used

| Technique | Reason |
|---|---|
| PyThaiNLP (newmm tokenizer) | KNIME does not natively support Thai NLP |
| TF-IDF Vectorization (scikit-learn) | KNIME Bag-of-Words does not support Thai |
| 5-Fold Cross Validation | More reliable than single train/test split |
| ROC Curve + AUC | Better evaluation than accuracy alone |
| Time-Varying Sentiment Score | Reflects real market dynamics |

---


## Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![KNIME](https://img.shields.io/badge/KNIME-FDD800?style=flat-square&logo=knime&logoColor=black)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)

**Libraries:** PyThaiNLP · scikit-learn · pandas · numpy
**Platform:** KNIME Analytics Platform 5.10.0

---

**Course:** 254486 Data Science · Faculty of Science · Naresuan University · 2025
