# US Census (Trade and Retail Sales)

US imports and exports by product and country, and monthly retail sales, from the US Census Bureau.

**Source:** `uscensus` | **Credentials:** Required ([free key](https://api.census.gov/data/key_signup.html))

## Example

```json
{
    "tickerlist": [
        "gold",
        "gold:SWITZERLAND"
    ],
    "start": "2020-01-01",
    "end": "2025-01-01",
    "interval": "1m",
    "source": "uscensus",
    "credentials": {
        "key": "YOUR_CENSUS_KEY"
    }
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `dataset` | `str` | No | `"trade"` (default) or `"retail"` (retail and food services sales) |
| `adjusted` | `bool` | No | `retail` only: seasonally adjusted (default `true`) or not |

## Ticker Format

**`trade`**: `<product>` or `<product>:<partner>`

- **product**: HS code of 2, 4, 6 or 10 digits, or the same names as [UK Trade](hmrc.md).
- **partner**: Census country code (`4419` Switzerland, `4120` United Kingdom, `5330` India, `5700` China) or the country name in capitals (`SWITZERLAND`); omitted = all countries.

**`retail`**: a category name or Census category code:

| Ticker | Category |
|---|---|
| `total` | Retail and food services, total (the headline figure) |
| `ex_autos`, `ex_gas`, `ex_autos_gas` | Total excluding motor vehicles and parts, gasoline stations, or both |
| `retail` | Retail only (excluding food services) |
| `nonstore` | Nonstore retailers (mostly online) |
| `food_services` | Restaurants and bars |
| `autos`, `furniture`, `electronics`, `building`, `food_beverage_stores`, `grocery`, `health`, `gasoline`, `clothing`, `sporting_books`, `general_merchandise`, `misc_stores` | Store types |
| a code, e.g. `4451` | Any category code of the [Census retail survey](https://www.census.gov/retail/) |

## Fields

**`trade`**: `import_value_usd`, `export_value_usd`; plus `import_quantity`, `export_quantity` for 10-digit codes (Census only reports quantities there)

**`retail`**: `sales_musd` (USD millions), `change_pct` (from the previous month), and the sampling errors `sales_cv_pct` (coefficient of variation, %) and `change_se` (standard error of the change) where Census gives them. Monthly from 1992.

## Intervals

`1m`, stamped at the first day of the month. Census publishes trade about five weeks after the month ends, and the advance retail estimate about two weeks after; retail figures are revised in the next two releases and the values here are the latest estimates.

## Notes

This product uses the Census Bureau Data API but is not endorsed or certified by the Census Bureau.
