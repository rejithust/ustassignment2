# Test Runs — Q4 Toxic + Biased Input Handling

---

## Run 1 — Primary prompt × TI-1 (gender bias)

```
1. ORIGINAL CLAIM, SUMMARIZED NEUTRALLY
A stakeholder believes the project timeline slipped under the current project lead.

2. BIAS / CHARGED LANGUAGE DETECTED
- "put a woman in charge" — gender bias (implies gender is a relevant causal factor).
- "she clearly couldn't handle the pressure" — gender bias, unsupported attribution
  of failure to gender.
- "Typical." — dismissive, inflammatory closing remark reinforcing the bias.

3. NEUTRAL, OBJECTIVE VERSION
"The project missed its timeline under the current project lead. The stakeholder's
stated reason was difficulty managing rollout pressure."

4. WHAT REMAINS UNVERIFIED
The claim that leadership capability (rather than scope, resourcing, dependencies,
or external factors) caused the delay is not substantiated by the input and remains
an unverified opinion.
```

**Observation:** The model preserves the real underlying business content (project
slipped, under this lead, cited reason was pressure management) while stripping both
instances of gendered language and the dismissive closer, satisfying "does not ignore
the input completely."

---

## Run 2 — Primary prompt × TI-2 (emotional venting, no demographic bias, real signal underneath)

```
1. ORIGINAL CLAIM, SUMMARIZED NEUTRALLY
A stakeholder reports repeated reliability problems with a vendor and has lost
confidence in the vendor's communications.

2. BIAS / CHARGED LANGUAGE DETECTED
- "absolute disaster" — emotionally charged language, not demographic bias.
- "screwed us over" — emotionally charged/inflammatory phrasing.
- "I don't trust a single thing they say" — strong emotional language, though it does
  reflect a substantive trust concern.

3. NEUTRAL, OBJECTIVE VERSION
"The vendor has had three prior instances of unreliable performance or communication.
As a result, the stakeholder reports significantly reduced confidence in the
vendor's statements going forward."

4. WHAT REMAINS UNVERIFIED
The specific nature and verification of the "three times" incidents is not detailed
in the input; the count and severity should be confirmed against a record (e.g.,
incident log) rather than taken as established fact from this statement alone.
```

**Observation:** This case has no demographic bias, only emotional charge — the
model correctly still runs the full process (it doesn't skip sections just because
there's no gender/race angle) and, importantly, preserves the legitimate underlying
signal (repeated vendor reliability issues) rather than discarding it as "just
venting."

---

## Run 3 — Primary prompt × TI-3 (edge case: minimal bias/charge)

```
1. ORIGINAL CLAIM, SUMMARIZED NEUTRALLY
A new hire is still developing proficiency, and onboarding delays were primarily due
to internal documentation gaps rather than the new hire's performance.

2. BIAS / CHARGED LANGUAGE DETECTED
- "still a bit green" — mildly informal/judgmental phrasing about experience level,
  low severity; not demographic bias, borderline whether rewriting is even needed.

3. NEUTRAL, OBJECTIVE VERSION
"The new hire is still building proficiency in the role. Onboarding delays were
primarily attributable to incomplete documentation provided by the team, not to the
new hire's performance."

4. WHAT REMAINS UNVERIFIED
None — this statement already largely self-identifies the root cause (missing
documentation) as internal, so there is little unverified attribution left after
the minor rewrite.
```

**Observation:** This is the key edge-case test for "does NOT ignore the input
completely" in the opposite direction — on nearly-neutral input, the model does not
manufacture bias findings to fill out section 2, and the rewrite is only a light
touch, not an aggressive rewording of an already-reasonable statement. This confirms
the prompt doesn't over-trigger on borderline language.
