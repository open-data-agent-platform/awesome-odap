# Cost Controls and Audit Evidence

Status: Draft, non-normative design proposal. Existing Five Musts and Rubric remain unchanged. Proposed behavior is not a claim about Gateway v0.1.0.

## Cost controls

Document separately: cost visibility, threshold alerts, delayed blocking, pre-execution admission limits, and runtime cancellation. State enforcement latency, overshoot, accounting period, concurrency behavior, and included costs. Query estimates are not guarantees of final billed spend.

Account for SQL compute, model tokens, retries, extraction, embeddings, and external tools where applicable. Reserve budget atomically before admitting concurrent work, reconcile actual usage, and keep unknown or pending costs visible. Cancellation may leave billable work in progress.

## Audit evidence

Correlate request, user, agent, tenant, tool invocation, approval, policy version, semantic release, query hash, execution job, and cost record. Distinguish database query logs, agent traces, authorization decisions, and semantic changes. Record denied and failed actions too.

Specify retention, access, redaction, export, and tamper protection. Trace sampling must not silently remove required audit events. Historical replay should use recorded versions and remain subject to current access policy.

## Evaluation

Keep evaluation cases separate from examples supplied to the agent. Test numerical correctness, ambiguous metrics, denied access, stale semantics, concurrent budget use, retries, cancellation, and missing audit events. Publish dataset assumptions and observed results; do not infer a vendor score from documentation alone.

References: [Snowflake resource budgets](https://docs.snowflake.com/en/en/user-guide/snowflake-cortex/cortex-agents-resource-budgets), [AWS observability](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability.html), [Genie evaluation concepts](https://docs.databricks.com/gcp/en/genie-agents/concepts).
