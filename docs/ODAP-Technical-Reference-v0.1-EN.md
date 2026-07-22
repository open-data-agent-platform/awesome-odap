# ODAP Technical Reference

**Version 0.1 · July 2026 · Companion to the ODAP White Paper v1.0**

Maintained by the Open Data Agent Platform community
github.com/open-data-agent-platform · License: CC0

> **Scope note.** This document describes *conformance principles and reference
> architecture* — how a system that satisfies the ODAP Five Musts should be
> designed. It is not a description of any single implementation. Where it
> refers to the reference implementation (odap-gateway), it does so by example,
> and the authoritative statement of what that implementation actually ships
> is its own [clause mapping](https://github.com/open-data-agent-platform/odap-gateway/blob/main/ODAP_CLAUSE_MAPPING.md),
> which is deliberately partial. Principles here describe the target; the
> implementation is explicit about the distance still to travel.

---

## 1. Purpose and Audience

The [ODAP White Paper v1.0](https://github.com/open-data-agent-platform/awesome-odap) defines the category, the Five Musts, and the case for a governed gateway between AI agents and enterprise data. This document is its technical companion, written for platform engineers, security architects, and integration teams who need to reason about *how* an ODAP-conformant gateway is structured — its layers, its control points, and the design principles that let it extend across heterogeneous data sources without abandoning the Five Musts.

It is a reference, not a specification. It does not mandate APIs or wire formats; it describes the invariants any conformant design must preserve and the reasoning behind them. Concrete interface standardization is future work (see §8).

## 2. Architectural Overview

An ODAP gateway sits on the single path between agents and data, and decomposes into five layers, each implementing one or more of the Five Musts.

```
        ┌─────────────────────────────────────────────┐
Agent → │  L5  Governance & Observability (cross-cut)  │
        ├─────────────────────────────────────────────┤
        │  L4  Persistent Memory (enhancement)         │
        ├─────────────────────────────────────────────┤
        │  L3  Multi-Agent Execution Pipeline          │  ← Governed, Cost (enforce)
        │      plan → generate → validate → execute    │
        ├─────────────────────────────────────────────┤
        │  L2  Semantic Layer                          │  ← Semantic
        ├─────────────────────────────────────────────┤
        │  L1  Universal Data Gateway                  │  ← Open
        └─────────────────────────────────────────────┘
                          ↓
          BigQuery · Snowflake · object stores · HDFS · APIs
```

The ordering encodes a dependency: a request descends L5→L1 gaining context and constraints, executes at the source, and ascends L1→L5 accumulating evidence. The critical design invariant is that **no layer may be bypassed by the agent**. An agent cannot reach L1 without passing L3's validation, and cannot receive an answer that L5 has not recorded. This is what separates a gateway from a library: a library is called when convenient; a gateway is unavoidable.

## 3. Data Access Governance Model

Governance in an ODAP gateway is defined by *where* in the request lifecycle each control is resolved. The model distinguishes three temporal control points, and the ODAP position is that the earliest viable point wins.

**Pre-generation.** Identity, role, and object-level permissions are resolved *before* the execution pipeline generates a query. An agent operating on behalf of a user who lacks access to a table must not be able to draft a query against it — the object should be absent from the schema context the generator sees, not merely rejected at execution. This collapses an entire class of attacks (prompt-injected access to forbidden objects) because the forbidden object never enters the reasoning surface.

**Pre-execution.** Cost, blast radius, and policy conditions are resolved *after* generation but *before* the query touches production. This is the dry-run boundary: estimated bytes, estimated cost, affected rows, and any policy triggers (e.g., queries over PII columns, queries exceeding a budget threshold) are evaluated here. High-risk queries route to human approval at this point; they do not execute and then get reported.

**Post-execution.** Audit, attribution, and lineage are recorded *after* execution. Crucially, in a conformant design post-execution controls are *evidentiary, not protective* — nothing that only happens after execution can protect the system, because the money is already spent and the data already read. Post-execution is where you prove what happened, not where you prevent it.

The governance model's single organizing principle: **a control that can only observe is not a control; enforcement lives in the path, before the irreversible act.** A permission checked only at the database, a budget reported only on the invoice, an approval sought only after damage — each is a control point resolved one step too late.

### 3.1 Policy resolution order

For any incoming request, a conformant gateway resolves controls in this order, failing closed at the first violation:

1. Authenticate the actor (user and/or agent identity).
2. Resolve the authorized object set; construct schema context from it alone.
3. Generate against that context only.
4. Validate: syntax, permissions (re-check), security (injection, exfiltration patterns), cost estimate.
5. Evaluate policy conditions; route to approval if triggered.
6. Execute within the approved scope.
7. Record the full chain (§5).

Failing closed means: absent an explicit allow, deny. An ODAP gateway that fails open under load or error is not conformant — degradation must preserve the gate.

## 4. Agent Tool-Call Security

When agents invoke the gateway as a tool, the tool boundary itself becomes an attack surface. Three principles govern conformant tool-call handling.

**Least-authority tool exposure.** The tool interface exposed to an agent must carry no more authority than the agent's resolved permissions. A gateway must not offer a single high-privilege "run SQL" tool to all agents and rely on the agent to self-restrict. Authority is bound at the tool boundary, per the resolved object set (§3), not delegated to agent behavior.

**Instruction/data separation.** Content retrieved from data sources (rows, column values, metadata) must never be promotable to instructions. A conformant gateway treats all source-derived content as data even when it contains text that resembles commands — the classic indirect prompt-injection vector where a malicious row value instructs the agent. The execution pipeline's generation stage consumes schema and semantics as context but does not execute directives found in returned data.

**Non-repudiable invocation.** Every tool call carries an attributable agent identity that is bound server-side, not asserted by the agent. "Which agent made this call" must be answerable from the gateway's own records, not from a field the caller populated. This is the precondition for the audit model in §5.

**Idempotency and blast-radius bounds.** Tool calls that mutate or spend must be bounded: rate limits per agent, cost ceilings per agent (§6), and — for conformant designs targeting the higher anchors — idempotency keys so that an agent retry storm cannot multiply effect. Machine-speed callers require machine-speed bounds.

## 5. Audit and Traceability

The ODAP audit model requires four distinct records, corresponding to Rubric clauses A4.1–A4.4. A design that produces only executed-SQL logs satisfies none of them fully.

**Generation trail (A4.1).** The complete chain for every generated query: prompt → resolved context → producing agent identity → generated SQL → execution result. Attributable and immutable. The design requirement is that this chain is a *first-class retained object*, not something reconstructable only by correlating separate logs after the fact.

**Decision trail (A4.2).** Every governance decision — approval, rejection, rollback, policy trigger — recorded with actor and timestamp. Decisions are events, and the absence of a decision (an auto-approved low-risk query) is itself a recorded event with the policy basis noted.

**Explainability (A4.3).** Work shown *by default*, not on request: for each answer, the tables and joins used, the assumptions applied (e.g., how "last quarter" resolved), a confidence indication, and the validation status. Explainability that requires a separate query to retrieve is a lower anchor than explainability rendered with every answer.

**Replayability (A4.4).** An auditor can reconstruct and re-execute any historical decision with its original context preserved. The design implication: context (schema version, policy version, semantic definitions in force at the time) must be versioned and referenceable, not just the query text.

The organizing principle: **"the AI did it" is never a sufficient audit answer.** Every irreversible act traces to an actor, an authorization, a cost, and a recorded decision — or the gateway is not conformant on Must 4.

## 6. Cost and Resource Control

Cost control is the Must that most sharply distinguishes machine-speed access from human-speed access, and its design has four elements (clauses C5.1–C5.4, with C5.5 emerging).

**Pre-execution estimation (C5.1).** Every query is estimated before execution — bytes scanned, monetary cost, runtime — surfaced by default, not on request. On sources that expose native dry-run (e.g., BigQuery), this is a direct call; on sources that do not, the gateway must synthesize an estimate from statistics and plan analysis (§7.3). The invariant is that *no query executes without an estimate having been available to the control layer.*

**Budget enforcement (C5.2).** Estimates feed circuit-breakers. Over-threshold queries are blocked with an approval-based exception path — not alerted-and-executed. Enforcement operates at multiple scopes: per-query ceilings, per-agent budgets, per-organization caps. A conformant design fails closed on budget: if the estimate cannot be produced, the query does not silently proceed.

**Attribution granularity (C5.3).** Cost attributes to the specific agent, user, and query — not merely to an account. This is what makes per-agent budgets (§4) enforceable and what lets an organization answer "which agent is spending our warehouse budget."

**Optimization loop (C5.4).** For expensive queries, the gateway proposes cheaper equivalents with estimated savings — a rewrite, a partition filter, a materialized alternative. Advisory at lower anchors, automated at higher ones.

**Resource & energy awareness (C5.5, emerging).** Monetary cost and resource/energy footprint are related but distinct; a workflow can be cheap in dollars and expensive in energy depending on pricing. Conformant designs are beginning to surface compute-resource and energy estimates alongside dollar cost, tracking [rubric proposal #2](https://github.com/open-data-agent-platform/awesome-odap/issues/2).

## 7. Heterogeneous Source Adaptation — Principles

An ODAP gateway must satisfy the Open Must (§Must 1) across warehouses, lakes, object stores, and operational databases. This section describes *how a conformant design abstracts source heterogeneity* — the adaptation principles — not a catalog of shipped adapters. (The reference implementation is single-source by design; multi-source is roadmap, and this section describes the target the roadmap builds toward.)

### 7.1 The adapter contract

Source heterogeneity is contained behind a uniform internal contract. Every source adapter must expose four capabilities to the layers above it, so that L2–L5 reason identically regardless of source:

1. **Schema introspection** — tables, columns, types (including nested), ownership metadata.
2. **Cost estimation** — a pre-execution estimate, native or synthesized (§7.3).
3. **Scoped execution** — execute a validated query within an authorized object set, returning results plus execution metadata.
4. **Capability declaration** — what the source supports (nested types, dry-run, branching, masking), so the gateway degrades gracefully rather than assuming.

A source that cannot satisfy capability *n* does not break the gateway; it lowers the achievable anchor on the affected clause, and the gateway declares that honestly (the same discipline as the clause mapping).

### 7.2 Source archetypes and their adaptation profile

**Cloud warehouses (BigQuery, Snowflake, Redshift).** The most complete profile: native dry-run cost estimation (C5.1 direct), fine-grained access controls to compose with pre-generation resolution (§3), and rich statistics for optimization (C5.4). The gateway's job is to map native capabilities onto the uniform contract — e.g., BigQuery's dry-run and Snowflake's `EXPLAIN` + warehouse-credit model both satisfy "cost estimation," expressed differently.

**Open table formats on object stores (Iceberg, Delta, Hudi on S3/GCS/Azure Blob).** Openness is highest here (O1.1), but cost estimation and governance are *not* uniformly provided by the storage layer — they must be supplied by the query engine sitting over the format (Trino, Spark, DuckDB, etc.). The adaptation principle: the gateway binds to the *engine's* capabilities for cost and execution, and to the *format's* capabilities for openness and exit. Governance that the object store cannot enforce is enforced at the gateway, above it.

**HDFS and on-prem lakes.** The hardest profile: often no native cost API, coarse-grained security, and schema-on-read ambiguity. The adaptation principle is *synthesis over assumption* — the gateway synthesizes cost estimates from table statistics and query plans (§7.3), applies governance at its own layer because the substrate provides little, and declares reduced capability honestly rather than claiming enforcement it cannot deliver. An ODAP gateway over HDFS scores lower on some clauses than one over BigQuery, and says so.

**Operational databases and APIs.** Bounded by the source's own transactional and rate constraints. The adaptation principle: respect source-side limits as hard bounds, and layer agent-appropriate controls (idempotency, per-agent rate limits, §4) that the source was never designed to provide for machine-speed callers.

### 7.3 Cost estimation where the source has none

Because pre-execution estimation (C5.1) is a Must and not all sources provide it, a conformant multi-source design needs a synthesis path: derive an estimate from table statistics (row counts, column cardinality, partition layout), query plan analysis (scan scope, join fan-out), and historical execution telemetry for similar queries. The estimate need not be exact; it must be *available to the control layer before execution* and *conservative* (over-estimation fails safe, under-estimation defeats the budget circuit-breaker). A source without native dry-run does not exempt a gateway from the Cost Must — it obligates the gateway to synthesize.

### 7.4 The invariant across all sources

Whatever the source, four things must remain true at the gateway boundary: permissions resolved before generation, cost available before execution, every action recorded, and open exit preserved. Sources differ in how much they help; they never change what the gateway owes. Where the source is weak, the gateway does more — it never does less and calls the difference "not supported."

## 8. Future Work

**Interface standardization.** This document describes invariants, not wire formats. A future ODAP interface specification could standardize the adapter contract (§7.1) and the audit record schema (§5) so that gateways, sources, and auditors interoperate — the natural next layer once multiple conformant implementations exist.

**Semantic layer depth.** The Semantic Must (L2) is the industry's least-implemented layer; business-editable metric definitions with ownership and versioning remain the open frontier (White Paper §9).

**Agent portability.** As agent protocols (MCP, A2A) standardize, whether agent definitions, workflows, and memory move between gateways in open formats becomes an extension of the Open Must — under consideration for a working-group position paper.

**Reference implementation depth.** odap-gateway advances single-source (BigQuery) depth first; multi-source adaptation per §7 is roadmap, sequenced to keep the clause mapping honest at every release.

---

## Appendix — Layer / Must / Clause Map

| Layer | Primary Must(s) | Clauses |
|---|---|---|
| L1 Universal Data Gateway | Open | O1.1–O1.4 |
| L2 Semantic Layer | Semantic | S2.1–S2.4 |
| L3 Execution Pipeline | Governed; Cost (enforce) | G3.1–G3.4, C5.1–C5.2 |
| L4 Persistent Memory | (enhancement) | — |
| L5 Governance & Observability | Auditable; Cost (attribute); Governed (evidence) | A4.1–A4.4, C5.3–C5.5 |

---

*ODAP Technical Reference v0.1 · CC0 · companion to the ODAP White Paper v1.0*
*Conformance principles describe the target; the reference implementation's [clause mapping](https://github.com/open-data-agent-platform/odap-gateway/blob/main/ODAP_CLAUSE_MAPPING.md) states what actually ships. The gap is the roadmap, and it is public.*
