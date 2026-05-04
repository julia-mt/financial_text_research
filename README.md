# Financial Text Alpha Research

> **Status:** Core pipeline implemented. Preliminary results show statistically significant return predictability from Q&A sentiment (p=0.048, Newey-West adjusted).

---

## Introduction
This project investigates whether linguistic signals from earnings call transcripts can predict future stock returns. Structured NLP features are extracted from earnings calls and tested for predictive power using CRSP return data. The focus is on the unscripted Q&A section of earnings calls, where analyst tone provides a more informative signal than prepared executive remarks.

---

## Preliminary Results
- Long-short portfolio based on Q&A sentiment generates 77bps mean monthly spread (p=0.048)
- Effect strengthens over longer horizons (1d vs. 10d), consistent with slow information diffusion
- Monotonic return pattern across sentiment quintiles confirms signal robustness

---

## Data
Raw data not included. Available via WRDS academic license:

| Source | Description | WRDS Table |
|--------|-------------|------------|
| Capital IQ | Earnings call transcripts | `ciq.wrds_transcript_detail` |
| CRSP | Daily stock returns, price, volume | `crsp.dsf_v2` |
| CRSP | Value-weighted market returns | `crsp.dsi` |
| CRSP | S&P 500 constituent history | `crsp.msp500list` |

Universe: S&P 500 constituents, 2020-2024

---
## Pipeline

### 1. Data Collection
Earnings call transcripts pulled from WRDS Capital IQ (`ciq.wrds_transcript_detail`). Universe filtered to S&P 500 constituents via `crsp.msp500list` linked through the CCM linking table.

### 2. Text Processing
Raw transcript components parsed into structured speaker-level records:
- Speaker name and role (Executive / Analyst)
- Section (Presentation / Q&A)
- Component text

### 3. Feature Engineering
NLP features computed using FinBERT at the speaker level then aggregated to event level.

### 4. CRSP Alignment
Company identifiers mapped from Capital IQ `companyid` to Compustat `gvkey` and CRSP `permno` via the CCM linking table. 

### 5. Return Construction
Cumulative abnormal returns (CAR) constructed at multiple horizons:
- Market-adjusted using value-weighted market return (`vwretd`)
- Horizons: 1d, 3d, 5d, 10d post earnings call

### 6. Backtest
Stocks sorted into quintiles by signal each month. Long-short portfolio constructed going long low-sentiment quintile and short high-sentiment quintile. Performance evaluated using time-series returns, Newey-West adjusted t-tests, and Sharpe ratio.

---

## Tech Stack
- Python (pandas, numpy, statsmodels)
- PyTorch + HuggingFace (FinBERT)
- WRDS (CRSP + Capital IQ)

---

## Limitations
- Sample covers 66 S&P 500 companies — full universe validation pending
- Returns are gross of transaction costs
- Not adjusted for Fama-French risk factors

---

*For research purposes only. Raw data not redistributed.*
