# FRED

Economic indicators from the Federal Reserve Economic Data (FRED) API, with every revision (vintage) of each data point.

## Source ID

```json
"source": "fred"
```

## Credentials

```json
"credentials": {
    "key": "your-fred-api-key"
}
```

Get your API key at [FRED](https://fred.stlouisfed.org/docs/api/api_key.html) (free).

## Ticker Format

Tickers are FRED series IDs, for example:

| Example | Series |
|---|---|
| `CPIAUCSL` | US CPI, all items |
| `UNRATE` | US unemployment rate |
| `DFF` | Fed funds effective rate |
| `GDPC1` | US real GDP |

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `as_of` | `"realtime_start"`, `"date"` | `"realtime_start"` | Index rows by publication date or by the period the value describes |

The `interval` parameter is accepted but ignored: each series comes at its native frequency (daily, monthly, quarterly...).

FRED stops updating some series, for example when the original publisher changes or retires them. Check a series' last observation on its FRED page before relying on it.

## Response Format

FRED values get revised. AuraStream returns every published version, so you can see what was known at any point in time. Rows hold a list called `updates` rather than OHLC fields.

With the default `as_of: "realtime_start"`, each row is a publication date and lists the values published that day, with the period they refer to:

```json
{
    "CPIAUCSL": {
        "2025-02-12 00:00:00": {
            "updates": [
                {"reference_date": "2025-01-01 00:00:00", "value": 317.671}
            ]
        }
    }
}
```

This is the version to use in backtests: a row only contains what was public on that date. `start` and `end` filter on the publication date.

With `as_of: "date"`, each row is the period the value describes and lists every version published for it, with `publication_date` in place of `reference_date`.

## Examples

### US series by ID

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["CPIAUCSL", "UNRATE"],
    "start": "2020-01-01",
    "end": "2025-06-01",
    "interval": "1m",
    "source": "fred",
    "credentials": {
        "key": "YOUR_FRED_KEY"
    }
})
```

### Latest value for each period

```python
import pandas as pd

data = response.json()
rows = []
for published, row in data["CPIAUCSL"].items():
    for u in row["updates"]:
        rows.append({"published": published, "period": u["reference_date"], "value": u["value"]})

df = pd.DataFrame(rows).sort_values("published")
latest = df.groupby("period")["value"].last()   # most recent revision of each period
```
