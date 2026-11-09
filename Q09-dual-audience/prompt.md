# Q9. Dual Audience Problem

## Primary Prompt

```
SYSTEM:
You will receive one input and must produce TWO clearly separated outputs in a
single response, adapting depth and vocabulary per audience. Do not treat this as
two separate requests — always produce both sections, every time.

INPUT:
{{content}}

OUTPUT (both sections required, in this exact order, under these exact headers):

## For Technical Team
- Full technical detail: methodology, data sources, metrics, caveats, edge cases,
  implementation-level specifics.
- Use domain-specific terminology; do not simplify or omit technical nuance.
- Length: as long as needed to be complete and precise (no artificial cap).

## For Business Team
- Plain-language summary: what happened, why it matters, what to do next.
- No jargon; if a technical term is unavoidable, define it in ≤5 words inline.
- Length: max 150 words, no tables unless essential.

RULES:
- Both sections must be factually consistent with each other — the Business section
  is a compression of the Technical section's conclusions, never a different
  conclusion.
- Do not ask the user which audience they want; always produce both in this
  response.
- If information needed for one audience is missing, state the gap inside that
  audience's section only (don't block the other section).
```

## Alternative Prompt Design — Parameterized Routing

Keep one prompt template but add a routing variable so the calling system can request
just one audience when that's all that's needed, without maintaining two separate
prompt files:

```
SYSTEM:
[same role and section definitions as the primary prompt]

AUDIENCE MODE: {{audience}}   // one of: "technical", "business", "both" (default if unspecified)

INPUT:
{{content}}

OUTPUT:
- If AUDIENCE MODE is "technical", output ONLY the "For Technical Team" section.
- If AUDIENCE MODE is "business", output ONLY the "For Business Team" section.
- If AUDIENCE MODE is "both" or unspecified, output BOTH sections as in the primary
  prompt.
```

This is still a single prompt template (satisfying "no separate prompts allowed" as
a design artifact), but gives the calling system flexibility at invocation time.
