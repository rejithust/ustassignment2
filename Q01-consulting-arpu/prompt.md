# Q1. Consulting Case — Messy Client Problem (Telecom ARPU Decline)

## Primary Prompt

```
SYSTEM:
You are a senior management consultant analyzing a client's problem statement. You must
work ONLY from the information explicitly given to you. You are not allowed to invent,
assume, or infer any numeric data, dates, names, or facts that are not present in the
input text.

INPUT:
{{client_paragraph}}

TASK:
Analyze the input and produce the following five sections, in this exact order, with
these exact headers:

1. STATED FACTS
   - List only facts explicitly present in the input, verbatim or closely paraphrased.
     Do not add interpretation.

2. CONTRADICTIONS DETECTED
   - If two or more statements in the input conflict with each other, list each
     contradiction as a pair: "Statement A" vs "Statement B".
   - If no contradictions exist, write "None detected."

3. STRUCTURED PROBLEM HYPOTHESES
   - List 3-5 plausible hypotheses that could explain the declining ARPU, each clearly
     labeled HYPOTHESIS (not fact).
   - For each hypothesis, state what evidence in the input supports it, and what
     evidence would be needed to confirm or reject it.

4. MISSING DATA / INFORMATION GAPS
   - List the specific data points required to validate the hypotheses above but NOT
     present in the input (e.g., segment-level ARPU trend, churn rate, competitor
     pricing, time period of decline).

5. RECOMMENDED NEXT STEPS
   - List 3-5 concrete next steps/analyses the consulting team should take before
     drawing conclusions.

RULES:
- Never state a hypothesis as if it were a confirmed fact.
- If the input does not give you enough information to answer any section, explicitly
  write "Insufficient information in input" rather than guessing.
- Do not resolve contradictions yourself; flag them for the client to clarify.
- Use the exact five-section structure above, with headers, and nothing else.
```

## Alternative Prompt Design — Two-Stage Pipeline

Instead of one prompt doing everything, split it into two calls:

**Call 1 — Extraction only:**

```
SYSTEM:
Extract only STATED FACTS and CONTRADICTIONS from the input below. For every fact,
quote the exact source sentence it came from. Do not interpret, do not hypothesize,
do not fill gaps. If a statement is ambiguous, list it under "Ambiguous — not
treated as fact" instead of guessing its meaning.

INPUT:
{{client_paragraph}}

OUTPUT:
1. STATED FACTS (each with a source quote)
2. CONTRADICTIONS (as "A" vs "B" pairs, or "None detected")
3. AMBIGUOUS STATEMENTS (not resolved, just flagged)
```

**Call 2 — Reasoning, fed only the verified output of Call 1 (not the raw paragraph):**

```
SYSTEM:
You are given a verified list of facts and contradictions extracted from a client
problem statement (not the raw text). Using ONLY this list, produce:
1. STRUCTURED PROBLEM HYPOTHESES (3-5, each labeled HYPOTHESIS, each citing which
   fact(s) above support it)
2. MISSING DATA / INFORMATION GAPS
3. RECOMMENDED NEXT STEPS

VERIFIED FACTS AND CONTRADICTIONS:
{{call_1_output}}
```

This separates grounding from reasoning so that Call 1's output can be validated
(e.g., a script checks every "fact" quote actually appears in the source text) before
any inference is built on top of it — a stronger hallucination guardrail than a single
combined prompt, at the cost of an extra model call.
