# Binance

Cryptocurrency spot and futures market data from Binance.

## Source ID

```json
"source": "binance"
```

## Credentials

**Not required.** Binance public market data is freely accessible.

## Ticker Format

Binance tickers are concatenated pairs without separators. The same symbols are used for spot and USD-M futures:

| Example | Instrument |
|---|---|
| `BTCUSDT` | Bitcoin / Tether (spot) |
| `ETHUSDT` | Ethereum / Tether (spot) |
| `SOLUSDT` | Solana / Tether (spot) |
| `ETHBTC` | Ethereum / Bitcoin (spot) |

## Supported Intervals

`1M`, `3M`, `5M`, `15M`, `30M`, `1h`, `2h`, `4h`, `6h`, `8h`, `12h`, `1d`, `3d`, `1w`

Spot also accepts `1s`. Monthly bars (`1m`) are not available from this source.

Each bar is stamped at its close time. Fields: `Open`, `High`, `Low`, `Close`, `Volume`, `QuoteAssetVolume`, `NumberTrades`, `TakerBuyBaseVolume`, `TakerBuyQuoteVolume` (prices and volumes as strings).

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `instrument` | `"spot"`, `"futures"` | `"spot"` | Market type. `"futures"` is USD-M perpetual futures |
| `bars` | `"candles"`, `"funding"` | `"candles"` | Futures only: `"funding"` returns the funding rate (`fundingRate`) at each funding time |

## Examples

### Spot data (daily)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT", "ETHUSDT"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "binance",
})
```

### Perpetual futures (hourly)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1h",
    "source": "binance",
    "instrument": "futures",
})
```

### Funding rates

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTCUSDT"],
    "start": "2025-06-01",
    "end": "2025-06-30",
    "interval": "1h",
    "source": "binance",
    "bars": "funding",
    "instrument": "futures",
})
```
