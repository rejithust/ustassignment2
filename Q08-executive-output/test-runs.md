# Test Runs — Q8 Executive-Ready Output

---

## Run 1 — Primary prompt × TI-1 (verbose draft, single clear finding)

```
**Headline:** Users who don't finish onboarding within 7 days churn at 3x the rate of those who do.

**Key Findings**
- Account setup step averages 12 minutes and is the main onboarding drop-off point.
- Users report setup is confusing; this likely contributes to slow completion.
- Churn-onboarding link is correlational, not yet isolated from plan-tier effects.

**Recommended Actions**
| Action | Owner | Timeframe | Expected Impact |
|---|---|---|---|
| Simplify account setup step | Product/Eng Lead | Next sprint cycle | Faster onboarding, likely lower early churn |
| Isolate churn driver from plan tier | Analytics Lead | 2 weeks | Confirms true causal driver before further investment |
| Budget engineering time for fix | VP Engineering | This quarter planning cycle | Unblocks the simplification work |

**Risk if No Action Taken:** Elevated early-stage churn likely continues uninvestigated into next quarter.
```

**Observation:** The verbose draft's hedging ("it's possible," "we haven't fully
isolated," "though it would require...") is compressed into direct, actionable
statements, with the genuine uncertainty (correlation vs. causation) preserved as a
Key Finding rather than deleted — important, since dropping it would overstate
confidence to the executive reader.

---

## Run 2 — Primary prompt × TI-2 (sparse input, edge case: nothing to report)

```
**Headline:** No material changes in tracked metrics this week.

**Key Findings**
- All tracked metrics remained within normal range this week.

**Recommended Actions**
| Action | Owner | Timeframe | Expected Impact |
|---|---|---|---|
| No action required this week | — | — | — |

**Risk if No Action Taken:** None identified this week.
```

**Observation:** This is the key stress test for forced structure vs. genuinely
empty content: the template doesn't force the model to manufacture 4 findings or 4
actions out of nothing — it correctly collapses to a single honest line per section,
which is itself the "crisp" and non-verbose behavior the brief asks for, rather than
padding the report to look busy.

---

## Run 3 — Primary prompt × TI-3 (four distinct issues competing for the 4-bullet/4-row caps)

```
**Headline:** Support volume, new signups, an outage, and a recurring sales feature gap all need attention this quarter.

**Key Findings**
- Support tickets up 14%, tied to the billing system migration.
- New signups down 5% MoM, likely linked to a competitor's cheaper tier.
- Outage on the 14th caused 40 min downtime for ~8% of customers.
- SSO with custom identity providers raised in 6 of last 10 enterprise renewals.

**Recommended Actions**
| Action | Owner | Timeframe | Expected Impact |
|---|---|---|---|
| Review billing migration for support-driver root cause | Eng/Support Lead | 1 week | Reduces ticket volume |
| Assess pricing response to competitor's new tier | Product/Pricing Lead | 2 weeks | Protects signup rate |
| Post-incident review of the 14th outage | Infra Lead | 1 week | Prevents recurrence |
| Scope SSO (custom IdP) for enterprise roadmap | Product Lead | Next roadmap cycle | Protects enterprise renewals |

**Risk if No Action Taken:** Compounding signup decline and recurring enterprise renewal friction, alongside unresolved support and reliability issues.
```

**Observation:** This input had exactly four distinct issues, which the 4-item caps
accommodate cleanly without forcing a cut — a useful boundary check showing the caps
aren't arbitrarily too tight for genuinely multi-issue quarters, while still forcing
each bullet to stay compressed (≤15 words) and each action to carry an owner and
timeframe, unlike the original dense paragraph.
