# Test Runs — Q9 Dual Audience Problem

---

## Run 1 — Primary prompt × TI-1 (technical incident report with real depth)

```
## For Technical Team
Root cause was a race condition in the payment-retry worker: two worker nodes could
pick up the same queued job within a ~200ms window because the distributed lock was
scoped per-queue rather than per-job-id, allowing concurrent duplicate processing of
a single job. This produced duplicate charge attempts for ~340 transactions over a
6-hour window. Remediation: lock granularity was changed from per-queue to
per-job-id, eliminating the race window. No data loss occurred. All duplicate
charges were reversed automatically via the existing reconciliation job; no manual
refund intervention was required. Residual risk: verify no other workers in the
retry pipeline share the same per-queue locking pattern before closing this out.

## For Business Team
A software bug briefly caused about 340 customers to be charged twice for the same
transaction over a 6-hour period. No customer data was lost, and every duplicate
charge (double payment) was automatically refunded without anyone needing to
contact support. The underlying bug has already been fixed. No further action is
needed from the business team; this is included for awareness only.
```

**Observation:** The business section stays at 56 words, well under the 150-word
cap, defines "double payment" inline instead of leaving "duplicate charge attempt"
unexplained, and reaches the exact same bottom line as the technical section (bug
found, fixed, no lasting impact) — satisfying the "must be factually consistent"
rule.

---

## Run 2 — Primary prompt × TI-2 (unresolved technical issue, edge case)

```
## For Technical Team
Memory usage on the ingestion service spiked 3x on Tuesday; root cause has not yet
been identified. Current mitigation is a scheduled restart of the service every 4
hours, which is a workaround, not a fix — it resets memory state before it reaches a
critical threshold but does not address the underlying leak or load-driven cause.
Gap: no root-cause hypothesis, affected component, or timeline for a permanent fix is
available yet; further investigation (heap profiling, load correlation analysis) is
needed before this can be summarized more specifically.

## For Business Team
We noticed a technical issue last Tuesday causing one system to use more memory than
normal. We've put a temporary safeguard in place (regular restarts) so it doesn't
cause a problem for customers while we investigate further. We don't yet know the
root cause or when a permanent fix will be ready — we'll share an update once we do.
```

**Observation:** This is the key edge-case test: neither section invents a root
cause or a resolution timeline that doesn't exist in the input. The technical
section explicitly states the information gap ("Gap: no root-cause hypothesis...")
per the rule "if information needed for one audience is missing, state the gap
inside that audience's section" — and the business section's plain-language version
is consistent with that same uncertainty rather than falsely reassuring the reader
with a resolution that hasn't happened yet.

---

## Run 3 — Primary prompt × TI-3 (input with very little technical depth available)

```
## For Technical Team
Change: password reset flow reduced from 5 steps to 2 steps. Source: qualitative
feedback via customer support, not a structured or quantitative user study.
Limitation: no specific metrics (e.g., completion rate, support ticket volume change,
time-to-reset) were provided to substantiate the reported improvement — this is
anecdotal support feedback, not measured data. Recommend instrumenting the flow (e.g.,
funnel completion rate, median time-to-complete) if a quantified before/after
comparison is needed for future decisions.

## For Business Team
Customers are responding positively to the shorter password reset process (down
from 5 steps to 2). This feedback is anecdotal so far, based on what customer
support has heard directly from users, not a formal study.
```

**Observation:** Even with minimal technical substance in the source input, the
Technical section does not pad itself with invented detail — instead it correctly
characterizes the actual nature of the evidence (anecdotal, not measured) and
suggests what would strengthen it. The Business section mirrors that same
"anecdotal, not yet measured" caveat in plain language rather than overstating
certainty, keeping both sections consistent even when the underlying input is
business-oriented rather than technical.
