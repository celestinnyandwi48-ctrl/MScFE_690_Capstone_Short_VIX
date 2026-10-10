# Data inventory and methodology

This directory stores the data distributed with the capstone notebooks. `raw/` contains the supplied input datasets; the Yahoo OHLC CSVs have already been cleaned and formatted by the retrieval workflow. `processed/` contains the selected futures term structure and the constructed SVXY scenarios. These labels describe the role of each file, not whether it is an untouched provider export.

All six CSVs have one header row, dates in ascending order, and a final date of **September 16, 2026**. The Excel input has its own coverage, shown below. The published files form a frozen snapshot; future downloads can differ if providers revise their histories.

## Files

| File | Coverage in the supplied file | Data rows | Columns used |
|---|---|---:|---|
| [raw/VIX_data.csv](raw/VIX_data.csv) | 2004-03-26–2026-09-16 | 5,656 | `Date, Open, High, Low, Close` |
| [raw/VVIX_data.csv](raw/VVIX_data.csv) | 2007-01-03–2026-09-16 | 4,948 | `Date, Open, High, Low, Close` |
| [raw/SVXY_data.csv](raw/SVXY_data.csv) | 2011-10-04–2026-09-16 | 3,759 | `Date, Open, High, Low, Close` |
| [raw/SPVIXSTR.xlsx](raw/SPVIXSTR.xlsx) | 2012-12-27–2026-09-18 | 3,452 | `Date, Price` in columns A:B of `Sheet1`, with headers on Excel row 2 |
| [processed/VIX_Futures_Term_Structure_F1_F2.csv](processed/VIX_Futures_Term_Structure_F1_F2.csv) | 2004-03-26–2026-09-16 | 5,656 | `Date, F1, F2, Days_to_exp1, Days_to_exp2` |
| [processed/SVXY_synth-ONEx.csv](processed/SVXY_synth-ONEx.csv) | 2011-10-04–2026-09-16 | 3,759 | `Date, Open, High, Low, Close` |
| [processed/SVXY_synth-HALFx.csv](processed/SVXY_synth-HALFx.csv) | 2011-10-04–2026-09-16 | 3,759 | `Date, Open, High, Low, Close` |

The Excel workbook also contains additional market-data columns. The synthetic notebooks read only `Date` and `Price` and sort the data for calculation.

## Market input data

`VIX_data.csv`, `VVIX_data.csv`, and `SVXY_data.csv` originate from Yahoo Finance through `yfinance`. They retain daily OHLC prices and the source trading calendars. Consult [the retrieval notebook](../notebooks/00_vix_vvix_svxy_data_retrieval.ipynb) for its download parameters and cleaning steps. The CSVs are the supplied study inputs; they are not independently reconstructed provider histories.

`SPVIXSTR.xlsx` is the manually downloaded [Investing.com SPVIXSTR history](https://www.investing.com/indices/sp-500-vix-short-term-futures-tri-historical-data) used by both synthetic notebooks. SPVIXSTR is the total-return version of the S&P 500 VIX Short-Term Futures Index. It represents rolling short-term VIX futures exposure and includes a collateral-return component; it is not spot VIX. See the [S&P index methodology](https://www.spglobal.com/spdji/en/documents/methodologies/methodology-sp-vix-futures-indices.pdf).

## Cboe F1/F2 history

[01_VIX_Futures_CBOE_F1_F2.ipynb](../notebooks/01_VIX_Futures_CBOE_F1_F2.ipynb) downloads monthly VX histories from [Cboe](https://www.cboe.com/markets/us/futures/market-statistics/historical-data/futures/) and constructs F1/F2 from the first two available, still-outstanding monthly contracts at the end of each observation date. Weekly contracts are excluded; early listings did not cover every calendar month.

F1 and F2 contain daily settlement prices. A contract enters the available universe at its first positive settlement. Leading zero placeholders are ignored; subsequent missing settlements are not filled or replaced with another contract. Prices before **March 26, 2007** are divided by ten, following the [Cboe contract rescaling](https://cdn.cboe.com/resources/regulation/circulars/general/CFE-IC-2007-003.pdf), so the history uses consistent volatility-point units. This construction extends the dataset to **March 26, 2004**.

`Days_to_exp1` and `Days_to_exp2` follow the notebook's **VolChart.io convention**: the third Friday of the month after the contract month, minus 31 calendar days, minus the observation date. Preserve this definition when using those fields; it is a documented convention rather than an independently substituted expiration calendar.

## SVXY synthetic scenarios

Both synthetic notebooks take `SVXY_data.csv` and `SPVIXSTR.xlsx` as inputs and retain the 3,759-date SVXY calendar. Their daily-exposure reconstruction uses simple percentage changes and daily compounding, not multiplication of historical price levels by a leverage factor.

The common event adjustment fits an OLS regression with intercept of daily SVXY returns on daily SPVIXSTR returns, through **February 2, 2018**. The supplied Excel begins December 27, 2012, so the first common return is December 28, 2012. February 5 is excluded from estimation. Its actual closing values are nevertheless used to estimate the February 5-to-6 return and a hypothetical February 6 SVXY close. The resulting correction factor rescales actual SVXY prices from February 6 through February 27. February 5's decline remains in the reference series.

| Scenario | Construction |
|---|---|
| **ONEx** | Keep actual OHLC through February 5, 2018; multiply February 6–27 OHLC by the correction factor; from February 28, compound twice the actual daily return from the corrected February 27 anchor. Open, High, and Low use twice their movements from the previous actual Close, anchored to the previous synthetic Close. |
| **HALFx** | Anchor February 27 Close to actual SVXY and reconstruct earlier closes backwards using half the corrected reference's daily returns. Reconstruct earlier OHL with the corresponding previous-Close rule. From February 28, preserve actual SVXY OHLC. The first candle uses its actual Open as the reference because no earlier Close is supplied. |

These are retrospective research scenarios intended to approximate consistent −1x and −0.5x daily exposures. The event correction, backwards anchor, return scaling, and reconstructed intraday prices are modeling choices, not observed tradable prices. Scaling ETF returns also scales their embedded fees and tracking differences. Synthetic backtests must be interpreted with those assumptions; the construction does not establish what an actual constant-exposure product would have achieved.

The notebooks contain construction checks and their existing plots. The organized snapshot preserves those saved outputs and does not rerun the calculations.

## Analysis cutoffs and use

The VIX study filters its VIX and futures inputs through December 29, 2023. The SMA experiment uses Development from October 4, 2011 through December 31, 2020 and Validation from January 4, 2021 through December 29, 2023. Data from 2024 onward remain reserved for later final evaluation rather than being used in the published SMA search or VIX study.

The notebooks retain their original path conventions. For execution, copy a notebook and its inputs from these distribution folders into one separate working folder, and use that folder as the current working directory. See the [main README](../README.md) for the retrieval notebook's `data/` output-subfolder exception and the file-dependency table.
