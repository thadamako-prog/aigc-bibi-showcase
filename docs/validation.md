# Validation notes

## Evidence recorded during development

These are historical observations from the implementation session, not a fresh production audit or a test suite running in this public repository.

- A real external customer's payment was recorded once in the payment ledger and their account showed 10 credits. Approximately 42 seconds elapsed between invoice creation and the recorded credit event; it must not be presented as measured time since bank settlement or since product launch.
- Cancellation and failed-job refund paths were exercised on database copies. Those checks do not establish correctness under every concurrency or crash scenario.
- Live provider checks returned successful video, caption and poster results. An invalid small-image submission failed without a net provider-credit charge in the observed check.
- The dashboard was read back and captured. Its prototype cash-based “profit” labels are not accounting net profit.
- Duplicate moderator instances were investigated and a file lock was introduced. This addresses that specific process-conflict problem, not all race conditions.

## Acceptance-test matrix for future releases

The following is a proposed regression checklist, not a claim that every scenario has already passed.

| Scenario | Expected invariant | Risk addressed |
| --- | --- | --- |
| Insufficient credit | No paid job is accepted and no balance becomes negative | Overspending |
| Duplicate confirmation | One accepted job for the same confirmation | Duplicate charge |
| Replayed payment event | At most one credit grant per invoice | Double credit |
| Provider rejects input | Defined failure state and eligible refund exactly once | Lost customer credit |
| Worker stops mid-job | Recoverable job state; reconciliation avoids duplicate effects | Interrupted work |
| Result delivery fails | Retry delivery without regenerating or charging again | Duplicate cost |
| Payment arrives late | Documented reconciliation/manual-review path | Missed payments |
| User declines promotion | No promotional archive or publication from that job | Consent violation |
| Notification phone is offline | Pending payments stay visible until reconciliation | Hidden dependency |
| Two moderators start | Only one instance processes updates | Conflicting workers |

## What I would improve next

1. A repeatable automated regression suite using isolated fixtures and no real customer data.
2. Structured monitoring of queue latency, error rate, duplicate events and refund reconciliation.
3. Separate cash receipts, earned service revenue and outstanding prepaid-credit obligations in reporting.
4. Gateway-backed payment verification if transaction volume justifies it.
5. Documented backup/restore drills, access controls and a deployment-security review.

## Publication boundary

This showcase deliberately excludes production code, database exports, secrets, user identifiers, customer photos, internal addresses and operational configuration. A public case study is evidence of project scope, not a substitute for a private technical review.
