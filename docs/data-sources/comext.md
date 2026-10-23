# EU Trade (Eurostat Comext)

EU imports and exports by product and partner country, from Eurostat.

**Source:** `comext` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": [
        "gold",
        "gold:CH",
        "copper_refined:CN"
    ],
    "start": "2020-01-01",
    "end": "2025-01-01",
    "interval": "1M",
    "source": "comext"
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `reporter` | `str` | No | `"EU27_2020"` (default) or a member state, e.g. `"DE"`, `"IT"` |

## Ticker Format

`<product>` or `<product>:<partner>`

- **product**: HS/CN code of 2 to 8 digits, or the same names as [UK Trade](hmrc.md) (`gold`, `silver`, `platinum_group`, `copper_refined`...).
- **partner**: ISO 2-letter country (`CH`, `CN`, `GB`, `US`), `EXT_EU27_2020` (all countries outside the EU) or `WORLD` (all countries, member-state reporters only). Omitted = `EXT_EU27_2020` for the EU, `WORLD` for a member state.

## Fields

`import_value_eur`, `import_mass_kg`, `export_value_eur`, `export_mass_kg`

## Intervals

`1M`, stamped at the first day of the month. Eurostat publishes each month about six weeks after it ends.

*Source: Eurostat.*
