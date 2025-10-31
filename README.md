# Can Investor Sentiment Predict Exchange Rate Volatility?  
### DSA210 – Introduction to Data Science (2025–2026 Fall Term)

---

## 1. Motivation  
This project aims to investigate whether **social media investor sentiment** can explain or predict **exchange rate volatility** during politically sensitive periods such as national elections.  
Understanding this relationship contributes to behavioral finance by showing how collective sentiment, expressed through social media, might reflect or even anticipate financial market movements.

---

## 2. Data Sources  

### a. Social Media Data  
- **Dataset:** #Secim2023 Twitter Dataset (VRL Lab, available on Harvard Dataverse)  
- **Access:** To be used strictly under academic permission from VRL Lab.  
- **Description:** Contains tweets collected during the 2023 Turkish general election period.  
- **Processing Goal:** Identify and analyze tweets related to financial topics using keywords such as *dolar*, *borsa*, *faiz*, *yatırım*, *enflasyon*.  
- **Note:** Due to dataset size (terabyte-scale), no additional raw Twitter data will be collected.

### b. Financial Data  
- **Source:** Yahoo Finance  
- **Variable:** Daily USD/TRY exchange rates  
- **Metrics:** Daily returns and volatility indicators (e.g., standard deviation of log returns)  
- **Purpose:** Examine short-term volatility patterns in relation to sentiment signals.

### c. Time Range  
**01 April 2023 – 30 June 2023**, corresponding to the Turkish general election period.

---

## 3. Methodology  

### Step 1: Data Cleaning & Preprocessing  
- Filter tweets using finance-related keywords.  
- Remove stopwords, emojis, URLs, and non-Turkish content.  
- Map timestamps to daily frequency for aggregation.  

### Step 2: Sentiment Analysis  
- Apply pretrained Turkish sentiment models (e.g., **BERTurk**, **TurkishSentiment**) to classify tweet polarity.  
- Calculate daily average sentiment scores for financial tweets.  

### Step 3: Financial Analysis  
- Compute daily log returns and volatility of the USD/TRY exchange rate.  
- Align exchange rate data with daily sentiment scores by date.  

### Step 4: Statistical Testing  
- Conduct **Pearson** and **Spearman correlation tests** to explore relationships between sentiment and volatility.  
- Perform hypothesis testing to assess whether sentiment changes precede volatility spikes.  

### Step 5: Machine Learning  
- Apply regression models (e.g., **Random Forest**, **XGBoost**) to predict volatility from sentiment indicators.  
- Evaluate models using cross-validation and error metrics (RMSE, R²).  

---

## 4. Expected Outcomes  
- Evidence of potential correlations between social media sentiment and currency volatility.  
- Insights into how collective investor psychology influences short-term market fluctuations during politically uncertain periods.  
- A replicable framework for combining social and financial data in behavioral finance research.

---

## 5. Limitations and Future Work  
- The dataset is limited to the election period and may not represent long-term market sentiment.  
- Financially relevant tweets form a small subset of the overall dataset, possibly introducing noise.  
- Future research could extend the analysis to stock market data or multiple election cycles for comparison.  

---

## 6. Ethical Considerations  
- All data will be used solely for academic research under the supervision of the course instructor.  
- No personal or identifiable user data will be published.  
- All AI tools (e.g., ChatGPT) used in code development or documentation will be explicitly acknowledged as required by the **DSA210 Academic Integrity Policy**.

---

## 7. Tools and Libraries  
- **Python Libraries:** pandas, numpy, scikit-learn, transformers, zemberek-python, matplotlib, seaborn  
- **Data Visualization:** matplotlib, plotly  
- **Version Control:** GitHub  
- **Dependencies:** listed in `requirements.txt`

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
