# Example: retrying a report export

This fictional example assumes the reviewer knows the technical terms below.
The recommendation uses the same mechanism in both versions.

## Input

> Report exports run as background jobs. A job stops on the first service error,
> so brief outages force users to start over. Design a small improvement; do not
> implement it yet.

## Mechanism only

> Add bounded exponential backoff around `ReportClient.export()` in the worker.

## With the reason and cost

> Brief service outages currently fail exports immediately, so add bounded
> exponential backoff around `ReportClient.export()` for transient failures.
> This can avoid manual restarts, but delays final failure and increases service
> load. Retrying ambiguous timeouts also requires service-side idempotency to
> avoid duplicate exports.
