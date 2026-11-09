# Test Runs — Q7 Classification with Edge Cases

Each test input is run against both the zero-shot and few-shot prompts so the
difference is directly comparable.

---

## Run 1 — TI-1 (multi-issue, billing-dominant, in-distribution with the few-shot examples)

**Zero-shot output:**
```json
{"primary_category": "Billing", "secondary_category": "Device", "reasoning": "The duplicate charge and inconsistent support answers are the stated primary grievance; the app glitchiness is explicitly downplayed as not the main issue."}
```

**Few-shot output:**
```json
{"primary_category": "Billing", "secondary_category": "Device", "reasoning": "The customer explicitly states the charge issue is the main concern, with the app glitch noted as secondary."}
```

**Observation:** Both prompts agree here. This complaint closely resembles the
pattern in the few-shot examples (a dominant issue explicitly flagged as "the main
thing"), so few-shot doesn't change the outcome — it mainly sharpens the reasoning
wording slightly.

---

## Run 2 — TI-2 (sarcasm + root-cause device issue causing a billing dispute — novel edge-case type)

**Zero-shot output:**
```json
{"primary_category": "Billing", "secondary_category": "Device", "reasoning": "The complaint centers on being charged overage fees the customer believes are unjustified, caused by a device malfunction."}
```

**Few-shot output:**
```json
{"primary_category": "Billing", "secondary_category": "Device", "reasoning": "The customer's complaint is framed around unjustified overage charges; the router issue is mentioned as the cause."}
```

**Observation:** Both prompts land on the same labels here too, but for a subtly
different reason than the few-shot examples modeled: in all three few-shot examples,
the "secondary" issue is a *separate, lesser* annoyance mentioned almost in passing.
Here, the device issue is actually the *root cause* of the billing dispute, not an
unrelated lesser annoyance — a meaningfully different relationship between primary
and secondary that the few-shot examples never demonstrated. The few-shot prompt's
reasoning text doesn't capture this causal distinction any better than zero-shot
does, which supports the claim in `analysis.md` that few-shot examples don't
generalize to edge-case *types* they didn't include, even when the final label still
happens to come out the same.

---

## Run 3 — TI-3 (ambiguous, no dominant category — stress test for "Other")

**Zero-shot output:**
```json
{"primary_category": "Other", "secondary_category": null, "reasoning": "The complaint is too vague and unspecific to attribute to a single defined category; it references a plan change but names no concrete billing, network, or device issue."}
```

**Few-shot output:**
```json
{"primary_category": "Billing", "secondary_category": null, "reasoning": "The complaint follows a plan change, which is most consistent with a billing-related disruption."}
```

**Observation:** This is the key divergence, and it illustrates exactly the risk
flagged in `analysis.md`: the few-shot prompt, having seen three examples that all
resolved to a concrete category, appears biased toward forcing a concrete label
("Billing," because a plan change was mentioned) even though the complaint itself
("nothing works right," no specific issue named) is genuinely a better fit for
"Other" per the category definitions. The zero-shot prompt, relying only on the
written definitions, correctly falls back to "Other" since no concrete billing,
network, or device problem is actually described. This is a concrete demonstration of
few-shot's example set anchoring the model away from a category (here, "Other") that
simply wasn't demonstrated in any example.
