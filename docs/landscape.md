# Open Data Agent Platform Landscape

This landscape maps the ODAP category by functional layer.

## Professional Reports

- [ODAP Landscape 2026 English Report](../reports/ODAP_Landscape_2026_English_Report.pdf)
- [ODAP Landscape 2026 Chinese Report](../reports/ODAP_Landscape_2026_Chinese_Report.pdf)
- [ODAP professional report page on GoogleSQL.com](https://googlesql.com/odap#professional-reports)

## Scoring and Challenges

Landscape scoring is governed by the [ODAP Scoring Rubric v1.0](../RUBRIC.md).
Challenge any score via a
[GitHub issue](https://github.com/open-data-agent-platform/awesome-odap/issues)
with public evidence and the relevant clause ID.

## Landscape v1.2 — July 2026

| Vendor | Open | Semantic | Governed | Auditable | Cost-controlled | Total /25 |
|---|---|---|---|---|---|---|
| Databricks | 5 | 4 | 5 | 5 | 5 | **24** |
| Google Agentic Data Cloud | 4 | 4 | 5 | 4 | 5 | **22** |
| Starburst | 5 | 4 | 3 | 3 | 3 | **18** |
| Acceldata | 3 | 3 | 3 | 4 | 3 | **16** |
| TiDB Cloud (PingCAP) | 3 | 2 | 2 | 3 | 3 | **13** |
| Agent-first cloud platforms | 3 | 2 | 2 | 2 | 3 | **12** |

**What changed since v1.1:** Databricks recalibrated 23 → 24 following the
DAIS 2026 releases (Unity AI Gateway hard spend caps and pre-execution
approvals; see [#1]). TiDB Cloud added as the first new vendor since launch
(first assessment; see [#3]). No platform reaches 25 — the most consistent
industry-wide gap remains the Semantic dimension.

## Layer 1: Data Gateway

Purpose: connect to warehouses, object storage, databases, APIs, and lakehouse
tables.

Examples:

- Apache Iceberg
- Delta Lake
- Apache Hudi
- Trino
- Spark
- DuckDB
- Presto

## Layer 2: Semantic Context

Purpose: define business meaning, metrics, entities, dimensions, joins, owners,
lineage, and policy.

Examples:

- OpenMetadata
- DataHub
- dbt Semantic Layer
- Cube
- MetricFlow

## Layer 3: Agentic SQL and Analytics

Purpose: plan, generate, validate, repair, and explain analytical workflows.

Examples:

- LangGraph
- CrewAI
- LlamaIndex
- AutoGen
- Wren AI
- Databricks Genie Agents
- Looker Conversational Analytics
- Tableau Next
- Snowflake Cortex AI

## Layer 4: Memory

Purpose: preserve corrections, successful query patterns, feedback, and
organizational context.

Examples:

- Qdrant
- Weaviate
- Milvus
- pgvector
- OpenMetadata memories/context graph patterns
- LangGraph memory

## Layer 5: Governance and Observability

Purpose: make agentic data work auditable, cost-aware, secure, and reliable.

Examples:

- OpenTelemetry
- Prometheus
- Grafana
- OpenLineage
- Great Expectations
- Soda
- Data access control in catalogs, warehouses, and semantic layers

