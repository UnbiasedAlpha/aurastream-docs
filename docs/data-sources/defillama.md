# DefiLlama

Crypto on-chain aggregates: stablecoin supply, DeFi total value locked, DEX volume and protocol fees. Daily, stamped at 00:00 UTC.

## Source ID

```json
"source": "defillama"
```

## Credentials

**Not required.**

## Ticker Format

| Ticker | Description | Field |
|---|---|---|
| `stablecoins`, `stablecoins:Ethereum` | USD stablecoin supply, all chains or one | `circulating_usd` |
| `tvl`, `tvl:Solana` | Total value locked in DeFi, all chains or one | `tvl` |
| `protocol:aave` | TVL of one protocol (DefiLlama slug) | `tvl` |
| `dex_volume`, `dex_volume:Ethereum` | Daily decentralized exchange volume | `volume` |
| `fees`, `fees:Ethereum` | Daily fees paid to protocols | `fees` |

Values are in USD. The `interval` parameter is ignored: the data is daily.

## Example

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["stablecoins", "tvl", "dex_volume"],
    "start": "2022-01-01",
    "end": "2025-01-01",
    "interval": "1d",
    "source": "defillama"
})
```
