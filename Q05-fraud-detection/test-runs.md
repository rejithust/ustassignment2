# Test Runs — Q5 Financial Fraud Detection

---

## Run 1 — Primary prompt × TI-1 (classic structuring pattern)

```
1. TRANSACTION ID / REFERENCE: A-10231 (3 deposits, 09:14-10:05)

2. OBSERVED PATTERN(S)
- Three cash deposits ($8,900 / $8,700 / $8,400), each individually just under the
  $10,000 reporting threshold.
- All three occurred within a ~50-minute window at the same branch.
- Account was opened only 11 days before these transactions.
- No prior cash deposit history exists on the account.

3. STEP-BY-STEP REASONING
1. Each deposit is individually below the $10,000 CTR threshold — a pattern
   consistent with deliberate threshold avoidance (data: three deposit amounts).
2. Multiple same-day, same-branch deposits in a short window is consistent with
   "smurfing"-style structuring rather than coincidental timing (data: 09:14/09:41/
   10:05 timestamps).
3. A brand-new account (11 days old) with no prior cash-deposit baseline makes this
   activity a sharp deviation from account history rather than a normal pattern
   (data: account age, no prior history).
4. The combination of (1)+(2)+(3) matches the named typology "structuring"
   specifically, rather than generic high cash usage.

4. RISK SCORE: 82 (High)
5. CONFIDENCE IN SCORE: High — three independent, corroborating data points (sub-
   threshold amounts, clustering, new-account deviation) support this score.
6. WHAT WOULD CHANGE THIS SCORE
- Evidence the account holder runs a legitimate cash-heavy business would lower it.
- A linked account showing the same pattern recurring weekly would raise it further.
```

**Observation:** The score stays at 82, not 90+, despite looking like a textbook
case — consistent with the rule that 90+ requires even stronger corroboration (e.g.,
a linked account or recurrence), which the single-day data here doesn't yet show.
This demonstrates the "avoid overconfidence" cap working as intended even on a
strong-looking pattern.

---

## Run 2 — Primary prompt × TI-2 (ambiguous: could be legitimate business)

```
1. TRANSACTION ID / REFERENCE: B-55102 (6-week daily cash deposit pattern)

2. OBSERVED PATTERN(S)
- Recurring daily cash deposits, $3,000-$7,000, six days a week, for 6 weeks.
- Account is 2 years old with an established transaction history.
- No new payees added recently.
- One $15,000 wire to a long-standing, recurring payee (a known supplier).

3. STEP-BY-STEP REASONING
1. Frequent sub-threshold cash deposits could resemble structuring in isolation
   (data: daily $3,000-$7,000 deposits).
2. However, the account is registered to a named wholesale trading business and has
   2 years of established history, which is consistent with routine cash-intensive
   retail/wholesale operations rather than a shell pattern (data: account age,
   business registration).
3. No new payees and a wire to a known recurring supplier argue against account
   takeover or mule-account typologies, which typically show new, unfamiliar payees
   (data: payee history).
4. The typology that best fits, if any concern exists at all, would be "potential
   unreported cash-intensive business activity" rather than structuring or takeover —
   and this is a regulatory/compliance question, not clearly a fraud pattern.

4. RISK SCORE: 35 (Medium)
5. CONFIDENCE IN SCORE: Medium — the business context and account tenure argue
   against fraud, but the data provided does not include independent verification
   that the stated business actually generates this volume of cash revenue.
6. WHAT WOULD CHANGE THIS SCORE
- Business revenue/registration verification showing cash volume is plausible for
  this business type would lower the score further.
- A sudden change in deposit pattern (timing, amounts, new payees) would raise it.
```

**Observation:** This is the key test for "avoid storytelling hallucination" and
overconfidence in the *other* direction — the model does not confidently declare this
"clearly legitimate" (it stays at Medium, not Low), because the input doesn't actually
verify the business's real cash volume; it also does not invent a dramatic laundering
narrative despite a surface resemblance to structuring.

---

## Run 3 — Primary prompt × TI-3 (sparse data, edge case)

```
1. TRANSACTION ID / REFERENCE: C-77890 (single transaction)

2. OBSERVED PATTERN(S)
- One $4,200 transfer to a payee not previously seen in the provided data ("J.
  Fernandes").

3. STEP-BY-STEP REASONING
1. A single transaction to a new payee is, on its own, an extremely common and
   usually benign event (data: one transaction only).
2. No account history, transaction frequency baseline, or prior pattern is provided
   to compare this against (data: absence of further records).
3. No fraud typology can be responsibly named from a single, isolated data point
   without a baseline.

4. RISK SCORE: Insufficient data
5. CONFIDENCE IN SCORE: N/A — a numeric score would not be falsifiable from this
   input alone.
6. WHAT WOULD CHANGE THIS SCORE
- Account transaction history (to establish a baseline) would allow a score to be
  assigned.
- Any additional context (e.g., payee relationship, account age, prior transfer
  patterns) would allow meaningful pattern comparison.
```

**Observation:** This is the critical stress test for "avoid overconfidence" taken to
its logical limit: with only one transaction, the model refuses to assign a numeric
risk score at all rather than anchoring on a default (e.g., "Low, 10") just to fill
the field — exactly the behavior the explicit "Insufficient data" fallback rule is
designed to produce.
