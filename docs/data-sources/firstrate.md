# FirstRate Data

Historical bars for US stocks, ETFs, futures, FX, indices and crypto, plus end-of-day option chains, downloaded from FirstRate Data's file API. You need your own FirstRate Data subscription.

## Source ID

```json
"source": "firstrate"
```

## Credentials

```json
"credentials": {
    "key": "your-firstrate-user-id"
}
```

`key` is the user ID FirstRate Data gives you for API downloads. It is required: requests without it fail. Your FirstRate subscription decides which datasets you can download.

## Ticker Format

| Instrument | Format | Example |
|---|---|---|
| `stock`, `etf` | Ticker symbol | `AAPL`, `SPY` |
| `futures` | Root symbol | `CL`, `GC`, `ZW` |
| `fx` | Concatenated pair | `EURUSD`, `GBPUSD` |
| `index` | FirstRate index symbol | `SPX` |
| `crypto` | Base symbol | `BTC`, `ETH` |
| `options` | Underlying symbol | `SPY` |

Tickers must match the file names FirstRate uses (the part before the first `_`).

Option results are keyed per contract as `UNDERLYING-YYYYMMDD-STRIKE-C` or `-P`. Individual futures contracts (`continuous_contracts: false`) are keyed by root plus contract code.

## Supported Intervals

`1M`, `5M`, `30M`, `1h`, `1d`, as offered by FirstRate Data. Weeks and months are not supported.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `instrument` | `"stock"`, `"etf"`, `"futures"`, `"fx"`, `"index"`, `"crypto"`, `"options"` | `"stock"` | Dataset to download |
| `adjustment` | `"auto"`, or a FirstRate adjustment code (see below) | `"auto"` | Price adjustment. `auto` means `adj_splitdiv` for stocks and ETFs, `contin_adj_absolute` for futures |
| `continuous_contracts` | `true`, `false` | `true` | Futures only: continuous series, or one series per individual contract |
| `period` | `"full"`, `"month"`, `"week"`, `"day"` | `"full"` | Which FirstRate file to download: full history, or only the latest month, week or day |

Adjustment codes: for stocks and ETFs `adj_splitdiv`, `adj_split` or `UNadjusted`; for continuous futures `contin_adj_absolute`, `contin_adj_ratio` or `contin_UNadj`.

### Fields

- Bars: `Open`, `High`, `Low`, `Close`, plus `Volume` and `OpenInterest` where FirstRate provides them.
- Options: `Close`, `Bid`, `Ask`, `BidIV`, `AskIV`, `OpenInterest`, `Volume`, `Delta`, `Gamma`, `Vega`, `Theta`, `Rho`.

FirstRate delivers whole files (full history by default), which Aurastream trims to `start` and `end`. Large intraday requests take a while.

## Examples

### Stock (daily, split and dividend adjusted)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL", "MSFT"],
    "start": "2024-01-01",
    "end": "2025-01-01",
    "interval": "1d",
    "source": "firstrate",
    "instrument": "stock",
    "credentials": {
        "key": "YOUR_FIRSTRATE_USER_ID"
    }
})
```

### Continuous futures (hourly)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["CL"],
    "start": "2025-01-01",
    "end": "2025-02-01",
    "interval": "1h",
    "source": "firstrate",
    "instrument": "futures",
    "credentials": {
        "key": "YOUR_FIRSTRATE_USER_ID"
    }
})
```

### Individual futures contracts

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["CL"],
    "start": "2025-01-01",
    "end": "2025-02-01",
    "interval": "1d",
    "source": "firstrate",
    "instrument": "futures",
    "continuous_contracts": False,
    "credentials": {
        "key": "YOUR_FIRSTRATE_USER_ID"
    }
})
```

### FX

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["EURUSD"],
    "start": "2025-01-01",
    "end": "2025-02-01",
    "interval": "1d",
    "source": "firstrate",
    "instrument": "fx",
    "credentials": {
        "key": "YOUR_FIRSTRATE_USER_ID"
    }
})
```

### Option chains

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["SPY"],
    "start": "2025-01-02",
    "end": "2025-01-03",
    "interval": "1d",
    "source": "firstrate",
    "instrument": "options",
    "credentials": {
        "key": "YOUR_FIRSTRATE_USER_ID"
    }
})
```
