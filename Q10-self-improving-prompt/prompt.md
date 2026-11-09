# Q10. Self-Improving Prompt

## Primary Prompt

```
SYSTEM:
You will answer the user's question, then critique your own answer, then produce one
improved final answer. You get exactly ONE critique-and-revise cycle — do not iterate
further.

USER QUESTION:
{{question}}

OUTPUT FORMAT (all three sections required, in order):

1. DRAFT ANSWER
   Your first-pass answer to the question.

2. SELF-CRITIQUE (max 3 bullet points, each ≤ 20 words)
   Identify concrete weaknesses only: factual gaps, unsupported claims, missing
   caveats, logical issues. Do not critique style/tone. If there is no real
   weakness, write "No material weakness found" — do not invent a critique just to
   fill space.

3. FINAL ANSWER
   A revised answer fixing only the issues raised in section 2. If section 2 found
   no weakness, FINAL ANSWER may simply restate DRAFT ANSWER.

HARD RULES:
- Exactly one critique cycle. Never critique the FINAL ANSWER a second time.
- SELF-CRITIQUE must never exceed 3 bullets.
- Output nothing after FINAL ANSWER.
```

## Alternative Prompt Design — Externally Controlled Loop

Instead of trusting the model to honor "exactly one cycle" in-context, have an
orchestration layer make three *separate* calls, with the stopping rule enforced in
code rather than in the prompt text:

```
CALL 1 — DRAFT:
SYSTEM: Answer the following question directly.
USER QUESTION: {{question}}

CALL 2 — CRITIQUE (fed only the draft, not told it will be revised):
SYSTEM: Critique the following answer for factual gaps, unsupported claims, missing
caveats, or logical issues only — not style or tone. Max 3 bullets. If there is no
material weakness, respond with exactly: "No material weakness found."
ANSWER TO CRITIQUE: {{call_1_output}}

CALL 3 — REVISE (only invoked if Call 2 did not return "No material weakness found"):
SYSTEM: Revise the following answer to address only the critique points listed. Do
not introduce new changes beyond what the critique raised.
ORIGINAL ANSWER: {{call_1_output}}
CRITIQUE: {{call_2_output}}
```

Orchestration code enforces: at most one Call 2 and one Call 3 per question; if Call
2 returns "No material weakness found", Call 3 is skipped entirely and Call 1's
output is returned as final. This is more robust than relying purely on the model's
compliance with an in-prompt stopping rule, since the loop simply cannot run again —
there is no "cycle 2" call for the code to make.
