# Auto Ontology Integration for ODAP

**Status:** Draft proposal (informative, not normative)
**Applies to:** Semantic definitions and governance in the ODAP reference architecture
**Relation to the standard:** This document does **not** modify the ODAP Five Musts or the Scoring Rubric. Auto ontology is an *optional* capability.

---

## 1. Problem

ODAP's Smart Semantic Layer resolves business meaning through governed metrics, entities, dimensions, time logic, ownership, and versioning. Definitions may be authored manually or generated with assistance. Reviewing their business meaning creates a cold-start cost for a new data source.

Auto ontology (automatically deriving concepts, relations, and constraints from schemas, data, and documents) can reduce that cost. It also introduces a new risk: machine-generated semantics can be wrong, inconsistent, or hallucinated.

This document proposes how to use auto ontology **without weakening ODAP's governance guarantees**.

## 2. Core Principle

> Auto-generated semantics are **candidates**, never **authority**.

A generated definition must not influence agent-generated SQL until a human reviewer has approved it and its immutable version has been released. Each query records the semantic release it used. Deprecated versions remain available for audit replay only, not new agent queries.

## 3. Proposed Flow

```
Sources (schemas, sample data, docs, existing SQL)
        |
        v
[1] Extraction        -> candidate entities, dimensions, metrics, relations
        |
        v
[2] Alignment         -> dedupe, map to existing semantic registry / catalogs
        |
        v
[3] Validation        -> consistency checks, dry-run against real data
        |
        v
[4] Review queue      -> human approval (owner / steward)
        |
        v
[5] Versioned release -> approved semantics enter the Semantic Layer
        |
        v
[6] Agent use         -> resolver reads approved versions only
```

Every step writes an audit event linked to the candidate ID, actor, source provenance, and immutable version.

### 3.1 Candidate Lifecycle

| State | Meaning | Usable by agents? |
|---|---|---|
| `proposed` | Produced by an extractor | No |
| `validated` | Passed automated checks | No |
| `approved` | Accepted by a named owner; awaiting release | No |
| `released` | Published as an immutable, approved version | Yes, subject to access policy |
| `deprecated` | Replaced or retired | Read-only, for audit |
| `rejected` | Declined, reason recorded | No |

Rejections are kept as feedback so extractors do not re-propose the same error.

## 4. Proposed Design Controls

The following controls are recommendations for this optional design proposal; they introduce no new ODAP conformance requirements:

1. **Default to unapproved.** New candidates start as `proposed`.
2. **Require an owner.** Each approved definition has a named owner and version.
3. **Record provenance.** Store the source (table, column, document), the extractor (rule or model and version), and a confidence score.
4. **Audit all transitions.** Log who approved, rejected, or changed a definition, and when.
5. **Block silent drift.** A re-run of extraction must produce a new candidate, not overwrite an approved definition.
6. **Respect data policy.** Extractors must follow the same permission and PII rules as agents. Sample data used for extraction is subject to masking.
7. **Support rollback.** Restore a previously approved release through an audited release action, subject to current access policy. Preserve immutable history.

### Adapter boundary and prototype acceptance

Use the same candidate envelope for NVIDIA Auto Ontology and other generators: source identifier, source revision, extractor/model version, definition, provenance, owner, and lifecycle state. Confidence is optional and does not confer approval. Imported existing definitions retain their provenance but require explicit acceptance into the local release process.

Schema changes create a new candidate and trigger impact review. Queries pin a released semantic version and record it alongside the policy decision. A changed definition invalidates prior approval. Demonstrate that unreleased, rejected, deprecated, or unauthorized definitions cannot influence a new query.

Related drafts: [identity and session policy](agent-identity-and-session-policy.md), [cost controls and audit evidence](cost-controls-and-audit-evidence.md), and [ecosystem review](ecosystem-review-2026-11.md).

## 5. Synthetic Example

All names, fields, values, and business rules below are fictional demonstration data. The example is not a production accounting definition.

Source table:

```
orders(order_id, customer_id, amount, status, created_at)
```

Extractor output (candidate):

```yaml
entity:
  name: Order
  key: order_id
  relations:
    - belongs_to: Customer
      foreign_key: customer_id
dimensions:
  - name: order_status   # from `status`
    values_observed: [paid, refunded, cancelled]
  - name: order_date     # from `created_at`
metrics:
  - name: gross_revenue
    expression: SUM(amount)
    state: proposed
    confidence: 0.62
    note: "Unclear whether refunded/cancelled orders should be excluded"
```

Review outcome (approved by an owner, v1):

```yaml
metric:
  name: paid_order_amount
  expression: SUM(amount) FILTER (WHERE status = 'paid')
  owner: finance-data
  version: 1
  approved_by: demo-finance-reviewer
  state: released
  release_id: demo-semantic-release-v1
```

The reviewer renames the metric to `paid_order_amount`: this measures paid orders under the stated demo rule. Calling it `net_revenue` would additionally require explicit treatment of refunds, taxes, discounts, currency, and recognition timing.

The ambiguity flagged in the candidate is exactly why human approval is required: the extractor could not know the business rule.

## 6. Relation to Existing Work

[NVIDIA Auto Ontology](https://github.com/NVIDIA/auto-ontology) is a relevant upstream project for semantic context generation. This proposal describes a possible ODAP integration boundary; it does not claim an implemented connector, NVIDIA endorsement, or a verified absence of approval controls in the upstream project. An adapter would preserve provenance and stage outputs as candidates before review and release.

| Approach | How it relates |
|---|---|
| OBDA (e.g. Ontop) | Maps a formal ontology to relational data via declarative mappings. Auto ontology could draft the ontology and mappings. |
| dbt Semantic Layer / MetricFlow / Cube | Hand-authored metric definitions. Auto ontology could propose entries for these. |
| GraphRAG-style schema building | LLM-derived graph schemas. Approval and audit behavior varies by implementation; this proposal requires explicit review and release at the ODAP boundary. |
| Data catalogs (OpenMetadata, DataHub) | Possible source of existing metadata for the alignment step. |

## 7. Risks and Open Questions

- **Hallucination:** Models may invent relations or metrics not supported by data. Validation against real data (dry-run, constraint checks) mitigates but does not eliminate this.
- **Granularity inconsistency:** Extractors may mix levels of abstraction. Alignment against the existing registry is needed.
- **Review fatigue:** Large candidate volumes can overwhelm reviewers. Prioritization (by usage, confidence, risk) is an open question.
- **Evaluation:** There is no agreed benchmark for semantic-layer quality. Candidate acceptance rate and post-approval correction rate may be useful proxies.
- **Scope:** Should extraction cover unstructured documents, or schemas and query logs only?

## 8. Non-Goals

- Making auto ontology a requirement of ODAP conformance.
- Allowing agents to use unapproved semantics, even with high confidence.
- Defining a new ontology language. Implementations may use OWL/SHACL, YAML registries, or existing semantic-layer formats.

## 9. Possible Next Steps

1. Gather feedback on this proposal via issues or discussions.
2. If there is interest, prototype an experimental extractor in `odap-gateway` that emits `proposed` candidates only.
3. Evaluate on a small set of public schemas and report acceptance and correction rates.

---

*This is a design proposal. It describes intended behavior, not features currently implemented in ODAP Gateway v0.1.0.*
