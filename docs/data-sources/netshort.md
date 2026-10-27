# EU Net Short Positions

Disclosed net short positions in European shares. Under the EU Short Selling Regulation, any holder with a net short position of 0.5% or more of a company's issued shares must publish it. Aurastream serves the registers of the regulators whose terms allow reuse: France (AMF), the Netherlands (AFM) and Spain (CNMV).

**Source:** `netshort` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["ubisoft", "NL0000009538"],
    "start": "2026-01-01",
    "end": "2026-10-24",
    "interval": "1d",
    "source": "netshort",
    "country": ["FR", "NL"]
}
```

## Ticker Format

- An ISIN, e.g. `FR0000054470`
- Part of the issuer's name, e.g. `ubisoft`, `philips` (case-insensitive)
- `all` for every position in the period

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `country` | `list[str]` | No | `"FR"`, `"NL"`, `"ES"` (default: all three) |
| `holder` | `str` | No | Keep positions whose holder name contains this text, e.g. `"citadel"` |

## Fields

One row per published position, stamped at the position date. Several positions on the same date are separated by a microsecond.

| Field | Description |
|---|---|
| `holder` | Position holder |
| `issuer`, `isin` | Company and ISIN |
| `net_short_pct` | Net short position, percent of issued share capital |
| `country` | `FR`, `NL` or `ES` |
| `holder_lei` | Holder's Legal Entity Identifier (France only) |
| `published_from`, `published_to` | Dates the position was on the public register (France only) |
| `source` | The attribution each regulator requires, delivered with the data |

## Coverage

| Country | Regulator | History |
|---|---|---|
| France | Autorité des marchés financiers | October 2012 |
| Netherlands | Autoriteit Financiële Markten | November 2012 |
| Spain | Comisión Nacional del Mercado de Valores | June 2010 |

## Notes

- Positions are published by the next trading day. A falling position is published once when it drops below 0.5%, then no longer.
- The registers are refreshed daily; Aurastream re-reads them at most hourly.
- Holders can be individuals, so treat holder names as personal data under the GDPR.
- France: source Autorité des marchés financiers (AMF), data.gouv.fr, Licence Ouverte 2.0. Netherlands: source AFM. Spain: this information can be obtained free of charge from the CNMV website (www.cnmv.es).

## OpenBB

This source is also available in the [OpenBB Platform](../getting-started/openbb.md) with `pip install openbb-aurastream`:

```python
obb.aurastream.net_short("ubisoft", country="FR")
```
