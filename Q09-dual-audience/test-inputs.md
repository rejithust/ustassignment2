# Test Inputs — Q9 Dual Audience Problem

## TI-1: A technical incident report (clear technical content to adapt)

```
Root cause: a race condition in the payment-retry worker caused duplicate charge
attempts when two worker nodes picked up the same queued job within a 200ms window
due to an insufficiently granular distributed lock (lock scope was per-queue, not
per-job). Affected approximately 340 transactions over a 6-hour window before the
lock granularity was corrected to per-job-id. No data loss occurred; duplicate
charges were refunded automatically via the existing reconciliation job.
```

## TI-2: Input with a genuine technical gap not fit for a confident business summary (edge case)

```
We're still investigating why memory usage on the ingestion service spiked 3x on
Tuesday. No root cause identified yet; a workaround (restarting the service every 4
hours) is in place but not a real fix.
```

## TI-3: Input that is already business-friendly with very little technical depth available

```
Customer support reported that users are happier since we shortened the password
reset flow from 5 steps to 2.
```
