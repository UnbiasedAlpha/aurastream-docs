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

### Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---|---|---|
| `tickerlist` | `list[str]` | Yes | — | List of ticker symbols to fetch |
| `start` | `str` | Yes | — | Start date in `YYYY-MM-DD` or `YYYY-MM-DD HH:MM` format, UTC |
| `end` | `str` | Yes | — | End date in `YYYY-MM-DD` or `YYYY-MM-DD HH:MM` format, UTC |
| `interval` | `str` | Yes | — | Bar interval (see [Intervals](#intervals)) |
| `source` | `str` | Yes | — | Data source identifier (see [Data Sources](../data-sources/overview.md)) |
| `credentials` | `dict` or `null` | No | `null` | Source-specific credentials (see [Credentials](../getting-started/credentials.md)) |

### Source-Specific Parameters

Any other field in the body is passed to the source. Sources ignore fields they don't use.

| Parameter | Used by | Values (default first) |
|---|---|---|
| `dtype` | [Alpaca](../data-sources/alpaca.md) | `"bars"`, `"trades"`, `"quotes"` |
| `dtype` | [Oanda](../data-sources/oanda.md) | `"bars"`, `"orderbook"`, `"positionbook"` |
| `dtype` | [Yahoo Finance](../data-sources/yahoo.md) | `"candles"`, `"earnings"` |
| `price_type` | Oanda | `"M"` (mid), `"A"` (ask), `"B"` (bid) |
| `assetclass` | Alpaca | `"equity"`, `"crypto"` |
| `feed` | Alpaca | `"iex"`, `"sip"` |
| `adjustment` | Alpaca | `"raw"`, `"split"`, `"dividend"`, `"all"` |
| `instrument` | [Binance](../data-sources/binance.md) | `"spot"`, `"futures"` |
| `instrument` | [Kraken](../data-sources/kraken.md) | `"futures"`, `"spot"` |
| `bars` | Binance | `"candles"`, `"funding"` (futures only) |
| `bars` | [FRED](../data-sources/fred.md) | not set (series IDs), `"cpi"`, `"gdp"`, `"3mrate"`, `"10yrate"` |
| `category` | [Bybit](../data-sources/bybit.md) | `"linear"`, `"inverse"` |
| `as_of` | FRED | `"realtime_start"`, `"date"` |
| `as_of` | [CFTC](../data-sources/cftc.md) | `"release"`, `"report"` |
| `dataset` | [FINRA](../data-sources/finra.md) | `"short_interest"`, `"short_volume"` |

### Intervals

The `interval` parameter accepts these formats:

| Interval | Meaning | Examples |
|---|---|---|
| `1s`, `5s`, `30s` | Seconds | Polygon, Binance spot (`1s` only) |
| `1M`, `5M`, `15M`, `30M` | Minutes | Most sources |
| `1h`, `4h` | Hours | Most sources |
| `1d` | Daily | All sources |
| `1w` | Weekly | Most sources |
| `1m` | Monthly | Some sources |

!!! warning "Case matters"
    Upper-case `M` is minutes, lower-case `m` is months: `15M` is 15 minutes, `1m` is one month.

!!! note "Interval support varies by source"
    Not every source supports every interval. See individual [data source pages](../data-sources/overview.md) for supported intervals.

### Data Types

Most price sources return bars by default. Other data types:

| Data | How to request it | Source |
|---|---|---|
| Individual trades | `"dtype": "trades"` | Alpaca |
| Bid/ask quotes | `"dtype": "quotes"` | Alpaca |
| Funding rates | `"bars": "funding"`, `"instrument": "futures"` | Binance |
| Order book snapshots (every 20 minutes) | `"dtype": "orderbook"` | Oanda |
| Position book snapshots (every 20 minutes) | `"dtype": "positionbook"` | Oanda |
| Earnings dates and EPS | `"dtype": "earnings"` | Yahoo Finance |

## Response

### Success (200)

```json
{
    "BTCUSDT": {
        "2025-01-01 00:00:00": {
            "Open": 94500.0,
            "High": 95200.0,
            "Low": 93800.0,
            "Close": 94900.0
        },
        "2025-01-02 00:00:00": {
            "Open": 94900.0,
            "High": 96100.0,
            "Low": 94200.0,
            "Close": 95800.0
        }
    }
}
```

The response is a nested dictionary:

```
{ticker: {timestamp: {field: value}}}
```

### Fields

| Field | Type | Description |
|---|---|---|
| `Open` | `float` | Opening price |
| `High` | `float` | Highest price |
| `Low` | `float` | Lowest price |
| `Close` | `float` | Closing price |
| `Volume` | `float` | Trading volume |

!!! info "Fields vary by source"
    Field names come from the source. Bybit returns mark-price `Open`, `High`, `Low`, `Close` without volume; Polygon adds `VWAP` and `NTrades`; Alpaca and Kraken futures use lower-case names (`open`, `close`, ...). Some sources (Binance, Bybit, Kraken, Oanda) send prices as strings, so convert with `astype(float)`. Each [data source page](../data-sources/overview.md) lists its fields.

### Error Responses

| Status | Meaning |
|---|---|
| `400 Bad Request` | Invalid parameters |
| `403 Forbidden` | Invalid or missing API key |
| `429 Too Many Requests` | Monthly allowance used up (`Limit Exceeded`) or requests too fast (`Too Many Requests`) |
| `500 Internal Server Error` | Upstream source error |

See [Errors](errors.md) for messages and when to retry.

## Examples

=== "Python"

    ```python
    import requests

    URL = "https://api.aurastream.unbiased-alpha.com/timeseries"
    HEADERS = {"x-api-key": "YOUR_API_KEY"}

    # Fetch daily gold data from Oanda
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
