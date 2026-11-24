# S&P 500 Market Analysis and Stock Segmentation Using RFM

**Module:** 6BUIS017W — CRM & CM with Business Intelligence

## Project Overview

This project examines S&P 500 shares using historical weekly return, weekly beta and an adapted Recency, Frequency and Monetary (RFM) framework. The aim is to compare shares based on historical risk-return characteristics and unusual trading-volume activity.

The analysis covers **1 January 2021 to 31 December 2024**, using the S&P 500 constituent list retrieved on **19 November 2026**. From 503 candidate tickers, 488 shares were retained with complete weekly price histories.

## Data Sources

- **Wikipedia:** S&P 500 constituents, original ticker symbols and GICS sectors.
- **Yahoo Finance (`yfinance`):** Daily historical prices, volume, corporate actions and S&P 500 benchmark (`^GSPC`).
- **GitHub:** Saved input files preserved at commit `df93fcd20c3d3854eb2ed0626639787eb550976e`.

The fixed snapshot is used to avoid changing results when online data is updated.

## Methodology

### Task 1 — Data Preparation and Feature Engineering

- Validated the retrieved prices and excluded incomplete weekly histories.
- Calculated mean weekly return and beta using 207 matched weekly returns.
- Used adjusted closing prices for returns and `Close × Volume` for Monetary.
- Identified unusually high-volume days when volume exceeded twice the preceding 20-session average.
- Calculated Recency as calendar days since the latest qualifying event, Frequency over the final 60 trading sessions and Monetary as estimated traded value over those sessions.

### Task 2 — Clustering and Sensitivity Analysis

- Prepared separate Risk–Return and RFM feature matrices.
- Applied `log1p` to Monetary and standardised the matrices separately.
- Compared Ward hierarchical clustering and K-means using silhouette scores, cluster profiles and Adjusted Rand Index.
- Selected Ward k = 3 for both matrices, K-means k = 3 for Risk–Return and k = 4 for RFM.
- Repeated Ward clustering without Monetary to examine its influence.

### Task 3 — Business Intelligence and Critical Review

- Proposed a dimensional data mart for daily share observations, benchmark data and derived analysis results.
- Compared Risk–Return and RFM clusters using original-unit profiles and GICS sector composition.
- Examined return dependencies, price-adjustment sensitivity and methodological limitations.

## Selected Findings

- **354 of 488 shares (72.5%)** had their latest qualifying high-volume event on 20 December 2024.
- Risk–Return and RFM cluster memberships showed very low agreement: ARI = 0.010 for Ward and 0.012 for K-means.
- Removing Monetary substantially changed Ward RFM memberships (ARI = 0.268).
- Information Technology represented 31.5% of Ward's higher-return/higher-beta cluster, compared with 15.2% of the overall sample.

These results describe historical patterns. They do not predict future returns or establish reduced portfolio risk.

## Repository Files

| File | Description |
|---|---|
| `w2149484_CWK1_SP500.ipynb` | Main coursework notebook |
| `sp500_ticker_list.csv` | Saved constituent list |
| `sp500_shares_daily_raw.csv.gz` | Saved daily share data |
| `sp500_benchmark_daily.csv` | Saved S&P 500 benchmark data |
| `task1_feature_table.csv` | Final calculated features |
| `task1_selection_log.csv` | Share inclusion and exclusion decisions |
| `task2_cluster_results.csv` | Feature table with five clustering assignments |

## Reproducing the Analysis

1. Open `w2149484_CWK1_SP500.ipynb` in Google Colab.
2. Keep `COLLECT_NEW_SNAPSHOT = False`.
3. Run the notebook cells in order.
4. The notebook reads the saved data from the pinned GitHub commit if local input files are unavailable.
5. Compare the generated feature tables and cluster assignments with the saved CSV outputs.

A fresh download of the constituent list or historical prices may produce different results and should not be used when reproducing the reported analysis.

## Limitations

The 2026 constituent snapshot does not represent historical S&P 500 membership throughout 2021–2024, introducing potential survivorship and membership-selection bias. RFM Monetary measures estimated traded value not investment profitability or direct trading liquidity. Cluster assignments are descriptive and depend on the selected features, transformations and algorithms.
