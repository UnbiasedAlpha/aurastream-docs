# Response Format

All successful responses from the `/timeseries` endpoint return data in the same nested dictionary format, regardless of the data source.

## Structure

```
{ticker: {timestamp: {field: value}}}
```

### Example

```json
{
    "EUR_USD": {
        "2025-01-02 00:00:00": {
            "Open": 1.03515,
            "High": 1.03680,
            "Low": 1.02980,
            "Close": 1.03120,
            "Volume": 58234
        },
        "2025-01-03 00:00:00": {
            "Open": 1.03120,
            "High": 1.03450,
            "Low": 1.02860,
            "Close": 1.03010,
            "Volume": 62100
        }
    },
    "GBP_USD": {
        "2025-01-02 00:00:00": {
            "Open": 1.25230,
            "High": 1.25510,
            "Low": 1.24890,
            "Close": 1.25100,
            "Volume": 41200
        }
    }
}
```

## Converting to a Pandas DataFrame

### Single ticker

```python
import pandas as pd

ticker = "EUR_USD"
df = pd.DataFrame.from_dict(data[ticker], orient="index")
df.index = pd.to_datetime(df.index)
df = df.astype(float).sort_index()
```

### Multiple tickers

```python
frames = {}
for ticker, values in data.items():
    df = pd.DataFrame.from_dict(values, orient="index")
    df.index = pd.to_datetime(df.index)
    frames[ticker] = df.astype(float).sort_index()

# Access individual DataFrames
eur = frames["EUR_USD"]
gbp = frames["GBP_USD"]
```

### Multi-level DataFrame

```python
panels = []
for ticker, values in data.items():
    df = pd.DataFrame.from_dict(values, orient="index")
    df.index = pd.to_datetime(df.index)
    df = df.astype(float).sort_index()
    df.columns = pd.MultiIndex.from_product([[ticker], df.columns])
    panels.append(df)

combined = pd.concat(panels, axis=1)
# Access: combined["EUR_USD"]["Close"]
```

## Extracting a Single Field

To pull just the closing prices for plotting or analysis:

```python
def to_series(data, ticker, field="Close"):
    ts = data[ticker]
    s = pd.Series({k: float(v[field]) for k, v in ts.items()})
    s.index = pd.to_datetime(s.index)
    return s.sort_index()

close = to_series(data, "EUR_USD", "Close")
```

## Empty Responses

If a ticker has no data for the requested range, its entry will be an empty dictionary:

```json
{
    "INVALID_TICKER": {}
}
```

Check for this before processing:

```python
if not data.get("INVALID_TICKER"):
    print("No data returned for this ticker")
```
