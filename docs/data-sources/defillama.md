# DefiLlama

Crypto on-chain aggregates: stablecoin supply, DeFi total value locked, DEX volume and protocol fees.

**Source:** `defillama` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [
        "stablecoins",
        "tvl",
        "dex_volume"
    ],
    "start": "2022-01-01",
    "end": "2025-01-01",
    "interval": "1d",
    "source": "defillama"
}
```

## Ticker Format

| Ticker | Description |
|---|---|
| `stablecoins`, `stablecoins:Ethereum` | USD stablecoin supply (`circulating_usd`), all chains or one |
| `tvl`, `tvl:Solana` | Total value locked in DeFi, all chains or one |
| `protocol:aave` | TVL of one protocol (DefiLlama slug) |
| `dex_volume`, `dex_volume:Ethereum` | Daily decentralized-exchange volume |
| `fees`, `fees:Ethereum` | Daily fees paid to protocols |

## Intervals

`1d` (stamped at 00:00 UTC)
