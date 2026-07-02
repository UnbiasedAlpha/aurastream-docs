# Deribit

Crypto derivatives (futures and options) data.

**Source:** `deribit` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["BTC-PERPETUAL"],
    "start": "2025-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "deribit",
    "instrument": "derivative"
}
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `instrument` | `str` | `"derivative"` | Instrument type |

## Ticker Format

Deribit contract names: `BTC-PERPETUAL`, `ETH-PERPETUAL`
