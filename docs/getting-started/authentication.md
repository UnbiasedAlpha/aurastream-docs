# Authentication

Aurastream uses two layers of authentication:

1. **Aurastream API key** — authenticates you against the Aurastream service
2. **Data source credentials** — your own credentials for third-party data providers (when required)

## Aurastream API Key

Every request must include your Aurastream API key in the `x-api-key` header:

```python
headers = {"x-api-key": "YOUR_AURASTREAM_API_KEY"}
```

You can find your API key in the [Aurastream dashboard](https://aurastream.unbiased-alpha.com) under **API Keys**.

!!! warning "Keep your API key secret"
    Never commit your API key to version control or share it publicly. Use environment variables or a secrets manager.

```python
import os

headers = {"x-api-key": os.environ["AURASTREAM_API_KEY"]}
```

## Data Source Credentials

Some data sources require your own API credentials to access their data. These are passed in the `credentials` field of the request body — they are **not stored** on Aurastream servers.

```python
response = requests.post(URL, headers=headers, json={
    "tickerlist": ["EUR_USD"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1h",
    "source": "oanda",
    "credentials": {
        "id": "YOUR_OANDA_ACCOUNT_ID",
        "token": "YOUR_OANDA_TOKEN",
        "environment": "practice"
    }
})
```

!!! info "Credentials are never stored"
    Aurastream forwards your credentials directly to the upstream data source in real time. They are not saved, logged, or cached.

## Which sources need credentials?

| Source | Credentials Required | Notes |
|---|---|---|
| Oanda | Yes | Account ID + API token |
| Alpaca | Yes | API key + secret |
| Polygon | Yes | API key |
| FRED | Yes | API key |
| Finnhub | Yes | API key |
| CoinAPI | Yes | API key |
| FirstRate | Yes | API key |
| Nasdaq Data Link | Yes | API key |
| Trading Economics | Yes | API key |
| TradeStation | Yes | Contact + key + secret + refresh token + account ID |
| City Index | Yes | Username + password + key + account IDs |
| Nemeton | Yes | API key |
| Visual Crossing | Yes | API key |
| Binance | No | Public market data |
| Bybit | No | Public market data |
| Deribit | No | Public market data |
| dYdX | No | Public market data |
| KuCoin | No | Public market data |
| Kraken | No | Public market data |
| CoinGecko | No | Public market data |
| Hyperliquid | No | Public market data |
| Lyra | No | Public market data |
| Yahoo Finance | No | Public data |
| SDMX | No | Public statistical data |
| Open-Meteo | No | Public weather data |
| Calendar | No | Public calendar data |

See the [Credentials guide](credentials.md) for the exact format each source expects.
