# Analysis — Q13 Prompting Strategy (Final Challenge)

A single prompt cannot safely cover data extraction, reasoning, and validation for a
consulting assistant — each stage has a different risk profile and needs its own
prompt design. The strategy is a **three-stage pipeline**, each stage a distinct
prompt (or prompt family), plus cross-cutting rules for hallucination control.

## Stage 1 — Data Extraction

- **Goal:** pull structured facts from messy inputs (client documents, call
  transcripts, spreadsheets).
- **Technique:** zero-shot with a strict output schema (JSON) and a "do not infer"
  rule, as in `Q01-consulting-arpu` / `Q02-healthcare-risk`. Few-shot is reserved for
  narrow cases where a single client's source documents follow a highly
  idiosyncratic, recurring template worth demonstrating — otherwise zero-shot, to
  avoid overfitting extraction patterns to examples that won't represent the next
  client's documents.
- **Reasoning:** suppressed. Extraction is grounding/pattern-matching, not judgment;
  exposing chain-of-thought here adds hallucination surface area without benefit.
  Run 2 in `test-runs.md` demonstrates this directly on a clean contract-data input.
- **Hallucination control:** require a source-span citation/quote for every
  extracted field; mandate an "insufficient data" fallback; pair with a
  deterministic schema/regex validator that rejects any output field that can't be
  traced back to the input text.

## Stage 2 — Reasoning

- **Goal:** turn extracted facts into hypotheses, scenarios, and recommendations
  (the kind of work done in `Q01`, `Q05`, `Q06`).
- **Technique:** explicit, **bounded/controlled chain-of-thought** (numbered steps,
  step cap, each step tied to a cited fact) for the large majority of tasks. Reserve
  **Tree-of-Thought** / multi-branch exploration only for high-ambiguity, high-stakes
  decisions (market entry, M&A, crisis response) where several competing hypotheses
  genuinely need to be weighed against each other — see the escalation patterns in
  `Q05-fraud-detection/prompt.md` and `Q06-ev-market-entry/prompt.md`. ToT is too
  expensive and unnecessary for routine, well-structured reasoning like
  classification or standard risk scoring.
- **When to enforce visible reasoning vs. suppress it:**
  - *Enforce* visible, structured reasoning whenever the task is judgment-based,
    ambiguous, or will be questioned/audited by a client — so each step can be
    defended — and whenever there are safety/compliance stakes (fraud, healthcare,
    legal, financial).
  - *Suppress* (produce only the final structured answer, hide the scratch
    reasoning, or skip the stage entirely) whenever the task is routine
    extraction/classification, the output is consumed programmatically, or showing
    raw reasoning increases hallucination risk without adding decision value. Run 2
    shows this concretely: Stage 2 is skipped altogether for a 4-field contract
    extraction, since there is no judgment call to reason about.
- **Few-shot vs. zero-shot:** use few-shot when there's a small, stable,
  representative set of edge cases demonstrating a judgment call that's hard to
  state as a rule (`Q07`-style classification). Use zero-shot with explicit decision
  criteria when the task is novel per engagement and worked examples would
  anchor/overfit the model to irrelevant specifics (`Q06`-style strategic questions,
  which differ by client and market every time).

## Stage 3 — Validation

- **Goal:** catch hallucination, bias, unaddressed contradictions, and
  overconfidence before output reaches a human consultant or client.
- **Technique:** a separate LLM-as-judge "critic" prompt (`Q11`-style) that checks
  Stage 2's output *against Stage 1's extracted facts only* — flags any claim that
  can't be traced to an extracted fact, flags confidence language not justified by
  evidence volume, flags any contradiction from Stage 1 that Stage 2 silently
  ignored.
- Prefer **deterministic, non-LLM validators** wherever possible (schema validation,
  numeric sanity checks, "does this citation exist in the source" checks) — cheaper
  and more reliable than another LLM pass for purely structural correctness. Reserve
  the LLM-based critic for semantic/judgment correctness (is the conclusion
  well-supported, is the tone appropriate, is residual bias present).
- **Human-in-the-loop gate:** any output carrying a Low-confidence flag, an
  unresolved contradiction, or a missing-data note is routed to human review before
  client delivery, rather than auto-published.

## What the test runs show

- **Run 1** (full pipeline, end to end) shows the three stages working together
  correctly on realistic, messy meeting notes: the ops-vs-product contradiction is
  flagged at Stage 1, Stage 2 declines to pick a side given the unresolved
  contradiction and missing data, and Stage 3 confirms nothing needs escalation.
- **Run 2** demonstrates the "when to suppress reasoning" decision concretely — a
  clean, unambiguous extraction task correctly skips Stage 2 entirely, which is
  itself part of the strategy, not an afterthought.
- **Run 3** is the most important demonstration in this repository: a deliberately
  constructed, plausible-looking Stage 2 output that quietly converts an
  explicitly-flagged, unresolved two-sided contradiction into a confident, invented,
  90%+-confidence causal recommendation. Stage 3's validation prompt — checking
  strictly against Stage 1's facts, never against its own assumptions — catches both
  the unsupported causal claim and the overstated confidence, and correctly escalates
  to human review. This is a direct, concrete demonstration of why a three-stage
  pipeline with an independent validation stage is systematically safer than a single
  prompt (or even a two-stage extract-then-reason pipeline) that has no check between
  "the model reasoned about it" and "the output reaches the client."

## Cross-cutting: systematic hallucination reduction

- Ground every extracted fact in a cited source span, checked at Stage 1.
- Keep fact / hypothesis / inference in separate, labeled buckets at every stage —
  never let them blend into one undifferentiated paragraph.
- Make "insufficient data" / "unknown" an always-available, required output option
  at every stage, so the model is never forced to fill a gap with a guess.
- Calibrate confidence to evidence volume, not to how convincing the language
  sounds; ban false-precision (e.g., arbitrary percentages) unless justified by
  actual data — Run 3's "90%+ confidence" is exactly the kind of false precision
  this rule is designed to catch.
- Treat Stage 2's output as untrusted input to Stage 3 — validation checks it
  against Stage 1's grounded facts, never against the model's own unexamined
  assumptions.
- Bound reasoning steps by default (no open-ended chains) to limit drift and
  speculation; only escalate to expensive, unbounded multi-branch reasoning (ToT) for
  the small subset of explicitly high-stakes, high-ambiguity cases.
