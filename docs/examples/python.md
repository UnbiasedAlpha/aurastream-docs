# Python Examples

## Setup

```python
import requests
import pandas as pd

URL = "https://api.aurastream.unbiased-alpha.com/timeseries"
HEADERS = {"x-api-key": "YOUR_AURASTREAM_API_KEY"}
```

## Helper: Convert Response to DataFrame

```python
def to_dataframe(data, ticker):
    """Convert Aurastream response to a pandas DataFrame."""
    df = pd.DataFrame.from_dict(data[ticker], orient="index")
    df.index = pd.to_datetime(df.index)
    return df.astype(float).sort_index()

def to_series(data, ticker, field="Close"):
    """Extract a single field as a pandas Series."""
    ts = data[ticker]
    s = pd.Series({k: float(v[field]) for k, v in ts.items()})
    s.index = pd.to_datetime(s.index)
    return s.sort_index()
```

## Fetch Crypto Data (No Credentials)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT", "ETHUSDT"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "bybit",
})
data = response.json()

btc = to_dataframe(data, "BTCUSDT")
eth = to_dataframe(data, "ETHUSDT")

print(btc.head())
```

## Fetch Forex Data (With Credentials)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["EUR_USD", "GBP_USD", "USD_JPY"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1h",
    "source": "oanda",
    "price_type": "M",
    "credentials": {
        "id": "YOUR_OANDA_ACCOUNT_ID",
        "token": "YOUR_OANDA_TOKEN",
        "environment": "practice"
    }
})
data = response.json()

eur = to_dataframe(data, "EUR_USD")
```

## Multi-Source Portfolio

Combine data from different sources into a single analysis:

```python
# US equities from Yahoo (free)
equities = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["SPY", "QQQ", "IWM"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "yahoo",
}).json()

# Gold from Oanda
gold = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["XAU_USD"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "oanda",
    "credentials": {
        "id": "YOUR_OANDA_ACCOUNT_ID",
        "token": "YOUR_OANDA_TOKEN",
        "environment": "practice"
    }
}).json()

# Crypto from Bybit
crypto = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "bybit",
}).json()

# Combine closing prices. Sources stamp daily bars at different times of day
# (Yahoo at midnight New York in UTC, Oanda at the bar close, Bybit at 00:00 UTC),
# so align them on the calendar date first.
closes = {
    "SPY": to_series(equities, "SPY"),
    "QQQ": to_series(equities, "QQQ"),
    "Gold": to_series(gold, "XAU_USD"),
    "BTC": to_series(crypto, "BTCUSDT"),
}
portfolio = pd.DataFrame({k: s.groupby(s.index.normalize()).last() for k, s in closes.items()})

# Percentage returns (crypto trades at weekends, so carry the others forward)
returns = portfolio.ffill().pct_change().dropna()
print(returns.corr())
```

## Error Handling

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "bybit",
}, timeout=120)

if response.status_code == 200:
    data = response.json()
elif response.status_code == 403:
    print("Invalid API key")
else:
    print(f"Error {response.status_code}: {response.text}")
```

## Batch Processing

For large analyses, loop through date ranges or ticker batches:

```python
import datetime

tickers = ["AAPL", "MSFT", "GOOGL", "AMZN", "TSLA", "NVDA", "META"]
all_data = {}

# Fetch in batches of 5
batch_size = 5
for i in range(0, len(tickers), batch_size):
    batch = tickers[i:i + batch_size]
    resp = requests.post(URL, headers=HEADERS, json={
        "tickerlist": batch,
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "yahoo",
    }, timeout=120)
    all_data.update(resp.json())

# Build a DataFrame of closing prices
closes = pd.DataFrame({
    ticker: to_series(all_data, ticker) for ticker in tickers
})
```
