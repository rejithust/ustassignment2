# Analysis — Q10 Self-Improving Prompt

## a. Why this structure?

Hard-capping the critique at 3 bullets and explicitly banning a second cycle solves
both named risks (verbosity, infinite loop) structurally, rather than hoping the
model self-regulates its own enthusiasm for "improving further."

## b. Why this prompting technique?

A zero-shot, single-bounded-iteration self-refine (Reflexion-style) meta-prompt —
CoT applied to self-evaluation rather than problem decomposition.

## c. Failure modes prevented

"Critique theater" (inventing fake issues to appear thorough); runaway refinement
loops that never terminate; a self-critique section that grows unboundedly and
buries the actual answer.

## What the test runs show

- **Run 1** demonstrates the intended core value: the critique catches a genuine,
  substantive gap (average- vs. worst-case complexity, and an unstated ambiguity in
  "searching") and the final answer is a real improvement, not cosmetic rewording.
- **Run 2** is the most important test in this set: a trivially correct, simple
  answer where a weaker design would pressure the model into manufacturing filler
  critique to look thorough. The explicit "No material weakness found" escape hatch
  lets the model correctly stop, confirming the "do not invent a critique just to
  fill space" rule is doing real work, not just sitting unused in the prompt text.
- **Run 3** shows the critique mechanism adding genuine safety value on a
  factually-correct-but-incomplete first answer, by surfacing missing
  mechanism/exposure-response information a reader would actually benefit from —
  demonstrating the critique step isn't just a formality even on an already-"right"
  draft answer.

## Comparison with the alternative (externally controlled loop)

Across all three runs, the in-prompt "exactly one cycle" rule was followed correctly
by the model — none of the three final answers showed evidence of a second,
unrequested critique pass. That said, the in-prompt rule is a request the model
could in principle ignore under enough pressure (e.g., a sufficiently long or
adversarial question), whereas the externally controlled loop variant makes a second
cycle structurally impossible, since the orchestration code simply does not issue a
third call unless Call 2 found a weakness. For a single interactive Q&A use case
(as tested here), the primary in-prompt design is simpler to deploy and worked
correctly in testing; for an automated, unsupervised pipeline processing many
questions without a human watching each one, the externally controlled version is
the safer default because its termination guarantee doesn't depend on the model's
continued compliance.
