# CoinGecko

Crypto market data using coin names.

**Source:** `coingecko` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": "bitcoin",
    "start": "2025-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "coingecko"
}
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `vs_currency` | `str` | `"USD"` | Quote currency (e.g., `"USD"`, `"EUR"`) |

## Ticker Format

CoinGecko uses **coin names** (not symbols): `bitcoin`, `ethereum`, `solana`

!!! note
    `tickerlist` can be a single string or a list of strings for this source.
