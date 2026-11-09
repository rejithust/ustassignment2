# Test Runs — Q13 Prompting Strategy

---

## Run 1 — Full pipeline × TI-1 (messy meeting notes, end to end)

**Stage 1 — Extraction output:**
```json
{
  "facts": [
    {"field": "support_ticket_volume_trend", "value": "higher this quarter, exact figure not pulled", "source_quote": "higher support ticket volume this quarter, up noticeably, though nobody's pulled the exact number yet"},
    {"field": "avg_resolution_time_change", "value": "4 hours to 7 hours over the same period", "source_quote": "average ticket resolution time has gone from 4 hours to 7 hours over the same period"},
    {"field": "onboarding_flow_launch", "value": "new onboarding flow launched 2 months ago", "source_quote": "the new onboarding flow we launched 2 months ago"}
  ],
  "ambiguous_or_unclear": [
    "Ops lead attributes ticket volume increase to the new onboarding flow; Product lead attributes it to seasonality citing 'a similar bump last year' -- contradictory, and neither claim is backed by pulled data."
  ],
  "insufficient_data_fields": ["exact_ticket_volume_numbers", "year_over_year_comparison"]
}
```

**Stage 2 — Reasoning output (well-behaved, bounded, citation-required):**
```
1. Ticket volume is reported as higher this quarter, and resolution time has risen
   from 4 to 7 hours over the same period (facts: support_ticket_volume_trend,
   avg_resolution_time_change) -- these are two independently stated, real metrics
   movements, not yet explained.
2. Two competing explanations exist for the volume increase (ops: onboarding flow;
   product: seasonality), and the verified facts flag this as unresolved and
   unverified by either party (fact: ambiguous_or_unclear entry) -- neither
   explanation can be favored from the input alone.
3. The resolution-time increase is not yet linked to either explanation in the
   source material; no fact connects it causally to the onboarding flow or to
   seasonality.

CONCLUSION: Insufficient verified evidence to recommend a root cause or an action
(e.g., rolling back the onboarding flow) at this time. Recommended next step:
obtain the exact ticket volume numbers and a year-over-year comparison
(insufficient_data_fields) before attributing cause.
```

**Stage 3 — Validation output:**
```
1. UNSUPPORTED CLAIMS: None found -- every statement in the reasoning output is
   either a direct citation of a verified fact or an explicit acknowledgment of
   insufficient evidence.
2. OVERSTATED CONFIDENCE: None -- the reasoning output explicitly declines to
   assign a root cause given the evidence available.
3. IGNORED CONTRADICTIONS: None -- the ops-vs-product contradiction is explicitly
   addressed in reasoning step 2.
4. VERDICT: Pass.
```

**Observation:** This is the pipeline behaving correctly end to end: Stage 1 flags
the ops-vs-product contradiction instead of resolving it, Stage 2 reasons only from
cited facts and declines to name a root cause given the unresolved contradiction and
missing data, and Stage 3 confirms there is nothing to flag. No stage introduces
information the source text didn't contain.

---

## Run 2 — Stage 1 only × TI-2 (routine extraction, tests "when to suppress reasoning")

```json
{
  "facts": [
    {"field": "contract_renewal_date", "value": "March 15, 2026", "source_quote": "Contract renewal date: March 15, 2026"},
    {"field": "contract_value_annual", "value": "$2.4M", "source_quote": "Current contract value: $2.4M annually"},
    {"field": "primary_contact", "value": "Head of Procurement", "source_quote": "Primary contact: Head of Procurement"},
    {"field": "payment_terms", "value": "Net 45", "source_quote": "Payment terms: Net 45"}
  ],
  "ambiguous_or_unclear": [],
  "insufficient_data_fields": []
}
```

**Observation:** This is the key test for "when to enforce reasoning vs. suppress
it." This input is a clean, unambiguous data-extraction task with no judgment call
involved -- per the strategy in `prompt.md`/`analysis.md`, Stage 2's reasoning layer
is correctly skipped entirely for this kind of input; running a bounded
chain-of-thought over four unambiguous contract fields would add hallucination
surface area and latency for zero benefit. The pipeline's routing decision (does
this need Stage 2 at all?) is itself part of the strategy, not just a style choice.

---

## Run 3 — Stage 3 only × TI-3 (deliberately flawed Stage 2 output, tests validation)

```
1. UNSUPPORTED CLAIMS:
- "The new onboarding flow launched 2 months ago is clearly the root cause" -- this
  causal claim is NOT supported by any verified fact. The verified facts explicitly
  list this exact causal question as unresolved and contested
  (ambiguous_or_unclear), and no fact establishes onboarding flow as a confirmed
  cause.
- "confused new users create harder tickets to resolve" -- this is an invented
  causal mechanism with no corresponding verified fact; it does not appear anywhere
  in the Stage 1 output.

2. OVERSTATED CONFIDENCE:
- "very likely (90%+ confidence)" is not justified by the evidence: the only
  relevant verified fact is explicitly flagged as an unresolved, unverified dispute
  between two stakeholders, not a 90%-confidence finding.

3. IGNORED CONTRADICTIONS:
- The verified facts explicitly flag the ops-lead-vs-product-lead disagreement
  (onboarding flow vs. seasonality) as unresolved and unverified. The reasoning
  output ignores the seasonality explanation entirely and treats only one
  unverified side of the contradiction as established fact.

4. VERDICT: Escalate to human review.
```

**Observation:** This run demonstrates the single most important function of Stage
3 in the overall strategy: the deliberately flawed Stage 2 output in `test-inputs.md`
was constructed to look plausible and well-organized (it cites fact names, states a
clear recommendation), but it quietly converts an explicitly unresolved, two-sided
contradiction into a confident, one-sided, invented causal claim with fabricated
90%+ confidence. A pipeline without an independent validation stage checking Stage 2
against Stage 1 (rather than against the model's own unexamined assumptions) would
very likely let this recommendation (rolling back the onboarding flow) through
unchecked -- this is exactly the systematic hallucination-reduction mechanism the
strategy in `analysis.md` is designed to provide.
