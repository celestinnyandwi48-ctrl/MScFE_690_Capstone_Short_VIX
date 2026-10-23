# MScFE 690 Capstone — Short-VIX Exposure: An ETF-Based Trading System

**WorldQuant University · Group 17843**  
**Authors:** Edgar NAVA and Celestin NYANDWI

This project investigates whether the return-risk profile of short-volatility exposure, using a short-VIX exchange-traded fund (ETF), can be improved by incorporating information from VIX and the VIX futures curve. The idea is to develop a trading system based on conditional exposure, using a set of rules derived from signals provided by those time series. VVIX is also explored as a possible additional signal, although the current model does not use it.

Our studies use two synthetic versions of SVXY to examine -1x and -0.5x exposure, since the ETF changed its investment objective from -1x to -0.5x on February 28th 2018, after Volmageddon. We also apply the model to actual SVIX data, both over the test period and over its full available history.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── 00_vix_vvix_svxy_data_retrieval.ipynb
│   ├── 01_VIX_Futures_CBOE_F1_F2.ipynb
│   ├── 02_Synthetic_SVXY-ONEx.ipynb
│   ├── 03_Synthetic_SVXY-HALFx.ipynb
│   ├── 04_VIX_Close_Study_1.ipynb
│   ├── 05_SMA_crossover_trading_system.ipynb
│   ├── 06_SMA_crossover_trading_system-HALFx.ipynb
│   └── 07-Model_2073_SVXY-1.ipynb
└── data/
    ├── README.md
    ├── raw/
    │   ├── VIX_data.csv
    │   ├── VVIX_data.csv
    │   ├── SVXY_data.csv
    │   └── SPVIXSTR.xlsx
    └── processed/
        ├── VIX_Futures_Term_Structure_F1_F2.csv
        ├── SVXY_synth-ONEx.csv
        └── SVXY_synth-HALFx.csv
```

The repository structure organizes the published files by separating the notebooks from the data. The data, in turn, is divided into raw data (obtained from data providers and necessary to perform the studies) and the processed data. Several outputs like tables, graphs, etc are not stored as separate files in the processed data folder, but rather within the notebooks themselves.

## Current notebooks and execution order

The `00` to `07` prefixes used to sort the notebooks indicate the workflow order that should be followed. All notebook links below point into `notebooks/`. The paths in the table show where the data is located in this repository. Each notebook specifies where its input files must be placed when running the code.
The notebooks include comments to help readers follow the steps. The Capstone document provides the detailed methodological discussion.

| Stage | Notebook | Inputs | Main output or study |
|---|---|---|---|
| 00 | [VIX, VVIX and SVXY retrieval](notebooks/00_vix_vvix_svxy_data_retrieval.ipynb) | Yahoo Finance via `yfinance` and Cboe | `data/raw/VIX_data.csv`, `data/raw/VVIX_data.csv`, `data/raw/SVXY_data.csv` |
| 01 | [Cboe F1/F2 retrieval](notebooks/01_VIX_Futures_CBOE_F1_F2.ipynb) | Cboe monthly VX contract histories | `data/processed/VIX_Futures_Term_Structure_F1_F2.csv` |
| 02 | [Synthetic SVXY −1x](notebooks/02_Synthetic_SVXY-ONEx.ipynb) | `data/raw/SVXY_data.csv`, `data/raw/SPVIXSTR.xlsx` | `data/processed/SVXY_synth-ONEx.csv` |
| 03 | [Synthetic SVXY −0.5x](notebooks/03_Synthetic_SVXY-HALFx.ipynb) | `data/raw/SVXY_data.csv`, `data/raw/SPVIXSTR.xlsx` | `data/processed/SVXY_synth-HALFx.csv` |
| 04 | [VIX Close Study](notebooks/04_VIX_Close_Study_1.ipynb) | `data/raw/VIX_data.csv`; `data/processed/VIX_Futures_Term_Structure_F1_F2.csv` | Descriptive statistics, daily ranges, percentile states, transition and episode studies; tables and plots remain in the notebook |
| 05 | [SMA crossover experiment −1x](notebooks/05_SMA_crossover_trading_system.ipynb) | `data/processed/SVXY_synth-ONEx.csv` | Development search and Validation comparison with buy and hold; tables and plots remain in the notebook |
| 06 | [SMA crossover experiment −0.5x](notebooks/06_SMA_crossover_trading_system-HALFx.ipynb) | `data/processed/SVXY_synth-HALFx.csv` | Development search with a 7.5% trailing stop and evaluation of the SMA 5/21 substitute baseline versus buy and hold in Development and Validation; tables and plots remain in the notebook |
| 07 | [Model 2073 — Synthetic SVXY −1x](notebooks/07-Model_2073_SVXY-1.ipynb) | `data/raw/VIX_data.csv`; `data/processed/SVXY_synth-ONEx.csv` | Model comparison with buy and hold in Development, Validation, Test and a continuous full-history simulation; tables and plots remain in the notebook |

For the −0.5x experiment, no pair met the original Development requirements. We use the 5-day/21-day pair, which had the highest Development CAGR, as a substitute baseline with a 7.5% trailing stop. It does not meet the original return requirement.

Model 2073 uses VIX states to determine entries, additional purchases and exits. Its results include CAGR, Sharpe, Calmar and maximum drawdown after costs.

## Data and next stages

See [data/README.md](data/README.md) for sources, coverage, columns, futures rescaling, and the construction of the synthetic SVXY.

This repository is still under development. Notebook 07 now includes the Test results for Model 2073 on synthetic SVXY −1x. Further notebooks will cover the −0.5x model, rolling-window stability and the application to actual SVIX data.
