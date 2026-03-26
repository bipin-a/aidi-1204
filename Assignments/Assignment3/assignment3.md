# Assignment 3: Data Pipeline for Statistical Analysis

**Part 1 of 2 — Backend Data Engineering**

---

## Overview

In this assignment, you will build a small data pipeline that pulls data from public APIs, stores raw snapshots locally, and transforms the data into a clean, joined dataset using a **medallion architecture**:

**Bronze → Silver → Gold**

This is **Part 1** of a two-part project. In Part 2, you will use your Gold dataset in a **Streamlit** app to explore the data and run statistical tests such as:
- one-sample t-tests,
- two-sample t-tests,
- paired t-tests,
- and proportion-based z-tests.

That means your engineering choices in Part 1 directly shape what statistical analysis is possible in Part 2.

As you build your pipeline, you should already be thinking about:
- What question do I want to answer?
- What variables do I need?
- What groups do I want to compare?
- What kind of test should my dataset support?

This assignment uses **local folders and GitHub for simplicity**. In a more realistic workflow, Bronze / Silver / Gold data would usually be stored in cloud object storage such as **Google Cloud Storage** or **Amazon S3**, not directly in a GitHub repo. We will introduce cloud storage later in the course. For now, focus on the pipeline logic, file organization, and dataset design.

---

## Learning Goals

By the end of this assignment, you should be able to:

1. Pull data programmatically from public APIs
2. Save raw snapshots and organize them into Bronze / Silver / Gold layers
3. Clean and standardize raw API data
4. Join multiple sources into one analysis-ready dataset
5. Design a dataset with future hypothesis testing in mind
6. Use AI tools productively while still understanding and documenting your work

---

## AI Tools

You are encouraged to use **GitHub Copilot**, **ChatGPT**, or similar AI tools for this assignment.

Good uses include:
- writing starter API request code,
- helping parse JSON,
- generating pandas or DuckDB transformation boilerplate,
- debugging errors,
- exploring unfamiliar libraries,
- and brainstorming possible hypothesis tests.

You are still responsible for:
- understanding the code you submit,
- verifying that joins and transformations are correct,
- deciding which features to create,
- and explaining how your dataset supports future analysis.

### AI Disclosure Requirement

In your `README.md`, include a short section called **AI Usage** that describes:
- which AI tools you used,
- what they helped with,
- and one example of something you had to verify or fix yourself.

---

## Architecture

```text
Public APIs
   ↓
Ingestion Scripts (Python)
   ↓
data/bronze/   ← raw snapshots, untouched
   ↓
Transform Code (Python and/or DuckDB SQL)
   ↓
data/silver/   ← cleaned, one table per source
   ↓
Join + Feature Engineering
   ↓
data/gold/     ← analysis-ready dataset for Part 2
```

| Layer | Purpose | Format | Example |
|---|---|---|---|
| **Bronze** | Raw API responses saved exactly as returned | JSON or raw CSV | `bronze/binance/btc_klines_2026-03-25T14-00-00.json` |
| **Silver** | Parsed, cleaned, typed, standardized | CSV or Parquet | `silver/btc_daily_clean.csv` |
| **Gold** | Joined, feature-engineered, analysis-ready | CSV or Parquet | `gold/crypto_sentiment_daily.csv` |

---

## Choose One API Pack

Each team must choose **one** of the following packs and use the **2 required APIs** in that pack.

These packs are meant to make the project easier to think about. Each one gives you:
- a natural data story,
- a clean way to join data,
- and a clear path toward hypothesis testing in Part 2.

If you want to use a different public API, you may propose it with instructor approval, but your sources should still share a simple join key such as `date`.

---

### Pack A — Crypto & Sentiment

Best for: one-sample t-tests, two-sample t-tests, proportion z-tests

| API | Auth | What You Pull |
|---|---|---|
| **Binance Market Data** | None | Daily OHLCV for BTC or ETH |
| **Alternative.me Fear & Greed Index** | None | Daily crypto sentiment score |

**Example Gold columns:**  
`date, btc_close, btc_volume, btc_daily_return, fear_greed_value, fear_greed_label, positive_return`

**Example Part 2 questions:**
- Is mean daily BTC return different from 0?
- Do returns differ on Fear vs Greed days?
- Is the proportion of positive-return days higher on Greed days?

---

### Pack B — Weather & Air Quality

Best for: paired tests, two-sample tests, proportion z-tests

| API | Auth | What You Pull |
|---|---|---|
| **Open-Meteo Historical Weather** | None | Daily weather for a city |
| **Open-Meteo Air Quality** | None | Hourly PM2.5, PM10, AQI |

**Example Gold columns:**  
`date, city, temp_max, precipitation, pm25, aqi, bad_air_day`

**Example Part 2 questions:**
- Is average PM2.5 different from a benchmark value?
- Do PM2.5 levels differ between two cities?
- Is the proportion of bad-air days higher in one city than another?

---

### Pack C — NASA Events & Weather

Best for: one-sample t-tests, two-sample t-tests, proportion z-tests

| API | Auth | What You Pull |
|---|---|---|
| **NASA EONET** | None | Natural events metadata by date and category |
| **Open-Meteo Historical Weather** | None | Daily weather for a city or region |

**Example Gold columns:**  
`date, event_count, wildfire_count, storm_count, temp_max, precipitation_sum, rainy_day, event_day`

**Example Part 2 questions:**
- Is mean daily event count different from a benchmark value?
- Do event counts differ on rainy vs non-rainy days?
- Is the proportion of event days different across seasons?

---

## API Quick-Start Examples

These are minimal working examples to help you get started. Do **not** submit these as your entire solution. Add file saving, timestamps, and basic error handling.

### Binance — daily BTC candles

```python
import requests

url = "https://api.binance.com/api/v3/klines"
params = {"symbol": "BTCUSDT", "interval": "1d", "limit": 365}
response = requests.get(url, params=params)
data = response.json()

# Returns a list of lists:
# [open_time, open, high, low, close, volume, ...]
```

### Alternative.me Fear & Greed Index

```python
import requests

url = "https://api.alternative.me/fng/"
params = {"limit": 365, "format": "json"}
response = requests.get(url, params=params)
data = response.json()

# Returns:
# {"data": [{"value": "74", "value_classification": "Greed", "timestamp": "1711324800", ...}]}
```

### Open-Meteo Historical Weather

```python
import requests

url = "https://archive-api.open-meteo.com/v1/archive"
params = {
    "latitude": 43.65,
    "longitude": -79.38,
    "start_date": "2024-01-01",
    "end_date": "2025-03-20",
    "daily": "temperature_2m_max,temperature_2m_min,precipitation_sum"
}
response = requests.get(url, params=params)
data = response.json()

# Returns:
# {"daily": {"time": [...], "temperature_2m_max": [...], ...}}
```

### Open-Meteo Air Quality

```python
import requests

url = "https://air-quality-api.open-meteo.com/v1/air-quality"
params = {
    "latitude": 43.65,
    "longitude": -79.38,
    "start_date": "2024-01-01",
    "end_date": "2025-03-20",
    "hourly": "pm2_5,pm10,us_aqi"
}
response = requests.get(url, params=params)
data = response.json()

# Returns hourly data that you will aggregate to daily in Silver.
```

### NASA EONET — natural events metadata

```python
import requests

url = "https://eonet.gsfc.nasa.gov/api/v3/events"
params = {
    "status": "all",
    "start": "2024-01-01",
    "end": "2024-12-31",
    "category": "wildfires,severeStorms"
}
response = requests.get(url, params=params)
data = response.json()

# Returns event metadata with titles, categories, geometry, and dates.
# Good for aggregating to daily event counts in Silver.
```

---

## Step-by-Step Instructions

### Step 1 — Set up your environment

1. Create a new GitHub repo and clone it locally.
2. Create a virtual environment.
3. Install the packages you need.
4. Create a `.env` file if needed.
5. Create the local folders:
   - `data/bronze/`
   - `data/silver/`
   - `data/gold/`

**Note:** none of the default APIs in these packs require API keys. You should still include a `.env.example` file to practice the habit of separating configuration from code.

---

### Step 2 — Write ingestion scripts (Bronze)

For each API source, write a script in `ingest/` that:

1. Calls the API using `requests`
2. Saves the raw response locally with a timestamp in the filename
3. Places the file in the correct `data/bronze/<source>/` folder

General pattern:

```python
import requests, json
from datetime import datetime

response = requests.get(url, params=params)
response.raise_for_status()
data = response.json()

ts = datetime.now().strftime("%Y-%m-%dT%H-%M-%S")
filename = f"data/bronze/binance/btc_klines_{ts}.json"

with open(filename, "w") as f:
    json.dump(data, f, indent=2)

print(f"Saved {filename}")
```

You must run your ingestion pipeline **at least twice** so Bronze contains multiple snapshots. These runs may be on the same day or different days.

---

### Step 3 — Transform data (Silver)

Write transformation code in Python, DuckDB SQL, or both.

For each source:

1. Parse raw JSON into a flat table
2. Convert timestamps into proper date columns
3. Rename columns clearly and consistently
4. Handle missing values and document what you did
5. Cast data types correctly
6. Save cleaned outputs in `data/silver/`

#### Example: Binance Bronze → Silver

```python
import json
import pandas as pd

with open("data/bronze/binance/btc_klines_example.json", "r") as f:
    raw = json.load(f)

df = pd.DataFrame(raw, columns=[
    "open_time", "open", "high", "low", "close", "volume",
    "close_time", "quote_asset_volume", "num_trades",
    "taker_buy_base", "taker_buy_quote", "ignore"
])

df["date"] = pd.to_datetime(df["open_time"], unit="ms").dt.date
df["btc_close"] = df["close"].astype(float)
df["btc_volume"] = df["volume"].astype(float)

silver = df[["date", "btc_close", "btc_volume"]]
silver.to_csv("data/silver/btc_daily_clean.csv", index=False)
```

#### Notes for Pack B

Open-Meteo Air Quality returns **hourly** values. In Silver, you should aggregate them to daily values such as:
- daily mean PM2.5,
- daily max AQI,
- or another clearly documented rule.

#### Notes for Pack C

NASA EONET returns **event metadata**, not a ready-made daily table. In Silver, you will likely:
- extract the event date from the geometry or event fields,
- group by date,
- and create daily counts such as `event_count`, `wildfire_count`, or `storm_count`.

---

### Step 4 — Create Gold dataset

Read your Silver datasets and join them into one analysis-ready table.

1. Join all Silver datasets on a clear key such as `date`
2. Handle mismatched dates and explain your strategy
3. Create derived columns for Part 2
4. Save the final dataset in `data/gold/`

Examples of useful derived columns:
- `btc_daily_return`
- `positive_return`
- `bad_air_day`
- `rainy_day`
- `event_day`
- `is_weekend`

Your Gold table should be **small, intentional, and analysis-ready**. Do not dump every raw column into Gold.

#### DuckDB option

```sql
SELECT
    b.date,
    b.btc_close,
    b.btc_volume,
    (b.btc_close - LAG(b.btc_close) OVER (ORDER BY b.date))
        / LAG(b.btc_close) OVER (ORDER BY b.date) AS btc_daily_return,
    f.fear_greed_value,
    f.fear_greed_label
FROM read_csv_auto('data/silver/btc_daily_clean.csv') b
JOIN read_csv_auto('data/silver/fear_greed_clean.csv') f
    ON b.date = f.date
ORDER BY b.date;
```

---

### Step 5 — Write your Statistical Analysis Preview

Before you finalize your Gold table, write a short planning memo in `analysis_preview.md` that answers:

1. What is one statistical question you plan to answer in Part 2?
2. What is your outcome variable?
3. What is your grouping variable, if any?
4. What is one binary variable you created, and why?
5. What null and alternative hypotheses might you test?
6. Which test do you think fits best, and why?

This does not need to be perfect. The goal is to show that you are already thinking about how your pipeline supports later analysis.

---

## Repo Requirements

Your GitHub repo should include:

```text
your-repo/
├── README.md
├── requirements.txt
├── .env.example
├── .gitignore
├── analysis_preview.md
├── data/
│   ├── bronze/
│   ├── silver/
│   └── gold/
├── ingest/
├── transform/
└── notebooks/
```

### Notes on data files

You may include:
- small representative Bronze files,
- cleaned Silver outputs,
- and your final Gold dataset.

If your raw files are too large, include a smaller sample and explain your approach in the `README`.

---

## Submission Format

Submit:

1. **A link to your GitHub repository**
    - *Must include the analysis_preview.md and the README.md etc. files as described above.*
2. **A link to one recorded demo video**

Your video must:
- be **15 minutes or less**
- include **facecam on**
- include **screen recording**
- include **audio narration**
- clearly walk through your ETL pipeline
- explain how your Gold dataset is designed for later statistical analysis
- describe at least one hardship, bug, or challenge your team encountered and how you handled it

**Screenshots are not required.**

Your repo and your demo video are the only required submission items.

---

## Demo Expectations

In your demo video, you should show and explain:
- which APIs you chose and why
- how your Bronze, Silver, and Gold layers are organized
- one example of raw API data and how it was transformed
- how you cleaned and joined the data
- at least one derived feature you created for Part 2
- one statistical question you may answer later
- which statistical test you think may be appropriate, and why
- one hardship, debugging issue, or design challenge your team faced

The goal of the demo is not just to prove that the code runs. The goal is to show that you understand:
- the pipeline you built,
- the dataset you created,
- and how it connects to future statistical analysis.

---

## Looking Ahead — Part 2

In Part 2, you will build a **Streamlit app** that reads your Gold dataset, not the raw APIs.

Your app will likely:
- display charts and filters,
- let the user choose variables or groups,
- run hypothesis tests,
- and explain results in plain language.

Your Gold dataset should support at least:

| Test | What It Needs |
|---|---|
| One-sample t-test | One continuous numeric column |
| Two-sample or paired t-test | One continuous column + one grouping column |
| Proportion z-test | One binary outcome column + one grouping column |

If your Gold dataset has:
- a continuous metric,
- a binary variable,
- and a grouping variable,

then you are in good shape for Part 2.


---

## Tips for Success

- Start with one API and get it working end-to-end before adding the second.
- Print sample JSON before trying to parse everything.
- Keep your join key simple.
- Do not over-engineer this.
- Use AI tools strategically, but verify what they produce.
- Think about the statistical question early.
- Build Gold so Part 2 feels easier, not harder.

---

## Recommended Resources

| Resource | Link |
|---|---|
| Binance API docs | `https://developers.binance.com/docs/binance-spot-api-docs/rest-api/market-data-endpoints#klinecandlestick-data` |
| Alternative.me Fear & Greed API | `https://alternative.me/crypto/fear-and-greed-index/#api` |
| Open-Meteo Historical Weather API | `https://open-meteo.com/en/docs/historical-weather-api` |
| Open-Meteo Air Quality API | `https://open-meteo.com/en/docs/air-quality-api` |
| NASA EONET docs | `https://eonet.gsfc.nasa.gov/docs/v3` |
| DuckDB documentation | `https://duckdb.org/docs/` |
| Pandas documentation | `https://pandas.pydata.org/docs/` |
| GitHub Copilot for Students | `https://education.github.com/pack` |
