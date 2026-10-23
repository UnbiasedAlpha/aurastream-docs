# TradeStation

Bars for US stocks, ETFs, futures and indices from your TradeStation account.

## Source ID

```json
"source": "tradestation"
```

## Credentials

```json
"credentials": {
    "key": "your-client-id",
    "secret": "your-client-secret",
    "refresh_token": "your-refresh-token"
}
```

`key` and `secret` are your TradeStation API client ID and secret; the refresh token comes from TradeStation's OAuth sign-in. See [TradeStation's API docs](https://api.tradestation.com/docs/).

## Ticker Format

TradeStation symbols:

| Example | Instrument |
|---|---|
| `AAPL` | Apple Inc. |
| `SPY` | S&P 500 ETF |
| `@ES` | E-mini S&P 500, continuous |

## Supported Intervals

Minutes (`1M`, `5M`, ...), hours (`1h`, ...) and days (`1d`). Only closed bars are returned. Bars are stamped at their close, in UTC.

## Fields

`Open`, `High`, `Low`, `Close`, `TotalVolume`, `UpVolume`, `DownVolume`, `UnchangedVolume`, `TotalTicks`, `UpTicks`, `DownTicks`, `UnchangedTicks`, `OpenInterest`, `Epoch`, `BarStatus`, `IsRealtime`, `IsEndOfHistory`

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `sessiontemplate` | `"Default"`, `"USEQPreAndPost"`, ... | `"Default"` | Trading session the bars cover |
| `closed_only` | `true`, `false` | `true` | Leave out the bar still forming |

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["AAPL", "SPY"],
    "start": "2025-01-01",
    "end": "2025-03-01",
    "interval": "1d",
    "source": "tradestation",
    "credentials": {
        "key": "your-client-id",
        "secret": "your-client-secret",
        "refresh_token": "your-refresh-token",
    },
})
```
