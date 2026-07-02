# Bybit

Crypto perpetual contract data.

**Source:** `bybit` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["BTCUSDT", "ETHUSDT"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "bybit",
    "category": "linear"
}
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `category` | `str` | `"linear"` | Contract category (e.g., `"linear"`, `"inverse"`) |

## Ticker Format

Standard Bybit symbols: `BTCUSDT`, `ETHUSDT`, `SOLUSDT`
