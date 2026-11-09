# Analysis — Q12 When the Model is Wrong

## a. Why this structure?

Forcing an explicit enumeration of assumptions *before* re-deriving prevents the
common failure where a model "apologizes" and produces a superficially different
answer without actually fixing the underlying reasoning error.

## b. Why this prompting technique?

Zero-shot structured chain-of-thought re-derivation — a "self-correction via
assumption audit" pattern. The failure here is a reasoning error, not a formatting
one, so exposing and re-examining the chain of logic (rather than just asking for a
nicer-worded answer) is the actual fix.

## c. Failure modes prevented

Superficial correction (same answer, more hedging words); sycophantic flip-flopping
— changing the answer just because it was flagged, even when the original was right;
and repeating the same hidden assumption that caused the original error.

## What the test runs show

- **Run 1** is the clean positive case: a genuine, well-defined math error (applying
  a discount percentage to the wrong base). The assumption audit correctly isolates
  the exact flawed step, and the re-derivation produces the mathematically correct
  answer with an explanation that matches the real arithmetic fix rather than a
  generic "I made an error" statement.
- **Run 2** is the most important test in this set: the "flagged incorrect" answer
  was, in fact, completely correct (sqrt(2) is irrational — a well-established proof).
  The prompt's explicit sycophancy guard ("if re-derivation confirms the previous
  answer was actually correct, say so explicitly... rather than changing the answer
  just to seem responsive") correctly prevents the model from inventing a false
  "correction" just because it was told the answer was wrong. This is the failure
  mode most prompts in this category get wrong by default — an unguarded correction
  prompt will often comply with the flag regardless of truth.
- **Run 3** shows a subtler, arguably more realistic failure mode: the previous
  answer wasn't factually incorrect so much as it silently resolved a genuine
  ambiguity (which "Georgia") without surfacing that choice. The re-derivation
  correctly treats "the question itself was ambiguous and this wasn't flagged" as the
  real issue, rather than simply swapping in the other reading as if that alone were
  now the single correct answer.

## Comparison with the alternative (blind independent re-solve)

The primary prompt's assumption-audit approach was directly tested against a case
where blind anchoring could plausibly have caused trouble (Run 2): since the model
saw the "flagged incorrect" label, there was real risk it would defer to the flag.
It did not, in this run, correctly reasoning independently despite seeing the label.
The blind re-solve alternative sidesteps this risk entirely by never showing the
model the flagged answer during its own solving pass, which is a structurally
stronger guarantee against anchoring — but it loses something Run 1 demonstrates
clearly: the ability to name the *exact* flawed assumption in the original reasoning,
since a blind re-solve never examines the original reasoning at all, only the final
answers. For cases where diagnosing *why* the original answer was wrong matters (e.g.,
for learning or process improvement), the primary prompt's design is more valuable;
for cases where only the correct final answer matters and anchoring risk is a larger
concern, the blind re-solve alternative is safer.
