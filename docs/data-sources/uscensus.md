# US Trade (Census)

US imports and exports by product and country, from the US Census Bureau.

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

## Ticker Format

`<product>` or `<product>:<partner>`

- **product**: HS code of 2, 4, 6 or 10 digits, or the same names as [UK Trade](hmrc.md).
- **partner**: Census country code (`4419` Switzerland, `4120` United Kingdom, `5330` India, `5700` China) or the country name in capitals (`SWITZERLAND`); omitted = all countries.

## Fields

`import_value_usd`, `export_value_usd`; plus `import_quantity`, `export_quantity` for 10-digit codes (Census only reports quantities there)

## Intervals

`1m`, stamped at the first day of the month. Census publishes each month about five weeks after it ends.
