# Kraken

Cryptocurrency spot and futures data from Kraken.

## Source ID

```json
"source": "kraken"
```

## Credentials

**Not required.** Kraken public market data is freely accessible.

## Ticker Format

| Example | Instrument |
|---|---|
| `PF_XBTUSD` | Bitcoin perpetual, multi-collateral (futures) |
| `PF_ETHUSD` | Ethereum perpetual, multi-collateral (futures) |
| `PI_XBTUSD` | Bitcoin inverse perpetual (futures) |
| `XXBTZUSD` | Bitcoin / USD (spot) |
| `XETHZUSD` | Ethereum / USD (spot) |

## Supported Intervals

**Futures**: `1M`, `5M`, `15M`, `30M`, `1h`, `4h`, `12h`, `1d`, `1w`. Long ranges are fetched in pages of 5000 bars.

**Spot**: `1M`, `5M`, `15M`, `30M`, `1h`, `4h`, `1d`, `1w`

!!! warning "Spot returns only recent bars"
    Kraken's spot endpoint serves only the most recent 720 bars at each interval, and AuraStream does not filter them by `start` and `end`: a spot request returns the latest 720 bars whatever dates you send. Use futures for longer history.

## Response Fields

| Market | Fields |
|---|---|
| Futures | `open`, `high`, `low`, `close`, `volume` (strings), stamped at the bar open |
| Spot | `Open`, `High`, `Low`, `Close`, `VWAP`, `Volume`, `Trades`, stamped at the bar open |

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `instrument` | `"futures"`, `"spot"` | `"futures"` | Market type |
| `ticktype` | `"trade"`, `"mark"`, `"spot"` | `"trade"` | Futures only: bars from traded prices, the mark price, or the index (spot) price |

## Examples

### Spot data

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["XXBTZUSD"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1h",
    "source": "kraken",
    "instrument": "spot",
})
```

### Futures data

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["PF_XBTUSD", "PF_ETHUSD"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1h",
    "source": "kraken",
    "instrument": "futures",
})
```
