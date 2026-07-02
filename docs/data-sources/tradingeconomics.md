# Trading Economics

Global economic indicators by country.

**Source:** `tradingeconomics` | **Credentials:** Required

## Credentials

```json
{"credentials": {"key": "YOUR_TE_KEY"}}
```

## Example

```json
{
    "tickerlist": ["united states"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "tradingeconomics",
    "indicator": "gdp",
    "credentials": {"key": "YOUR_KEY"}
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `indicator` | `str` | Yes | Economic indicator: `"gdp"`, `"inflation rate"`, `"interest rate"`, etc. |

## Ticker Format

Country names in lowercase: `united states`, `united kingdom`, `germany`, `japan`
