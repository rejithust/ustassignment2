# Test Runs — Q6 Strategy Recommendation Under Uncertainty

---

## Run 1 — Primary prompt × TI-1 (no context provided, edge case)

```
1. DECISION CRITERIA
- Market growth trajectory (EV adoption curve in India) — determines whether early
  entry captures disproportionate share or whether waiting loses little.
- Regulatory/policy stability (subsidies, emission norms) — directly affects
  near-term economics of entry.
- Capital intensity of chosen approach — determines affordability and risk exposure.
- Client's existing capabilities/assets — determines cost and speed of entry.
- Competitive density at entry time — determines achievable share and pricing power.
- Time-to-breakeven — determines how long capital is at risk before returns appear.

ASSUMPTION FLAGGED: No information about the client (industry, existing assets,
capital position, or risk appetite) was provided. All six criteria above are listed
generically; sections 3-6 below necessarily remain generic and conditional until
client-specific context is supplied.

2. SUB-DECISIONS
(a) Vehicle segment — 2-wheeler / 4-wheeler / commercial — cannot be addressed
    without knowing the client's existing business.
(b) Manufacturing approach (build / import-assemble / JV) — depends on client
    capital and capability, not provided.
(c) Timing (now vs. wait for infrastructure maturity) — depends on client's risk
    appetite, not provided.

3. SCENARIO ANALYSIS
Three generic scenarios are sketched below, but none can be meaningfully scored
against the criteria above without knowing who the client is:
- Aggressive early entry: assumes strong capital access and high risk tolerance.
- Partnered phased entry: assumes moderate capital, risk-averse posture.
- Wait-and-see: assumes limited capital or capability, or crowded near-term
  competitive field.

4. COUNTERARGUMENTS
Cannot be meaningfully produced — a counterargument to "which scenario to lean
toward" requires first knowing which scenario actually fits this specific client.

5. STRUCTURED RECOMMENDATION
No recommendation is given. Insufficient information in input: the client's
industry, existing capabilities, capital position, and risk appetite are all
unstated, and any recommendation given now would be generic rather than genuinely
tailored advice.

6. CONFIDENCE & CAVEATS
Confidence: N/A (no recommendation made). The single highest-value piece of
information needed before this question can be meaningfully answered: who the client
is and what assets/constraints they bring to a potential entry.
```

**Observation:** This is the central edge-case test for Q6 — "Do not state a
recommendation before sections 1-4 are complete" combined with "state the assumption
explicitly rather than silently picking one" correctly produces a memo that stops
short of a real recommendation given a genuinely contextless question, rather than
inventing a plausible-sounding generic client profile to proceed.

---

## Run 2 — Primary prompt × TI-2 (established 2-wheeler ICE manufacturer, low risk appetite)

```
1. DECISION CRITERIA
- Leverageable existing assets (dealer network, brand) — client already has this for
  2-wheelers, which lowers go-to-market cost if segment is kept the same.
- Capital intensity vs. stated moderate reserves — must fit within what the client
  can actually fund.
- Risk tolerance given stated history — client explicitly flagged low risk appetite
  after a prior difficult expansion; this weighs heavily on scenario choice.
- In-house EV technical capability (currently none) — determines build-vs-partner
  choice.
- Competitive timing in the 2-wheeler EV segment specifically.

2. SUB-DECISIONS
(a) Segment: 2-wheeler is the natural fit given existing dealer network and brand
    recognition; 4-wheeler/commercial would require assets the client doesn't have.
(b) Manufacturing approach: given no in-house EV component expertise, a joint
    venture or component-sourcing partnership is more realistic than fully in-house
    build.
(c) Timing: given stated low risk appetite, a phased entry rather than a first-mover
    push is more consistent with the client's stated posture.

3. SCENARIO ANALYSIS
- Aggressive early entry (in-house build, fast launch): Outcome — fastest potential
  share capture; Risk — capital intensity and technical-capability gap directly
  conflict with stated low risk appetite and lack of EV expertise. Poor fit.
- Partnered phased entry (JV/component sourcing, leverage existing dealer network):
  Outcome — moderate speed, shares technical risk with a partner, uses the client's
  real asset (distribution); Risk — margin-sharing with partner, dependency on
  partner's technology. Good fit given stated constraints.
- Wait-and-see (delay entry pending clearer policy/infrastructure signals): Outcome —
  avoids near-term capital risk; Risk — competitors may capture dealer and brand
  mindshare first, eroding the client's main existing advantage over time.

4. COUNTERARGUMENTS (against Partnered phased entry, the likely lean)
- A JV dilutes long-term control and margin in a business the client has historically
  run independently.
- If the chosen partner's technology underperforms, the client's brand is exposed to
  a partner's product risk.
These are not fully outweighed by the fit with stated risk appetite — they should be
treated as conditions to negotiate around (e.g., exit clauses, technology escrow)
rather than reasons to abandon the approach.

5. STRUCTURED RECOMMENDATION
Recommend Partnered phased entry in the 2-wheeler EV segment, leveraging the
existing dealer network, because it is the only scenario consistent with both the
client's stated low risk appetite and its real competitive asset (distribution),
while addressing the capability gap (criteria: risk tolerance, capital intensity,
leverageable assets, in-house capability).

6. CONFIDENCE & CAVEATS
Confidence: Medium. The client's actual capital reserve figure was not disclosed
("moderate," not quantified); a specific budget figure would most change this
recommendation, particularly whether it rules out a JV requiring meaningful upfront
investment.
```

**Observation:** With real context, the memo produces a genuine, criteria-linked
recommendation, and correctly treats the "moderate capital reserves" vagueness as a
stated caveat in section 6 rather than silently assuming a number.

---

## Run 3 — Primary prompt × TI-3 (internal tension: aggressive target vs. self-funding only)

```
1. DECISION CRITERIA
- Achievability of the 20%-in-3-years target under a strict no-external-capital
  constraint — this tension is the central criterion for this client.
- Capital intensity of each entry approach relative to current operating cash flow
  (figure not disclosed).
- Speed-to-revenue of each approach, given the 3-year target window.
- Risk of under-delivery if the self-funding constraint caps achievable scale.

2. SUB-DECISIONS
(a) Segment choice should favor the lowest capital-intensity option achievable
    within cash-flow-only funding, since segment economics vary widely.
(b) Manufacturing approach: a fully self-funded build is unlikely to reach 20%
    revenue share in 3 years without external capital; this needs to be tested
    explicitly rather than assumed, since the target and the funding constraint may
    be in tension.
(c) Timing: an aggressive 3-year target argues for immediate action, but
    self-funding-only limits how aggressively the client can move at once.

3. SCENARIO ANALYSIS
- Aggressive early entry, self-funded: Outcome — fastest start, but cash-flow-only
  funding likely caps scale well below what's needed for 20% share in 3 years; high
  risk of missing the board's own target.
- Partnered phased entry, self-funded for the client's share only: Outcome — shares
  capital burden with a partner, improving odds of reaching scale within the funding
  constraint; more realistic path to the stated target than going fully alone.
- Wait-and-see: Outcome — directly conflicts with the board's 3-year target; not a
  realistic option given the explicit timeline pressure from leadership.

4. COUNTERARGUMENTS (against Partnered phased entry)
- Sharing revenue/margin with a partner could itself make hitting a 20%-revenue
  target harder, not easier, depending on deal structure — this is a real tension
  that is not automatically resolved by partnering.
- This is not fully outweighed; it should be explicitly modeled (does a JV's
  consolidated or attributed revenue count toward the 20% target?) before finalizing.

5. STRUCTURED RECOMMENDATION
Recommend Partnered phased entry, but flag to the client that the stated 20%-in-
3-years target and the no-external-capital constraint may be in direct tension
regardless of entry approach chosen — this should be resolved with the board/CFO
before committing to either the target or the funding constraint as fixed.

6. CONFIDENCE & CAVEATS
Confidence: Low. This is lower than Run 2's Medium specifically because the input
itself contains an unresolved internal contradiction (ambitious target vs. a hard
funding limit) that no entry-strategy choice alone can fully resolve. The single most
important next input: clarification from the client on which constraint (the target
or the funding limit) is more flexible if both cannot be satisfied simultaneously.
```

**Observation:** This run is the key test for handling a genuine strategic
contradiction embedded in the client's own stated goals (not just an EV-market
ambiguity). The prompt's "state the assumption explicitly" and "show your work"
rules correctly surface the target-vs-funding tension as a first-class issue in
sections 1, 3, and 6, rather than quietly producing a confident recommendation that
ignores the conflict.
