# ECON 320 — Walk-Forward CAPM & Return Predictability Study

Econometrics research project examining the relationship between lagged risk 
factors and future stock returns using a walk-forward CAPM estimation framework 
and panel data methods across 15 large-cap U.S. equities (2000–2024).

---

##  Research Question

Can lagged systematic risk (CAPM beta), idiosyncratic volatility, and market 
return variables predict future quarterly stock returns — and does the 
relationship implied by GBM drift hold empirically in panel data?

---

##  Methodology

### Walk-Forward Expected Return Estimation
- For each quarter `t`, the model is trained **exclusively on data prior to `t`**
- Predictions for quarter `t` are generated using only information available 
  at time `t` — eliminating any forward-looking data leakage
- Minimum training window: **20 quarters** before first prediction

### Rolling CAPM Beta
- Betas estimated via rolling OLS window (20-quarter window, 12-quarter minimum)
- Uses lagged excess returns for both stock and market to avoid simultaneity bias
- Risk-free rate: **3-Month Treasury Bill (DGS3MO)** from FRED

### Panel Regression Model
Dependent variable: `return_current` (quarterly stock return at time `t`)

Predictors (all lagged — no contemporaneous variables):

| Variable | Description |
|---|---|
| `capm_beta` | Rolling CAPM beta |
| `beta_squared` | Nonlinear / convex risk effect |
| `lagged_volatility` | Idiosyncratic risk proxy |
| `volatility_squared` | Volatility risk premium |
| `beta_volatility` | Beta × volatility interaction |
| `beta_market` | Conditional beta × lagged market return |
| `lagged_market_return` | Lagged S&P 500 return (market momentum) |

---

##  Dataset

- **Universe**: 15 large-cap U.S. equities — AAPL, MSFT, GOOGL, AMZN, TSLA, 
  NVDA, META, JPM, V, WMT, JNJ, PG, MA, DIS, NFLX
- **Benchmark**: S&P 500 (SPY) — quarterly returns from Yahoo Finance
- **Risk-Free Rate**: 3-Month Treasury Bill (FRED: `DGS3MO`)
- **Frequency**: Quarterly
- **Period**: Q1 2000 – Q4 2024
- **Source**: Yahoo Finance via `quantmod`

---

##  Repository Structure
```
├── econ320final.ipynb    # Full analysis: data pipeline, model estimation, results
```

---

##  Installation
```r
install.packages(c("quantmod", "dplyr", "lubridate"))
```

---

##  Tech Stack

- **R** — data pipeline and econometric modeling
- **quantmod** — stock price and volume retrieval
- **dplyr / lubridate** — data wrangling and time series manipulation
- **FRED API** — risk-free rate data
- **OLS (`lm`)** — rolling CAPM beta estimation and walk-forward regression

---

##  Data Integrity Notes

- All predictor variables are **strictly lagged** — no contemporaneous regressors
- Excess returns computed using the **lagged** risk-free rate to avoid leakage
- Rolling beta uses **lagged** stock and market excess returns
- Walk-forward design ensures **no future information** enters the training window
