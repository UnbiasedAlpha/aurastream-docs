# Credentials Reference

Each data source that requires authentication expects a specific credential format in the `credentials` field of the request body.

## Simple Key Sources

### Polygon

```json
{"credentials": {"key": "YOUR_POLYGON_KEY"}}
```

### FRED

```json
{"credentials": {"key": "YOUR_FRED_KEY"}}
```

### Finnhub

```json
{"credentials": {"key": "YOUR_FINNHUB_KEY"}}
```

### CoinAPI

```json
{"credentials": {"key": "YOUR_COINAPI_KEY"}}
```

### FirstRate

```json
{"credentials": {"key": "YOUR_FIRSTRATE_KEY"}}
```

### Nasdaq Data Link

```json
{"credentials": {"key": "YOUR_NASDAQ_KEY"}}
```

### Trading Economics

```json
{"credentials": {"key": "YOUR_TE_KEY"}}
```

### Nemeton

```json
{"credentials": {"key": "YOUR_NEMETON_KEY"}}
```

### Visual Crossing

```json
{"credentials": {"key": "YOUR_VISUALCROSSING_KEY"}}
```

## Multi-Field Sources

### Oanda

```json
{
    "credentials": {
        "id": "YOUR_OANDA_ACCOUNT_ID",
        "token": "YOUR_OANDA_TOKEN",
        "environment": "practice"
    }
}
```

| Field | Description |
|---|---|
| `id` | Your Oanda account ID (e.g., `101-004-12345678-001`) |
| `token` | Your Oanda API token |
| `environment` | `"practice"` for demo, `"live"` for real accounts |

### Alpaca

```json
{
    "credentials": {
        "key": "YOUR_ALPACA_KEY",
        "secret": "YOUR_ALPACA_SECRET"
    }
}
```

### TradeStation

```json
{
    "credentials": {
        "contact": "YOUR_CONTACT",
        "key": "YOUR_CLIENT_KEY",
        "secret": "YOUR_CLIENT_SECRET",
        "refresh_token": "YOUR_REFRESH_TOKEN",
        "account_id": "YOUR_ACCOUNT_ID"
    }
}
```

### City Index

```json
{
    "credentials": {
        "username": "YOUR_USERNAME",
        "password": "YOUR_PASSWORD",
        "key": "YOUR_APP_KEY",
        "client_account_id": "YOUR_CLIENT_ACCOUNT_ID",
        "trading_account_id": "YOUR_TRADING_ACCOUNT_ID"
    }
}
```

## Public Sources (No Credentials Needed)

The following sources do not require credentials — omit the `credentials` field or pass `null`:

Binance, Bybit, Deribit, dYdX, KuCoin, CoinGecko, Hyperliquid, Lyra, Kraken, Yahoo Finance, SDMX, Open-Meteo, Calendar.
