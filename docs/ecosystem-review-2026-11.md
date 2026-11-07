# Ecosystem Review: November 2026

Original evidence reviewed: 2026-11-05. Additional interchange, protocol, runtime
and Gateway-preview sources checked: 2026-11-07, as recorded in the
[update notes](update-notes-2026-11.md). This is an informative documentation
review; no product testing, score changes or conformance certifications.

| Area | Official evidence | Follow-up |
|---|---|---|
| Semantic generation | [NVIDIA Auto Ontology](https://github.com/NVIDIA/auto-ontology), [Snowflake Autopilot and modeling](https://docs.snowflake.com/en/user-guide/views-semantic/best-practices-modeling), [Databricks YAML generation](https://docs.databricks.com/gcp/en/uc-semantics/metric-views/yaml-reference) | Assess ownership, approval, immutable releases, and drift handling individually. |
| Governed metric access | [dbt Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl) exposes metrics through MCP. | Examine identity and policy scope at the interface. |
| Tool governance | [Databricks agent tools](https://docs.databricks.com/aws/en/agents/agent-framework/agent-tool) | Distinguish transport support from effective authorization. |
| Session policy | [AWS identity propagation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/policy-session-based-temporal.html) | Test caller identity, scope, and session isolation. |
| Analytics context | [BigQuery conversational analytics](https://docs.cloud.google.com/bigquery/docs/conversational-analytics) | Review glossary ownership and verified queries. |
| Cost enforcement | [Snowflake resource budgets](https://docs.snowflake.com/en/en/user-guide/snowflake-cortex/cortex-agents-resource-budgets) describes periodic enforcement and possible overspend. | Separate delayed actions from pre-execution hard limits. |
| Semantic interchange | [Apache Ossie (incubating), formerly OSI](https://github.com/apache/ossie), specifies semantic model exchange. | Assess format compatibility separately from approval, policy enforcement and runtime binding; this entry claims no Gateway import/export support. |
| Agent protocols | [MCP](https://modelcontextprotocol.io/) connects AI applications to tools/data; [A2A](https://a2a-protocol.org/) supports agent-to-agent communication. | Protocol compatibility alone does not prove effective authorization or ODAP conformance. |
| Operations runtime | [NVIDIA AI Factory Operations Agent](https://github.com/NVIDIA/AI-Factory-Operations-Agent) is an experimental cluster-operations blueprint. | Distinguish command isolation and module-specific access from data-semantic governance; it is not NVIDIA Auto Ontology. |
| Reference implementation | [ODAP Gateway v0.2.0-preview.1](https://github.com/open-data-agent-platform/odap-gateway/releases/tag/v0.2.0-preview.1) describes partial implementation of the proposed governance directions. | Maintainer-affiliated; deployment prerequisites and independent acceptance remain open. Do not treat its July 2026 v0.1.0 mapping as a current preview assessment. |

## Historical claims requiring review

Reassess any statement that the entire industry lacks business-defined semantic metrics. Natural-language authoring, expert configuration, certification, and approval are distinct capabilities. Current documentation establishes reasons to revisit earlier claims, not automatic full compliance.

Keep existing Landscape artifacts dated. For each revised score, record product edition, region, feature status, official evidence, tested behavior if available, and the applicable rubric clause. Publish revisions with a changelog and challenge route.

[Reassessment issue #10](https://github.com/open-data-agent-platform/awesome-odap/issues/10)
lists the six dated entries and their evidence gaps. Reviewers and target dates
are unassigned, not implied commitments or completed independent review.

## Proposed implementation sequence

This sequence is retained from the November 5 proposal. The preview release
above is an implementation checkpoint, not completion of the end-to-end sequence
or its held-out evaluation.

1. Discuss the semantic candidate lifecycle and its adapters.
2. Review identity/session and cost/audit proposals.
3. Prototype one public-data workflow: candidate, validation, human approval, immutable release, permission check, cost admission, execution, and correlated audit record.
4. Evaluate against held-out cases before describing the workflow as implemented.
