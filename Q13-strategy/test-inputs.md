# Test Inputs — Q13 Prompting Strategy

## TI-1: End-to-end scenario — a messy client meeting snippet, run through all 3 stages

```
Client meeting notes (raw): "We're seeing higher support ticket volume this quarter,
up noticeably, though nobody's pulled the exact number yet. The ops lead thinks it's
tied to the new onboarding flow we launched 2 months ago, but the product lead
disagrees and says it's just seasonal — we saw a similar bump last year too, though
again nobody's checked the actual year-over-year numbers. We do know for certain
that average ticket resolution time has gone from 4 hours to 7 hours over the same
period."
```

## TI-2: A routine extraction-only task (tests "when to suppress visible reasoning")

```
Client document: "Contract renewal date: March 15, 2026. Current contract value:
$2.4M annually. Primary contact: Head of Procurement. Payment terms: Net 45."
```

## TI-3: A deliberately flawed Stage 2 reasoning output, fed directly into Stage 3 (tests whether validation catches an unsupported/overconfident claim)

```
VERIFIED FACTS (Stage 1 output, for the TI-1 scenario above):
{"facts": [
  {"field": "support_ticket_volume_trend", "value": "higher this quarter, exact figure not pulled", "source_quote": "higher support ticket volume this quarter, up noticeably, though nobody's pulled the exact number yet"},
  {"field": "avg_resolution_time_change", "value": "4 hours to 7 hours over the same period", "source_quote": "average ticket resolution time has gone from 4 hours to 7 hours over the same period"}
 ],
 "ambiguous_or_unclear": ["Ops lead attributes ticket volume increase to new onboarding flow vs. Product lead attributes it to seasonality -- contradictory, unresolved, and neither is backed by actual year-over-year data"],
 "insufficient_data_fields": ["exact_ticket_volume_numbers", "year_over_year_comparison"]
}

REASONING OUTPUT TO VALIDATE (Stage 2 output -- deliberately flawed for this test):
"1. The new onboarding flow launched 2 months ago is clearly the root cause of the
support ticket increase (fact: support_ticket_volume_trend).
2. This is confirmed by the resolution time also worsening (fact:
avg_resolution_time_change), since confused new users create harder tickets to
resolve.
CONCLUSION: We recommend immediately rolling back the new onboarding flow; this is
very likely (90%+ confidence) the cause of both issues."
```
