# Q13. Design a Prompting Strategy (Not Just a Prompt) — AI Assistant for Consulting Teams

A single prompt cannot safely cover data extraction, reasoning, and validation for a
consulting assistant — each stage has a different risk profile and needs its own
prompt design. This is a **three-stage pipeline**, each stage its own prompt (or
prompt family), with cross-cutting rules for hallucination control. The full
reasoning for each design choice is in `analysis.md`; this file gives the concrete
prompt skeletons for each stage.

## Stage 1 — Data Extraction Prompt (zero-shot, grounded)

```
SYSTEM:
Extract structured facts from the input below. Work ONLY from what is explicitly
present. For every extracted field, include a direct quote or close paraphrase of
the source sentence it came from. Do not infer, estimate, or fill in any value that
is not stated.

INPUT:
{{client_document_or_transcript}}

OUTPUT (JSON):
{
  "facts": [
    {"field": "...", "value": "...", "source_quote": "..."}
  ],
  "ambiguous_or_unclear": ["..."],
  "insufficient_data_fields": ["..."]
}

RULES:
- Every item in "facts" must have a traceable source_quote from the input.
- If a value cannot be determined from the input, list its field name under
  "insufficient_data_fields" instead of guessing.
- Flag any internally contradictory statements under "ambiguous_or_unclear" rather
  than resolving them.
```

## Stage 2 — Reasoning Prompt (controlled chain-of-thought, default)

```
SYSTEM:
You are reasoning over VERIFIED extracted facts (not raw source text) to produce
hypotheses, scenarios, or a recommendation. Use only the facts provided below; do not
reintroduce assumptions that were flagged as insufficient or ambiguous without
explicitly noting the gap.

VERIFIED FACTS (from Stage 1):
{{stage_1_output}}

TASK: {{specific reasoning task, e.g., "generate hypotheses", "produce a scenario
comparison", "produce a structured recommendation"}}

OUTPUT: numbered reasoning steps (max {{N}}, e.g., 5), each step citing which fact(s)
above it relies on, followed by a clearly labeled conclusion/hypothesis/recommendation
section separate from the reasoning steps.

RULES:
- Every step must cite a specific fact; a step with no citation is not allowed.
- Never state a conclusion with higher confidence than the cited evidence supports.
- If the available facts are insufficient to complete the task, say so explicitly
  instead of filling the gap with a plausible-sounding guess.
```

For high-ambiguity, high-stakes decisions only, escalate to a Tree-of-Thought variant
(see the escalation pattern in `Q05-fraud-detection/prompt.md` and
`Q06-ev-market-entry/prompt.md` for worked examples of this same escalation idea
applied to fraud scoring and strategy recommendations respectively).

## Stage 3 — Validation Prompt (LLM-as-judge critic, checked against Stage 1 only)

```
SYSTEM:
You are a validation critic. You will check a REASONING OUTPUT against the VERIFIED
FACTS it was supposed to be based on — not against your own general knowledge or
assumptions.

VERIFIED FACTS (Stage 1 output):
{{stage_1_output}}

REASONING OUTPUT TO VALIDATE (Stage 2 output):
{{stage_2_output}}

TASK:
1. UNSUPPORTED CLAIMS: List any claim in the reasoning output that cannot be traced
   to a specific fact in the verified facts above.
2. OVERSTATED CONFIDENCE: List any conclusion whose stated or implied confidence
   exceeds what the cited evidence volume would justify.
3. IGNORED CONTRADICTIONS: List any item from the verified facts' "ambiguous_or_
   unclear" list that the reasoning output should have addressed but did not.
4. VERDICT: "Pass" (no issues found), "Pass with caveats" (minor issues, listed
   above), or "Escalate to human review" (unsupported claims or ignored
   contradictions found).

RULES:
- Do not re-solve the original task yourself; only check the given reasoning output
  against the given facts.
```

Deterministic, non-LLM validators (schema checks, "does this source_quote actually
appear in the input text" string checks, numeric sanity checks) run alongside this
prompt and catch purely structural errors more cheaply and reliably than another LLM
call — see `analysis.md` for the full hallucination-reduction strategy.
