# Q7. Classification with Edge Cases (Billing / Network / Device / Other)

## Zero-Shot Prompt

```
SYSTEM:
Classify the following customer complaint into exactly one category: Billing,
Network, Device, or Other.

Definitions:
- Billing: charges, invoices, payments, refunds, plan pricing.
- Network: signal, coverage, call drops, data speed, outages.
- Device: physical phone/router/SIM hardware, device defects, settings.
- Other: anything that does not clearly fit the above, including multi-issue
  complaints where no single category is dominant.

If the complaint reasonably touches more than one category, choose the category
representing the PRIMARY cause the customer is complaining about, and note the
secondary category separately.

COMPLAINT:
"{{complaint_text}}"

OUTPUT (JSON only):
{"primary_category": "...", "secondary_category": "... or null", "reasoning": "one sentence"}
```

## Few-Shot Prompt

```
SYSTEM:
[same role + definitions as above]

EXAMPLES:

Complaint: "My bill went up by $20 and nobody told me why, also my calls keep
dropping."
Output: {"primary_category": "Billing", "secondary_category": "Network",
"reasoning": "The complaint opens and centers on an unexplained charge; the dropped
calls are a secondary, less emphasized issue."}

Complaint: "The new router you sent doesn't even turn on, waste of money."
Output: {"primary_category": "Device", "secondary_category": null, "reasoning":
"Complaint is about a defective physical unit, not price or connectivity."}

Complaint: "I've called support five times and nobody can tell me why my account
shows I owe money for a plan I cancelled last year, and when I do get signal my data
is unusably slow."
Output: {"primary_category": "Billing", "secondary_category": "Network", "reasoning":
"The overdue-charge-for-a-cancelled-plan issue is the dominant, unresolved grievance;
the data speed comment is appended at the end as secondary."}

Now classify:
COMPLAINT: "{{complaint_text}}"
OUTPUT (JSON only):
```

Both prompts are tested against the same set of edge-case complaints in
`test-inputs.md` / `test-runs.md` so the zero-shot vs. few-shot difference can be
compared directly rather than argued abstractly.
