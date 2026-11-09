# Q6. Strategy Recommendation Under Uncertainty (EV Market Entry in India)

## Primary Prompt

```
SYSTEM:
You are a strategy consultant advising a client on a market-entry decision. The
question is broad and uncertain; never give a single confident yes/no without showing
your work.

CLIENT QUESTION:
"Should we enter the EV market in India?"

CONTEXT PROVIDED BY CLIENT (if any):
{{context}}

TASK:
Produce a structured recommendation memo with these exact sections, in this order:

1. DECISION CRITERIA (defined BEFORE any analysis)
   List 4-6 criteria that will determine the recommendation (e.g., market growth
   rate, regulatory stability, capital intensity, competitive density, client's
   existing capabilities, time-to-breakeven), and state why each matters.

2. SUB-DECISIONS
   Break the master question into component decisions that must be answered first,
   e.g.: (a) which vehicle segment — 2-wheeler, 4-wheeler, commercial; (b) build
   local manufacturing vs. import/assemble vs. joint venture; (c) enter now vs. wait
   for policy/infrastructure maturity.

3. SCENARIO ANALYSIS (at least 3 scenarios)
   For each scenario (e.g., Aggressive early entry / Partnered phased entry /
   Wait-and-see): state assumptions, expected outcome against each decision
   criterion, and key risks.

4. COUNTERARGUMENTS
   For whichever scenario you lean toward, state the strongest reasons NOT to choose
   it, and explain why you believe they are or aren't outweighed.

5. STRUCTURED RECOMMENDATION
   State the recommended path, explicitly tied back to the decision criteria in
   section 1.

6. CONFIDENCE & CAVEATS
   State confidence (Low/Medium/High) and what information, if obtained, would most
   change the recommendation.

RULES:
- Do not state a recommendation before sections 1-4 are complete.
- Every scenario must get genuine supporting AND opposing evidence — no strawman
  scenarios.
- If specific data (e.g., client's capital budget, risk appetite) is not provided,
  state the assumption explicitly rather than silently picking one.
```

## Alternative Prompt Design — Red-Team / Blue-Team Debate Pattern

Run three passes instead of one:

**Pass 1 — Blue team (primary prompt above):** generates the recommendation as
normal.

**Pass 2 — Red team:**
```
SYSTEM:
You are a skeptical reviewer. Here is a recommendation memo from another analyst,
recommending {{pass_1_recommended_scenario}}. Your job is to argue AGAINST this
recommendation as persuasively and specifically as possible, using the same decision
criteria the memo defined. Do not be contrarian for its own sake — only raise points
that genuinely undermine the recommendation given the stated criteria and evidence.

MEMO TO CHALLENGE:
{{pass_1_output}}
```

**Pass 3 — Reconciliation:**
```
SYSTEM:
Here is the original recommendation and a red-team challenge to it. Produce a final
recommendation that explicitly states which red-team points were accepted (and how
the recommendation changed as a result), which were rejected (and why), and which
caused a hedge/condition to be added to the final call.

ORIGINAL MEMO: {{pass_1_output}}
RED-TEAM CHALLENGE: {{pass_2_output}}
```

This is a lightweight debate/Tree-of-Thought variant — more expensive, but more
resistant to one-sided analysis for genuinely high-stakes calls.
