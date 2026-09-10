# Example: retrying a report export

This fictional example illustrates output structure, not a required architecture.
No repository has been inspected. Names in the appendix are illustrative.

## Input

> Report exports sometimes fail when the report service is temporarily unavailable.
> Users must start over. Design a small improvement; do not implement it yet.

## Before: mechanics hiding the decision

> Add an `ExportRetryManager` around `ReportClient.export()`, with exponential
> backoff and a retry-state table. Update the worker and add retry tests.

This leaves the reviewer to infer whether duplicate reports can be created,
what users see, and why persistent retry state is necessary.

## After: a design that can be reviewed without the appendix

Automatically retry brief service interruptions within the existing export job,
so users can receive their report without submitting the same request again.

### Problem and goal

Users currently restart failed exports manually. The goal is to recover from
temporary service unavailability while keeping one visible export request and
giving users a clear final result if recovery fails.

Assumptions to verify: exports already run as background jobs; the report service
can recognize repeat attempts for the same export. These are not established facts.

### Proposed approach

The existing export job owns a bounded retry process. On a temporary failure, it
waits briefly before trying the same logical export again. The user continues to
see one request in progress and receives one completion result.

An invalid request or access denial fails immediately. If the retry allowance is
exhausted, the request becomes failed and the user can retry manually. A timeout
may mean the service accepted the request but the response was lost; retrying that
case is safe only if the service recognizes the same export across attempts.

### Decisions and rationale

- **Retry only temporary failures.** Retrying invalid input or denied access cannot
  resolve the cause and would delay useful feedback.
- **Bound and space attempts.** Short interruptions may recover, but indefinite or
  immediate repeated attempts keep users waiting and add pressure to a struggling
  service. Exact limits need to fit the existing job time budget.
- **Keep one logical export across attempts.** This prevents an ambiguous timeout
  from producing duplicate reports. If the service cannot recognize repeated
  attempts, narrow retries to failures known to occur before acceptance.
- **Use the existing job lifecycle.** A separate durable retry subsystem would add
  state and recovery rules without an established need. Manual retry alone is
  simpler but leaves the original interruption cost with the user.

### Observable changes and acceptance

| Situation | Current behavior | Proposed behavior |
|---|---|---|
| Brief service interruption | User restarts the export | Same request recovers if service returns within the retry allowance |
| Service remains unavailable | Export fails | Export fails after a bounded delay, with a clear final status |
| Invalid request or denied access | Export fails | Fails immediately, without automatic retry |
| Response lost after acceptance | Outcome may be unclear | No duplicate report if repeat-attempt recognition is supported |

Verify these cases with controlled service failures, including a timeout after
acceptance. Successful exports should keep their existing output and access rules.

### Scope

This change covers transient failures within an existing export job. It does not
add a scheduling service or recovery across worker restarts.

### Risks and open questions

- **Design dependency:** verify that the service recognizes repeated export
  attempts before enabling retries after ambiguous timeouts. Otherwise use the
  narrower behavior described above.
- **Operational tradeoff:** retries increase service traffic and time to final
  failure. Validate a retry budget against the current job deadline and expected
  service recovery time.
- **Implementation uncertainty:** inspect how the worker classifies errors and
  records job status before choosing exact integration points.

### Implementation sketch — optional reading

Inspect the existing worker and client first. Add a bounded retry loop around the
existing export call, preserve the logical export identifier across attempts, and
reuse current job status storage. Avoid adding a new persistence layer unless the
existing lifecycle proves insufficient for the agreed behavior.

Use controlled responses to verify transient recovery, retry exhaustion,
non-retryable failures, and duplicate prevention after a lost response.

| Decision | Why it exists | Observable effect | Likely implementation |
|---|---|---|---|
| Classify temporary failures | Retry only recoverable conditions | Invalid requests fail promptly | Existing client's error classification |
| Bound and space attempts | Limit waiting and service load | Request reaches a final state within the job budget | Existing worker's retry loop and deadline |
| Reuse logical export identity | Avoid duplicate work | One report per request despite lost responses | Service-supported repeat-attempt identifier |
| Reuse the job lifecycle | Keep scope small | One visible request throughout recovery | Existing job status storage |
