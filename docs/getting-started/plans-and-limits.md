# Plans & Limits

Every API key belongs to a plan. The plan sets how many calls you can make per month and how fast you can make them.

## Free plan

Start without a card: every new account starts on Free. Sign up at the [dashboard](https://aurastream.unbiased-alpha.com) and your API key is ready under **API Keys**.

| | Free |
|---|---|
| Price | $0, no card needed |
| API calls | 500 per month |
| Request rate | 1 request per second (short bursts of up to 5) |
| API keys | 1 |
| Data sources | All sources in [Data Sources](../data-sources/overview.md) |

The monthly allowance resets on the 1st of each month (00:00 UTC). Unused calls don't carry over.

## Paid plans

Paid plans raise the monthly allowance and the request rate, and let you add more API keys. Current plans and prices are on the [pricing page](https://aurastream.unbiased-alpha.com/prices). Some paid plans start with a free trial; trials need a card on file.

To move from Free to a paid plan, choose the new plan on the pricing page in the dashboard. Once the payment (or trial) starts, the Free plan ends and you get a new API key on the new plan; the Free key stops working.

## What happens at the limit

When you go over a limit, the API answers with **HTTP 429** instead of data:

| Response body | Meaning | What to do |
|---|---|---|
| `{"message": "Limit Exceeded"}` | Monthly allowance used up | Wait for the reset on the 1st, or upgrade |
| `{"message": "Too Many Requests"}` | Request rate too high | Slow down and retry after a short pause |

Calls rejected with 429 don't count against your allowance. See [Errors](../api-reference/errors.md) for all error responses.

## Making the most of your calls

One call can return many tickers and a long date range, so batch your requests:

- Put several tickers in one `tickerlist` instead of one call per ticker.
- Request the whole date range you need in one call, then cache it locally.
- Use the coarsest `interval` that works for your analysis.
