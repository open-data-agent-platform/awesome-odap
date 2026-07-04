# ODAP Scoring Rubric v1.0
# ODAP 评分细则标准 v1.0

**Status:** Public standard · **Version:** 1.0 · **Date:** July 2026
**Maintained by:** the Open Data Agent Platform community · github.com/open-data-agent-platform
**Normative basis:** [The ODAP Manifesto — The Five Musts](https://googlesql.com/odap/manifesto)

---

# ENGLISH VERSION

## How to Use This Rubric

Each of the Five Musts is decomposed into 3–4 assessable clauses. Each clause is scored on a three-anchor scale:

- **1 = Weak** — capability absent or purely manual
- **3 = Partial** — capability exists but is optional, incomplete, or not enforced by default
- **5 = Strong** — capability is enforced by default, in the path of every query

**Dimension score** = average of its clause scores, rounded to the nearest integer.
**Total score** = sum of the five dimension scores (maximum 25).

A platform qualifies as an ODAP when **every dimension scores ≥ 3**. A total of 25 indicates full ODAP readiness; no platform assessed to date reaches it.

Scoring is editorial and evidence-based: public documentation, product demos, and vendor disclosures. Any score may be challenged by filing a GitHub issue citing the relevant clause.

---

## MUST 1 — OPEN

*The gateway must speak open formats and heterogeneous sources. A gateway that locks you in is a wall with a toll booth.*

**O1.1 — Open table format support.** Does the platform natively read/write open table formats (Apache Iceberg, Delta Lake, Apache Hudi)?
- 1 = No open format support; proprietary storage only
- 3 = Read-only support, or one format via connector
- 5 = Native read/write across two or more open formats

**O1.2 — Heterogeneous source connectivity.** Can it query warehouses, lakes, object storage, and operational databases?
- 1 = Single source/engine only
- 3 = Multiple sources within one vendor ecosystem
- 5 = Cross-vendor federation including on-prem and object storage

**O1.3 — Exit cost.** Can semantic definitions, policies, and audit history be exported in open formats?
- 1 = No export; definitions locked to platform
- 3 = Partial export (e.g., SQL only, not policies)
- 5 = Full export of semantics, policies, and audit logs in documented open formats

**O1.4 — Protocol openness.** Does it expose open APIs/protocols (e.g., open catalogs, standard SQL interfaces) for third-party agents?
- 1 = Proprietary SDK only
- 3 = Documented REST API, vendor-specific
- 5 = Standards-based interfaces third parties can implement against

## MUST 2 — SEMANTIC

*Agents must query meaning, not tables. Without a semantic layer, a thousand agents produce a thousand truths.*

**S2.1 — Metric definition.** Can business metrics be defined once, in business language, by metric owners?
- 1 = No semantic layer; agents see raw schema
- 3 = Technical semantic layer (YAML/code) maintained by engineers only
- 5 = Business-editable definitions with ownership and versioning

**S2.2 — Consistent resolution.** Does the same business question resolve to the same logic for every agent and user?
- 1 = Each query generated independently from raw schema
- 3 = Consistent within one tool; not shared across consumers
- 5 = One definition resolves identically across all agents, tools, and departments

**S2.3 — Context awareness.** Does resolution incorporate time, entity, dimension, and policy context (e.g., "last quarter," "our EU customers")?
- 1 = Literal translation only
- 3 = Time/dimension handling with manual disambiguation
- 5 = Dynamic contextual resolution with explicit assumptions surfaced to the user

**S2.4 — Complex model support.** Does the semantic layer handle nested/complex types (STRUCT, ARRAY) without flattening?
- 1 = Flat tables only
- 3 = Partial nested support with workarounds
- 5 = Native nested-model semantics

## MUST 3 — GOVERNED

*Governance is enforced in the path of the query — not reported after the damage.*

**G3.1 — Pre-generation permission resolution.** Are user/agent permissions resolved before SQL is generated (not just at execution)?
- 1 = Permissions checked only at database level
- 3 = Checked at execution time by the platform
- 5 = Resolved before generation; agents cannot even draft queries against forbidden objects

**G3.2 — Approval routing.** Can sensitive or high-cost operations be routed to human reviewers before execution?
- 1 = No approval concept
- 3 = Manual/optional approval workflows
- 5 = Policy-driven automatic routing with configurable thresholds

**G3.3 — Pipeline separation.** Are planning, generation, validation, and execution separated stages with independent controls?
- 1 = Single-step generate-and-run
- 3 = Validation exists but can be bypassed
- 5 = Enforced staged pipeline; no stage skippable by the agent

**G3.4 — Sandbox validation.** Can generated SQL be validated against masked/sample data before touching production?
- 1 = No sandbox
- 3 = Manual sandbox available
- 5 = Automatic sandbox validation for defined risk classes

## MUST 4 — AUDITABLE

*"The AI did it" is not an acceptable line in an audit report. Work must be shown. Always.*

**A4.1 — Generation trail.** Is every generated query logged with its prompt, context, and producing agent identity?
- 1 = Only executed SQL logged
- 3 = Prompt + SQL logged; agent identity partial
- 5 = Full chain: prompt → context → agent → SQL → result, attributable and immutable

**A4.2 — Decision trail.** Are approvals, rejections, policy decisions, and rollbacks recorded with actor and timestamp?
- 1 = Not recorded
- 3 = Recorded for some action types
- 5 = All governance decisions recorded and queryable

**A4.3 — Explainability.** Does every answer expose its reasoning — tables used, joins chosen, confidence, and validation evidence?
- 1 = Answer only
- 3 = SQL visible on request
- 5 = Work shown by default: lineage, confidence score, and validation status per answer

**A4.4 — Replayability.** Can an auditor reconstruct and re-execute any historical query decision?
- 1 = No
- 3 = Reconstruction possible with manual effort
- 5 = One-click replay with original context preserved

## MUST 5 — COST-CONTROLLED

*When machines can spend money at machine speed, cost control at human speed is no control at all.*

**C5.1 — Pre-execution estimation.** Is every query estimated (bytes scanned, cost, runtime) before execution?
- 1 = No estimation
- 3 = Estimation available on request or for some engines
- 5 = Mandatory dry-run estimate on every query by default

**C5.2 — Budget enforcement.** Are over-budget queries blocked, or merely reported?
- 1 = Post-hoc cost reporting only
- 3 = Alerts issued but execution proceeds
- 5 = Automatic circuit-breaker with an approval-based exception path

**C5.3 — Attribution granularity.** Can cost be attributed to the specific agent, user, and query?
- 1 = Account-level only
- 3 = User-level
- 5 = Per-query and per-agent attribution

**C5.4 — Optimization loop.** Does the platform propose cheaper equivalents for expensive queries?
- 1 = No
- 3 = Static best-practice hints
- 5 = Automated rewrite suggestions with estimated savings

---

## Scoring Sheet Template

| Clause | Score (1/3/5) | Evidence / Source |
|---|---|---|
| O1.1 – O1.4 | | |
| S2.1 – S2.4 | | |
| G3.1 – G3.4 | | |
| A4.1 – A4.4 | | |
| C5.1 – C5.4 | | |

**Dimension score** = round(mean of clauses) · **Total** = Σ dimensions · **ODAP-qualified** = all dimensions ≥ 3

## Challenge Process

1. Open a GitHub issue titled `[Score Challenge] <Vendor> — <Clause ID>`
2. Cite the clause, the current score, and public evidence for a different score
3. Maintainers respond within 14 days; accepted challenges update the next Landscape revision with credit to the challenger

## Versioning

Clauses may be added or refined in minor versions (v1.x). The Five Musts themselves change only in major versions and only with community consensus.

---
---

# 中文版

## 使用方法

五个必须各分解为 3–4 条可评估条款，每条按三档锚点打分：

- **1 = 弱** — 能力缺失或纯手工
- **3 = 部分** — 能力存在但可选、不完整或非默认强制
- **5 = 强** — 默认强制执行，位于每条查询的必经之路上

**维度得分** = 该维度条款均值，四舍五入取整。**总分** = 五个维度之和（满分25）。

平台**每个维度均 ≥ 3 分**方可称为 ODAP。总分25代表完全就绪；截至目前尚无平台达到。

评分为编辑性、基于证据：公开文档、产品演示与厂商披露。任何分数均可通过 GitHub issue 引用相应条款提出挑战。

## 必须一 —— 开放（OPEN）

*入口必须支持开放格式与异构数据源。把你锁死的入口不是入口，是收过路费的墙。*

**O1.1 开放表格式支持** — 是否原生读写开放表格式（Iceberg、Delta、Hudi）？
1=不支持，仅私有存储 · 3=只读或仅一种格式经连接器 · 5=两种以上格式原生读写

**O1.2 异构源连接** — 能否查询数仓、数据湖、对象存储与业务数据库？
1=仅单一源 · 3=单一厂商生态内多源 · 5=跨厂商联邦，含本地部署与对象存储

**O1.3 退出成本** — 语义定义、策略与审计历史能否以开放格式导出？
1=不可导出 · 3=部分导出（仅SQL不含策略） · 5=语义/策略/审计日志以文档化开放格式完整导出

**O1.4 协议开放性** — 是否为第三方智能体暴露开放API/协议？
1=仅私有SDK · 3=有文档的厂商专属REST API · 5=第三方可对接的标准化接口

## 必须二 —— 语义化（SEMANTIC）

*智能体查询的必须是业务含义。没有语义层，一千个智能体产出一千个真相。*

**S2.1 指标定义** — 业务指标能否由指标主人用业务语言一次定义？
1=无语义层，直接暴露物理表 · 3=仅工程师维护的技术语义层 · 5=业务可编辑、有归属与版本管理

**S2.2 一致解析** — 同一业务问题对所有智能体是否解析出同一逻辑？
1=每次从物理表独立生成 · 3=单工具内一致，跨消费方不共享 · 5=一处定义，全智能体/全部门解析一致

**S2.3 上下文感知** — 解析是否融合时间、实体、维度与策略上下文？
1=仅字面翻译 · 3=支持时间/维度但需手动消歧 · 5=动态上下文解析并向用户显式呈现假设

**S2.4 复杂模型支持** — 语义层能否不压平地处理嵌套类型（STRUCT/ARRAY）？
1=仅平面表 · 3=部分支持需变通 · 5=原生嵌套模型语义

## 必须三 —— 受治理（GOVERNED）

*治理在查询的必经之路上执行——而不是损失之后出报告。*

**G3.1 生成前权限解析** — 权限是否在SQL生成之前（而非仅执行时）解析？
1=仅数据库层校验 · 3=平台在执行时校验 · 5=生成前解析，智能体对无权对象连草稿都写不出

**G3.2 审批路由** — 敏感或高成本操作能否在执行前路由给人工审核？
1=无审批概念 · 3=手动/可选审批流 · 5=策略驱动自动路由，阈值可配置

**G3.3 流水线分离** — 规划、生成、校验、执行是否为带独立控制的分离阶段？
1=一步生成即运行 · 3=有校验但可绕过 · 5=强制分阶段，智能体无法跳过任何一段

**G3.4 沙箱验证** — 生成的SQL能否先在脱敏/抽样数据上验证再触碰生产？
1=无沙箱 · 3=有手动沙箱 · 5=对定义的风险类别自动沙箱验证

## 必须四 —— 可审计（AUDITABLE）

*"是AI干的"不是合格审计报告里允许出现的句子。过程必须可见，永远。*

**A4.1 生成链路** — 每条生成的查询是否连同提示词、上下文与产出智能体身份一并记录？
1=仅记录已执行SQL · 3=记录提示词+SQL，身份不全 · 5=完整链路：提示词→上下文→智能体→SQL→结果，可归因不可篡改

**A4.2 决策链路** — 审批、驳回、策略决定与回滚是否连同操作者与时间戳记录？
1=不记录 · 3=部分动作类型记录 · 5=全部治理决策在案且可查询

**A4.3 可解释性** — 每个答案是否默认展示推理过程（所用表、连接选择、置信度、验证证据）？
1=只给答案 · 3=SQL可按需查看 · 5=默认展示：血缘、置信度评分与验证状态

**A4.4 可回放性** — 审计员能否重建并重放任一历史查询决策？
1=不能 · 3=可手动重建 · 5=一键重放，原始上下文完整保留

## 必须五 —— 成本可控（COST-CONTROLLED）

*当机器能以机器速度花钱时，人类速度的成本管控等于没有管控。*

**C5.1 执行前估算** — 每条查询是否在执行前估算扫描量/费用/时长？
1=无估算 · 3=按需或仅部分引擎可估算 · 5=每条查询默认强制dry-run估算

**C5.2 预算强制** — 超预算查询是被熔断拦截还是仅告警？
1=仅事后成本报表 · 3=告警但照常执行 · 5=自动熔断+基于审批的例外通道

**C5.3 归因粒度** — 成本能否归因到具体智能体、用户与单条查询？
1=仅账户级 · 3=用户级 · 5=单查询+单智能体级归因

**C5.4 优化闭环** — 平台是否为高成本查询主动建议更廉价的等价写法？
1=无 · 3=静态最佳实践提示 · 5=自动改写建议并附节省估算

## 挑战流程

1. 提交 GitHub issue，标题格式 `[Score Challenge] <厂商> — <条款编号>`
2. 引用条款、当前分数及支持不同分数的公开证据
3. 维护者14日内回应；被采纳的挑战将更新至下一版 Landscape 并署名致谢挑战者

## 版本管理

条款可在小版本（v1.x）中增补细化；五个必须本身仅在大版本中、且需社区共识方可变更。

---

*ODAP Scoring Rubric v1.0 · CC0 · github.com/open-data-agent-platform*
