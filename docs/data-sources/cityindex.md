# City Index

Bars and ticks for City Index CFD markets (indices, FX, commodities, shares) from your City Index account.

## Source ID

```json
"source": "cityindex"
```

## Credentials

```json
"credentials": {
    "username": "your-username",
    "password": "your-password",
    "key": "your-app-key"
}
```

The app key comes from City Index's API access. A wrong login fails with City Index's message.

## Ticker Format

City Index market IDs (numbers, as text):

| Example | Market |
|---|---|
| `"401204130"` | A City Index market ID |

## Supported Intervals

Minutes (`1M`, `5M`, ...), hours (`1h`, ...) and days (`1d`).

## Fields

`Open`, `High`, `Low`, `Close` for bars; `Price` for ticks.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `bartype` | `"bar"`, `"tick"` | `"bar"` | Bars or ticks |
| `dtype` | `"MID"`, `"BID"`, `"ASK"` | `"MID"` | Price type |

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["401204130"],
    "start": "2025-02-03",
    "end": "2025-02-10",
    "interval": "1h",
    "source": "cityindex",
    "credentials": {
        "username": "your-username",
        "password": "your-password",
        "key": "your-app-key",
    },
})
```
