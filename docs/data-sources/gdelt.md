# GDELT

How much worldwide online news covers a topic, and in what tone, from the GDELT Project. From 2017.

**Source:** `gdelt` | **Credentials:** Not required

!!! warning "Beta"
    Search queries go to GDELT live, which allows one request every few seconds per server and throttles bursts, so they can fail or be slow: keep ranges short and queries few. [News themes](#news-themes) have no such limit.

## Example

```json
{
    "tickerlist": [
        "gold price"
    ],
    "start": "2024-01-01",
    "end": "2024-02-01",
    "interval": "1d",
    "source": "gdelt",
    "mode": "timelinetone"
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `mode` | `str` | No | `"timelinevol"` (default, share of all coverage), `"timelinevolraw"` (article count) or `"timelinetone"` (average tone, negative = pessimistic) |

## Ticker Format

Search queries: `gold price`, `"federal reserve" inflation`, `tariffs`

## Intervals

Set by GDELT from the request length: 15 minutes for a few days, hourly up to about a week, daily beyond. Long ranges are fetched in yearly chunks.

## News Themes

`"dataset": "themes"` counts articles by GDELT theme instead of by search query, from GDELT's raw news files: no throttling, 15-minute resolution, from 2017.

```json
{
    "tickerlist": [
        "ECON_GOLDPRICE",
        "WB_1235_CENTRAL_BANKS"
    ],
    "start": "2024-01-01",
    "end": "2024-02-01",
    "interval": "1h",
    "source": "gdelt",
    "dataset": "themes"
}
```

| Theme | Topic |
|---|---|
| `ECON_GOLDPRICE`, `WB_2936_GOLD` | Gold price, gold |
| `ECON_INFLATION`, `WB_442_INFLATION` | Inflation |
| `WB_1235_CENTRAL_BANKS`, `EPU_POLICY_INTEREST_RATES` | Central banks, interest rates |
| `ECON_STOCKMARKET`, `ECON_CURRENCY_EXCHANGE_RATE` | Stock markets, exchange rates |
| `ENV_MINING`, `WB_1699_METAL_ORE_MINING`, `TAX_FNCACT_GOLD_MINERS` | Mining, gold miners |
| `ARMEDCONFLICT`, `SANCTIONS`, `TERROR`, `PROTEST` | Conflict and unrest |

All `ECON_*` and `EPU_*` (economic policy uncertainty) themes are available too.

Fields: `articles` (articles mentioning the theme), `share` (fraction of all articles), `tone` (their average tone). Each 15-minute period is stamped at its end, when GDELT publishes it; longer intervals add up the periods inside them.
