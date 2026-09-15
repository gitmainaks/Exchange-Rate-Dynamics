# 💱 INR/USD Exchange Rate Dynamics — A Vector Autoregression Study

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-VAR-blue)
![pandas](https://img.shields.io/badge/pandas-data%20wrangling-150458?logo=pandas&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

> 📈 **A macro-financial VAR model exploring what actually moves the Indian Rupee against the US Dollar — interest rate differentials, global risk, RBI intervention, portfolio flows, oil prices, and current account sustainability — using monthly data from January 2013 to December 2025.**

---

## 📖 Overview

This project builds a **reduced-form Vector Autoregression (VAR)** model to study the short-run dynamic transmission mechanisms driving the **INR/USD exchange rate**. Rather than assuming textbook interest-rate parity holds cleanly, the model tests it empirically alongside four competing (and complementary) theoretical channels:

| 🔑 Channel | Theoretical Basis |
|---|---|
| 💵 **Interest Rate Parity** | Uncovered Interest Parity (UIP) — Fama (1984) |
| 🌍 **Global Financial Cycle** | VIX-driven risk-on/risk-off capital flows |
| 🏦 **RBI Intervention** | Central bank FX reserve management |
| 📊 **Portfolio Balance** | Net FPI (equity + debt) flows — Gabaix & Maggiori (2015) |
| 🛢️ **Commodity / Import Cost** | Oil price pass-through to the trade balance |
| ⚖️ **External Sustainability** | Current Account Balance as % of GDP — Kremens et al. (2024) |

The central empirical question: **does global risk and RBI intervention suppress the classical interest-parity transmission mechanism?**

---

## 🗂️ Notebook Structure

The analysis in [`Model-1__VAR_.ipynb`](./Model-1__VAR_.ipynb) runs through 12 sequential parts:

| Part | Description |
|:---:|---|
| 1️⃣ | ⚙️ Environment Setup & Library Imports |
| 2️⃣ | 📥 Data Import & Initial Inspection |
| 3️⃣ | 🩹 Missing Value Imputation |
| 4️⃣ | 💱 Data Manipulation (INR → USD conversion) |
| 5️⃣ | 🛠️ Feature Engineering (CAB/GDP, interest differentials, inflation, logs) |
| 6️⃣ | 🔍 Exploratory Data Analysis (descriptives, correlations, rolling windows) |
| 7️⃣ | 🧪 Unit Root Tests — ADF + KPSS + Phillips-Perron |
| 8️⃣ | 📐 Stationarity Transformations |
| 9️⃣ | 🧮 Vector Autoregression — lag selection, estimation, diagnostics |
| 🔟 | 📡 Impulse Response Functions (IRF) & Forecast Error Variance Decomposition (FEVD) |
| 1️⃣1️⃣ | 🧠 Interpretation |
| 1️⃣2️⃣ | ✅ Conclusion |

---

## 🧮 Model Specifications

Three nested variable sets test increasingly rich theoretical claims:

- **`CORE_VARS`** — replicates a baseline EViews-style specification (exchange rate, interest differential, forward premium, FPI flows)
- **`EXTD_VARS`** — adds the global financial cycle (VIX) and RBI intervention proxy
- **`FULL_VARS`** *(primary model)* — adds oil prices and CAB/GDP for full robustness

```text
FULL_VARS = [
    LOG_INR_USD,              # exchange rate (log)
    NOM_INT_DIFF_MKT,         # India–US interest rate differential
    3M_ForwardPremium_IND(%), # forward market premium
    FPI_Total,                # net portfolio flows (equity + debt)
    LOG_VIX_US,               # global risk / financial cycle
    RBI_INTV,                 # RBI FX intervention proxy (ΔFCA)
    LOG_OIL,                  # Brent crude oil price (log)
    CAB_PCT_GDP,              # current account balance, % of GDP
]
```

### Methodology at a glance
- ✅ **Unit root testing** via ADF, KPSS, and Phillips-Perron for a robust stationarity consensus
- ✅ **Stationarity transformations** (first-differencing for I(1) series) before estimation
- ✅ **Lag-order selection** via AIC/BIC/HQIC — HQIC used as the operative criterion to avoid AIC's small-sample over-selection
- ✅ **Cholesky ordering** from most exogenous (global shocks) → most endogenous (exchange rate)
- ✅ **Stability, whiteness, and Granger-causality diagnostics**
- ✅ **Orthogonalized IRFs, cumulative IRFs, and FEVD** over a 24-month horizon

---

## 🔑 Key Findings

- 🧩 **The UIP puzzle holds**: the interest rate differential is the *only* variable that Granger-causes the exchange rate, but a **widening differential is followed by rupee depreciation**, not appreciation — consistent with Fama (1984)'s classic puzzle rather than textbook parity.
- 🐌 **Current account sustainability is a slow-moving channel**: CAB/GDP's effect builds gradually, becoming the second-largest cumulative driver by month 24.
- 🌐 **Global risk and oil both carry the theoretically correct sign** — higher VIX or costlier oil precedes rupee depreciation, with oil's effect being the largest in the system by the two-year horizon.
- 🔀 **A spot-forward disconnect**: forward premium shocks imply rupee *appreciation*, the opposite sign to the interest-differential shock.
- 🏦 **RBI intervention and FPI flows Granger-fail** individually, likely due to endogeneity and offsetting fast/slow flow dynamics — yet FPI still retains a meaningful variance share.
- 📊 Roughly **half of the rupee's monthly forecast error variance** is explained by the six external shocks; the rest is attributable to the exchange rate's own innovations.

*(Full discussion in Parts 11–12 of the notebook.)*

---

## 📚 Data

| Attribute | Detail |
|---|---|
| 🗓️ **Frequency** | Monthly |
| 📅 **Period** | January 2013 – December 2025 |
| 📁 **Source file** | `Sub_Sample.xlsx` |
| 🔢 **Observations** | ~154–158 (varies by transformation) |

> ⚠️ The raw data file (`Sub_Sample.xlsx`) is **not included** in this repository. Place it in the project root before running the notebook.

---

## 🛠️ Tech Stack

| Library | Purpose |
|---|---|
| `pandas`, `numpy` | Data wrangling & numerical computation |
| `matplotlib`, `seaborn` | Visualization |
| `statsmodels` | ADF, KPSS, Granger causality, VAR estimation, IRF/FEVD |
| `arch` | Phillips-Perron unit root test |
| `scipy` | Statistical utilities |

---

## 🚀 Getting Started

```bash
# Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# Install dependencies
pip install pandas numpy matplotlib seaborn statsmodels arch scipy openpyxl

# Launch the notebook
jupyter notebook "Model-1__VAR_.ipynb"
```

Make sure `Sub_Sample.xlsx` is placed in the same directory before running.

---

## 🔮 Future Extensions

- 🧬 A **structural VAR (SVAR)** with theory-restricted contemporaneous identification, beyond Cholesky ordering
- ⛓️ A **VECM**, since four of the eight series are individually I(1)
- ⏱️ **Higher-frequency (weekly/daily) data** to better isolate fast-moving FPI and RBI intervention channels
- 🧪 Robust/HAC or bootstrapped standard errors given residual autocorrelation and non-normality

---

## 📄 License

This project is released under the [MIT License](./LICENSE).

## 🙏 References

- Fama, E. F. (1984) — *Forward and Spot Exchange Rates*
- Gabaix, X. & Maggiori, M. (2015) — *International Liquidity and Exchange Rate Dynamics*
- Gourinchas, P-O. & Rey, H. (2005) — External Adjustment Sustainability
- Granger, C.W.J. & Newbold, P. (1974) — *Spurious Regressions in Econometrics*
- Kremens, L. et al. (2024) — Current Account & Exchange Rate Dynamics
- Lütkepohl, H. (2005) — *New Introduction to Multiple Time Series Analysis*

---

<p align="center">
  <i>Built with 🧠 econometrics, ☕ patience, and a healthy respect for the UIP puzzle.</i>
</p>
