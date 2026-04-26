# Financial Text Alpha Research

## Introduction

This project investigates whether linguistic signals from earnings call transcripts can predict future stock returns.

Structured features are extracted from transcripts (e.g., sentiment, disagreement, Q&A pressure) and tested for their predictive power using CRSP return data.

The goal is to move beyond simple NLP and evaluate whether language contains economically meaningful signals.

---

## Data Sources

* **Earnings Call Transcripts**

  * Earnings call transcripts are obtained via Refinitiv and used to construct structured NLP features at the event level.

* **CRSP Daily Stock Data**

  * Returns (`dlyret`), prices, volume, shares outstanding

* **CRSP Index Data**

  * Value-weighted market returns (`vwretd`)

* **CRSP Names Table**

---

## Pipeline

### 1. Transcript Indexing

Parse filenames to extract:

* ticker
* event date
* file path

---

### 2. Text Processing

Convert raw transcripts into structured speaker-level data:

* speaker
* role (Executive / Analyst)
* section (presentation / Q&A)
* text

---

### 3. Feature Engineering

Several NLP-based features are computed:

* **Executive Sentiment**
* **Analyst Sentiment**
* **Sentiment Gap** (exec − analyst)
* **Dispersion** (std of sentiment)
* **Q&A Pressure** (analyst tone in Q&A)

Sentiment is computed using **FinBERT**

---

### 4. Aggregation (Event-Level)

Speaker-level data is aggregated to text, ticker and date.

---

### 5. CRSP

Tickers are mapped to PERMNO using the CRSP names table.

---

### 6. Return Construction

Forward returns:

* **1-day return**:

* **5-day return**

---

### Backtest:

The predictive power of NLP-derived features is evaluated using a cross-sectional, event-driven backtest.
Stocks are sorted into quantiles based on each signal, and long–short portfolios are constructed to 
measure return spreads over future horizons. Performance is assessed through time-series returns, 
statistical significance, and market-adjusted metrics.

---

## Results

Most predictive power comes from:

* disagreement (sentiment gap)
* uncertainty (dispersion, Q&A pressure)

---

## Tech Stack

* Python (pandas, numpy)
* PyTorch (FinBERT)
* WRDS (CRSP data)
* Regex-based NLP preprocessing

---

## Disclaimer

This project is for research purposes only.
