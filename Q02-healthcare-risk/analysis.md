# Analysis — Q2 Healthcare Risk Scenario

## a. Why this structure?

A table forces per-candidate accountability: every diagnosis must carry its own
evidence AND its own reason for doubt, which structurally prevents the model from
presenting one "winner." Separating "risky assumptions" from "unknowns" distinguishes
*inference the model is making right now* (dangerous, must be flagged) from *data
that simply doesn't exist yet* (an information request, not an error).

## b. Why this prompting technique?

Zero-shot with an explicit rubric and a fixed vocabulary (Low/Medium/High only).
Few-shot is deliberately avoided here: in a safety-critical domain, worked diagnostic
examples risk anchoring the model toward the example's *conditions* and *confidence
calibration* rather than genuinely reasoning from the new, unique patient note. This
is a case where few-shot's "pattern demonstration" benefit is outweighed by the
anchoring risk — we don't want every differential to subconsciously resemble the
worked example's disease.

## c. Failure modes prevented

- False certainty / liability exposure from a confidently stated single diagnosis.
- Fabricated lab values or history not present in the note.
- Ignoring the poor quality/ambiguity of the input instead of calling it out.
- Treating the assistant's output as if it had clinical authority.

## What the test runs show

- **Run 1** shows the prompt successfully extracting a usable differential from a
  genuinely messy, inconsistent note, while explicitly flagging the two real
  ambiguities (symptom onset timeline, smoking status) rather than silently resolving
  them one way.
- **Run 2** is the key stress test for "no definitive diagnosis allowed" and "must
  include unknowns": with only "patient tired" to go on, the model correctly refuses
  to produce a differential at all, rather than defaulting to a generic fatigue
  work-up list to appear thorough. This is the most likely failure mode for a
  healthcare prompt without this rule.
- **Run 3** surfaces something more dangerous than ordinary ambiguity: a documented
  penicillin allergy alongside an active amoxicillin prescription. The prompt's
  "risky assumptions flagged" section correctly elevates this as a safety-critical
  contradiction rather than treating it as routine note noise — this is the clearest
  demonstration that the structure does more than organize text; it actively
  surfaces the single most important issue in the input.

## Comparison with the alternative (self-check second pass)

The primary prompt produces a complete, safety-conscious differential in one pass.
The alternative second-pass design catches a different class of error: *subtle
overconfidence* that survives the first pass's own rules (e.g., a Medium-confidence
candidate that should really be Low once a "cannot miss" alternative is considered).
In Run 1, for example, a second-pass reviewer might reasonably ask whether a
pulmonary embolism should appear in the differential given exertional dyspnea, even
at Low confidence — something the first pass did not include. For high-stakes notes,
running both stages is the safer design; for routine triage support, the primary
prompt alone is likely sufficient.
