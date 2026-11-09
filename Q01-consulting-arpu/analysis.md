# Analysis — Q1 Consulting Case

## a. Why this structure?

Splitting the response into FACTS / CONTRADICTIONS / HYPOTHESES / GAPS / NEXT STEPS
forces the model into separate cognitive buckets. A hypothesis can never silently
become a "fact" because it lives in its own section with a mandatory evidence trace.
The contradictions section exists purely because the brief says the input may contain
conflicting stakeholder opinions — surfacing them explicitly, rather than silently
picking a side, is the safe behavior for a consultant.

## b. Why this prompting technique?

This is **zero-shot, schema-constrained instruction prompting** rather than few-shot.
The extraction/hypothesis task is generic across telecom (and other) clients;
providing worked examples risks anchoring the model to the *specific* hypotheses in
the example rather than reasoning from the actual input (a known few-shot failure
mode: category/content anchoring). A light internal chain-of-thought is implied
("identify facts, then compare them, then reason about causes") but only the final
structured sections are surfaced — this keeps the output auditable without exposing
noisy reasoning.

## c. Failure modes prevented

- Hallucinating missing facts (numbers, time periods, causes) that sound plausible.
- Treating a hypothesis as a conclusion.
- Silently resolving a contradiction instead of flagging it.
- Skipping the "we don't know X" acknowledgment that a consultant must make before
  recommending action.

## What the test runs show

- **Run 1** confirms the core contradiction-handling behavior: the CFO-vs-Sales
  disagreement is surfaced as a pair, not resolved, and both become separate,
  evidence-linked hypotheses rather than one being treated as the "right" answer.
- **Run 2** is the most important stress test. With almost no real content, the
  "Insufficient information in input" rule visibly stops the model from inventing a
  plausible-sounding but ungrounded hypotheses deck — the single most likely failure
  mode (confident hallucination to look useful) does not occur.
- **Run 3** shows the prompt catching a *subtler* contradiction (a seasonality claim
  that conflicts with the company's own historical trend data) and correctly
  downgrading an unsourced rumor (competitor price cut) to "unverified" rather than
  promoting it to a stated fact. This is the harder case: the contradiction isn't two
  people disagreeing outright, it's one claim quietly conflicting with data stated
  elsewhere in the same paragraph.

## Comparison with the alternative (two-stage pipeline)

The single-prompt design is faster (one call) and keeps the full context together,
which helps the hypothesis step see nuance in the original wording. The two-stage
alternative in `prompt.md` trades that convenience for an auditable checkpoint: Call
1's fact list can be mechanically verified (every quote must appear in the source
text) before Call 2 is allowed to reason over it. For a one-off client memo, the
single prompt is sufficient; for a pipeline that will run across many client inputs
unsupervised, the two-stage version is the safer default because a bad extraction
can be caught before it contaminates the reasoning step.
