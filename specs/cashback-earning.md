# Cashback Earning — Example Mapping

## Story

As a customer, I want to earn cashback on my purchases so that I am rewarded for my loyalty.

## Rules

### R1 — Standard members should earn 1% cashback on qualifying spend

| Scenario | Qualifying spend | Non-qualifying spend (e.g. account fee) | Cashback earned |
|---|---|---|---|
| The one where all spend qualifies | $150 | $0 | $1.50 |
| The one where spend is mixed | $150 | $50 | $1.50 (non-qualifying portion excluded) |

**Counter-example**
- The one where none of the spend qualifies (e.g. a gift-card purchase) — cashback earned must be $0, not 1% of the gift-card amount.

**Open questions**
- Q1.1 — What exactly counts as qualifying spend? Does it exclude taxes, shipping/fees, gift-card purchases, cashback redemptions, or transfers to other members?

---

### R2 — Premium members should earn 2% cashback on qualifying spend

| Scenario | Member tier at time of purchase | Qualifying spend | Cashback earned |
|---|---|---|---|
| The one where a premium member spends normally | Premium | $150 | $3.00 |
| The one where the member's tier changed mid-month | Upgraded Standard→Premium, purchase made after upgrade | $150 | $3.00 (rate follows tier at time of purchase) |

**Counter-example**
- The one where the purchase happens before the upgrade takes effect — cashback must be earned at the old (1%) rate, not 2%, even though it falls in the same calendar month as the upgrade.

**Open questions**
- Q2.1 — Is tier determined by the exact transaction timestamp, or by a daily/billing-cycle snapshot?
- Q2.2 — Does an upgrade take effect immediately, or at the start of the next billing day/cycle?

---

### R3 — Total cashback must be capped at $50 per member per calendar month

| Scenario | Cashback already earned this month | This transaction's computed cashback | Cashback actually credited |
|---|---|---|---|
| The one where the running total stays under the cap | $0 | $40 | $40 |
| The one where a single transaction pushes the total over the cap | $0 | $60 | $50 (truncated) |
| The one where the cap has already been reached | $50 | $5 | $0 |

**Counter-example**
- The one where a member's household includes multiple accounts — each member's own spend must be capped individually; cashback must NOT be pooled or combined across accounts to a shared $50 limit.

**Open questions**
- Q3.1 — Is the cap enforced in real time per transaction (halting further accrual once hit), or calculated as a month-end aggregate and truncated?
- Q3.2 — Is the $50 limit per member account only, or could a future household/family plan share a cap?

---

### R4 — Refunds must reverse any cashback earned on the refunded amount

| Scenario | Original purchase | Cashback earned | Refunded amount | Cashback reversed |
|---|---|---|---|---|
| The one where the purchase is fully refunded | $500 | $5.00 | $500 | $5.00 |
| The one where the purchase is partially refunded | $100 | $2.00 | $40 | $0.80 |

**Counter-example**
- The one where a pending order is cancelled before it settles — no cashback was ever earned on an unsettled transaction, so nothing must be reversed; cancellation of a pending order is not a refund for cashback purposes.

**Open questions**
- Q4.1 — If a purchase from a prior month is refunded in the current month, is cashback reversed from the month it was originally earned, or from the current month's total?
- Q4.2 — What happens if the reversal would make a member's cashback balance negative (e.g. the cashback was already redeemed)?

---

### R5 — Cashback amounts must be calculated in USD, rounded half-up to the nearest cent

| Scenario | Raw calculated amount | Rounded cashback |
|---|---|---|
| The one where the third decimal is below 5 | $0.3748 | $0.37 |
| The one where the third decimal is exactly 5 (the half-up boundary) | $0.1250 | $0.13 |

**Counter-example**
- The one where round-half-to-even ("banker's rounding") is applied to $0.1250 instead — this must NOT be used, since it would give $0.12 rather than the required $0.13.

**Open questions**
- Q5.1 — Is rounding applied per transaction and then summed, or once on the monthly aggregate total? These can produce different final totals.

---

### R6 — A "month" must be the calendar month in the customer's local timezone

| Scenario | Purchase timestamp (UTC) | Customer timezone | Local purchase time | Month attributed |
|---|---|---|---|---|
| The one where UTC and local time fall in the same month | Jan 15, 3:00 PM | UTC-5 (New York) | Jan 15, 10:00 AM | January |
| The one where local time rolls into the next month before UTC does | Jan 31, 11:58 PM | UTC+9 (Tokyo) | Feb 1, 8:58 AM | February |

**Counter-example**
- The one where the month is determined by the server/UTC timestamp instead of local time — the Tokyo purchase above would be wrongly attributed to January. This approach must NOT be used.

**Open questions**
- Q6.1 — If a customer changes their registered timezone mid-month, which timezone (old or new) determines the month boundary for purchases already made before the change?
