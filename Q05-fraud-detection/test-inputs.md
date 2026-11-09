# Test Inputs — Q5 Financial Fraud Detection

All transaction data below is synthetic, generated for prompt-testing purposes only.
No real account or customer data is used.

## TI-1: Classic structuring pattern

```
Account A-10231 (personal checking):
- 09:14 — Deposit $8,900 cash, Branch 4
- 09:41 — Deposit $8,700 cash, Branch 4
- 10:05 — Deposit $8,400 cash, different teller, Branch 4
- Account opened 11 days ago
- No prior cash deposit history on this account before today
```

## TI-2: Ambiguous pattern — could be legitimate business cash flow

```
Account B-55102 (business checking, registered to "Patel Wholesale Traders"):
- Daily cash deposits ranging $3,000-$7,000, Mon-Sat, for the past 6 weeks
- Account has been open 2 years
- No new payees added recently
- One $15,000 outgoing wire to a known regional supplier (recurring monthly payee)
```

## TI-3: Sparse data — insufficient to score confidently

```
Account C-77890:
- One transaction: $4,200 transfer to new payee "J. Fernandes"
- No other account history provided
```
