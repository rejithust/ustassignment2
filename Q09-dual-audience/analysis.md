# Analysis — Q9 Dual Audience Problem

## a. Why this structure?

A fixed dual-header output contract turns "adjust based on audience" from an
ambiguous instruction the model might resolve by guessing *which* audience is "the"
audience, into a deterministic behavior: always produce both, with explicit,
different depth/length rules for each.

## b. Why this prompting technique?

Zero-shot, single-pass conditioned generation. This is primarily an
output-contract/formatting technique rather than a reasoning technique — CoT isn't
the lever here; the fix is specifying the dual-mode output structure precisely.

## c. Failure modes prevented

Audience drift (jargon leaking into the business section, or over-simplification in
the technical one); inconsistent conclusions between the two sections — a known
failure when generating long dual outputs in one pass; and the model asking a
clarifying question ("which audience do you want?") instead of satisfying the "no
separate prompts" constraint.

## What the test runs show

- **Run 1** confirms the core contract on a case with real technical depth: the
  technical section keeps implementation-level detail (lock granularity, timing
  window), the business section compresses to under 150 words with inline jargon
  definitions, and both reach the identical bottom line.
- **Run 2** is the most important test: an *unresolved* technical issue. Both
  sections correctly preserve the genuine uncertainty (no root cause yet, no fix
  timeline) rather than one section accidentally implying more resolution than the
  other — this is exactly the "factually consistent, never a different conclusion"
  rule being exercised under pressure, since it would be easy for a business summary
  to over-simplify "we don't know yet" into a falsely reassuring "it's handled."
- **Run 3** shows the opposite pressure: input with almost no technical substance.
  The technical section does not manufacture detail to look thorough; it instead
  correctly characterizes the evidence as anecdotal and suggests what measurement
  would strengthen it, and the business section mirrors that same caveat rather than
  overstating certainty.

## Comparison with the alternative (parameterized routing)

All three runs above produced both sections every time, as required. The
parameterized-routing alternative doesn't change this default behavior — it adds an
opt-in single-audience mode for calling systems that only need one output at a time
(e.g., an internal tool that already knows the viewer's role and would otherwise
discard half the response). For the assignment's literal constraint ("no separate
prompts allowed," always produce both), the primary prompt is the direct answer; the
routing variant is a practical enhancement worth keeping in mind for a production
system where paying for and discarding the unused half of the response on every call
would be wasteful.
