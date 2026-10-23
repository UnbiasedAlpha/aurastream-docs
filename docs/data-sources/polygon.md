# Polygon

US equities, options, forex, and crypto data from Polygon.io.

## Source ID

```json
"source": "polygon"
```

## Credentials

```json
"credentials": {
    "key": "your-polygon-api-key"
}
```

Get your API key at [Polygon.io](https://polygon.io/).

## Ticker Format

Standard US equity tickers. Other asset classes use Polygon's prefixes (`X:` crypto, `C:` forex, `O:` options):

| Example | Instrument |
|---|---|
| `AAPL` | Apple Inc. |
| `SPY` | S&P 500 ETF |
| `X:BTCUSD` | Bitcoin |
| `C:EURUSD` | Euro / US Dollar |

## Supported Intervals

`1s`, `1M`, `5M`, `15M`, `30M`, `1h`, `1d`, `1w`, `1m`, `1q`, `1y`

Any multiple works (for example `10s`, `2h`): it is sent to Polygon as the bar size. Bars are stamped at their open time, in UTC.

## Response Fields

Polygon returns additional fields beyond standard OHLCV:

| Field | Description |
|---|---|
| `Open`, `High`, `Low`, `Close` | Standard price fields |
| `Volume` | Trading volume |
| `VWAP` | Volume-weighted average price |
| `NTrades` | Number of trades in the bar |
| `OTC` | Present only for OTC tickers |

## Examples

### US equities (daily)

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL", "TSLA"],
    "start": "2025-01-01",
    "end": "2025-06-01",
    "interval": "1d",
    "source": "polygon",
    "credentials": {
        "key": "YOUR_POLYGON_KEY"
    }
})
```
