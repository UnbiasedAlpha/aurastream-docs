# dYdX

OHLCV bars for dYdX v4 perpetual markets, from the public dYdX indexer.

## Source ID

```json
"source": "dydx"
```

## Credentials

**Not required.**

## Ticker Format

dYdX market names, `BASE-USD`:

| Example | Market |
|---|---|
| `BTC-USD` | Bitcoin perpetual |
| `ETH-USD` | Ether perpetual |
| `SOL-USD` | Solana perpetual |

## Supported Intervals

`1M`, `5M`, `15M`, `30M`, `1h`, `4h`, `1d`. Bars are stamped at their open time, in UTC.

## Fields

`open`, `high`, `low`, `close`, `baseTokenVolume`, `usdVolume`, `trades`, `startingOpenInterest`, `orderbookMidPriceOpen`, `orderbookMidPriceClose`

## Parameters

No source-specific parameters.

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTC-USD", "ETH-USD"],
    "start": "2025-01-01",
    "end": "2025-03-01",
    "interval": "1d",
    "source": "dydx",
})
```
