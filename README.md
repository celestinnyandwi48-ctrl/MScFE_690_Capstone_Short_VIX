# MScFE 690 Capstone — Short-VIX Exposure: An ETF-Based Trading System

**WorldQuant University · Group 17843**  
**Authors:** Edgar NAVA and Celestin NYANDWI

This project investigates if the return-risk profile of short-volatility exposure can be improved by information from VIX, VVIX and the VIX futures curve in an exchange-traded fund (ETF) based trading system.

Here are the current research notebooks and data: data retrieval, VIX futures history, semi-synthetic SVXY series, a descriptive VIX study, and SMA crossover experiment. The CSV snapshots published end on 16 September 2026. Analyzes use earlier cutoffs as detailed below

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

The folders organize the published files. Notebook contents, relative paths, saved outputs, and calculation logic are preserved. This is an organized research snapshot; the notebooks do not automatically resolve the new repository folders.

## Current notebooks and execution order

The `00_` to `05_` prefixes keep the notebooks sorted in the recommended workflow order. All notebook links below point into `notebooks/`. Input and output paths identify where the distributed files are stored, rather than paths embedded in notebook code.

| Stage | Notebook | Inputs | Main output or study |
|---|---|---|---|
| 00 | [VIX, VVIX and SVXY retrieval](notebooks/00_vix_vvix_svxy_data_retrieval.ipynb) | Yahoo Finance via `yfinance` | `data/raw/VIX_data.csv`, `data/raw/VVIX_data.csv`, `data/raw/SVXY_data.csv` |
| 01 | [Cboe F1/F2 retrieval](notebooks/01_VIX_Futures_CBOE_F1_F2.ipynb) | Cboe monthly VX contract histories | `data/processed/VIX_Futures_Term_Structure_F1_F2.csv` |
| 02 | [Synthetic SVXY −1x](notebooks/02_Synthetic_SVXY-ONEx.ipynb) | `data/raw/SVXY_data.csv`, `data/raw/SPVIXSTR.xlsx` | `data/processed/SVXY_synth-ONEx.csv` |
| 03 | [Synthetic SVXY −0.5x](notebooks/03_Synthetic_SVXY-HALFx.ipynb) | `data/raw/SVXY_data.csv`, `data/raw/SPVIXSTR.xlsx` | `data/processed/SVXY_synth-HALFx.csv` |
| 04 | [VIX Close Study](notebooks/04_VIX_Close_Study_1.ipynb) | `data/raw/VIX_data.csv`; `data/processed/VIX_Futures_Term_Structure_F1_F2.csv` | Descriptive statistics, daily ranges, percentile states, transition and episode studies; tables and plots remain in the notebook |
| 05 | [SMA crossover experiment](notebooks/05_SMA_crossover_trading_system.ipynb) | `data/processed/SVXY_synth-ONEx.csv` | Development search and Validation comparison with buy and hold; tables and plots remain in the notebook |

Stages 00 and 01 are not correlated . Each synthetic notebook uses the same two raw inputs independently . We use the VIX and futures files for the VIX study and the ONEx synthetic file for the SMA experiment. These downstream notebooks can be run on existing CSVs and do not require downloading/reconstructing the data again.

## Working with the unchanged notebooks

To run a notebook, make a separate working folder outside this repository. Copy the notebook you want and any input files it needs into that folder. Don't rename the original files. Open the notebook with that folder as the working directory (Path.cwd(). You can also copy a complete set of notebooks and all seven input/output data files into one working folder.

The VIX/VVIX/SVXY retrieval notebook writes its three CSVs to a data/ subfolder of the working directory. Copy those three CSVs to the working folder with the notebooks and then run the synthetic or analysis notebooks with the newly pulled data. Futures and synthetic notebooks save their CSVs to the current working directory; the futures downloader also creates _cboe_cache/ there. Copy a resulting file to its repository distribution folder. Update the published snapshot only if you are intentionally updating the published snapshot.

Install the packages in requirements.txt to the notebook environment. The notebook metadata reports python 3.11.15; python 3.11 is the documented baseline. The package list lists the imported dependencies and the current pandas constraint and not a new tested environment or a fully pinned environment.

The reorganization does not re-run the notebooks. Existing research results are their saved tables, charts and other outputs. Download cells may pull in provider revisions and may need internet access to run again. Download cells can overwrite the working copies of the CSVs.
## Study periods and experiment constraints
VIX Close Study: we analyze the Close-based states as of March 26, 2004 – December 29, 2023, stored in the database. For the selected states defined by P55, P76, P90, and P96, we consider the 5- and 15-session outcomes; VIX index-point changes are descriptive statistics and should not be confused with investment returns.
SMA Development: the fitting period is October 4, 2011 – December 31, 2020. We search for integer d1 = 5…30 and d2 = 15…200, where d1 < d2, which meet the criteria of CAGR > 10% and max drawdown < -40%, and calculate the Development Sharpe ratio.
SMA Validation: January 4, 2021 – December 29, 2023. We apply the selected SMA rule without optimization and use a fresh account with the earlier prices to initialize the moving averages. The latest data are excluded from consideration – they will be used to perform a final test in the future, but they are not available yet. Suffice it to say that this repository does not include the final test results.
The SMA experiment uses the synthetic ONEx prices, 100% cash allocation, 15% Close-based trailing stop, US$0.005 per-share commission, and 0.1% adverse slippage per trade to demonstrate long-only performance. Fractional units are allowed, and I use the same daily Close price for both signaling and execution bars. These are research assumptions; the saved results should not be confused with live performance. In this particular case, the rules were applied in exploration mode.
## Data and next stages

See [data/README.md](data/README.md) for sources, coverage, columns, futures rescaling, and the SVXY synthetic construction. The synthetic series are retrospective research scenarios, including an explicit February 2018 event adjustment.

Further strategy development, final test evaluation, and the final capstone report remain separate future stages. This repository currently documents the files listed above; it does not claim a production trading system or additional unimplemented modules.
