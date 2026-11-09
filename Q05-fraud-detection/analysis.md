# Analysis — Q5 Financial Fraud Detection

## Decision: Chain of Thought, Tree of Thought, or controlled reasoning?

**Controlled reasoning** (a bounded, structured chain-of-thought with a fixed step
cap, mandatory data citation per step, and a confidence field decoupled from
severity).

- Open-ended CoT ("think step by step" with no bounds) is rejected because
  unconstrained free-text reasoning is exactly what produces "storytelling
  hallucination" — the model fabricates a coherent-sounding narrative about customer
  intent that isn't actually derivable from a transaction summary.
- Tree of Thought is rejected as the *default* here because it's designed for
  exploring and comparing multiple competing hypotheses with backtracking — overkill
  and expensive for routine pattern-matching against known fraud typologies, where a
  single, boundable reasoning chain is sufficient and more auditable.
- Controlled/structured reasoning gives the transparency benefit of CoT (steps are
  visible and checkable) while adding the constraints a compliance-sensitive,
  auditable domain needs: a step cap, forced citations, and a severity/confidence
  split that prevents a scary-looking pattern from being reported as if it were
  certain.

## Failure modes prevented

Overconfidence (hard rule gating 90+ scores behind corroboration), storytelling
hallucination (ban on narrative, step cap, citation requirement), and conflating
"looks bad" with "is certain" (confidence judged on evidence volume, not pattern
severity).

## What the test runs show

- **Run 1** (textbook structuring pattern) demonstrates the overconfidence cap
  directly: despite three clean corroborating signals, the score lands at 82, not
  90+, because the rule requires even stronger (e.g., cross-account) corroboration
  for the top band — the model doesn't "round up" just because the pattern looks
  obviously bad.
- **Run 2** (ambiguous business cash flow) is the most important test for
  storytelling hallucination in the *other* direction: a surface-similar pattern to
  structuring is correctly down-scored to Medium because the account's tenure and
  payee history argue against fraud, and the model does not invent a laundering
  narrative just because the raw numbers superficially resemble Run 1. This is
  exactly the kind of case that would tempt an unconstrained CoT prompt into a
  dramatic but unsupported story.
- **Run 3** (single transaction, no baseline) shows the "Insufficient data" fallback
  firing correctly — the model declines to assign any numeric score rather than
  defaulting to a arbitrary low number, which would otherwise falsely imply the input
  was actually evaluated against a baseline that doesn't exist.

## Comparison with the alternative (escalation to Tree-of-Thought)

Across these three test cases, the controlled-reasoning prompt alone handled both the
clear-cut case (Run 1) and the ambiguous case (Run 2) adequately — it reasoned about
the competing "structuring vs. legitimate business" explanation internally within its
step cap. The ToT alternative would add the most value on a case like Run 2 if it
had *multiple* plausible non-fraud explanations with conflicting evidence (e.g., two
different registered business types claiming the same account, or additional data
suggesting both heavy cash business AND a recent ownership change) — situations where
weighing genuinely competing hypotheses side-by-side, rather than through one linear
chain, would surface a conclusion the single-pass version might miss. For the
transaction volumes typical of an automated screening pipeline, reserving ToT for
escalated Medium/High-risk clusters (as the alternative proposes) is the more
cost-effective design than running it on everything.
