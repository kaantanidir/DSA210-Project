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

### Step 1 — Network Data Preprocessing

For each relevant daily `*_network.json.gz` file:

1. Load and parse the interaction network (e.g., retweet or mention graph).  
2. Extract retweet/mention edges and nodes.  
3. Compute network-level metrics:
   - `daily_edges` (interaction count)  
   - `daily_nodes` (unique active accounts)  
   - `density` (E / N²)  
   - `avg_degree`  
   - `political_actor_interactions`  
   - `daily_bot_ratio` (optional, based on available metadata fields)  
4. Aggregate all metrics into a unified **daily network activity dataset**.

### Step 1b — Follower-Change Feature Extraction (Optional)

Using account-level metadata and/or follower-count time series derived from the #Secim2023 tools:

1. Construct a daily time series of follower counts for selected political accounts.  
2. Compute:
   - daily follower change per account  
   - total and average follower change across accounts  
   - maximum daily follower jump and drop among political figures  
3. Aggregate these into **daily follower-change metrics** that will be merged with the network activity dataset.

---

### Step 2 — Financial Data Processing

1. Download USD/TRY prices using `yfinance`.  
2. Compute:
   - **Log returns:**  
     \[
     \log\left(\frac{P_t}{P_{t-1}}\right)
     \]
   - **Realized volatility:** rolling standard deviation of log returns (e.g., 5-day window).  
3. Align the financial time series with social network and follower-change activity by date.

---

### Step 3 — Exploratory Data Analysis (EDA)

- Plot daily network metrics, follower-change metrics, and FX volatility on the same timeline.  
- Highlight major political dates:
  - April 30 (televised debate)  
  - May 14 (general election)  
  - May 28 (runoff election)  
- Examine whether spikes in network activity and follower growth coincide with movements in volatility.

---

### Step 4 — Statistical Testing

- Pearson correlation (linear relationships)  
- Spearman correlation (rank-based relationships)  
- Lagged cross-correlation analysis:
  - network and follower-change metrics at day *t* vs volatility at *t+1*, *t+2*, etc.

**Hypotheses:**

- **H1:** Higher network activity is associated with higher USD/TRY volatility.  
- **H2:** Increases in interaction density and follower-change metrics precede volatility spikes.  

---

### Step 5 — Machine Learning Models

#### Baseline Model (Model 0)

- Predict volatility using only lagged volatility (simple AR-style model).

#### Network- and Follower-Enhanced Models

- Random Forest Regressor  
- Gradient Boosting Regressor  
- Ridge Regression  

**Features:**

- Lagged volatility (e.g., `vol_5d(t−1)`)  
- Network-based metrics: `daily_edges`, `daily_nodes`, `density`, `avg_degree`, `political_actor_interactions`  
- Follower-based metrics: `daily_follower_growth_total`, `mean_follower_change`, `max_follower_jump_political_accounts`, etc.  
- Lagged versions of these metrics as needed  

**Evaluation:**

- Train/test split  
- Cross-validation  
- RMSE, MAE, R²  
- Feature importance (for tree-based models)

The main goal is to determine whether network and follower-change metrics add predictive power beyond historical price-based baselines.

---

## 4. Expected Outcomes

- A daily dataset of election-period Twitter network and follower-change activity metrics.  
- Empirical evidence on the relationship between social media network fluctuations and USD/TRY volatility.  
- Identification of possible time-lagged relationships between political engagement, public attention, and financial market uncertainty.  
- A reproducible pipeline connecting social network analysis and financial time-series modeling.

---

## 5. Limitations and Future Work

- The dataset does **not** include tweet text; sentiment analysis cannot be performed.  
- Network and follower-change metrics are proxies for collective engagement and coordination, not for emotional tone or individual intentions.  
- FX volatility is influenced by macroeconomic and geopolitical factors that are not explicitly modeled.  

**Possible future extensions:**

- More detailed bot-activity modeling based on additional metadata  
- Multi-layer network analysis (e.g., combining follower, retweet, and mention networks)  
- Comparison across multiple elections or other political events  

---

## 6. Ethical Considerations

- All data are used under the **CC0 1.0** public-domain license of the #Secim2023 dataset and in line with Yahoo Finance’s acceptable use policy.  
- Only anonymized network and account-level metadata are analyzed; no sensitive personal data are used.  
- All results are reported in aggregate form.  
- The use of AI tools (e.g., ChatGPT) for code assistance and documentation will be explicitly acknowledged in accordance with the DSA210 Academic Integrity Policy.

---

## 7. Tools and Libraries

- **Data handling:** `pandas`, `numpy`  
- **Network analysis:** `networkx`, `igraph`  
- **Financial data:** `yfinance`  
- **Visualization:** `matplotlib`, `seaborn`, `plotly`  
- **Machine learning:** `scikit-learn`  
- **Utilities:** `gzip`, `json`  
- **Environment:** Google Colab / Jupyter Notebook  
- **Version control:** Git and GitHub  

All dependencies will be listed in `requirements.txt`.

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
