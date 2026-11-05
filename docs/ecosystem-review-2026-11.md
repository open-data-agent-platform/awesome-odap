# Ecosystem Review: November 2026

Evidence reviewed: 2026-11-05. Informative documentation review; no product testing, score changes, or conformance certifications.

| Area | Official evidence | Follow-up |
|---|---|---|
| Semantic generation | [NVIDIA Auto Ontology](https://github.com/NVIDIA/auto-ontology), [Snowflake Autopilot and modeling](https://docs.snowflake.com/en/user-guide/views-semantic/best-practices-modeling), [Databricks YAML generation](https://docs.databricks.com/gcp/en/uc-semantics/metric-views/yaml-reference) | Assess ownership, approval, immutable releases, and drift handling individually. |
| Governed metric access | [dbt Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl) exposes metrics through MCP. | Examine identity and policy scope at the interface. |
| Tool governance | [Databricks agent tools](https://docs.databricks.com/aws/en/agents/agent-framework/agent-tool) | Distinguish transport support from effective authorization. |
| Session policy | [AWS identity propagation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/policy-session-based-temporal.html) | Test caller identity, scope, and session isolation. |
| Analytics context | [BigQuery conversational analytics](https://docs.cloud.google.com/bigquery/docs/conversational-analytics) | Review glossary ownership and verified queries. |
| Cost enforcement | [Snowflake resource budgets](https://docs.snowflake.com/en/en/user-guide/snowflake-cortex/cortex-agents-resource-budgets) describes periodic enforcement and possible overspend. | Separate delayed actions from pre-execution hard limits. |

## Historical claims requiring review

Reassess any statement that the entire industry lacks business-defined semantic metrics. Natural-language authoring, expert configuration, certification, and approval are distinct capabilities. Current documentation establishes reasons to revisit earlier claims, not automatic full compliance.

Keep existing Landscape artifacts dated. For each revised score, record product edition, region, feature status, official evidence, tested behavior if available, and the applicable rubric clause. Publish revisions with a changelog and challenge route.

## Proposed implementation sequence

1. Discuss the semantic candidate lifecycle and its adapters.
2. Review identity/session and cost/audit proposals.
3. Prototype one public-data workflow: candidate, validation, human approval, immutable release, permission check, cost admission, execution, and correlated audit record.
4. Evaluate against held-out cases before describing the workflow as implemented.
