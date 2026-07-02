# Nemeton

Order book depth and imbalance metrics for crypto markets.

**Source:** `nemeton` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_NEMETON_KEY"}}
```

## Example

```json
{
    "tickerlist": ["BTCUSDT_BINANCE_PERPETUAL"],
    "start": "2026-01-01",
    "end": "2026-06-15 23:59",
    "interval": "1d",
    "source": "nemeton",
    "credentials": {"key": "YOUR_NEMETON_KEY"},
    "metric": "cumulative-depth",
    "level": 1
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `metric` | `str` | Yes | Metric type (e.g., `"cumulative-depth"`, `"depth-ratio"`, `"dollar-value-imbalance"`) |
| `level` | `int` | No | Depth level (default: `1000`) |

## Ticker Format

Format: `{TICKER}_{EXCHANGE}_{INSTRUMENT}`

Example: `BTCUSDT_BINANCE_PERPETUAL`

!!! note
    Data available from 2023-12-27 onwards. Maximum 10,000 bars per request.
