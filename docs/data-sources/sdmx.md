# SDMX

Statistical series from central banks and international organisations that publish through SDMX web services (BIS, ECB, Eurostat, IMF, OECD and others).

## Source ID

```json
"source": "sdmx"
```

## Credentials

**Not required.**

## Ticker Format

An SDMX data query: the dataflow, then `/`, then the series key.

| Example | Series |
|---|---|
| `BIS,WS_CBPOL,1.0/D.US` | BIS central bank policy rate, United States, daily |
| `BIS,WS_CBPOL,1.0/D.GB` | BIS central bank policy rate, United Kingdom, daily |

The dataflow is written `AGENCY,DATAFLOW,VERSION` and the key follows that dataflow's dimensions (here frequency and country). Each ticker should identify a single series: if a key matches several, only the first value per date is kept.

## Supported Intervals

`interval` is ignored: data comes at the series' own frequency, which is usually part of the key.

`start` and `end` are not applied either: Aurastream returns the full history of each series.

## Parameters

| Parameter | Values | Default | Description |
|---|---|---|---|
| `db` | SDMX provider ID: `"BIS"`, `"ECB"`, `"ESTAT"`, `"IMF"`, `"OECD"`, ... | none (required) | Which provider to query. IDs follow the [sdmx1 source list](https://sdmx1.readthedocs.io/en/latest/sources.html) |
| `l1` | any string | `"Value"` | Name of the value field in the response |

## Examples

### Central bank policy rates

```python
response = requests.post(URL, headers=HEADERS, json={
    "tickerlist": ["BIS,WS_CBPOL,1.0/D.US", "BIS,WS_CBPOL,1.0/D.GB"],
    "start": "2021-01-01",
    "end": "2025-01-10",
    "interval": "1d",
    "source": "sdmx",
    "db": "BIS",
    "l1": "PolicyRate",
})
```
