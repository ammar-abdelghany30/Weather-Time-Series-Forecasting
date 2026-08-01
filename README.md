# 🌤️ Giza Weather EDA — Progress So Far

A time-series deep dive into 3 years of daily weather in Giza, Egypt — sourced straight from NASA POWER.

---

## ✅ What's Done

### 📡 Data Acquisition
- **Source:** NASA POWER 
- **Location:** Giza, Egypt (30.1355°N, 31.3251°E)
- **Range:** 2022-08-01 → 2025-07-31 (**1,096 days**, leap year included)
- **Variables:** temp (avg/max/min), precipitation, humidity, wind speed, cloud cover

### 🧹 Data Preparation
- Rebuilt `YEAR` + `DOY` into a real `datetime` index — no more guessing what row 482 means
- Verified: **zero duplicate dates, zero missing days** — the calendar is tight
- Verified: **zero missing values**
- Confirmed units are consistent and sane (°C, mm/day, %, m/s)
- Renamed cryptic NASA codes (`T2M`, `PRECTOTCORR`...) into human-readable, unit-labeled columns
- Exported the clean dataset to CSV ✔️

### 📊 EDA — Descriptive Overview (in progress)
- Temperature (avg/max/min) is **clearly bimodal** — Giza doesn't do "mild," it does winter *or* summer, with a dip in between
- Precipitation is **heavily zero-inflated** — rain is rare, and when it happens, it happens hard (up to 42mm in a day)
- Log-scale view lined up to properly reveal the shape of actual rainy days

---

## 🔜 Next Up
- Finish distribution plots for humidity, wind, cloud cover
- Line plots + rolling trends 
- Monthly seasonality boxplots
- Decomposition, ACF/PACF, stationarity test
- Correlation + outlier detection

---

*No missing data. No sketchy sentinels. No excuses. Just Giza, being Giza.* 🏜️