# Deribit

OHLCV bars for Deribit crypto futures, perpetuals and options.

## Source ID

```json
"source": "deribit"
```

## Credentials

**Not required.** Bars come from Deribit's public market data API.

## Ticker Format

Deribit instrument names:

| Example | Instrument |
|---|---|
| `BTC-PERPETUAL` | Bitcoin perpetual |
| `ETH-PERPETUAL` | Ether perpetual |
| `BTC-27MAR26` | Bitcoin future expiring 27 March 2026 |
| `BTC-27MAR26-100000-C` | Bitcoin call, strike 100,000, expiring 27 March 2026 |

## Supported Intervals

`1M`, `3M`, `5M`, `10M`, `15M`, `30M`, `1h`, `2h`, `3h`, `6h`, `12h`, `1d`

## Fields

`Open`, `High`, `Low`, `Close`, `Volume`, `QuoteVolume`

## Parameters

No source-specific parameters.

## Examples

### Perpetuals (daily)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTC-PERPETUAL", "ETH-PERPETUAL"],
    "start": "2025-01-01",
    "end": "2025-03-01",
    "interval": "1d",
    "source": "deribit",
})
```

### Hourly bars

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTC-PERPETUAL"],
    "start": "2025-06-01",
    "end": "2025-06-07",
    "interval": "1h",
    "source": "deribit",
})
```
