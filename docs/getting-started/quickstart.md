# Quickstart

Get your first data in under 2 minutes.

## Prerequisites

- An Aurastream API key: sign up at the [dashboard](https://aurastream.unbiased-alpha.com): every new account starts on the **Free** plan (500 calls a month, no card needed) and the key is under **API Keys**
- Python 3.8+ with `requests` installed, or any HTTP client

## 1. Install requests

```bash
pip install requests
```

## 2. Make your first request

This example fetches daily Bitcoin data from Bybit (no exchange credentials needed):

```python
import requests

URL = "https://api.aurastream.unbiased-alpha.com/timeseries"
HEADERS = {"x-api-key": "YOUR_AURASTREAM_API_KEY"}

response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1d",
    "source": "bybit",
})

data = response.json()
print(data)
```

## 3. Parse the response

The response is a nested dictionary: `{ticker: {timestamp: {field: value}}}`. Some sources, Bybit included, send prices as strings, hence the `astype(float)`.

```python
import pandas as pd

ticker = "BTCUSDT"
df = pd.DataFrame.from_dict(data[ticker], orient="index")
df.index = pd.to_datetime(df.index)
df = df.sort_index()
df = df.astype(float)

print(df.head())
#                  Open       High        Low      Close
# 2025-06-01  104636.10  105900.00  103900.50  105700.20
# 2025-06-02  105700.20  106200.00  104500.00  105400.00
# ...
# Bybit bars are built from the mark price, so there is no Volume column.
```

## 4. Try a source that requires credentials

Some sources (Oanda, Alpaca, etc.) require your own exchange credentials. Pass them in the request body:

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["EUR_USD", "GBP_USD"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1h",
    "source": "oanda",
    "credentials": {
        "id": "YOUR_OANDA_ACCOUNT_ID",
        "token": "YOUR_OANDA_API_TOKEN",
        "environment": "practice"  # or "live"
    }
})
```

## Next Steps

- [Authentication](authentication.md) — how API keys and credentials work
- [Credentials](credentials.md) — per-source credential formats
- [Plans & Limits](plans-and-limits.md) — monthly allowance and request rate
- [API Reference](../api-reference/timeseries.md) — full parameter documentation
- [Examples](../examples/python.md) — real-world usage patterns
