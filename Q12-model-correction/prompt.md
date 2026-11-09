# Q12. When the Model is Wrong

## Primary Prompt

```
SYSTEM:
You previously gave a confident answer that has been flagged as incorrect. Do not
defend the original answer. Re-derive the answer from scratch.

ORIGINAL QUESTION:
{{question}}

PREVIOUS (FLAGGED-INCORRECT) ANSWER:
{{previous_answer}}

TASK:
1. ASSUMPTIONS IN PREVIOUS ANSWER: List every factual or logical assumption the
   previous answer relied on, stated explicitly (even ones that seemed obvious).
2. WEAK / LIKELY-WRONG ASSUMPTIONS: For each assumption in (1), state whether it is
   well-supported, uncertain, or likely the source of the error — and why.
3. RE-DERIVATION: Starting only from the original question (treat the previous
   answer as untrusted), work through the problem again step by step, explicitly
   building on or contradicting the flagged assumptions.
4. CORRECTED ANSWER: State the new answer clearly.
5. WHAT CHANGED AND WHY: One or two sentences on specifically what was wrong before
   and what fixed it (not a generic apology).

RULES:
- Do not simply restate the previous answer with softer hedging language — you must
  genuinely re-derive it.
- If re-derivation confirms the previous answer was actually correct, say so
  explicitly and explain why it may have been flagged incorrectly, rather than
  changing the answer just to seem responsive.
```

## Alternative Prompt Design — Blind Independent Re-Solve

Don't show the model its previous (wrong) answer at all, to avoid anchoring on it.
Solve fresh, then diff the two answers in a separate step:

**Call 1 — blind re-solve:**
```
SYSTEM:
Answer the following question from scratch, showing your reasoning.

QUESTION:
{{question}}
```

**Call 2 — comparison (only now shown the original flagged answer):**
```
SYSTEM:
Here are two independently produced answers to the same question: one was flagged as
incorrect, the other was just generated without seeing the first. Compare them and
explain any discrepancy, stating which one (if either) is correct and why.

QUESTION: {{question}}
FLAGGED-INCORRECT ANSWER: {{previous_answer}}
INDEPENDENT FRESH ANSWER: {{call_1_output}}
```

This avoids anchoring bias from seeing the flagged-wrong answer before reasoning
begins, at the cost of losing the direct diagnostic of *which specific assumption*
caused the original error (since Call 1 never examines the original reasoning).
