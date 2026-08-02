# 🌤️ Giza Weather dataset Cleaning then EDA 

A full time-series deep dive into 3 years of daily weather in Giza, Egypt — sourced straight from NASA POWER.

---

## ✅ What's Done

### 📡 Data Acquisition
- **Source:** NASA POWER (daily)
- **Location:** Giza, Egypt (30.1355°N, 31.3251°E)
- **Range:** 2022-08-01 → 2025-07-31 (**1,096 days**, leap year included)
- **Variables:** temp (avg/max/min), precipitation, humidity, wind speed, cloud cover
- Raw response saved before any cleaning ✔️

### 🧹 Data Preparation
- Rebuilt `YEAR` + `DOY` into a real `datetime` index — no more guessing what row 482 means
- Verified: **zero duplicate dates, zero missing days** — the calendar is airtight
- Verified: **zero missing values**
- Confirmed units are consistent and sane (°C, mm/day, %, m/s)
- Renamed cryptic NASA codes (`T2M`, `PRECTOTCORR`...) into human-readable, unit-labeled columns
- Exported the clean dataset to CSV ✔️

### 📊 3.1 — Descriptive Overview
- Temperature (avg/max/min) is **clearly bimodal** — Giza doesn't do "mild," it does winter *or* summer, with a dip in between
- Precipitation is **heavily zero-inflated** — rain is rare, and when it happens, it happens hard (up to 42mm in a day)
- Log-scale + rainy-days-only views used to properly reveal the shape of actual rainfall

### 📈 3.2 — Temporal Patterns
- Full-range line plots confirm a clean, repeating annual temperature wave (3 visible summer/winter cycles)
- 7-day and 30-day rolling means show short-term persistence riding on top of the seasonal curve
- **Monthly boxplots** reveal a smooth seasonal arc — coldest in **January (~14°C)**, hottest in **July (~31°C)**
- **March–April (spring)** is the most variable/unpredictable season; summer and winter are tight and consistent
- Rolling std dev shows local volatility isn't constant — some periods are noticeably choppier than others

### 🔬 3.3 — Decomposition & Structure
- `seasonal_decompose` cleanly split the series into trend, seasonal, and residual components
- **Trend**: a mild oscillation (~22.3°C → 23.1°C → 22.4°C) over the 3 years — not a clear directional warming/cooling signal
- **Seasonal**: a stable, repeating ±10–13°C swing around the trend, every year, like clockwork
- **Residual**: random noise around 0 (±4°C) — confirms the model captured the structure well, nothing left unexplained
- **ACF/PACF**: strong short-lag autocorrelation + a repeating bump at ~365-day lag — independent confirmation of yearly seasonality
- **ADF stationarity test**: p-value = 0.32 → **series is NOT stationary** (as expected, given strong seasonality) — a seasonally-aware model (e.g. SARIMA) would be needed for any future forecasting work

### 🔗 3.4 — Relationships & Anomalies
- **Correlation matrix**: strongest relationship is **temp vs humidity (r = −0.73)** — classic dry-heat desert signature
- Secondary links: temp vs cloud cover (−0.31), temp vs wind speed (+0.35, a mild surprise worth flagging)
- Precipitation correlates weakly with everything else — consistent with rain being rare and event-driven, not smoothly continuous
- **Outlier detection** done properly via seasonally-adjusted (decomposition residual) z-scores — avoids the classic mistake of flagging a normal winter day as "extreme" just because it's cold compared to the yearly average
- **Data-quality sweep**: zero physically impossible values found (no negative rain, no humidity/cloud outside 0–100%, no min > max temp) — confirms the dataset is clean at the source, and the only "unusual" values are real weather extremes, not sensor errors

### 🧮 Bonus — Quick-Answer Stats
- Hottest / coldest / rainiest / cloudiest / windiest months identified via groupby aggregation
- Yearly breakdown of days above vs. below 25°C max temp (noting 2022 & 2025 are partial years)

---

## 🧠 Key Takeaway
Giza's weather is **highly seasonal and predictable** for temperature (strong autocorrelation + repeating annual cycle), but **precipitation is rare, extreme-driven, and not reliably learnable** from 3 years of data (only ~3 seasonal cycles, and rain is a near-zero-inflated event). A future ML pipeline built on this data would be reasonably feasible for temperature forecasting, but not for precipitation prediction.

---

## 📦 Deliverables Ready
- ✔️ Cleaned dataset (CSV)
- ✔️ Full EDA notebook (Parts 1–3.4)
  

---

*No missing data. No sketchy sentinels. No excuses. Just Giza, being Giza.* 🏜️
