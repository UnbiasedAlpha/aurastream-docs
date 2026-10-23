# EIA

Energy inventories, production and prices from the US Energy Information Administration.

**Source:** `eia` | **Credentials:** Required ([free key](https://www.eia.gov/opendata/register.php))

## Example

```json
{
    "tickerlist": [
        "crude_stocks",
        "natgas_storage"
    ],
    "start": "2020-01-01",
    "end": "2025-01-01",
    "interval": "1w",
    "source": "eia",
    "credentials": {
        "key": "YOUR_EIA_KEY"
    }
}
```

## Ticker Format

EIA series IDs (e.g. `PET.WCESTUS1.W`) or these names:

| Ticker | Description |
|---|---|
| `crude_stocks` | US commercial crude inventories (weekly) |
| `cushing_stocks` | Cushing, OK crude inventories (weekly) |
| `gasoline_stocks`, `distillate_stocks` | Product inventories (weekly) |
| `crude_production`, `crude_imports` | US crude production and imports (weekly) |
| `refinery_utilization` | Refinery utilization (weekly) |
| `natgas_storage` | Natural gas in storage, lower 48 (weekly) |
| `wti_spot`, `brent_spot`, `henry_hub_spot` | Spot prices (daily) |

## Fields

`value`, `units`

!!! warning "Publication lag"
    Rows are stamped at the period they describe. Weekly petroleum data for the week ending Friday is published the next Wednesday at 10:30 New York, natural gas storage on Thursday.
