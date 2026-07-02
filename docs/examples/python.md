# Python Basics

How to fetch data from Aurastream and work with it in Python using `requests` and `pandas`.

## Setup

```python
import requests
import pandas as pd

URL = "https://api.aurastream.unbiased-alpha.com/timeseries"
HEADERS = {"x-api-key": "YOUR_AURASTREAM_API_KEY"}
```

## Fetching a Single Ticker

```python
payload = {
    "tickerlist": ["BTCUSDT"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "bybit"
}

response = requests.post(URL, json=payload, headers=HEADERS)
data = response.json()

# Convert to DataFrame
df = pd.DataFrame(data["BTCUSDT"]).T
df.index = pd.to_datetime(df.index)
df = df.astype(float)
print(df.head())
```

## Fetching Multiple Tickers

```python
payload = {
    "tickerlist": ["AAPL", "MSFT", "GOOGL"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "yahoo"
}

response = requests.post(URL, json=payload, headers=HEADERS)
data = response.json()

# Build a dict of DataFrames
frames = {}
for ticker, bars in data.items():
    df = pd.DataFrame(bars).T
    df.index = pd.to_datetime(df.index)
    df = df.astype(float)
    frames[ticker] = df

# Compare closing prices
closes = pd.DataFrame({t: f["Close"] for t, f in frames.items()})
print(closes.tail())
```

## Using Credentials for Paid Sources

Some data sources require their own API key or credentials. Pass them in the request body:

```python
payload = {
    "tickerlist": ["EUR_USD", "GBP_USD"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1h",
    "source": "oanda",
    "credentials": {
        "id": "YOUR_OANDA_ACCOUNT_ID",
        "token": "YOUR_OANDA_TOKEN",
        "environment": "practice"
    }
}

response = requests.post(URL, json=payload, headers=HEADERS)
data = response.json()
```

## Reusable Helper Function

```python
def fetch(tickerlist, source, start, end, interval="1d",
          credentials=None, **kwargs):
    """Fetch data from Aurastream and return a dict of DataFrames."""
    payload = {
        "tickerlist": tickerlist,
        "start": start,
        "end": end,
        "interval": interval,
        "source": source,
        **kwargs,
    }
    if credentials:
        payload["credentials"] = credentials

    resp = requests.post(URL, json=payload, headers=HEADERS)
    resp.raise_for_status()
    result = {}
    for ticker, bars in resp.json().items():
        df = pd.DataFrame(bars).T
        df.index = pd.to_datetime(df.index)
        df = df.astype(float)
        result[ticker] = df
    return result


# Usage
btc = fetch(["BTCUSDT"], "bybit", "2025-01-01", "2025-06-01")["BTCUSDT"]
print(btc.tail())
```

## Plotting

```python
import matplotlib.pyplot as plt

frames = fetch(["AAPL", "MSFT"], "yahoo", "2024-01-01", "2025-06-01")

fig, ax = plt.subplots(figsize=(12, 5))
for ticker, df in frames.items():
    ax.plot(df.index, df["Close"], label=ticker)

ax.set_title("AAPL vs MSFT")
ax.legend()
ax.set_ylabel("Price")
plt.tight_layout()
plt.show()
```

## Combining Sources

Aurastream's unified format makes it easy to combine data from different sources:

```python
# Equity prices from Yahoo
equities = fetch(["SPY"], "yahoo", "2024-01-01", "2025-06-01")

# Macro data from FRED
fred = fetch(
    ["USACPIALLMINMEI"],
    "fred",
    "2024-01-01",
    "2025-06-01",
    credentials={"key": "YOUR_FRED_KEY"}
)

# Both return the same {ticker: {timestamp: {fields}}} format
spy = equities["SPY"]
cpi = fred["USACPIALLMINMEI"]

print(f"SPY data points: {len(spy)}")
print(f"CPI data points: {len(cpi)}")
```
