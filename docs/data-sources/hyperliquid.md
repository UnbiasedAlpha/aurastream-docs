# Hyperliquid

OHLCV bars for Hyperliquid perpetuals, from Hyperliquid's public API.

## Source ID

```json
"source": "hyperliquid"
```

## Credentials

**Not required.**

## Ticker Format

Hyperliquid coin names:

| Example | Market |
|---|---|
| `BTC` | Bitcoin perpetual |
| `ETH` | Ether perpetual |
| `HYPE` | HYPE perpetual |

## Supported Intervals

`1M`, `3M`, `5M`, `15M`, `30M`, `1h`, `2h`, `4h`, `8h`, `12h`, `1d`, `3d`, `1w`, `1m`. Bars are stamped at their close, in UTC.

Hyperliquid only serves the most recent 5,000 bars of each interval, so long ranges need a coarser interval.

## Fields

`Open`, `High`, `Low`, `Close`, `Volume`, `Count` (number of trades)

## Parameters

No source-specific parameters.

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BTC", "ETH"],
    "start": "2025-01-01",
    "end": "2025-03-01",
    "interval": "1d",
    "source": "hyperliquid",
})
```
