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

