# Analysis — Q8 Executive-Ready Output

## a. Why this structure?

"Crisp" and "action-oriented" are vague style requests a model can't reliably
self-police. Hard numeric caps (max 4 bullets, ≤15 words each, max 4 table rows)
turn a subjective style goal into an operational, checkable constraint.

## b. Why this prompting technique?

Zero-shot with hard format constraints as the primary mechanism — the caps themselves
are the enforcement, so worked examples aren't strictly required (see the few-shot
alternative for when they add more value).

## c. Failure modes prevented

Verbosity creep and hedging language typical of LLM output; unranked, undifferentiated
findings with no clear "so what"; actions with no owner or timeframe, which
executives can't act on; filler preamble that burns the reader's limited attention.

## What the test runs show

- **Run 1** confirms the core behavior on a typical case: the verbose draft's
  hedging language ("it's possible," "we haven't fully isolated") is compressed, but
  the genuine underlying uncertainty survives as an honest Key Finding rather than
  being deleted along with the verbosity — an important distinction, since cutting
  hedging words is good, but cutting the uncertainty itself would mislead the
  executive reader.
- **Run 2** is the critical edge case: input with essentially nothing to report. The
  fixed template does not force the model to manufacture four findings and four
  actions to look thorough — it collapses honestly to one line per section. This
  confirms the structure enforces brevity without also forcing false
  busy-work-signaling.
- **Run 3** is a boundary check in the other direction: dense input with exactly
  four genuinely distinct issues. The caps accommodate this cleanly, and each item
  still gets compressed into the ≤15-word / owner-timeframe format rather than the
  model exceeding the caps to "fit everything in." This suggests the chosen limits
  (4 and 4) are a reasonable real-world ceiling, not an arbitrarily tight one that
  would force information loss on a normal quarter's update.

## Comparison with the alternative (before/after few-shot example)

The primary prompt's numeric caps performed well across all three test cases,
including the two edge cases, suggesting hard limits alone are often sufficient. The
few-shot alternative would likely add the most value in a case not tested here: input
whose *problem* isn't length but tone — e.g., a draft that is already short but still
reads as hedgy, academic, or passive ("results suggest a possible association")
rather than genuinely action-oriented. The worked example teaches that
transformation directly, whereas numeric caps alone only constrain length, not
voice. For most routine reporting, the primary prompt is sufficient; for a
high-visibility recurring exec brief where tone consistency across many authors
matters, pairing it with the worked example is the stronger default.
