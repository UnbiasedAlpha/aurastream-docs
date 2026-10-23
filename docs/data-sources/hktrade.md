# Hong Kong Trade

Hong Kong imports and exports of gold, silver, platinum and palladium by country, from the Census and Statistics Department. Hong Kong's gold exports to the Chinese mainland are the classic gauge of Chinese demand.

**Source:** `hktrade` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [
        "gold:CN",
        "gold",
        "silver:CH"
    ],
    "start": "2020-01-01",
    "end": "2025-01-01",
    "interval": "1M",
    "source": "hktrade"
}
```

## Ticker Format

`<metal>` or `<metal>:<partner>`

- **metal**:

| Name | HKHS codes |
|---|---|
| `gold` | 710811, 710812, 710813 (non-monetary gold: powder, unwrought, semi-manufactured) |
| `gold_unwrought` | 710812 |
| `gold_monetary` | 710820 |
| `silver` | 710691, 710692 |
| `platinum` | 711011 |
| `palladium` | 711021 |

- **partner**: one of `CN` (Chinese mainland), `CH`, `GB`, `US`, `AU`, `ZA`, `AE`, `IN`, `SG`, `TH`, `JP`, `TW`, `KR`, `CA`, `TR`, `MO`, `MY`, `ID`, `PH`, `DE`; omitted = all countries.

## Fields

`import_value_hkd`, `import_mass_kg` (imports by country of origin), `export_value_hkd`, `export_mass_kg` (total exports, domestic plus re-exports, by destination)

## Intervals

`1M`, stamped at the first day of the month, from 2014. The Census and Statistics Department publishes each month about four weeks after it ends.

*Source: Census and Statistics Department, Hong Kong SAR (DATA.GOV.HK).*
