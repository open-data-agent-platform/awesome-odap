# Ecosystem Update: November 2026

Date: 2026-11-07. Scope: the `awesome-odap` ecosystem README, its informative
design index and evidence review, and the changelog. This is not a Gateway
software release or a change to the Five Musts, Manifesto or Scoring Rubric.

## Changes

- Replace the historical v0.1.0-only implementation disclaimer with a link to
  the published Gateway preview's scope and open requirements.
- Add Apache Ossie (incubating), formerly Open Semantic Interchange (OSI), as
  a semantic interchange effort, without claiming Gateway compatibility.
- Add a separate context section for MCP, A2A and NVIDIA's experimental
  cluster-operations blueprint. These are different layers, not a conformance
  ranking or a list of equivalent products.
- Label Gateway and GoogleSQL.com as maintainer-affiliated and retain the
  separately disclosed CloudSystem Association adoption statement.
- Track the six historical Landscape entries in
  [issue #10](https://github.com/open-data-agent-platform/awesome-odap/issues/10),
  without changing their scores, assigning reviewers or inventing deadlines.

## Reviewed Sources

The following were checked against official sources on November 7. The existing
[November ecosystem review](ecosystem-review-2026-11.md) retains the earlier
November 5 evidence separately; this update does not claim to retest those products.

| Source | Evidence used | Boundary |
|---|---|---|
| [Apache Ossie README at `8437d59`](https://github.com/apache/ossie/blob/8437d59e07c7fa56fcabc4a16d46864fd88d7313/README.md) and [license](https://github.com/apache/ossie/blob/8437d59e07c7fa56fcabc4a16d46864fd88d7313/LICENSE) | The official repository now identifies the project as Apache Ossie (incubating), formerly OSI, with a JSON/YAML semantic specification under Apache-2.0. | Use the canonical repository, not outdated future-tense foundation-governance claims. No release-date or market-leadership claim is needed for this entry. |
| [MCP introduction](https://modelcontextprotocol.io/docs/getting-started/intro) | An open protocol connecting AI applications with external tools and data sources. | Communication support is not proof of complete data governance. |
| [A2A official documentation](https://a2a-protocol.org/) | An open protocol for interoperability and collaboration between agents. | Do not conflate it with a semantic model specification or Gateway authorization. |
| [NVIDIA AI Factory Operations Agent README at `307afa7`](https://github.com/NVIDIA/AI-Factory-Operations-Agent/blob/307afa7e485a334389b4f2c8c68128fbf84ff007/README.md) and [license](https://github.com/NVIDIA/AI-Factory-Operations-Agent/blob/307afa7e485a334389b4f2c8c68128fbf84ff007/LICENSE) | An experimental Apache-2.0 cluster-operations blueprint with inspection and isolated command workflows. | It also has module-specific credentials and optional integrations; not every operation is read-only. Published installation artifacts currently require early-access permission. This is not NVIDIA Auto Ontology or a completed Gateway integration. |
| [Gateway v0.2.0-preview.1 release](https://github.com/open-data-agent-platform/odap-gateway/releases/tag/v0.2.0-preview.1) | The prerelease describes semantic review, contracts, delegation/budget admission, receipts and authentication boundaries. | It explicitly leaves deployment setup and independent acceptance requirements open; test counts are not scores. |
| [Gateway README at `f79db3e`](https://github.com/open-data-agent-platform/odap-gateway/blob/f79db3e1c27252cb11ef416e04764d8e4d1283aa/README.md) | Current main already describes the preview and governed objects. | Do not repeat the outdated claim that it still presents only v0.1.x. Main-branch additions after the release tag must not be attributed to that fixed release. |
| [Historical Gateway clause mapping](https://github.com/open-data-agent-platform/odap-gateway/blob/af57728ffbe96f67dcf5341582b8ac04307cba54/ODAP_CLAUSE_MAPPING.md) | The mapping explicitly states July 2026 / v0.1.0 as its review baseline. | Retained as historical evidence, not presented as a fresh preview evaluation. |

The distinction between semantic interchange, communication, runtime isolation
and in-path governance is explanatory, not a new definition or amendment of the
Semantic Must. The unchanged [Rubric](../RUBRIC.md) remains the scoring authority.
Listing OSI/Ossie does not claim that Gateway implements its importer or exporter.

## Follow-up Boundaries

- Keep primary-source evidence in the existing ecosystem review rather than
  maintaining a second competing review queue.
- Reassess the dated Landscape entries individually through issue #10 and the
  existing public challenge process. Preserve the historical image and reports.
- Review Gateway's implementation-to-clause mapping separately in its own
  repository. Documentation updates must not silently revise scores or be
  represented as independent acceptance.
- Any proposed normative compatibility wording belongs in a separate public
  discussion. This update makes no normative proposal or change.

No product was installed, no live business data was queried, and no website,
Cloudflare deployment, Gateway code or database was changed by this update.
