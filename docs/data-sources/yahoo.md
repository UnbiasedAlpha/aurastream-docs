# Yahoo Finance

Free market data for equities, ETFs, indices, commodities, and forex.

## Source ID

```json
"source": "yahoo"
```

## Credentials

**Not required.**

## Ticker Format

Yahoo Finance uses standard ticker symbols with special suffixes for some instruments:

| Example | Instrument |
|---|---|
| `AAPL` | Apple Inc. |
| `SPY` | S&P 500 ETF |
| `^GSPC` | S&P 500 Index |
| `^DJI` | Dow Jones Industrial Average |
| `BZ=F` | Brent Crude Oil Futures |
| `GC=F` | Gold Futures |
| `CL=F` | WTI Crude Oil Futures |
| `SI=F` | Silver Futures |
| `EURUSD=X` | EUR/USD |
| `BRL=X` | USD/BRL |
| `DX-Y.NYB` | US Dollar Index (DXY) |
| `BTC-USD` | Bitcoin |

## Supported Intervals

`1M`, `2M`, `5M`, `15M`, `30M`, `60M`, `90M`, `1h`, `1d`, `5d`, `1w`, `1m`, `3m`

!!! warning "Intraday data limitations"
    Yahoo Finance serves 1-minute bars for the last 7 days only, and other intraday intervals (below `1d`) for the last 60 days. For longer historical ranges, use daily or weekly intervals.

## Response Fields

Bars have `Open`, `High`, `Low`, `Close`, `Volume`, plus the corporate action columns yfinance returns (`Dividends`, `Stock Splits`). Prices are adjusted for splits and dividends (the yfinance default). Timestamps are converted to UTC, so a daily US stock bar is stamped at 04:00 or 05:00 UTC (midnight New York); normalize to the date before joining with other sources.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `dtype` | `"candles"`, `"earnings"` | `"candles"` | Price bars, or earnings dates with `EPS Estimate`, `Reported EPS` and `Surprise(%)` (about the last 100 reports, plus the next scheduled one) |
| `corporate_actions` | `true`, `false` | `false` | Return the full history of `Dividends` and `Stock Splits` instead of bars (not filtered by `start` and `end`) |

## Examples

### Equities (daily)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL", "MSFT", "GOOGL"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "yahoo",
})
```

### Commodities and indices

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BZ=F", "GC=F", "DX-Y.NYB"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "yahoo",
})
```

### Forex

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["EURUSD=X", "GBPUSD=X", "BRL=X"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "yahoo",
})
```

### Earnings dates

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL", "MSFT"],
    "start": "2023-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "yahoo",
    "dtype": "earnings",
})
```
