# Predicting Short-Term Popularity Growth of YouTube Trending Videos Using Platform Metrics and Machine Learning

## Abstract
This project investigates whether external search interest data from Google Trends
can improve the prediction of short-term popularity growth of YouTube trending videos.
Using publicly available YouTube Trending data enriched with Google Trends search
interest scores, we apply exploratory data analysis, statistical hypothesis testing,
and machine learning models to evaluate the contribution of external signals.

## 1. Overview & Motivation

YouTube is one of the most influential media platforms in the world. Videos sometimes go “viral” in a very short time, but it is not always clear which factors drive this rapid popularity.

This project aims to model and predict short-term popularity growth of YouTube trending videos by combining internal platform metrics (views, likes, comments, category, channel information) with external search interest signals from Google Trends.

The objective is to determine which features best predict next-day view growth and to build machine learning models that classify ‘high-growth’ outcomes.

---

## 2. Research Questions

- **RQ1:** Which video- and channel-level features (category, publication time, engagement ratios) are associated with higher short-term popularity growth?  
- **RQ2:** Does Google search interest improve prediction accuracy?  
- **RQ3:** Can a supervised ML model reliably predict high next-day growth?

---

## 3. Data Sources

### 3.1 YouTube Trending Dataset (Primary)
CSV dataset (Kaggle) including:  
video_id, title, channel_title, category_id, publish_time, views, likes, dislikes, comment_count, tags, description.

### 3.2 Google Trends Dataset (Enrichment)
Daily search interest scores exported as CSV:  
date, interest_score (0–100).

---

## 4. Data Collection & Integration

### YouTube Data Processing
- Download CSV files from Kaggle.  
- Filter to a specific timeframe (e.g., several months).  
- Engineer derived metrics:  
  - like_to_view_ratio  
  - comment_to_view_ratio  
  - days_since_publication  
  - one-hot encoded categories  

### Google Trends Processing
- Export daily interest scores for selected topic keywords.  
- Convert date fields into a standard format.

### Dataset Integration
- Merge YouTube and Google Trends on the “date” field.  
- Handle missing values and scale features as necessary.

---

## 5. Methods

### 5.1 Exploratory Data Analysis (EDA)
- View, like, and comment distributions  
- Category-level engagement comparisons  
- Publication time and growth patterns  

### 5.2 Feature Engineering
- Next-day view growth  
- Growth rate = (views_next_day – views_today) / views_today  
- High-growth labels are defined using category-normalized next-day view growth, where each video is compared against others within the same category.
- Ratios: like/view, comment/view  
- One-hot encoding  
- Google Trends score  

### 5.3 Statistical Analysis
- Correlations between features and growth  
- Hypothesis tests comparing high vs low trends days
- A non-parametric Mann–Whitney U test was conducted to compare Google Trends
search interest between high-growth and low-growth videos prior to model training.


### 5.4 Hypotheses

H0: Google Trends search interest has no association with short-term popularity growth
of YouTube trending videos.

H1: Higher Google Trends search interest is associated with significantly higher
short-term popularity growth.

These hypotheses are tested using non-parametric statistical tests.

### 5.5 Machine Learning
Tasks: binary classification (high-growth vs low-growth) 
Models: Logistic Regression, Random Forest, Gradient Boosting  
Evaluation metrics: accuracy, F1-score, ROC-AUC, PR-AUC  
Model interpretation via feature importances

---

## 6. Findings

Statistical analysis and machine learning experiments indicate that external
search interest captured by Google Trends provides a modest but consistent
contribution to predicting short-term popularity growth of YouTube trending videos.

A non-parametric Mann–Whitney U test shows a statistically significant difference
in Google Trends scores between high-growth and low-growth videos (p < 0.001),
with higher average trend scores observed during high-growth periods.

In predictive modeling, incorporating Google Trends features improves performance
over using platform metrics alone. While overall gains are limited, the improvement
is more pronounced in PR-AUC, which is particularly relevant given the class
imbalance in high-growth prediction.

---

## 7. Machine Learning Results

Two feature sets were evaluated:

- **Model A:** YouTube platform metrics only  
- **Model B:** YouTube platform metrics + Google Trends features  

```markdown
| Feature Set | ROC-AUC | PR-AUC |
|------------|--------:|-------:|
| YouTube only | 0.614 | 0.335 |
| YouTube + Trends | 0.620 | 0.366 |

The results show that Google Trends features provide a small but consistent
improvement in predictive performance, particularly in precision–recall space.

---

## 8. Limitations and Future Work

### Current Limitations
- Trending dataset reflects only already popular content  
- Google Trends measures general topic interest, not individual video interest  

### Future Directions
- NLP on titles, descriptions, and tags  
- Multi-country dataset expansion  
- More advanced time-series modeling approaches  

During initial modeling attempts, unrealistically perfect performance was observed
due to target leakage, as growth-based variables were inadvertently included in the
feature set. After explicitly removing all future-dependent features and applying
video-level train–test splits, model performance decreased to realistic levels,
highlighting the importance of leakage-aware evaluation.

---

## 9. Project Structure (Directory Layout)

The recommended folder structure is below:
```bash
DSA210-Project
│  
├── data  
│   ├── raw  
│   │   ├── USvideos.csv
│   │   ├── google_trends_category.csv
│   ├── processed
│       ├── features.csv
│       ├── features_with_trends.csv
│  
├── notebooks
│   ├── 00_fetch_google_trends_final.ipynb
│   ├── 01_eda.ipynb
│   ├── 02_feature_engineering.ipynb 
│   ├── 03_modeling.ipynb  
│  
├── src  
│   ├── trends_fetcher.py
│  
├── requirements.txt  
└── README.md
```
---

## 10. Reproducibility

All code is written in Python, and all dependencies are listed in `requirements.txt`.  
The entire data pipeline is implemented through Jupyter notebooks and is fully reproducible end-to-end once the raw dataset is provided.

To reproduce the project:

### 1. Clone the repository
```bash
git clone https://github.com/kaantanidir/DSA210-Project.git
cd DSA210-Project

```
### 2. (Optional) Create and activate a virtual environment
```bash
python -m venv venv
source venv/bin/activate       # macOS / Linux
venv\Scripts\activate          # Windows
```
### 3. Install dependencies
```bash
pip install -r requirements.txt
```
### 4. Download the Kaggle dataset

Place the following file in the raw data folder:
```bash
data/raw/USvideos.csv
```
### 5. Run the notebooks in order

- **00_fetch_google_trends.ipynb**
– Fetches and saves google_trends_category.csv
– Skips the download if the file already exists (prevents API rate limits)

- **01_eda.ipynb**
– Explores the raw YouTube dataset

- **02_feature_engineering.ipynb**
– Generates features.csv and features_with_trends.csv
– Integrates Google Trends signals and computes rolling averages

- **03_modeling.ipynb**
– Trains ML models and evaluates performance using a video-level split (GroupShuffleSplit by video_id)


Once these steps are completed, all results in the repository can be reproduced exactly.

---

## 11. Ethical Considerations

This project relies exclusively on publicly available, aggregated data and does not
involve any personal or private user information. However, algorithmic bias may arise
from the nature of YouTube’s trending mechanism, which can favor large channels or
specific content categories. The results should therefore be interpreted as patterns
within an already curated subset of content rather than the entire YouTube ecosystem.


## 12. AI Usage Disclosure

AI tools may be used for:
- Drafting and refining documentation  
- Brainstorming ideas  
- Suggesting code components  

AI tools were used for drafting documentation and debugging code logic;
all modeling decisions and interpretations were made by the author.
All AI usage will be documented in a dedicated ai_usage.md file, as required by the course.

---

## 13. Project Timeline

| Date | Milestone |
|------|-----------|
| October 31 | Submission of proposal (this README.md) |
| November 28 | Data collection, cleaning, and EDA |
| January 2 | ML model development and evaluation |
| January 9 | Final submission (23:59 deadline) |

---

## 14. Author

**Kaan Tanıdır**  
Department of Computer Science and Engineering  
Sabancı University  
Fall 2025–2026
