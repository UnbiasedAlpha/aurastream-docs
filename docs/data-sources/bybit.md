# Bybit

Cryptocurrency perpetual futures data from Bybit: mark-price bars.

## Source ID

```json
"source": "bybit"
```

## Credentials

**Not required.** Bybit public market data is freely accessible.

## Ticker Format

Bybit tickers are concatenated pairs:

| Example | Instrument |
|---|---|
| `BTCUSDT` | Bitcoin / Tether perpetual |
| `ETHUSDT` | Ethereum / Tether perpetual |
| `SOLUSDT` | Solana / Tether perpetual |
| `BTCUSD` | Bitcoin / USD inverse perpetual |

## Supported Intervals

`1M`, `3M`, `5M`, `15M`, `30M`, `1h`, `2h`, `4h`, `6h`, `12h`, `1d`, `1w`, `1m`

## Response Fields

Bars are built from the **mark price**, not traded prices, so there is no volume. Each bar is stamped at its open time.

| Field | Description |
|---|---|
| `Open`, `High`, `Low`, `Close` | Mark price (sent as strings) |

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `category` | `"linear"`, `"inverse"` | `"linear"` | Contract type |

## Examples

### Linear perpetuals (daily)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT", "ETHUSDT", "SOLUSDT"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "bybit",
})
```

### Inverse perpetuals (hourly)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSD"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1h",
    "source": "bybit",
    "category": "inverse",
})
```
