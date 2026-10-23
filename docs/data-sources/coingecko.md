# CoinGecko

Daily price, market capitalisation and volume for thousands of coins, from CoinGecko. You need your own CoinGecko key (a free Demo key works).

## Source ID

```json
"source": "coingecko"
```

## Credentials

A paid (Pro) key:

```json
"credentials": {
    "key": "your-coingecko-pro-key"
}
```

or a free Demo key:

```json
"credentials": {
    "demo_key": "your-coingecko-demo-key"
}
```

Get a key at [CoinGecko](https://www.coingecko.com/en/api). Demo keys only reach the last 365 days of history: older requests fail with CoinGecko's message.

## Ticker Format

CoinGecko coin IDs (the last part of the coin's page URL on coingecko.com):

| Example | Coin |
|---|---|
| `bitcoin` | Bitcoin |
| `ethereum` | Ether |
| `solana` | Solana |

## Supported Intervals

`1d`. CoinGecko picks the granularity from the length of the range, so Aurastream asks for at least 91 days to get daily points and returns the ones in your range, stamped at 00:00 UTC.

## Fields

`prices`, `market_caps`, `total_volumes`

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `vs_currency` | `"USD"`, `"EUR"`, ... | `"USD"` | Quote currency |

## Examples

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["bitcoin", "ethereum"],
    "start": "2026-01-01",
    "end": "2026-03-01",
    "interval": "1d",
    "source": "coingecko",
    "credentials": {"demo_key": "your-coingecko-demo-key"},
})
```
