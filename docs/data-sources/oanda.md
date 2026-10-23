# Oanda

Forex, metals and CFD bars, plus order book and position book snapshots, from Oanda's v20 API.

## Source ID

```json
"source": "oanda"
```

## Credentials

```json
"credentials": {
    "id": "101-004-XXXXXXXX-XXX",
    "token": "your-api-token",
    "environment": "practice"
}
```

| Field | Description |
|---|---|
| `id` | Oanda account ID |
| `token` | API token from [Oanda portal](https://developer.oanda.com/) |
| `environment` | `"practice"` (demo) or `"live"` |

## Ticker Format

Oanda uses underscore-separated pairs: `BASE_QUOTE`

| Example | Instrument |
|---|---|
| `EUR_USD` | Euro / US Dollar |
| `GBP_JPY` | British Pound / Japanese Yen |
| `XAU_USD` | Gold / US Dollar |
| `XAG_USD` | Silver / US Dollar |
| `XCU_USD` | Copper / US Dollar |
| `USD_NOK` | US Dollar / Norwegian Krone |

## Supported Intervals

`1M`, `2M`, `4M`, `5M`, `10M`, `15M`, `30M`, `1h`, `2h`, `3h`, `4h`, `6h`, `8h`, `12h`, `1d`, `1w`, `1m`

Each bar is stamped at its **close** (open time plus the interval), and only completed bars are returned. Fields: `Open`, `High`, `Low`, `Close` (strings) and `Volume` (tick count).

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `price_type` | `"M"` (mid), `"A"` (ask), `"B"` (bid) | `"M"` | Which price the bars are built from |
| `dtype` | `"bars"`, `"orderbook"`, `"positionbook"` | `"bars"` | Data type |

## Order Book and Position Book

`"dtype": "orderbook"` and `"dtype": "positionbook"` return Oanda's snapshots of client orders and open positions, one every 20 minutes between `start` and `end` (the `interval` is ignored). Oanda publishes books for a subset of its instruments.

Each snapshot has the current `price` and, for each price bucket `n`:

| Data type | Fields |
|---|---|
| `orderbook` | `Bid{n}:price`, `Ask{n}:price`, `Bid{n}:perc` (Oanda's `longCountPercent`), `Ask{n}:perc` (`shortCountPercent`) |
| `positionbook` | `Long{n}:price` and `Short{n}:price` (the bucket price), `Long{n}:perc` (`longCountPercent`), `Short{n}:perc` (`shortCountPercent`) |

## Examples

### Forex pair (hourly)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["EUR_USD", "GBP_USD"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1h",
    "source": "oanda",
    "price_type": "M",
    "credentials": {
        "id": "YOUR_ACCOUNT_ID",
        "token": "YOUR_TOKEN",
        "environment": "practice"
    }
})
```

### Precious metals (daily)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["XAU_USD", "XAG_USD"],
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
```

### Order book snapshots

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["EUR_USD"],
    "start": "2025-01-06",
    "end": "2025-01-07",
    "interval": "1d",
    "source": "oanda",
    "dtype": "orderbook",
    "credentials": {
        "id": "YOUR_ACCOUNT_ID",
        "token": "YOUR_TOKEN",
        "environment": "practice"
    }
})
```
