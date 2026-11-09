# Analysis — Q6 Strategy Recommendation Under Uncertainty

## a. Why this structure?

Defining decision criteria *before* any scenario is evaluated prevents motivated
reasoning (picking a conclusion first and rationalizing backward). The mandatory
counterarguments section forces genuine engagement with the opposing view rather
than a one-sided pitch.

## b. Why this prompting technique?

Zero-shot, structured chain-of-thought via explicit decomposition (criteria →
sub-decisions → scenarios → recommendation). Few-shot doesn't help here: there's no
small, representative set of "correct" market-entry decisions to demonstrate — every
client/market combination is different, so an example would anchor the model to
irrelevant specifics (e.g., a prior example's conclusion) rather than reasoning from
this client's actual criteria.

## c. Failure modes prevented

A confident recommendation stated before the analysis is actually done; one-sided,
cherry-picked evidence (confirmation bias); hidden/implicit assumptions about budget,
risk appetite, or timeline; and an unfalsifiable recommendation with no stated
condition that would change it.

## What the test runs show

- **Run 1** (no context at all) is the most important stress test: the "do not state
  a recommendation before sections 1-4 are complete" rule, combined with the
  explicit-assumption requirement, correctly produces a memo that *stops* rather than
  inventing a plausible-sounding generic client profile just to deliver a confident
  answer. This is the single most valuable guardrail in this prompt — a weaker
  version would likely hallucinate a client profile and proceed.
- **Run 2** (real, specific context) shows the structure doing its job well: the
  recommendation is directly traceable to the stated criteria (low risk appetite,
  existing dealer network, no in-house EV capability), and the one piece of vague
  input ("moderate capital reserves") is correctly carried through to the final
  caveat rather than silently resolved into a number.
- **Run 3** (internal contradiction in the client's own goals) is the hardest case:
  an aggressive growth target stated alongside a hard no-external-capital
  constraint. The prompt surfaces this tension explicitly across criteria, scenario
  analysis, and confidence — and appropriately lowers confidence to Low rather than
  picking one of the two conflicting goals as authoritative and quietly ignoring the
  other.

## Comparison with the alternative (red-team / blue-team debate)

The primary prompt alone already builds in a counterarguments section, which did
real work in Run 2 and Run 3 (e.g., flagging JV dilution risk, flagging the
target-vs-funding tension). The debate alternative would go further by having an
independent pass argue against the *specific* recommendation with no stake in
defending it — likely to surface sharper objections than a single model playing
both roles in one pass, as in Run 3's counterarguments being somewhat tightly coupled
to the same reasoning chain that produced the recommendation. For a client memo
headed to a steering committee (genuinely high-stakes, hard-to-reverse capital
commitment), the three-pass debate design is worth the extra cost; for an internal
first-draft exploration, the primary single-pass prompt is sufficient.
