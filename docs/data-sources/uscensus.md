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
    "interval": "1M",
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

`import_value_usd`, `import_quantity`, `export_value_usd`, `export_quantity` (quantity in the code's first unit, e.g. grams of gold content for 7108)

## Intervals

`1M`, stamped at the first day of the month. Census publishes each month about five weeks after it ends.
