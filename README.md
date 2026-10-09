# MScFE 690 Capstone — Short-VIX Exposure: An ETF-Based Trading System

**WorldQuant University · Group 17843**  
**Authors:** Edgar Nava and Celestin NYANDWI

This project studies whether information from VIX, VVIX, and the VIX futures curve can improve the return–risk balance of short-volatility exposure through an ETF-based trading system.

This repository contains the current research notebooks and data: data retrieval, VIX futures history, semi-synthetic SVXY series, a descriptive VIX study, and an SMA crossover experiment. The published CSV snapshots end on **September 16, 2026**. The analyses use earlier cutoffs as described below.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── notebooks_00_vix_vvix_svxy_data_retrieval.ipynb
│   ├── VIX_Futures_CBOE_F1_F2.ipynb
│   ├── Synthetic_SVXY-ONEx.ipynb
│   ├── Synthetic_SVXY-HALFx.ipynb
│   ├── VIX_Close_Study_1.ipynb
│   └── SMA_crossover_trading_system.ipynb
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

The folders organize the published files. Notebook contents, relative paths, saved outputs, and calculation logic are preserved. This is an organized research snapshot; the notebooks do not automatically resolve the new repository folders.

## Current notebooks and execution order

All notebook links below point into `notebooks/`. Input and output paths identify where the distributed files are stored, rather than paths embedded in notebook code.

| Stage | Notebook | Inputs | Main output or study |
|---|---|---|---|
| 1a | [VIX, VVIX and SVXY retrieval](notebooks/notebooks_00_vix_vvix_svxy_data_retrieval.ipynb) | Yahoo Finance via `yfinance` | `data/raw/VIX_data.csv`, `data/raw/VVIX_data.csv`, `data/raw/SVXY_data.csv` |
| 1b | [Cboe F1/F2 retrieval](notebooks/VIX_Futures_CBOE_F1_F2.ipynb) | Cboe monthly VX contract histories | `data/processed/VIX_Futures_Term_Structure_F1_F2.csv` |
| 2a | [Synthetic SVXY −1x](notebooks/Synthetic_SVXY-ONEx.ipynb) | `data/raw/SVXY_data.csv`, `data/raw/SPVIXSTR.xlsx` | `data/processed/SVXY_synth-ONEx.csv` |
| 2b | [Synthetic SVXY −0.5x](notebooks/Synthetic_SVXY-HALFx.ipynb) | `data/raw/SVXY_data.csv`, `data/raw/SPVIXSTR.xlsx` | `data/processed/SVXY_synth-HALFx.csv` |
| 3a | [VIX Close Study](notebooks/VIX_Close_Study_1.ipynb) | `data/raw/VIX_data.csv`; `data/processed/VIX_Futures_Term_Structure_F1_F2.csv` | Descriptive statistics, daily ranges, percentile states, transition and episode studies; tables and plots remain in the notebook |
| 3b | [SMA crossover experiment](notebooks/SMA_crossover_trading_system.ipynb) | `data/processed/SVXY_synth-ONEx.csv` | Development search and Validation comparison with buy and hold; tables and plots remain in the notebook |

Stages 1a and 1b are independent. Each synthetic notebook independently uses the same two raw inputs. The VIX study uses the VIX and futures files; the SMA experiment uses the ONEx synthetic file. Existing CSVs allow these downstream notebooks to be used without downloading or reconstructing the data again.

## Working with the unchanged notebooks

To execute a notebook, make a **separate working folder** outside this repository. Copy the desired notebook and its required input files into that folder, retaining their filenames. Launch the notebook with that folder as its current working directory (`Path.cwd()`). Copying an entire set of notebooks and all seven input/output data files into one working folder is also possible.

The VIX/VVIX/SVXY retrieval notebook writes its three CSVs into a `data/` subfolder of the working directory. Before running the synthetic or analysis notebooks with newly retrieved data, copy those three CSVs into the working folder alongside the notebooks. The futures and synthetic notebooks save their CSVs in the current working directory; the futures downloader also creates `_cboe_cache/` there. Copy a resulting file into its repository distribution folder only when intentionally updating the published snapshot.

Install the packages listed in [requirements.txt](requirements.txt) in the notebook environment. The inspected notebook metadata reports Python **3.11.15**; Python 3.11 is the documented baseline. The package list records imported dependencies and the existing pandas constraint; it is not a newly tested or fully pinned environment.

The reorganization does not rerun the notebooks. Their saved tables, charts, and other outputs remain the existing research results. Rerunning download cells requires internet access, may retrieve provider revisions, and can overwrite the working copies of the CSVs.

## Study periods and experiment boundaries

- **VIX Close Study:** March 26, 2004–December 29, 2023 in the stored data. It studies Close-based states defined by P55, P76, P90, and P96, including 5- and 15-session outcomes. VIX index-point changes are descriptive measures, not investment returns.
- **SMA Development:** October 4, 2011–December 31, 2020. The search tests integer windows `d1 = 5…30`, `d2 = 15…200`, with `d1 < d2`. Candidates require CAGR strictly above 10% and maximum drawdown magnitude strictly below 40%; Development Sharpe selects among qualifying candidates.
- **SMA Validation:** January 4, 2021–December 29, 2023. The selected rule is applied without retuning, with a fresh account and earlier prices used only to initialize the moving averages.
- **Final test:** data from 2024 onward are excluded from these analyses and reserved for a later final evaluation. This repository does not present a completed final test.

The SMA experiment uses synthetic ONEx prices, 100% cash allocation, a 15% Close-based trailing stop, US$0.005 commission per share, and 0.1% adverse slippage per trade. It permits fractional units and uses the same daily Close for signals and the execution reference. These are research assumptions; saved results do not establish achievable live performance. The current rules followed exploratory experiments.

## Data and next stages

See [data/README.md](data/README.md) for sources, coverage, columns, futures rescaling, and the SVXY synthetic construction. The synthetic series are retrospective research scenarios, including an explicit February 2018 event adjustment.

Further strategy development, final test evaluation, and the final capstone report remain separate future stages. This repository currently documents the files listed above; it does not claim a production trading system or additional unimplemented modules.
