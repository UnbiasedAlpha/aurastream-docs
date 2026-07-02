# POST /timeseries

Fetch historical time series data from any supported source.

## Endpoint

```
POST https://api.aurastream.unbiased-alpha.com/timeseries
```

## Headers

| Header | Required | Description |
|---|---|---|
| `x-api-key` | Yes | Your Aurastream API key |
| `Content-Type` | Yes | `application/json` |

## Request Body

```json
{
    "tickerlist": ["BTCUSDT", "ETHUSDT"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "bybit",
    "credentials": null
}
```

### Core Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `tickerlist` | `list[str]` or `null` | Yes | — | List of ticker symbols to fetch |
| `start` | `str` | Yes | — | Start date in `YYYY-MM-DD` or `YYYY-MM-DD HH:MM` format |
| `end` | `str` | Yes | — | End date in `YYYY-MM-DD` or `YYYY-MM-DD HH:MM` format |
| `interval` | `str` | Yes | — | Bar interval (see [Intervals](#intervals)) |
| `source` | `str` | Yes | — | Data source identifier (see [Data Sources](../data-sources/overview.md)) |
| `credentials` | `dict` or `null` | No | `null` | Source-specific credentials (see [Credentials](../getting-started/credentials.md)) |

### Source-Specific Parameters

| Parameter | Type | Used By | Description |
|---|---|---|---|
| `dtype` | `str` | Alpaca, CoinAPI, Oanda | Data type: `"bars"`, `"trades"`, `"quotes"`, `"orderbook"`, `"positionbook"` |
| `price_type` | `str` | Oanda | `"M"` (mid), `"A"` (ask), `"B"` (bid) |
| `instrument` | `str` | Binance, Deribit, CoinAPI, FirstRate | Binance: `"spot"` / `"futures"`. Deribit: `"derivative"`. CoinAPI: `"PERPETUAL"` / `"spot"`. FirstRate: `"stock"` / `"etf"` / `"futures"` / `"fx"` / `"options"` / `"crypto"` |
| `assetclass` | `str` | Alpaca | Asset class: `"equity"` or `"crypto"` |
| `category` | `str` | Bybit | Category (e.g., `"linear"`) |
| `bartype` | `str` | City Index | Bar type: `"bar"` or `"tick"` |
| `exchange` | `str` | CoinAPI | Exchange identifier for trades/orderbook (e.g., `"KRAKENFTS"`) |
| `indicator` | `str` | Trading Economics | Economic indicator (e.g., `"stock-market"`, `"gdp"`) |
| `metric` | `str` | Nemeton | Metric type (e.g., `"cumulative-depth"`, `"depth-ratio"`) |
| `level` | `int` | Nemeton | Depth level (default: `1000`) |
| `variables` | `list[str]` | Open-Meteo | Weather variables (e.g., `["temperature_2m"]`) |
| `db` | `str` | SDMX | Database identifier (e.g., `"BIS"`) |
| `l1` | `str` | SDMX | Aggregation label (e.g., `"CBRate"`) |
| `filters` | `dict` | Nasdaq | Per-ticker data filters |
| `continuous_contracts` | `bool` | FirstRate | Use continuous contracts for futures (default: `true`) |
| `vs_currency` | `str` | CoinGecko | Quote currency (default: `"USD"`) |

### Intervals

| Interval | Meaning | Examples |
|---|---|---|
| `1s`, `5s`, `30s` | Seconds | Bybit, Binance |
| `1m`, `5m`, `15m`, `30m` | Minutes | Most sources |
| `1h`, `4h` | Hours | Most sources |
| `1d` | Daily | All sources |
| `1w` | Weekly | Most sources |
| `1M` | Monthly | Some sources |

!!! note "Interval support varies by source"
    Not every source supports every interval. See individual [data source pages](../data-sources/overview.md) for supported intervals.

## Response

### Success (200)

```json
{
    "BTCUSDT": {
        "2025-01-01 00:00:00": {
            "Open": 94500.0,
            "High": 95200.0,
            "Low": 93800.0,
            "Close": 94900.0,
            "Volume": 12345.67
        }
    }
}
```

See [Response Format](response-format.md) for full details.

### Error Responses

| Status | Meaning |
|---|---|
| `403 Forbidden` | Invalid or missing API key |
| `400 Bad Request` | Invalid parameters |
| `500 Internal Server Error` | Upstream source error |

## Examples

=== "Python"

    ```python
    import requests

    URL = "https://api.aurastream.unbiased-alpha.com/timeseries"
    HEADERS = {"x-api-key": "YOUR_API_KEY"}

    response = requests.post(URL, headers=HEADERS, json={
        "tickerlist": ["XAU_USD"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "oanda",
        "credentials": {
            "id": "YOUR_ACCOUNT_ID",
            "token": "YOUR_TOKEN",
            "environment": "practice"
        }
    })

    data = response.json()
    ```

=== "curl"

    ```bash
    curl -X POST https://api.aurastream.unbiased-alpha.com/timeseries \
      -H "x-api-key: YOUR_API_KEY" \
      -H "Content-Type: application/json" \
      -d '{
        "tickerlist": ["XAU_USD"],
        "start": "2025-01-01",
        "end": "2025-06-01",
        "interval": "1d",
        "source": "oanda",
        "credentials": {
          "id": "YOUR_ACCOUNT_ID",
          "token": "YOUR_TOKEN",
          "environment": "practice"
        }
      }'
    ```

## Timeout

Requests have a **120-second timeout**. For large date ranges or many tickers, consider splitting into smaller requests.
