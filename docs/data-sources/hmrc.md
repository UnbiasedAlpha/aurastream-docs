# UK Trade (HMRC)

UK imports and exports by product and country, from HM Revenue & Customs. London holds most of the world's vaulted gold, so UK flows show where bullion is moving.

**Source:** `hmrc` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [
        "gold",
        "gold:CH",
        "silver:US"
    ],
    "start": "2020-01-01",
    "end": "2025-01-01",
    "interval": "1M",
    "source": "hmrc"
}
```

## Ticker Format

`<product>` or `<product>:<partner>`

- **product**: an HS/CN code of 2 to 8 digits (`7108` sums all gold codes, `71081200` is non-monetary unwrought gold) or a name:

| Name | HS code |
|---|---|
| `gold` | 7108 |
| `silver` | 7106 |
| `platinum_group` | 7110 |
| `copper_refined` | 7403 |
| `copper_ore` | 2603 |
| `aluminium` | 7601 |
| `nickel` | 7502 |
| `zinc` | 7901 |

- **partner**: ISO 2-letter country code (`CH`, `US`, `IN`, `AE`, `HK`); omitted = all countries.

## Fields

`import_value_gbp`, `import_mass_kg`, `export_value_gbp`, `export_mass_kg`

## Intervals

`1M`, stamped at the first day of the month. HMRC publishes each month about six weeks after it ends.

*Source: HMRC, Open Government Licence v3.0.*
