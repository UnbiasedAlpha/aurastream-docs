# Federal Reserve Texts

FOMC statements and minutes, and speeches by Federal Reserve Board members, as plain text with their release times. For text analysis and sentiment models.

**Source:** `fedtext` | **Credentials:** Not required

## Example

```json
{
    "tickerlist": ["statement", "minutes"],
    "start": "2024-01-01",
    "end": "2026-10-24",
    "interval": "1d",
    "source": "fedtext"
}
```

## Ticker Format

| Ticker | Texts | Stamped at | History |
|---|---|---|---|
| `statement` | FOMC policy statements, including unscheduled ones | Release time printed on the statement; otherwise 14:00 New York (14:15 before 2013) | 2006 |
| `minutes` | FOMC minutes | 14:00 New York on the release date, about three weeks after the meeting | Late 2007 |
| `speeches` | Speeches by Board members | The date (the time of day is not published) | 2006 |

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `speaker` | `str` | No | `speeches` only: a speaker's last name, e.g. `"powell"` |

## Fields

| Field | Description |
|---|---|
| `title` | Title of the release or speech |
| `text` | Full text |
| `url` | Link to the page on federalreserve.gov |
| `meeting` | `statement` and `minutes`: the meeting's last day |
| `speaker`, `event` | `speeches`: speaker and where the speech was given |
| `source` | "Source: Board of Governors of the Federal Reserve System" |

## Notes

- Use `econcalendar` (`fomc`) for scheduled meeting times ahead of the release.
- Board content is in the public domain; cite the Board as the source.
- Minutes are long (about 40,000 characters each), so request a few years at a time.
