# EIA Energy Data

Energy inventories, production and prices from the US Energy Information Administration.

## Source ID

```json
"source": "eia"
```

## Credentials

```json
"credentials": {
    "api_key": "your-eia-api-key"
}
```

Get a free key at [EIA Open Data](https://www.eia.gov/opendata/register.php). `{"key": "..."}` is accepted too.

## Ticker Format

EIA series IDs, or these built-in names:

| Name | Series | Description |
|---|---|---|
| `crude_stocks` | `PET.WCESTUS1.W` | US commercial crude inventories (weekly) |
| `cushing_stocks` | `PET.W_EPC0_SAX_YCUOK_MBBL.W` | Cushing, OK crude inventories (weekly) |
| `gasoline_stocks` | `PET.WGTSTUS1.W` | Gasoline inventories (weekly) |
| `distillate_stocks` | `PET.WDISTUS1.W` | Distillate inventories (weekly) |
| `crude_production` | `PET.WCRFPUS2.W` | US crude production (weekly) |
| `crude_imports` | `PET.WCRIMUS2.W` | US crude imports (weekly) |
| `refinery_utilization` | `PET.WPULEUS3.W` | Refinery utilization (weekly) |
| `natgas_storage` | `NG.NW2_EPG0_SWO_R48_BCF.W` | Natural gas in storage (weekly) |
| `wti_spot`, `brent_spot` | `PET.RWTC.D`, `PET.RBRTE.D` | Crude spot prices (daily) |
| `henry_hub_spot` | `NG.RNGWHHD.D` | Natural gas spot price (daily) |

## Fields

`value` and `units` (for example `MBBL` for thousand barrels). The `interval` parameter is ignored: each series comes at its own frequency.

!!! warning "Publication lag"
    Rows are stamped at the period they describe. Weekly petroleum data for the week ending Friday is published the next Wednesday at 10:30 New York, natural gas storage on Thursday. Shift it before using it in a backtest.

## Example

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["crude_stocks", "natgas_storage"],
    "start": "2020-01-01",
    "end": "2025-01-01",
    "interval": "1w",
    "source": "eia",
    "credentials": {"api_key": "YOUR_EIA_KEY"}
})
```
