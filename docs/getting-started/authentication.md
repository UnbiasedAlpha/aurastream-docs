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
    Aurastream uses your credentials only to call the upstream data source for your request. They are not saved to your account.

## Which sources need credentials?

| Source | Credentials Required | Notes |
|---|---|---|
| Oanda | Yes | Account ID + API token |
| Alpaca | Yes | API key + secret |
| Polygon | Yes | API key |
| FRED | Yes | API key |
| Finnhub | Yes | API key |
| EIA | Yes | API key (free) |
| US Census | Yes | API key (free) |
| SEC EDGAR | Yes | Name and contact email, no registration |
| US Economic Calendar (`bls`) | Yes | Name and contact email, no registration; other agencies need none |
| Binance | No | Public market data |
| Bybit | No | Public market data |
| Kraken | No | Public market data |
| Yahoo Finance | No | Public data |
| CFTC, US Treasury, DefiLlama, TSA | No | Public data |
| GDELT, World Bank, UK Trade (HMRC), EU Trade (Eurostat), Hong Kong Trade | No | Public data |

See the [Credentials guide](credentials.md) for the exact format each source expects.
