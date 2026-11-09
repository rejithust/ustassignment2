# Analysis — Q4 Toxic + Biased Input Handling

## a. Why this structure?

Requiring an explicit "bias detected" section before the rewrite makes the
de-biasing auditable — a reader can see exactly what was changed and why, instead of
trusting a silent rewrite. Section 4 stops the model from accidentally "laundering"
an opinion into something that reads as an established fact just because the charged
language was removed.

## b. Why this prompting technique?

Zero-shot with an explicit role and rubric is the primary design. This task benefits
from a light one-shot anchor (the alternative design) because "neutralize without
erasing" is a subtle behavioral line that's easier to demonstrate than fully specify
in rules — hence the alternative's single worked example.

## c. Failure modes prevented

- Over-refusal/over-censorship that discards legitimate business content along with
  the bias.
- Silently perpetuating bias by rewriting it without ever flagging it.
- Conflating a neutralized opinion with a verified fact.
- Re-inflaming the issue by repeating toxic language more than necessary.

## What the test runs show

- **Run 1** (the assignment's own gender-bias example) confirms the core behavior:
  both biased phrases are quoted and labeled, the real business claim (missed
  timeline, stated reason) survives the rewrite, and the dismissive "Typical." is
  removed as inflammatory rather than treated as content worth preserving.
- **Run 2** shows the prompt correctly handling *emotional* charge that is not
  demographic bias — it still runs the full four-section process, and critically it
  does not discard the legitimate signal (three prior vendor incidents) just because
  it was expressed angrily. This guards against an over-aggressive filter that would
  throw out real business information along with the tone.
- **Run 3** is the important over-triggering check: on input with only mild,
  borderline language and no real demographic bias, the model doesn't manufacture
  findings to pad section 2, and section 4 correctly reports nothing remains
  unverified, since the statement already explains its own root cause. This confirms
  the prompt calibrates to the actual severity of what's in the input rather than
  applying a fixed amount of "neutralization" regardless of need.

## Comparison with the alternative (one-shot calibration example)

The primary, rules-only prompt performed consistently well across all three test
inputs, including the edge cases, which suggests the explicit rubric (four sections,
explicit "do not refuse" and "do not invent a counter-claim" rules) is often enough
on its own. The one-shot alternative would likely sharpen consistency further across
a very large, varied batch of inputs — particularly in matching the *tone* of the
rewrite (e.g., exact phrasing patterns like "the stated reason given was...") across
many different analysts or runs — but risks subtly anchoring every rewrite toward the
specific example's sentence structure. For a single analyst processing occasional
inputs, the rules-only primary prompt is sufficient; for a high-volume, multi-team
pipeline where rewrite consistency across outputs matters, the one-shot version is
the better default.
