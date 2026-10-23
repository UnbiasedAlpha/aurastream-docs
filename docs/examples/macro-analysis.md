# Macro Analysis Example

This example replicates a real macro analysis: comparing oil prices, commodity-linked currencies, and the dollar index to visualize how macro forces connect.

## The Setup

We fetch data from multiple sources to build a cross-asset view:

- **Brent Crude Oil** from Yahoo Finance
- **USD/NOK** (oil-linked currency) from Oanda
- **USD/BRL** (commodity-linked currency) from Yahoo Finance
- **Dollar Index (DXY)** from Yahoo Finance

## Full Code

```python
import requests
import pandas as pd
import matplotlib.pyplot as plt

URL = "https://api.aurastream.unbiased-alpha.com/timeseries"
HEADERS = {"x-api-key": "YOUR_AURASTREAM_API_KEY"}


def to_series(data, ticker, field="Close"):
    """Extract closing prices as a pandas Series."""
    ts = data[ticker]
    s = pd.Series({k: float(v[field]) for k, v in ts.items()})
    s.index = pd.to_datetime(s.index).normalize()
    return s.sort_index()


# --- 1. Oil (Brent Crude) from Yahoo ---
oil = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BZ=F"],
    "start": "2025-01-01",
    "end": "2026-07-01",
    "interval": "1d",
    "source": "yahoo",
}).json()

# --- 2. USD/NOK from Oanda (requires credentials) ---
fx_nok = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["USD_NOK"],
    "start": "2025-01-01",
    "end": "2026-07-01",
    "interval": "1d",
    "source": "oanda",
    "price_type": "A",
    "credentials": {
        "id": "YOUR_OANDA_ACCOUNT_ID",
        "token": "YOUR_OANDA_TOKEN",
        "environment": "practice"
    }
}).json()

# --- 3. USD/BRL from Yahoo ---
fx_brl = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BRL=X"],
    "start": "2025-01-01",
    "end": "2026-07-01",
    "interval": "1d",
    "source": "yahoo",
}).json()

# --- 4. Dollar Index (DXY) from Yahoo ---
dxy = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["DX-Y.NYB"],
    "start": "2025-01-01",
    "end": "2026-07-01",
    "interval": "1d",
    "source": "yahoo",
}).json()


# --- Build normalized comparison ---
brent  = to_series(oil,    "BZ=F")
usdnok = to_series(fx_nok, "USD_NOK")
usdbrl = to_series(fx_brl, "BRL=X")
dollar = to_series(dxy,    "DX-Y.NYB")

# Normalize: percentage change from start date
norm = pd.DataFrame({
    "Brent Crude":  brent  / brent.iloc[0]  * 100 - 100,
    "USD/NOK":      usdnok / usdnok.iloc[0] * 100 - 100,
    "USD/BRL":      usdbrl / usdbrl.iloc[0]  * 100 - 100,
    "Dollar Index": dollar / dollar.iloc[0]  * 100 - 100,
}).ffill()


# --- Plot ---
fig, ax = plt.subplots(figsize=(12, 6))
norm.plot(ax=ax, linewidth=1.8)
ax.set_title("Three stories. One macro picture.", fontsize=16, fontweight="bold")
ax.set_ylabel("% change from Jan 2025")
ax.axhline(0, color="grey", linewidth=0.5, linestyle="--")
ax.legend(loc="best", fontsize=11)
ax.grid(True, alpha=0.3)
fig.tight_layout()
plt.savefig("aurastream_macro.png", dpi=150)
plt.show()
```

## What This Shows

Normalizing each series to percentage change from a common start date puts them on one scale, so you can check:

- whether oil and the oil-linked currencies (NOK, BRL) move together over the period
- how the dollar index moves against commodity prices
- how much larger the moves in an emerging-market currency (BRL) are

Each source is one request and returns the same `{ticker: {timestamp: {field: value}}}` shape. Daily bars are stamped at different times of day by different sources (Yahoo at midnight New York in UTC, Oanda at the bar close), which is why `to_series` normalizes the index to the date.

## Adding Inflation from FRED

FRED rows hold every published version of a value rather than OHLC fields (see [FRED](../data-sources/fred.md)). To line up US CPI with the prices above as it was known at the time, index it by publication date:

```python
cpi = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["CPIAUCSL"],
    "start": "2025-01-01",
    "end": "2026-07-01",
    "interval": "1m",
    "source": "fred",
    "credentials": {"key": "YOUR_FRED_KEY"}
}).json()

# One row per publication date; keep the newest period released that day
released = pd.Series({
    pd.Timestamp(published).normalize(): max(row["updates"], key=lambda u: u["reference_date"])["value"]
    for published, row in cpi["CPIAUCSL"].items()
}).sort_index()

norm["US CPI (as released)"] = (released / released.iloc[0] * 100 - 100).reindex(norm.index, method="ffill")
# then run the plot block again
```
