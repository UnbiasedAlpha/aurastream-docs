# SDMX

Statistical data from central banks and international organizations (BIS, ECB, etc.).

**Source:** `sdmx` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["BIS,WS_CBPOL,1.0/D.AR", "BIS,WS_CBPOL,1.0/D.US"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "sdmx",
    "db": "BIS",
    "l1": "CBRate"
}
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `db` | `str` | — | SDMX database identifier (e.g., `"BIS"`, `"ECB"`) |
| `l1` | `str` | `"Value"` | Aggregation level label |

## Ticker Format

SDMX data flow keys: `BIS,WS_CBPOL,1.0/D.US` (BIS central bank policy rate for US)

The format is `provider,dataflow,version/frequency.country_code`.
