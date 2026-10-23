# Derive (Lyra)

Options and perpetuals trades on Derive (formerly Lyra), aggregated into bars per instrument, from Derive's public API.

## Source ID

```json
"source": "lyra"
```

## Credentials

**Not required.**

## Ticker Format

The underlying currency; the response has one series per traded instrument (e.g. `BTC-20250131-100000-C`):

| Example | Underlying |
|---|---|
| `BTC` | Bitcoin |
| `ETH` | Ether |

## Supported Intervals

Any interval (`1h`, `1d`, ...). Trades are grouped into bars by trade time, in UTC.

## Fields

`Open`, `High`, `Low`, `Close` (volume-weighted trade prices), `Mark_Price_open` ... `Mark_Price_close`, `Index_Price_open` ... `Index_Price_close`, `Volume`

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `instrument` | `"option"`, `"perp"` | `"option"` | Instrument type |

!!! note
    Derive's API was unavailable (HTTP 503) when this page was written. Requests then fail with "Lyra (Derive) API unavailable".

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["ETH"],
    "start": "2025-01-01",
    "end": "2025-01-08",
    "interval": "1d",
    "source": "lyra",
    "instrument": "option",
})
```
