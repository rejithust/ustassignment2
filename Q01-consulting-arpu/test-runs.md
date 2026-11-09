# Test Runs — Q1 Consulting Case

Simulated runs of the **primary prompt** (see `prompt.md`) against each input in
`test-inputs.md`. These represent the expected, well-behaved output a model should
produce when it correctly follows the prompt's rules.

---

## Run 1 — Primary prompt × TI-1 (genuine contradiction present)

```
1. STATED FACTS
- ARPU has been sliding for a few quarters.
- The CFO attributes the decline to the "Unlimited Lite" plan cannibalizing premium
  subscribers (downgrades).
- Sales states premium subscriber numbers are stable and attributes the decline to
  heavier discounting during the festive season promo.
- Customer care reported a spike in complaints about network speed in the southern
  circles around the same time ARPU started dropping.
- No segment-wise ARPU breakdown is available, only the blended figure from finance.

2. CONTRADICTIONS DETECTED
- "CFO: premium subscribers are downgrading to Unlimited Lite" vs "Sales: premium
  subscriber numbers are stable."

3. STRUCTURED PROBLEM HYPOTHESES
- HYPOTHESIS A: Premium-to-Lite downgrade migration is driving ARPU down.
  Supporting evidence: CFO's stated view. Needed to confirm: segment-wise plan
  migration data (counts of premium accounts moving to Unlimited Lite).
- HYPOTHESIS B: Festive season discounting reduced realized revenue per user without
  subscriber loss. Supporting evidence: Sales' stated view. Needed to confirm:
  discount redemption rates and realized price vs. list price by period.
- HYPOTHESIS C: Network quality degradation in southern circles is depressing usage
  or triggering churn to lower-value plans. Supporting evidence: customer care
  complaint spike coinciding with ARPU decline. Needed to confirm: circle-wise ARPU
  trend compared to circle-wise complaint volume.
- HYPOTHESIS D: Blended ARPU decline is a mix-shift effect (more low-ARPU new
  subscribers added) rather than existing-customer behavior change. Supporting
  evidence: none directly stated — input does not break out new vs. existing
  subscriber ARPU. Needed to confirm: new-subscriber ARPU vs. base-subscriber ARPU.

4. MISSING DATA / INFORMATION GAPS
- Segment-wise (premium vs. Lite) ARPU and subscriber count trend.
- Discount/promo redemption rate and realized price data for the festive period.
- Circle-wise ARPU trend for the southern circles specifically.
- New vs. existing subscriber ARPU split.
- Time period definition of "a few quarters" (exact quarters affected).

5. RECOMMENDED NEXT STEPS
- Pull segment-wise ARPU and plan-migration data to test Hypothesis A directly.
- Reconcile CFO's and Sales' competing explanations with actual plan-migration counts
  before accepting either narrative.
- Cross-reference southern-circle network complaint data with circle-level ARPU.
- Request a mix-shift adjusted ARPU view (new vs. existing subscribers) from finance.
- Clarify the exact quarters in scope with leadership before further analysis.
```

**Observation:** The model correctly isolates the CFO-vs-Sales claim as a
contradiction instead of picking one side, and every hypothesis is traceable to a
specific stated claim. No numeric values were invented.

---

## Run 2 — Primary prompt × TI-2 (sparse input, edge case)

```
1. STATED FACTS
- ARPU is reported as down (no magnitude, time period, or segment given).
- Leadership has requested a hypotheses deck by Friday.

2. CONTRADICTIONS DETECTED
None detected.

3. STRUCTURED PROBLEM HYPOTHESES
Insufficient information in input to generate evidence-backed hypotheses. Generic
candidate causes of ARPU decline in telecom (e.g., plan downgrades, discounting,
competitive pricing pressure, usage decline, subscriber mix shift) exist, but none can
be tied to this specific input without further data, so they are not listed as
hypotheses here to avoid implying they are supported by the client's input.

4. MISSING DATA / INFORMATION GAPS
- Magnitude and time period of the ARPU decline.
- Whether the decline is blended, segment-specific, or circle-specific.
- Any stakeholder commentary on suspected causes.
- Baseline ARPU trend history for comparison.

5. RECOMMENDED NEXT STEPS
- Before building hypotheses, request the underlying ARPU trend data (magnitude,
  period, segment) from finance.
- Clarify with leadership what decision the Friday deck needs to support, given the
  current lack of data.
- Schedule a short intake call with Finance/Sales/Customer Care to surface any
  informal theories already circulating (as in a fuller brief) rather than guessing.
```

**Observation:** This is the key edge-case test. A weaker prompt would likely
invent plausible-sounding ARPU hypotheses anyway ("probably due to competitive
pricing...") to look useful. This prompt's explicit "Insufficient information in
input" rule and ban on inference correctly suppress that temptation and redirect to
information-gathering instead.

---

## Run 3 — Primary prompt × TI-3 (subtle contradiction + unsourced claim)

```
1. STATED FACTS
- ARPU dropped 6% in Q2 compared to Q1.
- Marketing attributes the Q2 drop to seasonality, describing it as "same as last
  year."
- Finance's trend report shows ARPU was flat quarter-over-quarter in the
  corresponding period last year.
- There is an informal, unverified claim that the largest competitor cut prices by
  15% just before Q2 began; no one has seen the competitor's actual rate card.

2. CONTRADICTIONS DETECTED
- "Marketing: this year's Q2 drop is seasonal, same as last year" vs "Finance: last
  year's equivalent quarter-over-quarter ARPU was flat (no comparable drop
  occurred)."

3. STRUCTURED PROBLEM HYPOTHESES
- HYPOTHESIS A: The Q2 drop is NOT purely seasonal, since finance's own data
  contradicts marketing's seasonality claim; an unaccounted factor specific to this
  year is likely involved. Supporting evidence: the contradiction itself. Needed to
  confirm: a direct year-over-year comparison of Q1-to-Q2 ARPU deltas.
- HYPOTHESIS B: Competitive price cuts (the 15% claim) may have forced reactive
  discounting or subscriber attrition. Supporting evidence: informal, unverified
  claim only — flagged as low-reliability since the source rate card has not been
  seen. Needed to confirm: verified competitor pricing data and any internal
  discount response triggered in Q2.

4. MISSING DATA / INFORMATION GAPS
- A verified year-over-year Q1-to-Q2 ARPU delta to properly test the seasonality
  claim.
- Verified competitor pricing/rate-card data (the 15% figure is currently hearsay).
- Any internal record of a pricing or discount response to the competitor's move.

5. RECOMMENDED NEXT STEPS
- Ask Finance to produce the prior-year Q1-to-Q2 ARPU delta directly, to settle the
  seasonality contradiction with data rather than stakeholder opinion.
- Attempt to independently verify the competitor price-cut claim (e.g., public rate
  card, customer-reported pricing) before using it as a basis for any hypothesis.
- Reconcile Marketing's and Finance's statements in a joint review before finalizing
  the problem framing.
```

**Observation:** The model treats the unsourced competitor-pricing claim as
lower-reliability information rather than a confirmed fact, and flags it explicitly
as unverified inside Hypothesis B rather than silently upgrading it to a "fact" in
section 1 — exactly the behavior the "no hallucinating missing facts" rule is meant
to produce.
