# Macro Analysis

Building a multi-source macro dashboard using Aurastream's economics and alternative data sources.

## Central Bank Policy Rates

Compare policy rates across countries using SDMX (BIS data):

```python
import requests
import pandas as pd

URL = "https://api.aurastream.unbiased-alpha.com/timeseries"
HEADERS = {"x-api-key": "YOUR_AURASTREAM_API_KEY"}

payload = {
    "tickerlist": [
        "BIS,WS_CBPOL,1.0/D.US",
        "BIS,WS_CBPOL,1.0/D.GB",
        "BIS,WS_CBPOL,1.0/D.XM",
        "BIS,WS_CBPOL,1.0/D.JP"
    ],
    "start": "2020-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "sdmx",
    "db": "BIS",
    "l1": "CBRate"
}

response = requests.post(URL, json=payload, headers=HEADERS)
data = response.json()

# Parse into a single DataFrame of rates
rates = {}
labels = {"D.US": "Fed Funds", "D.GB": "BoE", "D.XM": "ECB", "D.JP": "BoJ"}
for ticker, bars in data.items():
    df = pd.DataFrame(bars).T
    df.index = pd.to_datetime(df.index)
    key = ticker.split("/")[-1]
    rates[labels.get(key, key)] = df.iloc[:, 0].astype(float)

rates_df = pd.DataFrame(rates).ffill()
print(rates_df.tail(10))
```

## Inflation Tracking with FRED

```python
indicators = {
    "CPIAUCSL": "US CPI",
    "USACPIALLMINMEI": "US CPI (OECD)",
    "UNRATE": "US Unemployment",
    "DFF": "Fed Funds Effective",
}

payload = {
    "tickerlist": list(indicators.keys()),
    "start": "2020-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "fred",
    "credentials": {"key": "YOUR_FRED_KEY"}
}

response = requests.post(URL, json=payload, headers=HEADERS)
data = response.json()

macro = {}
for ticker, bars in data.items():
    df = pd.DataFrame(bars).T
    df.index = pd.to_datetime(df.index)
    macro[indicators[ticker]] = df.iloc[:, 0].astype(float)

macro_df = pd.DataFrame(macro)
print(macro_df.dropna(how="all").tail())
```

## Economic Indicators from Trading Economics

```python
payload = {
    "tickerlist": ["united states"],
    "start": "2024-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "tradingeconomics",
    "indicator": "gdp",
    "credentials": {"key": "YOUR_TE_KEY"}
}

response = requests.post(URL, json=payload, headers=HEADERS)
gdp_data = response.json()
```

## Earnings Calendar

Track upcoming earnings events without credentials:

```python
payload = {
    "tickerlist": None,
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1d",
    "source": "calendar"
}

response = requests.post(URL, json=payload, headers=HEADERS)
calendar = response.json()
```

## Weather & Commodities

Combine weather data with commodity prices to explore weather-driven trading signals:

```python
# Wheat futures from FirstRate
wheat = requests.post(URL, json={
    "tickerlist": ["ZW"],
    "start": "2024-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "firstrate",
    "assetclass": "futures",
    "continuous_contracts": True,
    "credentials": {"key": "YOUR_FIRSTRATE_KEY"}
}, headers=HEADERS).json()

# Weather in Kansas (major wheat region)
weather = requests.post(URL, json={
    "tickerlist": [{"latitude": 38.5, "longitude": -98.0}],
    "start": "2024-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "openmeteo",
    "variables": ["temperature_2m"]
}, headers=HEADERS).json()

wheat_df = pd.DataFrame(list(wheat.values())[0]).T
wheat_df.index = pd.to_datetime(wheat_df.index)

weather_key = list(weather.keys())[0]
weather_df = pd.DataFrame(weather[weather_key]).T
weather_df.index = pd.to_datetime(weather_df.index)

print("Wheat prices:", wheat_df.shape)
print("Weather data:", weather_df.shape)
```

## Multi-Asset Dashboard

Pull everything together into a single analysis:

```python
from datetime import datetime

START = "2024-06-01"
END = "2025-06-01"

def fetch(tickerlist, source, **kwargs):
    payload = {
        "tickerlist": tickerlist,
        "start": START, "end": END,
        "interval": "1d",
        "source": source,
        **kwargs,
    }
    resp = requests.post(URL, json=payload, headers=HEADERS)
    resp.raise_for_status()
    frames = {}
    for t, bars in resp.json().items():
        df = pd.DataFrame(bars).T
        df.index = pd.to_datetime(df.index)
        df = df.astype(float)
        frames[t] = df
    return frames

# 1. Equities
equities = fetch(["SPY", "QQQ", "IWM"], "yahoo")

# 2. Crypto
crypto = fetch(["BTCUSDT", "ETHUSDT"], "bybit")

# 3. Forex
fx = fetch(["EUR_USD", "GBP_USD"], "oanda",
           credentials={"id": "YOUR_ID", "token": "YOUR_TOKEN",
                        "environment": "practice"})

# 4. Macro
rates = fetch(
    ["BIS,WS_CBPOL,1.0/D.US", "BIS,WS_CBPOL,1.0/D.XM"],
    "sdmx", db="BIS", l1="CBRate"
)

# Combine closes
closes = pd.DataFrame({
    **{t: f["Close"] for t, f in equities.items()},
    **{t: f["Close"] for t, f in crypto.items()},
    **{t: f["Close"] for t, f in fx.items()},
})

# Normalize to 100
normalized = closes / closes.iloc[0] * 100
print(normalized.tail())
```
