# Analysis — Q11 Prompt Evaluation Framework

## a. Why this structure?

A rubric table with named dimensions forces consistent, comparable scoring across
many prompts (useful for batch review of a prompt library). Capping feedback to one
"top improvement" keeps the output actionable instead of a wall of vague comments.

## b. Why this prompting technique?

Zero-shot rubric-anchored evaluation (the LLM-as-judge pattern), with explicit anchor
criteria for what a "5" requires. Anchoring reduces the score variance that's a
well-known weakness of unguided LLM judges.

## c. Failure modes prevented

The evaluator accidentally *executing* the prompt it's supposed to be judging;
inconsistent/arbitrary scoring with no fixed scale anchor; and injection via a
malicious "prompt under evaluation" that tries to instruct the evaluator itself.

## What the test runs show

- **Run 1** (a deliberately weak prompt) confirms the evaluator never attempts to
  actually perform the summarization task — it stays strictly in evaluation mode and
  scores low on both dimensions with specific, concrete justifications rather than
  vague ones.
- **Run 2** acts as a positive control, reusing Q1's own well-designed prompt from
  this repository. The evaluator correctly awards the maximum score per the stated
  5-requires-format-and-failure-handling rule, and — notably — still finds one real,
  non-trivial gap (no explicit defense against an injected instruction inside the
  client paragraph) rather than either over-praising a good prompt into "no
  improvements possible" or inventing a nitpick to seem thorough.
- **Run 3** is the most important test: a prompt under evaluation that embeds a
  direct instruction trying to force the evaluator into giving it perfect scores.
  The evaluator correctly refuses to comply, and — going further than simple
  refusal — treats the manipulation attempt itself as evidence of a Robustness
  failure, which is exactly the intended behavior per the last rule in the prompt.

## Comparison with the alternative (calibration few-shot)

The primary, rubric-only prompt produced internally consistent, well-justified scores
across all three test prompts without needing worked examples, including correctly
distinguishing the deliberately weak prompt (Run 1) from the deliberately strong one
(Run 2). The calibration few-shot alternative would likely matter more in a setting
where *absolute* score comparability across many different human reviewers and model
runs is required (e.g., a formal prompt-library audit where scores feed into a
go/no-go decision) — the worked gold-standard examples reduce the chance that two
different evaluation runs interpret "Clarity = 3" differently. For a single ad hoc
review, as tested here, the rubric-only primary prompt is sufficient.
