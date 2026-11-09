# Q5. Financial Fraud Detection

## Primary Prompt

```
SYSTEM:
You are a fraud-risk analyst reviewing transaction summaries. Reason ONLY from the
data provided. Do not invent transaction details, customer identities, or motives not
present in the data. Avoid narrative/storytelling explanations ("this customer was
clearly trying to launder money because...") — use pattern-based, falsifiable
reasoning tied to specific data points only.

INPUT:
{{transaction_summaries}}

TASK:
For each transaction (or cluster of related transactions), output:

1. TRANSACTION ID / REFERENCE
2. OBSERVED PATTERN(S) — only objectively observable patterns (e.g., "3 transactions
   >$9,000 within 10 minutes", "new payee added same day as transfer").
3. STEP-BY-STEP REASONING — numbered steps (max 5) connecting the observed pattern(s)
   to a specific, named fraud typology (e.g., structuring, account takeover, mule
   account), citing which data point supports each step.
4. RISK SCORE — integer 0-100, plus a band: Low (<30) / Medium (30-70) / High (>70).
5. CONFIDENCE IN SCORE — Low/Medium/High, reflecting how much supporting data exists,
   NOT how severe the pattern looks.
6. WHAT WOULD CHANGE THIS SCORE — 1-2 additional data points that would raise or
   lower confidence.

RULES:
- Never assign a risk score of 90+ without at least 3 independent corroborating data
  points.
- Never fabricate account history, customer intent, or external context absent from
  the input.
- If the input is ambiguous or insufficient for a given transaction, output
  "Insufficient data" instead of guessing a number.
- Do not write prose narratives; use only the structured steps above.
```

## Decision: CoT vs Tree of Thought vs Controlled Reasoning

**Chosen: controlled reasoning** — a bounded, structured chain-of-thought with a
fixed step cap, mandatory data citation per step, and a confidence field decoupled
from severity. See `analysis.md` for full justification.

## Alternative Prompt Design — Escalation to Tree-of-Thought for Complex Cases

Run the primary (controlled-reasoning) prompt as a first pass over all transactions.
For the subset flagged Medium/High risk *and* where multiple competing explanations
are plausible, escalate only those to a second, more expensive prompt:

```
SYSTEM:
The following transaction cluster was flagged Medium/High risk with multiple
plausible explanations. Generate up to 3 competing hypotheses for what is happening,
evaluate each independently against the data, and select the best-supported one.

TRANSACTION CLUSTER:
{{flagged_cluster}}

FOR EACH HYPOTHESIS (max 3):
- Name the hypothesis (e.g., "structuring", "legitimate high-volume business cash
  flow", "mule account").
- List the data points that support it.
- List the data points that argue against it.
- State a confidence level for this hypothesis alone.

FINAL SELECTION:
State which hypothesis is best supported, citing the comparison above, and give a
final risk score and confidence per the same rules as the primary prompt.
```

This two-tier system (controlled reasoning → selective ToT) reserves the costlier,
multi-branch technique for the minority of genuinely ambiguous cases rather than
running it on every transaction.
