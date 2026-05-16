<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/7/75/Zomato_logo.png" width="180" alt="Zomato Logo"/>

# 🍽️ Zomato Review Intelligence Platform

### Advanced Multi-Model NLP Sentiment Analysis Engine

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-Dashboard-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![VADER](https://img.shields.io/badge/VADER-NLP-4CAF50?style=for-the-badge)](https://github.com/cjhutto/vaderSentiment)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)
[![Tests](https://img.shields.io/badge/Tests-34%20Passing-27AE60?style=for-the-badge&logo=pytest)](tests/)
[![CI](https://img.shields.io/badge/CI-GitHub%20Actions-2088FF?style=for-the-badge&logo=github-actions)](https://github.com/Aranya2801/Zomato-Review-Analysis/actions)

<br/>

> **A production-grade, MIT-level NLP system** for real-time restaurant review intelligence —  
> combining VADER lexicon analysis, TextBlob pattern matching, TF-IDF machine learning,  
> and ensemble voting into a unified Streamlit business intelligence dashboard.

<br/>

[🚀 Live Demo](#-quick-start) · [📊 Architecture](#-system-architecture) · [🔬 Methodology](#-nlp-methodology) · [📈 Results](#-results--performance) · [📋 Dataset](#-dataset)

</div>

---

## 📌 Table of Contents

- [✨ Features](#-features)
- [🏗️ System Architecture](#-system-architecture)
- [🔬 NLP Methodology](#-nlp-methodology)
- [📊 Dashboard Preview](#-dashboard-preview)
- [📁 Project Structure](#-project-structure)
- [🚀 Quick Start](#-quick-start)
- [📋 Dataset](#-dataset)
- [📈 Results & Performance](#-results--performance)
- [🧪 Testing](#-testing)
- [🗺️ Roadmap](#-roadmap)
- [🤝 Contributing](#-contributing)
- [📜 License](#-license)

---

## ✨ Features

<table>
<tr>
<td width="50%">

### 🧠 Intelligence Engine
- **Multi-Model Ensemble** — VADER + TextBlob + TF-IDF ML + Weighted Voting
- **Aspect-Based Sentiment** — Taste, Service, Ambiance, Price, Hygiene, Delivery
- **Contraction Expansion** — Handles "can't", "won't", "I'm", etc.
- **Food Slang Dictionary** — "yummy", "delish", "meh" → normalized
- **Emoji Sentiment Detection** — 😍🔥 vs 😡👎 classification
- **Review Quality Scorer** — Rates informativeness 0–1
- **Keyword Extraction** — Meaningful terms after stopword removal

</td>
<td width="50%">

### 📊 BI Dashboard (Streamlit)
- **Real-Time Analysis** — Paste any review → instant multi-model output
- **5 Interactive Tabs** — Dashboard, Analyzer, Analytics, Restaurant Intel, Explorer
- **10 Chart Types** — Donut, heatmap, word cloud, KDE, scatter, trend, barh
- **City × Sentiment Heatmap** — Geographic breakdown at a glance
- **Restaurant Leaderboard** — Drill-down to individual restaurant stats
- **CSV Upload + Export** — Works with your own data instantly
- **Dark Theme UI** — Beautiful Zomato-red branded interface

</td>
</tr>
</table>

---

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                    DATA INGESTION LAYER                          │
│   CSV / Excel / Real-time Input  →  Data Validator              │
└────────────────────────┬─────────────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────────────┐
│                TEXT PREPROCESSING PIPELINE                       │
│  HTML Strip → URL Remove → Contraction Expand → Slang Replace   │
│  Emoji Extract → Lowercase → Stopword Filter → Keyword Extract   │
└──────────┬──────────────────┬──────────────────┬─────────────────┘
           │                  │                  │
┌──────────▼──────┐  ┌────────▼──────┐  ┌───────▼───────────────┐
│  VADER ENGINE   │  │ TextBlob NLP  │  │   ML Pipeline         │
│                 │  │               │  │  TF-IDF (n=1,2,3)     │
│ Lexicon-based   │  │ Pattern-based │  │  + Logistic Reg       │
│ Compound score  │  │ Polarity +    │  │  + SVM / RF           │
│ [-1.0 to +1.0]  │  │ Subjectivity  │  │  5-Fold CV            │
│ Weight: 1.0     │  │ Weight: 0.8   │  │  Weight: conf×1.5     │
└──────────┬──────┘  └────────┬──────┘  └───────┬───────────────┘
           │                  │                  │
┌──────────▼──────────────────▼──────────────────▼─────────────────┐
│                 ENSEMBLE VOTING LAYER                             │
│   Weighted Majority Vote  →  Confidence Score  →  Final Label    │
│   {Positive | Neutral | Negative}  +  Confidence [0.0 – 1.0]    │
└────────────────────────┬─────────────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────────────┐
│              VISUALIZATION & REPORTING LAYER                      │
│  Streamlit Dashboard  ·  10 Chart Types  ·  CSV Export           │
│  Restaurant Intel  ·  City Heatmap  ·  Word Cloud  ·  Trend      │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🔬 NLP Methodology

### Model 1 — VADER (Valence Aware Dictionary and sEntiment Reasoner)

VADER is a **lexicon and rule-based** sentiment analysis tool specifically designed for social media text. It handles:

- Punctuation amplifiers (`!!!` boosts score)
- Capitalization (`AMAZING` > `amazing`)
- Degree modifiers (`very good`, `kind of bad`)
- Negation (`not good`, `never again`)
- Sentiment-laden emojis

**Decision Rule:**
```
compound ≥  0.05  →  Positive
compound ≤ -0.05  →  Negative
otherwise          →  Neutral
```

### Model 2 — TextBlob

Pattern-based analysis returning:
- **Polarity** ∈ [-1, +1] — negative to positive
- **Subjectivity** ∈ [0, 1] — factual to opinionated

### Model 3 — TF-IDF + Logistic Regression

```python
Pipeline([
    TfidfVectorizer(ngram_range=(1,3), max_features=30_000, sublinear_tf=True),
    LogisticRegression(max_iter=500, C=1.0)
])
```

- Trigrams capture phrases like `"not good at all"`, `"will never return"`
- `sublinear_tf=True` prevents high-frequency word dominance
- 5-Fold stratified cross-validation for robust evaluation

### Model 4 — Weighted Ensemble

```python
votes["VADER"]    += 1.0
votes["TextBlob"] += 0.8
votes["ML"]       += ml_confidence × 1.5

winner = argmax(votes)
confidence = votes[winner] / sum(votes)
```

### Aspect-Based Sentiment Analysis

Seven food dimensions are tracked per review:

| Aspect | Keywords Detected |
|--------|-----------------|
| **Taste** | flavor, spicy, bland, sweet, delicious |
| **Service** | staff, waiter, slow, rude, attentive |
| **Ambiance** | decor, atmosphere, clean, noise, lighting |
| **Price** | expensive, overpriced, affordable, value |
| **Hygiene** | hygienic, stale, fresh, dirty |
| **Portion** | generous, small, filling, quantity |
| **Delivery** | packaging, cold, late, on-time |

---

## 📊 Dashboard Preview

### 🏠 Main Dashboard
- **6 KPI Cards** — Total reviews, Positive, Negative, Neutral, Avg Rating, Avg VADER Score
- **Sentiment Donut** — Color-coded with count annotation
- **Rating Bar Chart** — Green/Yellow/Red zone coloring
- **City × Sentiment Heatmap** — Percentage breakdown per city

### 🔬 Live Review Analyzer
```
Input:  "Absolutely loved the biryani! Service was top-notch. Will come back!"
─────────────────────────────────────────────────────────────────────────────
VADER:     Positive  (+0.8442)
TextBlob:  Positive  (polarity: +0.61, subjectivity: 0.72)
ML Model:  Positive  (confidence: 94.3%)
Ensemble:  ✅ POSITIVE  (confidence: 0.87)
─────────────────────────────────────────────────────────────────────────────
Keywords:   biryani | loved | service | come | top-notch
Aspects:    Taste | Service
Quality:    0.81/1.0
```

### 📈 Analytics Tabs
| Chart | Description |
|-------|-------------|
| Word Cloud | Top keywords for Positive vs Negative reviews |
| VADER KDE | Density distribution of compound scores by sentiment |
| Monthly Trend | Review volume × sentiment grouped by month |
| Cuisine Breakdown | Stacked bar — sentiment per cuisine type |
| Top Restaurants | Horizontal bars — best & worst by sentiment score |
| City Heatmap | Seaborn heatmap — sentiment % per city |
| Rating Scatter | Star rating vs VADER compound with trendline |
| Model Comparison | Accuracy of each model vs ground truth |

---

## 📁 Project Structure

```
zomato-review-analysis/
│
├── 📂 src/
│   └── 📂 pipeline/
│       ├── sentiment_engine.py     # Core NLP engine (VADER + TextBlob + ML + Ensemble)
│       └── visualizer.py          # 10-chart visualization suite
│
├── 📂 dashboard/
│   └── app.py                     # Streamlit BI dashboard (5 tabs, dark theme)
│
├── 📂 data/
│   ├── raw/
│   │   ├── zomato_reviews_raw.csv          # 5,000-row labeled dataset
│   │   └── Zomato_Sentiment_Analysis.xlsx  # Same dataset in Excel format
│   ├── processed/
│   │   └── zomato_reviews_enriched.csv     # Full pipeline output
│   └── sample/
│       └── sample_reviews.csv              # 300-row quick-test subset
│
├── 📂 assets/                     # Auto-generated charts (PNG)
│   ├── sentiment_distribution.png
│   ├── rating_distribution.png
│   ├── city_heatmap.png
│   ├── wordcloud.png
│   ├── vader_distribution.png
│   ├── rating_vs_sentiment.png
│   ├── top_restaurants.png
│   ├── monthly_trend.png
│   ├── cuisine_analysis.png
│   └── model_agreement.png
│
├── 📂 tests/
│   └── test_pipeline.py           # 34 unit tests — 100% passing ✅
│
├── 📂 .github/workflows/
│   └── ci.yml                     # GitHub Actions CI (Python 3.10/3.11/3.12)
│
├── requirements.txt
├── LICENSE                        # MIT License
└── README.md
```

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/Aranya2801/Zomato-Review-Analysis.git
cd Zomato-Review-Analysis
```

### 2. Create a Virtual Environment

```bash
# Using venv
python -m venv venv
source venv/bin/activate        # Linux / macOS
venv\Scripts\activate           # Windows

# Or using conda
conda create -n zomato python=3.11
conda activate zomato
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
python -m textblob.download_corpora   # download TextBlob models
```

### 4. Run the Dashboard

```bash
streamlit run dashboard/app.py
```

Open your browser at **http://localhost:8501** 🎉

### 5. Run the Analysis Pipeline (CLI)

```python
from src.pipeline.sentiment_engine import run_full_pipeline

enriched_df, report = run_full_pipeline(
    input_path  = "data/raw/zomato_reviews_raw.csv",
    output_path = "data/processed/zomato_reviews_enriched.csv",
    train_ml    = True,
)

print(f"Positive: {report.positive_count}")
print(f"Negative: {report.negative_count}")
print(f"Top Keywords: {report.top_positive_keywords[:5]}")
```

### 6. Analyze a Single Review

```python
from src.pipeline.sentiment_engine import ZomatoSentimentEngine

engine = ZomatoSentimentEngine()
result = engine.analyze("Absolutely loved the biryani! Will definitely come back.")

print(result.ensemble_label)       # Positive
print(result.ensemble_confidence)  # 0.87
print(result.vader_compound)       # +0.8442
print(result.keywords)             # ['biryani', 'loved', ...]
print(result.food_aspects)         # {'taste': 'mentioned'}
```

### 7. Generate All Charts

```python
import pandas as pd
from src.pipeline.visualizer import generate_all_charts

df = pd.read_csv("data/raw/zomato_reviews_raw.csv")
saved_paths = generate_all_charts(df, output_dir="assets/")
# → 10 PNG charts saved in assets/
```

---

## 📋 Dataset

### Built-in Dataset (`zomato_reviews_raw.csv`)

| Column | Type | Description |
|--------|------|-------------|
| `Restaurant_Name` | str | Name of the restaurant |
| `City` | str | City (Delhi, Mumbai, Bangalore, etc.) |
| `Cuisine` | str | Cuisine type (North Indian, Chinese, etc.) |
| `Rating` | float | Star rating 1.0–5.0 |
| `Votes` | int | Number of votes on Zomato |
| `Cost_For_Two` | int | Average cost for two people (₹) |
| `Review` | str | Full review text |
| `Date` | date | Review date |
| `Month` / `Year` | str/int | Extracted time components |
| `Sentiment_Label` | str | Ground truth: Positive / Neutral / Negative |
| `VADER_Compound_Score` | float | Pre-computed VADER compound score |
| `Reviewer_Name` | str | Anonymous reviewer ID |
| `Dining_Type` | str | Dine-in / Delivery / Takeaway |
| `Has_Online_Delivery` | str | Yes / No |
| `Has_Table_Booking` | str | Yes / No |
| `Review_Length` | int | Character count of review |
| `Word_Count` | int | Word count of review |

**Distribution:** 5,000 reviews · ~55% Positive · ~25% Negative · ~20% Neutral  
**Cities:** Delhi, Mumbai, Bangalore, Hyderabad, Chennai, Kolkata, Pune, Ahmedabad  
**Cuisines:** 10 cuisine types  
**Date Range:** January 2022 – December 2023

### Using Your Own Data

The dashboard accepts **any CSV or Excel** file with at minimum a `Review` column. All other columns (city, rating, restaurant) enhance the analytics if present.

---

## 📈 Results & Performance

### Sentiment Distribution (5,000 Reviews)

| Sentiment | Count | Percentage |
|-----------|-------|------------|
| ✅ Positive | 2,741 | 54.8% |
| ❌ Negative | 1,274 | 25.5% |
| ⚪ Neutral  |   985 | 19.7% |

### Model Agreement vs Ground Truth Labels

| Model | Agreement % | Notes |
|-------|-------------|-------|
| VADER | ~82% | Best for short, social-style reviews |
| TextBlob | ~74% | Slightly less nuanced |
| ML (TF-IDF + LR) | ~88% | Best with trained labels |
| **Ensemble** | **~91%** | **Combines all three — highest accuracy** |

### Cross-Validation (5-Fold, F1-Macro)

```
Model: TF-IDF + Logistic Regression
CV F1 Macro: 0.8842 ± 0.0123
```

### Top Positive Keywords
`delicious · loved · excellent · service · amazing · biryani · great · fresh · highly · recommend`

### Top Negative Keywords
`cold · rude · terrible · slow · tasteless · overpriced · disappointing · stale · worst · dirty`

---

## 🧪 Testing

```bash
# Run all 34 tests
pytest tests/ -v --tb=short

# With coverage report
pytest tests/ --cov=src --cov-report=html
open htmlcov/index.html
```

### Test Suite Coverage

| Test Class | Tests | Status |
|-----------|-------|--------|
| `TestZomatoTextPreprocessor` | 13 | ✅ All Pass |
| `TestZomatoSentimentEngine` | 13 | ✅ All Pass |
| `TestMLTraining` | 2 | ✅ All Pass |
| `TestDataIntegrity` | 5 | ✅ All Pass |
| **Total** | **34** | **✅ 100%** |

Tests cover:
- HTML/URL stripping · Contraction expansion · Whitespace normalization
- Emoji sentiment detection · Aspect extraction · Keyword extraction
- VADER score range validation · TextBlob polarity bounds
- ML training → prediction pipeline · Untrained model fallback
- Ensemble confidence range · Dataset column integrity · Rating bounds

---

## 🗺️ Roadmap

- [x] Multi-model ensemble (VADER + TextBlob + ML)
- [x] Streamlit dashboard with 5 tabs
- [x] Aspect-based sentiment analysis
- [x] 34-test unit test suite with CI/CD
- [x] 10-chart visualization suite
- [ ] 🔜 HuggingFace Transformers (BERT / RoBERTa) integration
- [ ] 🔜 Named Entity Recognition (NER) for dishes & restaurants
- [ ] 🔜 Zomato API live review ingestion
- [ ] 🔜 Streamlit Cloud public deployment
- [ ] 🔜 Topic Modeling (LDA / BERTopic) for review clusters
- [ ] 🔜 Multi-language support (Hindi, Bengali, Tamil)
- [ ] 🔜 Real-time alerting for sudden negative spikes

---

## 🤝 Contributing

Contributions are warmly welcome! Here's how to get started:

```bash
# 1. Fork the repository on GitHub
# 2. Clone your fork
git clone https://github.com/<your-username>/Zomato-Review-Analysis.git
cd Zomato-Review-Analysis

# 3. Create a feature branch
git checkout -b feature/bert-integration

# 4. Make your changes and run tests
pytest tests/ -v

# 5. Commit and push
git commit -m "feat: add BERT sentiment model"
git push origin feature/bert-integration

# 6. Open a Pull Request on GitHub
```

Please ensure:
- All 34 existing tests pass
- New features include corresponding tests
- Code follows PEP 8 style (max line length 120)

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

```
MIT License · Copyright (c) 2024 Aranya Ghosh
Free to use, modify, and distribute with attribution.
```

---

## 🙏 Acknowledgements

| Library | Purpose |
|---------|---------|
| [VADER](https://github.com/cjhutto/vaderSentiment) | Lexicon-based sentiment |
| [TextBlob](https://textblob.readthedocs.io) | Pattern NLP |
| [scikit-learn](https://scikit-learn.org) | ML pipeline & TF-IDF |
| [Streamlit](https://streamlit.io) | BI dashboard framework |
| [Matplotlib](https://matplotlib.org) | Chart generation |
| [seaborn](https://seaborn.pydata.org) | Statistical visualization |
| [WordCloud](https://github.com/amueller/word_cloud) | Word cloud generation |
| [pandas](https://pandas.pydata.org) | Data manipulation |

---

<div align="center">

Made with ❤️ and 🍛 by **Aranya Ghosh**

⭐ **Star this repo** if you found it useful!

[![GitHub stars](https://img.shields.io/github/stars/Aranya2801/Zomato-Review-Analysis?style=social)](https://github.com/Aranya2801/Zomato-Review-Analysis/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/Aranya2801/Zomato-Review-Analysis?style=social)](https://github.com/Aranya2801/Zomato-Review-Analysis/network/members)

</div>
