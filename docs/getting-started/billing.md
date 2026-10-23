# Billing & Renewals

## Free plan

The Free plan never charges you and never asks for a card. It renews automatically each month.

## Trials

Some paid plans start with a free trial. You need a card on file to start one, and each account can use one trial. When the trial ends, the plan renews automatically and your card is charged, unless you cancel before the end date shown in the dashboard.

## Renewals

Paid plans renew automatically at the end of each billing period (monthly or yearly), charging your default card. You get an email with the invoice after each successful payment. You can switch auto-renewal off, or change between monthly and yearly billing, in the dashboard.

## If a payment fails

If a renewal can't be charged, your subscription is put on hold, your API keys stop working (requests return `403 Forbidden`), and you get an email. This happens when:

- the card is declined or expired, or
- your bank requires an extra confirmation step (such as 3-D Secure), which can't be completed for automatic renewals.

We retry the charge once a day using your current default card. To restore access straight away, update your card under **Payment Methods**, then open **Manage Subscription** and click **Renew Subscription**. Your existing keys start working again as soon as the payment goes through.

If the payment still hasn't gone through 7 days after the renewal date, the subscription is cancelled and its keys are deleted. You can subscribe again at any time, including on the free plan.

## Cancelling

Cancel any time from the dashboard. Cancelling deletes your API keys immediately, so requests with them will return `403 Forbidden`.
