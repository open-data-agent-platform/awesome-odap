# Awesome ODAP

> A curated list of tools, standards, research, and product patterns around the
> emerging Open Data Agent Platform category.

An **Open Data Agent Platform (ODAP)** is a governed AI data platform that lets
people and software agents ask questions over open and enterprise data using
natural language, while still preserving semantic definitions, SQL review,
access control, lineage, cost awareness, and auditability.

ODAP is not a single product. It is a category that combines open data access,
semantic context, agentic SQL generation, persistent memory, and governance.

## Professional Reports

- [ODAP Landscape 2026 English Report](reports/ODAP_Landscape_2026_English_Report.pdf)
- [ODAP Landscape 2026 Chinese Report](reports/ODAP_Landscape_2026_Chinese_Report.pdf)
- [ODAP professional report page on GoogleSQL.com](https://googlesql.com/odap#professional-reports)

## Why This List Exists

There is no widely maintained encyclopedia entry for "Open Data Agent Platform"
yet. This repository collects the ecosystem around the term in a neutral,
vendor-aware way. Including adjacent and competing tools is intentional: a
category becomes more useful when the landscape is mapped, not when one vendor
pretends to be the only participant.

## Category Definition

An ODAP should usually include five layers:

1. **Universal Data Gateway** - connects warehouses, databases, object storage,
   and open table formats.
2. **Smart Semantic Layer** - maps business language to metrics, dimensions,
   entities, policies, and physical schemas.
3. **Multi-Agent SQL Engine** - plans, generates, validates, repairs, and
   explains SQL or analytical workflows.
4. **Persistent Memory** - learns from user corrections, query history,
   preferred definitions, and organizational context.
5. **Governance & Observability** - provides audit logs, access controls, cost
   controls, quality scoring, sandboxing, and lineage.

## Reference Architecture

```text
User or Agent
    |
Natural-language request
    |
Semantic context + policy + memory
    |
Planner agent -> SQL generator -> validator -> executor -> reviewer
    |
Open data / warehouses / lakehouse tables / APIs
    |
Answer + table + chart + confidence + audit trail
```

## Open Table Formats

- [Apache Iceberg](https://iceberg.apache.org/) - Open table format for large
  analytic datasets, with schema evolution, partition evolution, time travel,
  and multi-engine support.
- [Delta Lake](https://delta.io/) - Open source storage layer for reliable data
  lakes, widely used in lakehouse architectures.
- [Apache Hudi](https://hudi.apache.org/) - Open source data lake platform for
  incremental processing, transactional data lakes, and streaming ingestion.

## Query Engines and Lakehouse Execution

- [Trino](https://trino.io/) - Distributed SQL query engine for big data and
  federated analytics across heterogeneous sources.
- [PrestoDB](https://prestodb.io/) - Distributed SQL query engine originally
  developed at Facebook and now governed by the Presto Foundation.
- [DuckDB](https://duckdb.org/) - In-process analytical database often used for
  local analytics, data apps, and agent sandboxes.
- [Apache Spark](https://spark.apache.org/) - Large-scale data processing engine
  used across lakehouse, ETL, and analytical workloads.
- [ClickHouse](https://clickhouse.com/) - Fast columnar database for analytical
  workloads.

## Metadata, Catalog, and Semantic Context

- [OpenMetadata](https://open-metadata.org/) - Open source context layer for
  metadata, business semantics, lineage, collaboration, and AI agent context.
- [DataHub](https://datahubproject.io/) - Open source metadata platform for data
  discovery, observability, and governance.
- [dbt Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl)
  - Semantic layer for defining and querying business metrics in dbt.
- [Cube](https://cube.dev/) - Semantic layer and agentic analytics platform for
  governed metrics and embedded analytics.
- [MetricFlow](https://github.com/dbt-labs/metricflow) - Semantic layer query
  engine used by dbt Labs.

## Agent Orchestration and AI Frameworks

- [LangGraph](https://www.langchain.com/langgraph) - Agent orchestration
  framework for reliable, controllable, stateful agents.
- [CrewAI](https://crewai.com/) - Platform and framework for building,
  deploying, governing, and optimizing multi-agent workflows.
- [LlamaIndex](https://www.llamaindex.ai/) - Framework for connecting LLM
  applications and agents to enterprise data.
- [DSPy](https://github.com/stanfordnlp/dspy) - Framework for programming and
  optimizing language model pipelines.
- [AutoGen](https://github.com/microsoft/autogen) - Framework for multi-agent AI
  applications.

## Vector Memory and Retrieval

- [Qdrant](https://qdrant.tech/) - Vector database and similarity search engine.
- [Weaviate](https://weaviate.io/) - AI-native vector database for semantic
  search, RAG, and agent memory.
- [Milvus](https://milvus.io/) - Open source vector database for scalable
  similarity search.
- [pgvector](https://github.com/pgvector/pgvector) - PostgreSQL extension for
  vector search.

## Natural-Language Analytics and GenBI

- [Wren AI](https://www.getwren.ai/) - Open-source agentic GenBI platform with
  text-to-SQL, governed execution, semantic modeling, connectors, and memory.
- [Databricks Genie Agents](https://www.databricks.com/product/genie/agents) -
  Conversational analytics agents for business users on the Databricks
  platform.
- [Looker Conversational Analytics](https://docs.cloud.google.com/looker/docs/conversational-analytics-overview)
  - Natural-language analytics experience in Looker.
- [Tableau Next](https://www.tableau.com/products/tableau-next) - Agentic
  analytics and AI-powered data experience from Tableau.
- [Snowflake Cortex AI](https://www.snowflake.com/en/product/features/cortex/)
  - AI features inside Snowflake for natural language, analytical, and
  enterprise AI workflows.
- [MindsHub / MindsDB](https://mindshub.ai/) - Open-source AI agents and
  workspace patterns for working across connected apps and data.

## Governance, Observability, and Quality

- [OpenTelemetry](https://opentelemetry.io/) - Open observability framework for
  traces, metrics, and logs.
- [Prometheus](https://prometheus.io/) - Monitoring and alerting toolkit.
- [Grafana](https://grafana.com/) - Observability dashboards and visualization.
- [Great Expectations](https://greatexpectations.io/) - Data quality validation
  and documentation.
- [Soda](https://www.soda.io/) - Data quality monitoring and testing.
- [OpenLineage](https://openlineage.io/) - Open standard for data lineage.

## Reference Products and Implementations

- [Open Data Agent Platform](https://googlesql.com/) - A product direction
  focused on natural-language access to open data, semantic SQL generation,
  validation, persistent memory, and governance.
- [Wren AI](https://www.getwren.ai/) - A strong adjacent open-source GenBI
  implementation with context, semantic modeling, policies, memory, and
  governed text-to-SQL.
- [Databricks Genie Agents](https://www.databricks.com/product/genie/agents) -
  A major commercial implementation of conversational analytics in a lakehouse
  environment.

## Evaluation Checklist

Use this checklist to evaluate whether a product belongs in the ODAP category:

- Does it connect to open data sources, warehouses, lakehouse tables, or object
  storage?
- Does it preserve business semantics rather than only generating SQL from raw
  schema?
- Does it separate planning, generation, validation, execution, and review?
- Does it remember corrections and user feedback?
- Does it enforce permissions, policies, and audit logs?
- Does it estimate or control query cost?
- Does it expose answer confidence, lineage, or validation evidence?
- Does it support human review or sandbox execution before production actions?

## Related Terms

- Agentic analytics
- Conversational analytics
- GenBI
- Text-to-SQL
- Semantic layer
- Metrics layer
- Data catalog
- Lakehouse
- Open table format
- Data governance
- AI data agent

## Contributing

Pull requests are welcome. Please keep descriptions neutral and factual.

To add a project, include:

- Name
- Link to official website or repository
- One-sentence neutral description
- Which ODAP layer it supports
- License or commercial status when known

## License

This list is released under the Creative Commons Zero v1.0 Universal license.

