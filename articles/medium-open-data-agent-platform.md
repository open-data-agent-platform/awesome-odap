# Open Data Agent Platform: A Definition for the AI Data Stack

Business users do not want to learn table names. Data engineers do not want AI
systems inventing metrics. Analysts do not want to debug unreviewed SQL written
by a chatbot. Executives do not want three departments to get three different
answers to the same question.

That tension is exactly why the **Open Data Agent Platform** category is
emerging.

## The Definition

An **Open Data Agent Platform (ODAP)** is a governed data platform that lets
humans and AI agents ask questions over open and enterprise data using natural
language, while preserving semantic definitions, SQL validation, access control,
lineage, cost awareness, memory, and auditability.

The phrase matters because "chat with data" is too vague. The real enterprise
problem is not conversation. The real problem is governed execution.

## Why "Open" Matters

The modern analytical stack is becoming more open at the storage and table
format layer. Apache Iceberg, Delta Lake, and Apache Hudi let teams manage large
analytical tables with more portability than older warehouse-only patterns.
Query engines such as Trino, Spark, and DuckDB make it possible to run analysis
across multiple environments.

ODAP assumes the future is not a single database. It assumes the agent must work
across warehouses, lakehouses, object storage, catalogs, semantic layers, and
business applications.

## Why "Agent" Matters

An agent does more than answer a prompt. It plans, calls tools, checks its work,
and changes behavior based on feedback.

For data, that means the system should be able to:

- understand the business question
- map the question to governed metrics and dimensions
- generate SQL or an analytical plan
- validate syntax, permissions, and cost
- execute safely
- repair failures
- explain the answer
- remember corrections for future use

That is a very different design from a single text-to-SQL model.

## Why "Platform" Matters

A platform owns the hard parts around the model.

In ODAP, those hard parts are:

- connectors
- metadata
- semantics
- policy
- query validation
- execution safety
- observability
- feedback loops
- human review

The model is important, but the model is not the product. The product is the
controlled path from a question to a trusted answer.

## The Five-Layer Architecture

### Universal Data Gateway

ODAP starts by connecting to open and enterprise data: S3, Azure Blob, MinIO,
warehouses, databases, APIs, and lakehouse tables.

### Smart Semantic Layer

The semantic layer turns business language into governed definitions. "Revenue"
is not just a column. It is a definition, a formula, a policy, a grain, and
often a political agreement.

### Multi-Agent SQL Engine

A serious ODAP separates planning, generation, validation, repair, and review.
This makes the workflow observable and easier to improve.

### Persistent Memory

The platform should learn from corrections, query history, feedback scores,
approved patterns, and company-specific language.

### Governance and Observability

Every action should be traceable. Query cost, permissions, lineage, quality, and
sandbox execution are not extras. They are category requirements.

## Adjacent Categories

ODAP overlaps with several existing categories:

- BI
- semantic layers
- data catalogs
- text-to-SQL
- GenBI
- lakehouse platforms
- AI agent frameworks
- data observability

But none of those categories alone describes the full execution path.

## Competitive Landscape

Useful products and projects in this landscape include OpenMetadata, DataHub,
dbt Semantic Layer, Cube, LangGraph, CrewAI, LlamaIndex, Trino, DuckDB, Apache
Iceberg, Delta Lake, Apache Hudi, Wren AI, Databricks Genie Agents, Looker
Conversational Analytics, Tableau Next, and Snowflake Cortex AI.

The healthy move is to map the whole field. A category becomes credible when it
can name its neighbors and competitors.

## The Practical Test

If a platform can let a business user ask a question in natural language, route
that question through governed semantics, generate and validate SQL, execute
safely over open data, remember corrections, and produce an auditable answer,
then it belongs in the Open Data Agent Platform category.

If it only generates SQL, it is a feature.

If it governs the full path from question to trusted answer, it is a platform.

## Links

- Awesome ODAP: https://github.com/bestchatgpt8/awesome-odap
- Open Data Agent Platform: https://googlesql.com/
