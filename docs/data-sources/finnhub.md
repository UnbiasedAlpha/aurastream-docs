# Finnhub

Company financial data and metrics from Finnhub.

## Source ID

```json
"source": "finnhub"
```

## Credentials

```json
"credentials": {
    "key": "your-finnhub-api-key"
}
```

Get your API key at [Finnhub](https://finnhub.io/) (free tier available).

## Ticker Format

Standard US equity ticker symbols:

| Example | Company |
|---|---|
| `AAPL` | Apple Inc. |
| `MSFT` | Microsoft Corp. |
| `TSLA` | Tesla Inc. |

## Data

Finnhub returns the history of company basic financials: every metric Finnhub reports (earnings, margins, ratios and so on), one field per metric, stamped at the end of each reporting period. Metric names are Finnhub's own (for example `eps`, `netMargin`, `currentRatio`).

## Reporting Period

The `interval` selects the period: `"1y"` returns annual figures, any other interval returns quarterly figures.

The whole available history is returned: `start` and `end` do not filter it, so trim the result on your side.

## Examples

### Quarterly financials

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL"],
    "start": "2023-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "finnhub",
    "credentials": {
        "key": "YOUR_FINNHUB_KEY"
    }
})
```

### Annual financials

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL", "MSFT"],
    "start": "2015-01-01",
    "end": "2025-01-01",
    "interval": "1y",
    "source": "finnhub",
    "credentials": {
        "key": "YOUR_FINNHUB_KEY"
    }
})
```
