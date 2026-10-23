# Nasdaq Data Link

Tables from Nasdaq Data Link (formerly Quandl). You need your own Nasdaq Data Link API key, plus access to the tables you request.

## Source ID

```json
"source": "nasdaq"
```

## Credentials

```json
"credentials": {
    "key": "your-nasdaq-data-link-key"
}
```

Get a key at [Nasdaq Data Link](https://data.nasdaq.com/).

## Ticker Format

Table codes, `PUBLISHER/TABLE`:

| Example | Table |
|---|---|
| `QDL/FON` | Commitments of Traders, futures and options combined |

Only the Tables API is supported: older time-series dataset codes (such as `LBMA/GOLD`) do not work. The table must have a `date` column.

## Supported Intervals

`interval` is ignored: rows come at the table's own frequency.

`start` and `end` are not applied either. Aurastream downloads the whole table (after `filters`), so use `filters` to keep requests small.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `filters` | `{"<table code>": {"<column>": value}}` | `{}` | Column filters for each table, passed to Nasdaq Data Link (e.g. `{"QDL/FON": {"contract_code": "967654"}}`) |
| `assetcol` | column name | none | Splits the table into one series per value of this column (e.g. `"contract_code"`), instead of one series per table code |
| `index` | column name | `"date"` | Date column used as the time index |

Without `assetcol`, if several rows share a date only the first one is kept.

## Examples

### One contract from a table

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["QDL/FON"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "nasdaq",
    "filters": {"QDL/FON": {"contract_code": "967654"}},
    "credentials": {
        "key": "YOUR_NASDAQ_KEY"
    }
})
```

### Several contracts, one series each

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["QDL/FON"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "nasdaq",
    "filters": {"QDL/FON": {"contract_code": ["967654", "088691"]}},
    "assetcol": "contract_code",
    "credentials": {
        "key": "YOUR_NASDAQ_KEY"
    }
})
```
