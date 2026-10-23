# Trading Economics

Historical economic indicators by country from Trading Economics. You need your own Trading Economics API key.

## Source ID

```json
"source": "tradingeconomics"
```

## Credentials

```json
"credentials": {
    "key": "your-tradingeconomics-key"
}
```

Get a key at [Trading Economics](https://tradingeconomics.com/api/).

## Ticker Format

Country names, as Trading Economics writes them:

| Example | Country |
|---|---|
| `united states` | United States |
| `germany` | Germany |
| `mexico` | Mexico |

## Supported Intervals

`interval` is ignored: values come at the indicator's own frequency, stamped at the indicator's date.

## Fields

`Value`, `Category`, `Frequency`, `HistoricalDataSymbol`, `LastUpdate`, `DateTime`

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `indicator` | `"gdp"`, `"inflation rate"`, `"interest rate"`, `"stock market"`, ... | required | Indicator name, as on Trading Economics |

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["united states", "germany"],
    "start": "2015-01-01",
    "end": "2025-01-01",
    "interval": "1d",
    "source": "tradingeconomics",
    "indicator": "inflation rate",
    "credentials": {"key": "your-tradingeconomics-key"},
})
```
