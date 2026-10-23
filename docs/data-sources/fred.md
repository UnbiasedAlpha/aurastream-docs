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

By default tickers are FRED series IDs:

| Example | Series |
|---|---|
| `CPIAUCSL` | US CPI, all items |
| `UNRATE` | US unemployment rate |
| `DFF` | Fed funds effective rate |
| `GDPC1` | US real GDP |
| `USACPIALLMINMEI` | US CPI (OECD) |

### Country shortcuts

Set the `bars` parameter to use 2-letter country codes instead of series IDs. Note that the United Kingdom is `UK`, not `GB`.

| `bars` | Indicator | Country codes |
|---|---|---|
| `"cpi"` | Consumer price index | `AR`, `AT`, `AU`, `BE`, `BR`, `CA`, `CH`, `CL`, `CN`, `DE`, `DK`, `EU`, `FR`, `IN`, `IT`, `JP`, `KR`, `MX`, `NO`, `RU`, `TU` (Turkey), `UK`, `US`, `ZA` |
| `"gdp"` | Real GDP | `AU`, `BR`, `CA`, `CH`, `CN`, `DE`, `ES`, `EU`, `FR`, `ID`, `IN`, `JP`, `KR`, `MX`, `NO`, `PL`, `RU`, `SA`, `SE`, `TR`, `UK`, `US`, `ZA` |
| `"3mrate"` | 3-month interbank rate | `AU`, `CA`, `CN`, `DK`, `EU`, `IN`, `JP`, `KR`, `MX`, `NO`, `NZ`, `PL`, `SE`, `UK`, `US`, `ZA` |
| `"10yrate"` | 10-year government bond yield | `AU`, `CA`, `CH`, `DE`, `DK`, `ES`, `EU`, `FR`, `GR`, `IN`, `IT`, `JP`, `KR`, `MX`, `NL`, `NO`, `NZ`, `PL`, `PT`, `SE`, `UK`, `US`, `ZA` |

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `bars` | `"cpi"`, `"gdp"`, `"3mrate"`, `"10yrate"` | not set | Country shortcut (see above). Leave it out to use FRED series IDs |
| `as_of` | `"realtime_start"`, `"date"` | `"realtime_start"` | Index rows by publication date or by the period the value describes |

The `interval` parameter is accepted but ignored: each series comes at its native frequency (daily, monthly, quarterly...).

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

### CPI for several countries

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["US", "UK", "DE", "JP"],
    "start": "2020-01-01",
    "end": "2025-06-01",
    "interval": "1m",
    "source": "fred",
    "bars": "cpi",
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
for published, row in data["US"].items():
    for u in row["updates"]:
        rows.append({"published": published, "period": u["reference_date"], "value": u["value"]})

df = pd.DataFrame(rows).sort_values("published")
latest = df.groupby("period")["value"].last()   # most recent revision of each period
```
