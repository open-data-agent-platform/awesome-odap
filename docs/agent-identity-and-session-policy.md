# Agent Identity and Session Policy

Status: Draft, non-normative design proposal. No change to the Five Musts or Rubric. These are proposed controls, not ODAP Gateway v0.1.0 features.

Each invocation should bind the authenticated user, agent workload, tenant, session, tool, and resource. A caller-supplied session ID is correlation data, not proof of identity. Delegation should preserve the original principal while restricting scope and lifetime.

Evaluate policy at every tool call, including retries. Recheck permissions after approval and immediately before execution. Approval should bind the query hash, resource, semantic release, policy version, expiry, and cost limit; changes invalidate it.

Session controls should limit retries, tool calls, elapsed time, and sensitive-data egress. Unknown identity or unavailable policy evaluation should block protected operations. Define atomic counters for concurrent requests and cancellation propagation.

Record allow/deny decisions and policy versions using a trace ID without logging credentials or unrestricted result data.

Prototype acceptance: demonstrate two isolated tenants, denied delegation escalation, expired approval, a query changed after approval, and retries that cannot reset session limits.

Reference: [AWS AgentCore policy sessions](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/policy-session-based-temporal.html). Vendor behavior is evidence for this discussion, not a conformance determination.
