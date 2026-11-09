# Analysis — Q7 Classification with Edge Cases

## Why few-shot helps (or doesn't)

Few-shot helps precisely where the definitions alone can't disambiguate: multi-issue
complaints where the model must judge which issue is "primary" vs. "secondary." That
judgment call — "what's emphasized first/most, what's the actual unresolved
grievance" — is far easier to *demonstrate* than to state as a crisp rule, and the
examples also anchor output granularity (one-sentence reasoning, consistent JSON
shape). Run 1 shows this working as intended: on an in-distribution complaint
(explicit "main thing" framing, just like the examples), both prompts agree and the
few-shot version's reasoning is marginally sharper.

## When it breaks

- If the example set is small and unrepresentative, the model can overfit to surface
  features of the examples instead of genuinely reasoning about a new complaint's
  substance. **Run 3 demonstrates this directly:** the few-shot prompt forces a
  "Billing" label onto a genuinely vague complaint, apparently because every one of
  its three examples resolved to a concrete category — it has never been shown that
  "Other" is a valid, sometimes-correct answer. The zero-shot prompt, constrained
  only by the explicit written definitions (which do allow "Other" for unclear
  cases), gets this one right.
- It doesn't generalize to edge-case *types* not represented in the examples. **Run
  2 demonstrates this more subtly:** the complaint's primary/secondary relationship
  (a device defect *causing* a billing dispute, not just accompanying it) is a
  structurally different pattern from anything in the three few-shot examples, where
  secondary issues are always unrelated, minor annoyances. Both prompts happened to
  land on the same final label here, but neither demonstrates genuine understanding
  of the causal relationship — the few-shot examples simply didn't teach that
  pattern, for better or worse.
- As the category count or edge-case variety grows, the per-example cost (prompt
  length, latency) rises faster than the marginal disambiguation benefit. At that
  point, explicit decision rules plus a tie-breaker (e.g., "if genuinely torn,
  classify as Other") scale better than a small, unrepresentative example set — which
  is exactly the zero-shot prompt's design, and exactly why it outperformed on Run 3.

## Practical takeaway from testing

These three runs suggest a middle path worth considering in a real deployment:
keep the few-shot examples for their benefit on clearly multi-issue complaints
(Run 1-style), but explicitly add at least one "Other" example to the few-shot set
showing a vague, no-dominant-issue complaint resolving to "Other" — since the
current three-example set's complete absence of an "Other" example is plausibly the
direct cause of the Run 3 failure. This is a concrete, testable fix rather than
simply reverting to zero-shot for every case.
