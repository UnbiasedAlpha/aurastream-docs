# Credentials Reference

Each data source that requires authentication expects a specific credential format in the `credentials` field of the request body.

## Oanda

```json
{
    "credentials": {
        "id": "101-004-XXXXXXXX-XXX",
        "token": "your-oanda-api-token",
        "environment": "practice"
    }
}
```

| Field | Description |
|---|---|
| `id` | Your Oanda account ID (e.g., `101-004-12345678-001`) |
| `token` | Your Oanda API token (from the Oanda portal) |
| `environment` | `"practice"` for demo, `"live"` for real accounts |

Get your credentials at [Oanda's developer portal](https://developer.oanda.com/).

## Alpaca

```json
{
    "credentials": {
        "key": "your-alpaca-api-key",
        "secret": "your-alpaca-secret-key"
    }
}
```

| Field | Description |
|---|---|
| `key` | Alpaca API key ID |
| `secret` | Alpaca API secret key |

Get your credentials at [Alpaca](https://alpaca.markets/).

## Polygon

```json
{
    "credentials": {
        "key": "your-polygon-api-key"
    }
}
```

Get your API key at [Polygon.io](https://polygon.io/).

## FRED

```json
{
    "credentials": {
        "key": "your-fred-api-key"
    }
}
```

Get your API key at [FRED](https://fred.stlouisfed.org/docs/api/api_key.html).

## Finnhub

```json
{
    "credentials": {
        "key": "your-finnhub-api-key"
    }
}
```

Get your API key at [Finnhub](https://finnhub.io/).

## EIA

```json
{
    "credentials": {
        "api_key": "your-eia-api-key"
    }
}
```

Get a free key at [EIA Open Data](https://www.eia.gov/opendata/register.php). `{"key": "..."}` works too.

## US Census

```json
{
    "credentials": {
        "api_key": "your-census-api-key"
    }
}
```

Get a free key at [Census API](https://api.census.gov/data/key_signup.html). `{"key": "..."}` works too.

## SEC EDGAR and US Economic Calendar (BLS)

```json
{
    "credentials": {
        "user_agent": "Your Company contact@yourcompany.com"
    }
}
```

The SEC and BLS require automated requests to identify the sender. No registration is needed. `econcalendar` needs it only for the `bls` ticker.

## FirstRate Data

```json
{
    "credentials": {
        "key": "your-firstrate-user-id"
    }
}
```

`key` is the user ID FirstRate Data gives you for API downloads. Your FirstRate subscription decides which datasets you can download.

## Nasdaq Data Link

```json
{
    "credentials": {
        "key": "your-nasdaq-data-link-key"
    }
}
```

Get a key at [Nasdaq Data Link](https://data.nasdaq.com/). You also need access to the tables you request.

## Nemeton

```json
{
    "credentials": {
        "key": "your-nemeton-api-key"
    }
}
```

The Nemeton key comes with a [Nemeton subscription](https://aurastream.unbiased-alpha.com/prices).

## CoinAPI, Trading Economics, Visual Crossing

```json
{
    "credentials": {
        "key": "your-provider-key"
    }
}
```

Each with its own provider key: [CoinAPI](https://www.coinapi.io/), [Trading Economics](https://tradingeconomics.com/api/), [Visual Crossing](https://www.visualcrossing.com/weather-api).

## CoinGecko

```json
{
    "credentials": {
        "demo_key": "your-coingecko-demo-key"
    }
}
```

A free [CoinGecko](https://www.coingecko.com/en/api) Demo key (last 365 days), or `"key"` for a paid Pro key.

## TradeStation

```json
{
    "credentials": {
        "key": "your-client-id",
        "secret": "your-client-secret",
        "refresh_token": "your-refresh-token"
    }
}
```

## City Index

```json
{
    "credentials": {
        "username": "your-username",
        "password": "your-password",
        "key": "your-app-key"
    }
}
```

## Public Sources (No Credentials Needed)

The following sources do not require credentials — omit the `credentials` field or pass `null`:

- **Binance** — public spot and futures market data
- **Bybit** — public perpetuals market data
- **Kraken** — public spot and futures market data
- **Yahoo Finance** — public equities, ETFs, indices, commodities
- **CFTC**, **FINRA**, **US Treasury**, **DefiLlama** — public positioning, short interest, fiscal and on-chain data
- **GDELT**, **World Bank**, **UK Trade (HMRC)**, **EU Trade (Eurostat)**, **Hong Kong Trade** — public news and trade statistics
- **Deribit**, **KuCoin**, **Hyperliquid**, **dYdX**, **Derive (Lyra)** — public crypto derivatives market data
- **SDMX**, **Open-Meteo** — public statistical series and historical weather
