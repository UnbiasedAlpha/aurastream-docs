# World Bank Commodity Prices

Monthly commodity prices from the World Bank "Pink Sheet", nominal USD, from 1960: metals, energy, agriculture and fertilizers.

**Source:** `worldbank` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [
        "Gold",
        "Silver",
        "Platinum",
        "Copper",
        "Aluminum"
    ],
    "start": "2000-01-01",
    "end": "2025-01-01",
    "interval": "1M",
    "source": "worldbank"
}
```

## Ticker Format

A commodity name or part of it, case-insensitive: `Gold`, `Silver`, `Platinum`, `Copper`, `Aluminum`, `Nickel`, `Zinc`, `Lead`, `Tin`, `Iron ore`, `Crude oil, Brent`, `Natural gas, US`, `Wheat`...

## Fields

`price`, `units` (e.g. `$/troy oz`, `$/mt`), `commodity` (the matched column name)

## Intervals

`1M`, stamped at the first day of the month. Updated early each month.

*Source: World Bank Commodity Price Data (The Pink Sheet), CC BY 4.0.*
