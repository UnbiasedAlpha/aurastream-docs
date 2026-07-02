# Nasdaq Data Link

Economic and financial datasets.

**Source:** `nasdaq` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_NASDAQ_KEY"}}
```

## Example

```json
{
    "tickerlist": ["QDL/FON"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "nasdaq",
    "credentials": {"key": "YOUR_KEY"},
    "filters": {"QDL/FON": {"contract_code": 967654}}
}
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `filters` | `dict` | `{}` | Per-ticker data filters (e.g., `{"QDL/FON": {"contract_code": 967654}}`) |

## Ticker Format

Nasdaq Data Link dataset codes: `QDL/FON`, `LBMA/GOLD`, `FRED/GDP`
