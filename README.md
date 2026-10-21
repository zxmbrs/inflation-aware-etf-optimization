# Inflation-Aware Portfolio Optimization

**Balancing Downside Risk and Long-Term Purchasing Power**

**Author:** Xunmo Zhao  
**Project:** Independent quantitative research | October 2026

## Overview

This project studies whether portfolios designed to minimize downside risk also preserve investors' purchasing power over time. It compares equal-weighted allocations, global minimum variance (GMV), minimum Conditional Value-at-Risk (CVaR), and return-constrained CVaR strategies using Canadian-listed exchange-traded funds (ETFs).

The central question is not simply which portfolio has the lowest volatility, but how downside-risk protection trades off against **inflation-adjusted growth**. The analysis also tests transaction costs, gold concentration limits, historical stress periods, and a retrospectively constructed static allocation benchmark.

## Research Questions

1. Do minimum-risk portfolios preserve purchasing power after inflation?
2. How do nominal return constraints change CVaR, drawdowns, and real returns?
3. Does a dynamically optimized CVaR portfolio outperform a simpler fixed allocation?
4. How sensitive are the conclusions to turnover, trading costs, gold caps, and historical market shocks?

## Data and Study Design

| Item | Specification |
|---|---|
| Universe | Seven Canadian-listed ETFs covering equities, bonds, and gold |
| Core universe | XIC.TO, VFV.TO, XEF.TO, XEC.TO, XBB.TO |
| Expanded universe | Core universe plus CGL-C.TO and XSB.TO |
| ETF monthly return observations | 161, May 2013–September 2026 |
| Estimation | Rolling 60-month training window |
| Out-of-sample evaluation | 101 monthly observations, May 2018–September 2026 |
| CPI-matched evaluation | 99 months, June 2018–August 2026 |
| Constraints | Fully invested, long-only portfolios |
| CVaR confidence level | 95% |
| Nominal return targets | 0%, 2%, 4%, 6%, and 8% annual equivalents |

**Data sources:** ETF historical prices were obtained from Yahoo Finance through `yfinance`. Canadian inflation is based on Statistics Canada, Consumer Price Index, Table 18-10-0004-01 (Canada, all-items, not seasonally adjusted). Frozen processed data are included so reproducing the analysis does not require live market downloads.

The constrained CVaR optimizer applies a nominal monthly threshold converted from the selected annual target and compares it with the **arithmetic mean monthly return estimated in the training window**. This is an in-sample expected-return constraint, **not** a guaranteed future realized return. CPI is used to evaluate realized historical purchasing power; it is not an input to the portfolio optimizer.

## Methods

The 11 primary backtested strategies consist of:

- Core and expanded equal weighting (EW)
- Core and expanded GMV
- Core and expanded minimum CVaR
- Expanded-universe CVaR with five nominal return targets (0%, 2%, 4%, 6%, and 8%)

For each out-of-sample month, the model is fitted on the **preceding 60 months only** and its weights are applied to the subsequent month's observed ETF returns. The resulting monthly return paths are assessed using nominal compounded annual growth rate (CAGR), annualized volatility, maximum drawdown, and historical monthly CVaR at 95%.

Purchasing-power performance is computed by compounding exact monthly real returns:

$$
r_{\mathrm{real},t} = \frac{1+r_{\mathrm{nominal},t}}{1+\pi_t}-1
$$

Historical inflation-beating frequencies are calculated over overlapping 12-, 36-, and 60-month windows.

Additional robustness analyses include:

- Historical cumulative-return stress checks for February–March 2020 and calendar year 2022
- Drift-adjusted one-way monthly turnover and simplified trading-cost sensitivity at 0, 5, 10, and 20 basis points
- CVaR target-8% portfolios with 30% and 40% gold-weight caps
- A monthly-rebalanced static benchmark whose asset-class weights are matched **ex post** to the dynamic target-8% strategy's average allocation

## Selected Historical Findings

| Strategy | Nominal CAGR | Annualized volatility | Maximum drawdown | Monthly CVaR 95% |
|---|---:|---:|---:|---:|
| Expanded EW | 9.72% | 7.92% | 12.65% | 4.41% |
| Expanded minimum CVaR | 4.08% | 3.06% | 6.11% | 1.56% |
| CVaR target 6% | 7.39% | 5.37% | 8.01% | 2.63% |
| CVaR target 8% | 9.63% | 7.00% | 9.37% | 3.38% |
| Ex-post exposure-matched benchmark | 9.17% | 6.30% | 8.05% | 2.84% |

Over the 99 CPI-matched months, the target-8% portfolio achieved approximately **6.60% real CAGR**, while the expanded minimum-CVaR portfolio achieved approximately **1.25% real CAGR**. The target-8% strategy beat cumulative CPI over **85.23% of overlapping one-year windows** and **89.06% of overlapping three-year windows**.

The exposure-matched static benchmark had **lower volatility, drawdown, and CVaR** than the dynamic target-8% strategy, despite slightly lower returns. Consequently, the evidence does **not** establish that dynamic optimization uniformly dominates simple allocation. Full context and limitations appear in the research report.

## Repository Layout

```text
inflation-aware-etf-optimization/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 01_data_collection.ipynb
│   └── 02_portfolio_optimization.ipynb
├── data/
│   └── processed/
│       ├── core_returns.csv
│       ├── expanded_returns.csv
│       ├── expanded_monthly_prices.csv
│       ├── canada_monthly_inflation.csv
│       └── file_checksums.csv
├── results/
│   └── tables/
│       ├── all_11_strategy_monthly_returns.csv
│       ├── all_11_strategy_research_summary.csv
│       └── data_validation_summary.json
├── figures/
│   ├── figure1_risk_return_tradeoff.png
│   ├── figure2_inflation_adjusted_wealth.png
│   ├── figure3_inflation_beating_frequency.png
│   ├── figure4_historical_stress_testing.png
│   └── figure5_asset_allocation_dynamics.png
└── report/
    └── inflation_aware_portfolio_research_report.pdf
```

## How to Reproduce

**1. Obtain the complete repository.** Clone it locally, or download **Code → Download ZIP** from GitHub and extract it. You need the repository's `data/processed/` and `results/tables/` folders alongside the notebooks; downloading a notebook alone is insufficient.

**2. Install dependencies** in a dedicated Python environment, from the repository root:

```bash
python -m pip install -r requirements.txt
```

**3. Open and run the notebooks in this order:**

```bash
python -m jupyter notebook
```

- `notebooks/01_data_collection.ipynb` checks the frozen input data and internal consistency.
- `notebooks/02_portfolio_optimization.ipynb` recomputes the portfolio optimization, walk-forward returns, performance statistics, inflation analysis, and robustness checks, and compares key recalculated outputs with the frozen reference results.

In Jupyter, choose **Restart Kernel and Run All Cells**. The notebook searches for the complete project from its current working directory and parent directory.

**Google Colab:** Upload/open the notebook in Colab, then upload and extract the **complete GitHub repository ZIP** into `/content/` before using **Runtime → Run all**. The optimization notebook also recognizes the extracted folder `/content/inflation-aware-etf-optimization-main/`. Colab's `/content/` storage is temporary and may need restoring in later sessions. Avoid adding ZIP-upload or Google Drive-specific setup cells to the committed research notebook.

Numerical solver tolerances may cause small differences from the frozen monthly returns. Use the built-in comparisons rather than requiring bit-for-bit equality.

## Reproducibility and Validation

- Frozen ETF and CPI inputs passed structural and missing-data checks.
- ETF monthly returns were reconciled to frozen month-end prices, with maximum absolute difference approximately **4.45 × 10⁻¹⁶**.
- The data-validation notebook passed a separate fresh-kernel execution test.
- All **11 strategies × 101 test months** were recalculated in Google Colab. The largest observed difference from the frozen monthly strategy-return file was approximately **2.18 × 10⁻⁶** in decimal return units.
- Recomputed core performance metrics differed by no more than approximately **0.000076 percentage points** from metrics calculated using the frozen returns.
- Inflation evaluation, historical stress checks, cost sensitivity, gold-cap scenarios, and the matched benchmark were recalculated and checked against the research results.

The portfolio notebook was run successfully in Colab with the repository data restored. **An additional independently verified, clean-environment installation and execution of the full optimization notebook has not yet been completed.**

## Limitations

- The ETF universe and highlighted comparisons were chosen retrospectively; selection and interpretation biases remain possible.
- Historical expected-return estimates are sensitive to the training window, while realized investment outcomes are uncertain.
- Rolling inflation-beating windows overlap and are not independent observations.
- The static exposure-matched benchmark is constructed using information from the full evaluation period and is **ex post**, not a deployable ex-ante comparator.
- Trading cost assumptions are simplified; taxes, slippage, liquidity, and investor-specific constraints are not modeled in full.
- Historical stress periods and results do not establish future resilience or performance.

**For educational and research purposes only. This repository does not provide investment advice.**
