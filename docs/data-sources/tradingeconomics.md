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
    "tickerlist": ["Mexico"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "tradingeconomics",
    "indicator": "stock-market",
    "credentials": {"key": "YOUR_KEY"}
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `indicator` | `str` | Yes | Economic indicator: `"gdp"`, `"stock-market"`, `"inflation rate"`, `"interest rate"`, etc. |

## Ticker Format

Country names: `Mexico`, `united states`, `united kingdom`, `germany`, `japan`
