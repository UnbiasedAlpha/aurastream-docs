# Nemeton Signals

Datasets built by Unbiased Alpha from public records: crypto leverage and crowding, the tone of the Federal Reserve, insider conviction, 8-K risk flags, mutual fund flow pressure, federal contracts and layoffs per company, earnings surprises without analyst consensus, short crowding, Treasury auction strength, power demand surprises, and energy and agriculture surprises. They come with a Nemeton subscription.

**Source:** `signals` | **Credentials:** your Nemeton API key

```json
"credentials": {"key": "your-nemeton-api-key"}
```

Every value is stamped at the time it became available, so the series can be used in backtests without look-ahead (see [Point in time](#point-in-time) for the exceptions). A missing or rejected key returns an error.

## Example

```json
{
    "tickerlist": ["BTCUSDT", "ETHUSDT"],
    "start": "2026-01-01",
    "end": "2026-10-26",
    "interval": "1d",
    "source": "signals",
    "dataset": "crypto_leverage",
    "credentials": {"key": "YOUR_NEMETON_KEY"}
}
```

## Datasets

| `dataset` | Tickers | Frequency | History |
|---|---|---|---|
| `crypto_leverage` (default) | Perpetuals, e.g. `BTCUSDT` | Hourly | 2020 |
| `fed_tone` | `statement`, `minutes`, `speeches`, `index` | Per document; daily index | 2006 |
| `event_flags` | US stock tickers | Per 8-K | 2004 |
| `insider_conviction` | US stock tickers | Per insider trade | 2006 |
| `fund_flow_pressure` | US stock tickers | Monthly | 2019 |
| `gov_contracts` | US stock tickers | Monthly | 2007 |
| `layoffs` | US stock tickers | Per notice | 2004 (NJ), 2019 (CA) |
| `energy_ag` | `gas_storage`, `corn`, `soybeans`, `wheat` | Weekly (gas), monthly (crops) | 2010 |
| `earnings_surprise` | US stock tickers | Per quarterly report | 2009 (surprises from 2011) |
| `short_crowding` | US stock tickers | Twice a month | December 2017 |
| `treasury_auctions` | `auctions`, `2Y`, `3Y`, `5Y`, `7Y`, `10Y`, `20Y`, `30Y` | Per auction | 2008 |
| `power_demand` | `US48` | Daily | 2019 |

For stock datasets, a company with no record returns an empty series.

### crypto_leverage

Hourly per perpetual:

| Field | Description |
|---|---|
| `funding_rate`, `funding_annualized` | Binance funding rate, and annualized in % |
| `hl_funding_annualized`, `funding_spread_annualized` | Hyperliquid funding (annualized %) and Binance minus Hyperliquid |
| `basis_bps` | Perpetual premium over the index, in basis points |
| `oi`, `oi_value`, `oi_change_24h` | Open interest in contracts and USD, and its 24-hour change in % |
| `ls_account`, `ls_top_position`, `taker_buy_sell` | Long/short account ratio, top traders' long/short position ratio, taker buy/sell volume ratio |
| `z_funding`, `z_basis`, `z_oi_change`, `z_long_short` | Z-scores against the trailing 90 days (after 30 days of data) |
| `crowding_carry` | Mean of the funding and basis z-scores |
| `crowding_positioning` | Mean of the open interest and long/short z-scores |
| `crowding` | Mean of the two; positive = crowded long |

Funding and basis go back to 2020. Open interest and long/short ratios are collected from October 2026, so `crowding_positioning` starts once 30 days of them exist; until then `crowding` equals `crowding_carry`. Longer intervals keep the last hour of each period.

### fed_tone

Hawkish or dovish tone of every FOMC statement, set of minutes and speech by a Board member, from Unbiased Alpha's own word rules.

| Field | Description |
|---|---|
| `score` | From -1 (dovish) to +1 (hawkish) |
| `hawkish`, `dovish` | Number of hawkish and dovish matches |
| `net_per_1000` | Hawkish minus dovish matches per 1,000 words |
| `words`, `title`, `url` | Length, title and link of the document |
| `similarity_prev`, `delta_score` | Statements only: similarity to the previous statement, and the tone of the sentences added minus those removed |
| `meeting`, `speaker` | Minutes and statements: the meeting; speeches: the speaker |

`index` is a daily average of all documents with a 30-day half-life (`score`). Statements announcing rate hikes score +0.80 on average, holds +0.07 and cuts -0.34 (2006-2026).

### event_flags

Every 8-K since August 2004 at its SEC acceptance time:

| Field | Description |
|---|---|
| `items` | 8-K item codes |
| `flags` | Risk flags from the items: `restatement`, `auditor_change`, `delisting_notice`, `bankruptcy`, `debt_acceleration`, `impairment`, `cyber_incident`, `control_change`, `leadership_change`, `exit_costs` |
| `severe` | Whether the 8-K carries a severe flag |
| `severe_events_90d` | The company's severe 8-Ks in the trailing 90 days |
| `event_date`, `form`, `accession`, `cik` | Event date, form type (8-K or 8-K/A), filing number and SEC company number |

### insider_conviction

Every open-market purchase and sale by an insider (Form 4 codes P and S) since 2006, stamped at the end of the filing day (New York). Each insider is classified, after Cohen, Malloy and Pomorski (2012), as `routine` (traded in the same month in each of the three previous years), `opportunistic`, or `unclassified` (not enough history).

| Field | Description |
|---|---|
| `owner`, `officer_title`, `is_director`, `is_officer`, `is_ten_percent_owner` | Who traded |
| `classification` | `routine`, `opportunistic` or `unclassified` |
| `buy_shares`, `buy_value`, `sell_shares`, `sell_value` | The filing's purchases and sales |
| `shares_owned_after`, `plan_10b5_1` | Holding after the trades, and whether they were under a 10b5-1 plan |
| `opp_buyers_30d`, `opp_buy_value_30d`, `opp_sellers_30d`, `opp_sell_value_30d` | Opportunistic buyers and sellers in the company over the trailing 30 days, and their value |
| `buy_conviction` | Opportunistic buyers over 30 days, with chief executives and finance chiefs counted twice |
| `transaction_date`, `filing_date`, `accession` | Dates and filing number |

### fund_flow_pressure

How much money is flowing into or out of the mutual funds and ETFs that hold a stock, from their N-PORT reports to the SEC (after Coval and Stafford 2007, Lou 2012). One row per month, stamped 61 days after it ends (the filing deadline), from the reports filed by then.

| Field | Description |
|---|---|
| `flow_pressure` | Holdings-weighted average flow of the funds holding the stock (sales + reinvestments - redemptions, over net assets); positive = holders taking money in |
| `pressure_usd` | Implied buying (+) or selling (-) if each fund scaled its position by its flow, in US dollars |
| `fire_sale_share`, `inflow_share` | Share of fund-held value in funds with flows below -5% or above +5% |
| `n_funds`, `fund_value` | Funds holding the stock and the value they hold (US dollars) |
| `report_month`, `cusip`, `name` | The month and the stock |

Only each fund's fiscal quarter-end report is public, so a month covers the funds whose quarter ends then: most funds in March, June, September and December, a third to a half in other months (see `n_funds`).

### gov_contracts

Federal contract obligations per company and month, from USAspending (the 1,000 largest contract recipients of each month since October 2007), summed over the company's contracting entities. Stamped 100 days after the month, because Defense contracts are published with a 90-day delay.

| Field | Description |
|---|---|
| `obligations` | Contract obligations in the month (US dollars) |
| `ttm_obligations`, `yoy_growth` | Trailing 12 months, and its change against a year earlier |
| `momentum_3m` | Last 3 months against the 3 before |
| `n_recipients`, `recipients` | The company's contracting entities that month (largest first) |
| `match` | Least certain name match behind the month: `override`, `exact` or `prefix` |

### layoffs

WARN mass-layoff and closure notices (California, New Jersey) filed by the company or its subsidiaries: `company` as filed, `employees`, `state`, `notice_type`, `notice_date`, `received_date` (California's processed date), `effective_date`, `city`, `county` and `match`.

Each row is stamped when the state published it, not at the notice date. California updates its list on Tuesday and Thursday afternoons with the notices processed up to the day before, so a row is stamped at 17:00 UTC on the first Tuesday or Thursday after its processed date; in archived copies of the list about 2% of notices appeared later still. New Jersey gives only the month a notice was posted and adds notices throughout that month, so its rows are stamped at the start of the following month.

Records name companies without an identifier, so names are matched to tickers by Unbiased Alpha's own rules: whole names and leading words against the SEC company list, plus a curated list of subsidiaries. Matching favours precision: joint ventures and names that could belong to an unrelated listed company are left out.

### energy_ag

`gas_storage`: the weekly EIA natural gas storage change as first released, at the actual release time (usually Thursday 10:30 New York; earlier or later in holiday weeks).

| Field | Description |
|---|---|
| `storage`, `change` | Working gas in storage and its weekly change (Bcf), as first reported; `change` is EIA's implied flow, which leaves out reclassifications between categories |
| `expected_change` | Change expected from weather and season, fitted on the previous five years only |
| `surprise`, `surprise_z` | Change minus expected, and as a z-score |
| `hdd_week`, `cdd_week`, `hdd_anomaly` | The week's heating and cooling degree days, and heating degree days against normal |

`corn`, `soybeans`, `wheat`: the US WASDE balance at each report.

| Field | Description |
|---|---|
| `ending_stocks`, `use_total`, `stocks_to_use` | This report's projection for the market year |
| `ending_stocks_prev`, `stocks_to_use_prev` | The same in the previous report |
| `revision`, `revision_pct`, `revision_z` | Change in ending stocks from the previous report, in % and as a z-score against five years of revisions |
| `report`, `market_year` | Report month and market year |

These are not surprises against analyst consensus: they are against our model (gas) or the previous report (crops).

### earnings_surprise

Each company's quarterly diluted earnings per share and revenue from its XBRL filings to the SEC, as first reported, against the company's own history: no analyst consensus. One row per fiscal quarter, stamped at 22:00 New York on the filing date of the 10-Q or 10-K (EDGAR's cut-off; the earnings press release often comes days earlier).

| Field | Description |
|---|---|
| `eps`, `revenue` | Diluted EPS (basic where diluted is not reported) and revenue, from the first filing that reported the quarter |
| `sue` | EPS minus EPS four quarters earlier, over the standard deviation of that change in the previous 8 quarters (needs 6, and at least one cent), capped at ±10 |
| `revenue_growth`, `revenue_surprise` | Revenue growth against four quarters earlier (log), and its distance from the previous 8 quarters' mean in standard deviations, capped at ±10 |
| `period_end`, `fiscal_period`, `form`, `accession` | The quarter (as the company labels it) and the filing |

Fourth quarters are rarely reported on their own: they are the fiscal year minus the first three quarters, stamped at the 10-K. Later restatements do not change a row: the first figure is the one that was public. Some filings tag EPS with the wrong scale (millions instead of dollars); `eps` shows the figure as filed, and the caps keep such filings from dominating the surprises.

### short_crowding

How crowded each stock's short side is, at each FINRA short interest settlement date (twice a month), stamped at FINRA's publication: 18:00 New York on the 8th business day after settlement (FINRA's schedule is 7).

| Field | Description |
|---|---|
| `short_interest`, `avg_daily_volume`, `days_to_cover` | FINRA consolidated short interest, average daily volume and their ratio |
| `si_change` | Log change of short interest from the previous settlement date |
| `ftd_ratio`, `ftd_half` | Mean daily fails-to-deliver over the latest half-month the SEC had posted by publication, over average daily volume, and that half-month |
| `crowding` | Mean of the cross-sectional z-scores (capped at 3) of log days to cover, `si_change` and log(1 + `ftd_ratio`) over every symbol FINRA reports that date; positive = crowded short |
| `settlement_date` | The settlement date |

### treasury_auctions

Every 2- to 30-year nominal coupon auction (new issues and reopenings) since 2008, from the Treasury's results, stamped 5 minutes after the competitive bids close (usually 13:00 New York). `auctions` returns all tenors; `2Y` ... `30Y` one each.

| Field | Description |
|---|---|
| `bid_to_cover` | Competitive bids over accepted |
| `indirect_share`, `direct_share`, `dealer_share` | Each bidder group's share of the accepted competitive amount; dealers take what others do not |
| `dispersion_bp` | High yield minus median yield, in basis points |
| `z_*` | Each against the previous 8 auctions of the same tenor |
| `weakness` | Mean of z(dealer share), z(dispersion), -z(bid to cover), -z(indirect share); positive = weaker than usual |
| `tenor`, `security_term`, `reopening`, `cusip`, `offering_amt`, `high_yield`, `median_yield`, `auction_date` | The auction |

### power_demand

US Lower 48 electricity demand against the grid operators' own day-ahead forecast (EIA-930), per UTC day, stamped at 12:00 UTC the next day. Demand above forecast usually means weather the forecast missed, so more gas burned in power plants.

| Field | Description |
|---|---|
| `demand_mwh`, `forecast_mwh` | Sums of the 24 hourly values (a day appears only with all of them) |
| `surprise_pct` | Demand minus forecast, in % of the forecast |
| `surprise_z` | `surprise_pct` against the previous 60 days |

## Point in time

Each row's timestamp is when its source made the data public. From 28 October 2026, every row also carries:

| Field | Description |
|---|---|
| `first_seen` | When we first served the row (UTC); empty for rows served before tracking began |
| `revised` | The last time the row's values changed after that, or empty |

A row with `first_seen` well after its timestamp was backfilled; a row with `revised` was changed after it was first served. Keeping rows with `first_seen` up to a given date shows what was available then.

The timestamp and `first_seen` are both times by which the data was public (the timestamp is an estimate where the source gives none, as for California layoffs), so the row can be treated as known from the earlier of the two (or from the timestamp when `first_seen` is empty). They differ where the source does not say exactly when it published: a New Jersey layoff notice is stamped at the start of the month after it was posted, which can be in the future, while `first_seen` shows the day we first had it. The exception is `gov_contracts`: a month is served while USAspending is still filling it in, about 70 days before its timestamp, so its early values are partial (`revised` marks the later changes); use its timestamp.

Before 28 October 2026, the histories were rebuilt from the sources:

- `event_flags`, `fund_flow_pressure`, `fed_tone`, `energy_ag` and `crypto_leverage` use each source's own release times and filing dates, so they match what was public at each timestamp.
- `insider_conviction` is stamped at the end of the filing day, up to a day after the filing was public.
- `gov_contracts` uses USAspending's current figures, which include later changes to contracts.
- `layoffs` is stamped at the publication time estimated from each state's schedule (see [layoffs](#layoffs)).
- `earnings_surprise` uses each figure as first filed, stamped at the end of the filing day.
- `short_crowding` uses FINRA's figures with the publication lag above, and fails files only once the SEC had posted them.
- `power_demand` uses EIA's current values, which include late corrections by grid operators.

## Notes

- Company tickers are today's tickers from the SEC company list; a company that changed ticker is found under its current one.
- This is research data, not investment advice. Test it out of sample before relying on it.
