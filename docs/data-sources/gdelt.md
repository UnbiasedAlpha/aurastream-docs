# GDELT

How much worldwide online news covers a topic, and in what tone, from the GDELT Project. From 2017.

**Source:** `gdelt` | **Credentials:** Not required

!!! warning "Beta"
    GDELT allows one request every few seconds per server and throttles bursts, so requests can fail or be slow. Keep ranges short and queries few.

## Example

```json
{
    "tickerlist": [
        "gold price"
    ],
    "start": "2024-01-01",
    "end": "2024-02-01",
    "interval": "1d",
    "source": "gdelt",
    "mode": "timelinetone"
}
```

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `mode` | `str` | No | `"timelinevol"` (default, share of all coverage), `"timelinevolraw"` (article count) or `"timelinetone"` (average tone, negative = pessimistic) |

## Ticker Format

Search queries: `gold price`, `"federal reserve" inflation`, `tariffs`

## Intervals

Set by GDELT from the request length: 15 minutes for a few days, hourly up to about a week, daily beyond. Long ranges are fetched in yearly chunks.
