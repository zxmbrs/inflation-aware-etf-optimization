# Inflation-Aware Portfolio Optimization

**Balancing Downside Risk and Long-Term Purchasing Power**

**Author:** Xunmo Zhao  
**Project type:** Independent Research Project  
**Date:** October 2026

## Overview

This project investigates the trade-off between portfolio
downside-risk control and long-term inflation-adjusted
purchasing-power preservation.

It examines whether adding estimated-return constraints
to Conditional Value-at-Risk (CVaR) optimization improves
historical real-wealth outcomes while controlling tail losses.

The study uses Canadian-listed exchange-traded funds
(ETFs) covering equities, bonds, and gold.

## Research Questions

1. Can minimum-risk portfolios preserve purchasing power?
2. How do return constraints affect downside risk and
   inflation-adjusted performance?
3. Does dynamic CVaR optimization outperform simpler
   portfolio allocations?
4. How sensitive are results to trading costs,
   gold concentration, and historical market stress?

## Methodology

The following approaches are compared:

- Equal Weighting (EW)
- Global Minimum Variance (GMV)
- Minimum CVaR at the 95% confidence level
- Return-Constrained CVaR with nominal annual targets
  of 0%, 2%, 4%, 6%, and 8%
- A retrospective exposure-matched static benchmark

**Research design:**

- Seven Canadian-listed ETFs
- 60-month rolling estimation windows
- 101 historical test months: May 2018–September 2026
- 99 CPI-matched months: June 2018–August 2026

The portfolio optimization uses historical nominal returns.
Canadian CPI is used for retrospective evaluation,
not as an optimization input.

## Selected Results

| Strategy | Annualized nominal return | Monthly CVaR 95% |
|---|---:|---:|
| Expanded Equal Weight | 9.72% | 4.41% |
| Expanded Minimum CVaR | 4.08% | 1.56% |
| CVaR Target 6% | 7.39% | 2.63% |
| CVaR Target 8% | 9.63% | 3.38% |
| Exposure-Matched Benchmark | 9.17% | 2.84% |

The 8% target strategy achieved historical inflation-beating
frequencies of 85.23% for overlapping one-year windows
and 89.06% for overlapping three-year windows.

However, the retrospective exposure-matched benchmark
exhibited lower volatility, maximum drawdown, and CVaR
than the dynamic 8% target strategy.

These findings describe historical trade-offs, not
independent future success probabilities or guarantees.

## Data Sources

**ETF prices:** Yahoo Finance, accessed using `yfinance`.

**Canadian inflation:** Statistics Canada,
Consumer Price Index, Table 18-10-0004-01,
Canada All-items, not seasonally adjusted.

**Assets:**

- XIC.TO — Canadian equities
- VFV.TO — U.S. equities
- XEF.TO — Developed international equities
- XEC.TO — Emerging-market equities
- XBB.TO — Canadian aggregate bonds
- XSB.TO — Canadian short-duration bonds
- CGL-C.TO — Gold

## Project Structure

```text
notebooks/        Research notebooks
data/processed/   Frozen ETF and CPI datasets
results/figures/  Research figures
results/tables/   Results and validation summaries
report/           Full research report
requirements.txt  Python dependencies
```

## Reproducibility Status

The frozen ETF and inflation datasets passed structural,
missing-value, and internal consistency checks.

The saved monthly ETF returns were reconstructed from
the frozen monthly prices, with a maximum absolute
difference of approximately 4.45e-16.

The data validation notebook also passed independent
fresh-kernel execution.

**Full computational reproduction of all portfolio
optimization results remains pending.**

A complete clean-environment dependency installation
test has not yet been performed.

## Limitations

- ETF selection and highlighted strategies were
  evaluated retrospectively.
- Rolling inflation-beating windows overlap substantially.
- Estimated returns are sensitive to the historical window.
- The exposure-matched benchmark uses ex post weights.
- Trading costs are simplified and investor-specific
  taxation is excluded.

Results are historical and exploratory.

**This repository is for educational and research
purposes only. It does not provide investment advice.**

## Author

**Xunmo Zhao**  
Independent Research Project, October 2026
