# Network Activity and Exchange Rate Volatility During the 2023 Turkish Elections  
### DSA210 – Introduction to Data Science (2025–2026 Fall Term) 
---

## 1. Motivation

Social media platforms play a central role during politically sensitive periods such as national elections. Rather than using tweet text (which is not included in the publicly accessible dataset), this project investigates whether **structural social media activity signals** extracted from the *#Secim2023* Twitter network—such as retweet volume, interaction density, daily network size, and follower-count dynamics—are associated with, or predictive of, **short-term USD/TRY exchange rate volatility** during the 2023 Turkish general election period.

The central research questions of this project are:

- **RQ1:** Do fluctuations in daily Twitter network activity correlate with USD/TRY volatility?  
- **RQ2:** Does increased political engagement, coordinated interaction activity, or sudden follower-count changes precede volatility spikes?  
- **RQ3:** Can social media network and follower-based features improve short-term volatility prediction compared to models based solely on historical price data?

This work bridges computational social science and behavioral finance by examining how political network dynamics and public attention relate to financial uncertainty.

---

## 2. Data Sources

### a. Social Media Network Data (Harvard Dataverse)

- **Dataset:** *#Secim2023: First Public Dataset for Studying the Turkish General Election*  
- **Persistent ID:** doi:10.7910/DVN/QJA1ZW  
- **License:** CC0 1.0 (public domain)

This dataset includes:

- Daily `*_network.json.gz` files describing retweet/mention interactions  
- `TwitterAccountList.csv` containing metadata for participating accounts  
- `TwitterKeywordsList.tab` describing data collection strategy  

**Important:** The dataset **does not include tweet text**, and therefore sentiment analysis is not possible.  
Instead, this project extracts **interaction-based and follower-based activity metrics**.

#### Extracted Daily Activity Metrics

From the network and account-level metadata, the project will construct:

- Number of edges in daily interaction graphs (`daily_edges`)  
- Number of active nodes (unique users, `daily_nodes`)  
- Network density  
- Average degree  
- Interaction counts involving political actors (`political_actor_interactions`)  
- Bot-likelihood related indicators (optional, based on available fields)  

#### Additional Follower-Change Metrics (Inspired by VRL Lab Tools)

The VRL Lab provides a follower-count monitoring system for political figures in the #Secim2023 context, showing that daily follower changes can highlight organic and inorganic attention patterns. Inspired by this framework, the project will optionally derive **daily follower-change features**, such as:

- `daily_follower_growth_total`: sum of follower increases across tracked accounts  
- `mean_follower_change`: average daily follower change  
- `max_follower_jump_political_accounts`: maximum single-day follower increase among selected political actors  
- `follower_change_volatility`: rolling standard deviation of follower changes  

These metrics provide an additional dimension for studying whether **shifts in public attention toward political actors** correlate with, or precede, exchange rate volatility.

---

### b. Financial Data (Yahoo Finance)

- **Instrument:** USD/TRY exchange rate (`USDTRY=X`)  
- **Source:** Yahoo Finance via the `yfinance` Python library  
- **Metrics:**
  - Daily closing prices  
  - Daily log returns  
  - Realized volatility (e.g., 5-day rolling standard deviation of log returns)  

---

### c. Time Range

**01 April 2023 – 30 June 2023**, covering:

- Pre-election period  
- First round (14 May)  
- Runoff (28 May)  
- Post-election period  

---

## 3. Methodology

### Step 1 — Tweet Stream Activity Extraction

1. Load daily `*.txt.gz` tweet ID files.  
2. Reconstruct timestamps using the Snowflake formula:
3. Convert to `Europe/Istanbul` timezone.  
4. Compute:
- daily tweet count  
- hourly tweet activity distribution  
- rolling tweet-volume volatility  
5. Merge into a unified dataset.

---

### Step 2 — Follower-Change Feature Extraction

From VRL Lab follower-count time series:

- Compute per-account daily changes  
- Aggregate across accounts:
- total follower growth  
- average change  
- max follower jump  
- rolling follower-change volatility  

This yields a multi-dimensional **attention-shift signal**.

---

### Step 3 — Financial Time-Series Processing

Using `yfinance`:

- Fetch USD/TRY closing prices  
- Compute log returns  
- Compute 5-day rolling volatility  
- Align with attention metrics by date  

---

### Step 4 — Exploratory Data Analysis (EDA)

Visualization of:

- Daily tweet activity  
- Follower-change metrics  
- FX volatility  

Event-study windows centered on:

- **30 April** – Televised debate  
- **14 May** – General election  
- **28 May** – Runoff  

Goals:

- Identify co-movements  
- Detect spikes  
- Examine lead/lag dynamics  

---

### Step 5 — Statistical Testing

Performed:

- Pearson correlation  
- Spearman correlation  
- Lagged cross-correlation (t → t+1, t+2)  

Hypotheses:

- **H1:** Higher attention intensity is associated with higher FX volatility.  
- **H2:** Follower-change spikes precede volatility increases.  

---

### Step 6 — Predictive Modeling

**Baseline Model (Model 0):**
- AR-style volatility model using lagged volatility only.

**Extended Models:**
- Ridge Regression  
- Random Forest Regressor  
- Gradient Boosting Regressor  

Candidate predictors:

- lagged volatility  
- daily tweet_count  
- tweet_volume_volatility  
- follower-change metrics  
- lagged attention indicators  

Evaluation metrics:

- RMSE  
- MAE  
- R²  
- Cross-validation  
- Feature importance (tree models)  

Goal:  
Determine whether attention-based signals improve short-term volatility prediction.

---

## 4. Expected Outcomes

- A merged **daily dataset** of public attention measures and financial volatility.  
- Evidence of co-movement and event-driven interactions.  
- Analysis of lead-lag patterns between public attention and FX volatility.  
- Predictive models comparing baseline volatility forecasting with attention-enhanced models.  

---

## 5. Limitations

- The Twitter dataset is **sampled**, not complete—some days contain sparse data.  
- Tweet text is unavailable → no sentiment analysis.  
- VRL follower data covers selected political actors only.  
- FX volatility is influenced by macroeconomic factors not modeled here.  

Despite these constraints, relative activity measures show meaningful signals and allow a robust analysis of attention–volatility dynamics.

---

## 6. Ethical Considerations

- All data used is public and CC0-licensed.  
- No personal or sensitive data is processed.  
- Financial data usage complies with Yahoo Finance guidelines.  
- AI tools used only for code assistance and documentation, acknowledged per DSA210 academic integrity policy.  

---

## 7. Tools and Libraries

- `pandas`, `numpy`  
- `networkx`, `igraph`  
- `yfinance`  
- `matplotlib`, `seaborn`, `plotly`  
- `scikit-learn`  
- `gzip`, `json`  
- Google Colab / Jupyter Notebook  
- Git + GitHub  

---

## 8. Project Timeline  

| Date | Milestone |
|------|------------|
| October 31 | Project proposal submission (this README.md) |
| November 28 | Data collection, preprocessing, and exploratory data analysis |
| January 2 | Application of ML models and result evaluation |
| January 9 | Final submission before 23:59 |

---

## 9. Author  

**Kaan Tanıdır**  
Department of Computer Science and Engineering  
Sabancı University  
Fall 2025–2026  
