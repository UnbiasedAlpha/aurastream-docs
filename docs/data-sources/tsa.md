# TSA Checkpoint Numbers

Number of people screened at US airport security checkpoints each day, from the Transportation Security Administration. A daily, near-real-time read on travel demand and consumer activity.

**Source:** `tsa` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["throughput"],
    "start": "2025-01-01",
    "end": "2025-12-31",
    "interval": "1d",
    "source": "tsa"
}
```

## Ticker Format

One ticker: `throughput` (or leave `tickerlist` empty).

## Fields

One row per day from 2019, stamped at 00:00 of that day:

| Field | Description |
|---|---|
| `travelers` | People screened at TSA checkpoints that day |

## Notes

- TSA publishes each day's number the next morning (around 9:00 New York), so the latest row is usually yesterday.
- Compare a day with the same weekday a year earlier: traffic is strongly weekly and seasonal (holidays move between years).
- The `interval` parameter is ignored. Data is a US government work in the public domain.
