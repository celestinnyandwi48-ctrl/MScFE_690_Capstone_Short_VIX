# MScFE 690 Capstone — Short-VIX Exposure: An ETF-Based Trading System

**WorldQuant University · Group 17843**  
**Authors:** Edgar NAVA and Celestin NYANDWI

This project investigates whether the return-risk profile of short-volatility exposure, using a short-VIX exchange-traded fund (ETF), can be improved by incorporating information from VIX, VVIX and the VIX futures curve. The idea is to develop a trading system based conditional exposure, using a set of rules derived from signals provided by those time series. 

The ETFs selected to perform our studies are two synthetic versions of SVXY, since said ETF changed its investment objective from -1x to -0.5x on February 28th 2018, after Volmageddon.

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
│   └── 05_SMA_crossover_trading_system.ipynb
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

The repository structure organizes the published files by separating the notebooks from the data. The data, in turn, is divided into raw data (obtained from data providers and necessary to perform the studies) and the processed data. Several outputs like tables, graphs, etc are not stored as separate files in the the processed data folder, but rather within the notebooks themselves.

## Current notebooks and execution order

The `00_` to `05_` prefixes used to sort the notebooks indicate the workflow order that should be followed. All notebook links below point into `notebooks/`. The column `Main output or study` indicate where is the data located in this repository, but not where the data is required or stored when running the code.
All notebooks contain extensive explanations and comments to help readers understand the sequence of steps followed.

| Stage | Notebook | Inputs | Main output or study |
|---|---|---|---|
| 00 | [VIX, VVIX and SVXY retrieval](notebooks/00_vix_vvix_svxy_data_retrieval.ipynb) | Yahoo Finance via `yfinance` and Cboe| `data/raw/VIX_data.csv`, `data/raw/VVIX_data.csv`, `data/raw/SVXY_data.csv` |
| 01 | [Cboe F1/F2 retrieval](notebooks/01_VIX_Futures_CBOE_F1_F2.ipynb) | Cboe monthly VX contract histories | `data/processed/VIX_Futures_Term_Structure_F1_F2.csv` |
| 02 | [Synthetic SVXY −1x](notebooks/02_Synthetic_SVXY-ONEx.ipynb) | `data/raw/SVXY_data.csv`, `data/raw/SPVIXSTR.xlsx` | `data/processed/SVXY_synth-ONEx.csv` |
| 03 | [Synthetic SVXY −0.5x](notebooks/03_Synthetic_SVXY-HALFx.ipynb) | `data/raw/SVXY_data.csv`, `data/raw/SPVIXSTR.xlsx` | `data/processed/SVXY_synth-HALFx.csv` |
| 04 | [VIX Close Study](notebooks/04_VIX_Close_Study_1.ipynb) | `data/raw/VIX_data.csv`; `data/processed/VIX_Futures_Term_Structure_F1_F2.csv` | Descriptive statistics, daily ranges, percentile states, transition and episode studies; tables and plots remain in the notebook |
| 05 | [SMA crossover experiment](notebooks/05_SMA_crossover_trading_system.ipynb) | `data/processed/SVXY_synth-ONEx.csv` | Development search and Validation comparison with buy and hold; tables and plots remain in the notebook |


## Data and next stages

See [data/README.md](data/README.md) for sources, coverage, columns, futures rescaling, and the construction of the synthetic SVXY.

This repository is still under development. Further strategy development and final test evaluation will be the focus of future stages.
