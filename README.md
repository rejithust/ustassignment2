# UST Assignment 2 — Prompt Engineering on Real-World Scenarios

Author: Rejith Ramesh

This repository answers all 13 questions of the "Prompt Engineering on Real-World
Scenarios" assignment. Each question gets its own folder with four files:

| File | Contents |
|---|---|
| `prompt.md` | The full, copy-pasteable primary prompt, plus at least one alternative prompt design. |
| `test-inputs.md` | Synthetic test inputs used to exercise the prompt, including at least one edge case / adversarial case. |
| `test-runs.md` | Simulated runs of the prompt against each test input, showing the expected output and how guardrails behave. |
| `analysis.md` | Why this structure, why this prompting technique (zero-shot / few-shot / CoT / controlled reasoning), what failure modes are prevented, and what the test runs show. |

## Folder map

| Folder | Question |
|---|---|
| [Q01-consulting-arpu/](Q01-consulting-arpu/) | Q1 — Consulting Case: Messy Client Problem (telecom ARPU decline) |
| [Q02-healthcare-risk/](Q02-healthcare-risk/) | Q2 — Healthcare Risk Scenario |
| [Q03-prompt-injection/](Q03-prompt-injection/) | Q3 — Prompt Injection Attack |
| [Q04-toxic-biased-input/](Q04-toxic-biased-input/) | Q4 — Toxic + Biased Input Handling |
| [Q05-fraud-detection/](Q05-fraud-detection/) | Q5 — Financial Fraud Detection |
| [Q06-ev-market-entry/](Q06-ev-market-entry/) | Q6 — Strategy Recommendation Under Uncertainty (EV market entry) |
| [Q07-classification-edge-cases/](Q07-classification-edge-cases/) | Q7 — Classification with Edge Cases (zero-shot vs few-shot) |
| [Q08-executive-output/](Q08-executive-output/) | Q8 — Executive-Ready Output |
| [Q09-dual-audience/](Q09-dual-audience/) | Q9 — Dual Audience Problem |
| [Q10-self-improving-prompt/](Q10-self-improving-prompt/) | Q10 — Self-Improving Prompt |
| [Q11-prompt-evaluation-framework/](Q11-prompt-evaluation-framework/) | Q11 — Prompt Evaluation Framework |
| [Q12-model-correction/](Q12-model-correction/) | Q12 — When the Model is Wrong |
| [Q13-strategy/](Q13-strategy/) | Q13 — Final Challenge: Prompting Strategy for a Consulting AI Assistant |
| [appendix/test-log.md](appendix/test-log.md) | Consolidated test log across all 13 questions |

## Assignment instructions (for reference)

For every question:
1. Write the full prompt (not bullet points — actual usable prompt).
2. Explain: (a) why this structure, (b) why this prompting technique (zero-shot /
   few-shot / CoT), (c) what failure modes it prevents.
3. Provide at least one alternative prompt design.

This repo additionally includes, per question, synthetic test inputs and simulated
test runs so the prompt's behavior — including guardrails on ambiguous, contradictory,
or adversarial input — can be inspected directly rather than taken on faith.
