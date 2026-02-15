# OSIRIS JSON Producer Guidelines<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 08 February 2026 |
| Last revision date | 15 February 2026 |
| Status    | Draft |
| Document ID | OSIRIS-ADG-PR-1.0 |
| Document URI | [OSIRIS-ADG-PR-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-PRODUCER-GUIDELINES.md) |
| Document Name | OSIRIS JSON Producer Guidelines |
| Specification ID | OSIRIS-1.0 |
| Specification URI | [OSIRIS-1.0](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) |
| Schema URI | [OSIRIS-1.0](https://osirisjson.org/schema/v1.0/osiris.schema.json) |
| License   | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Repository | [github.com/osirisjson/osiris](https://github.com/osirisjson/osiris) |

# Table of Content
<!-- work in progress -->



# 1 The producer mindset
An OSIRIS producer (also commonly named a **parser**) is any component that reads a vendor device or platform inventory and emits an OSIRIS JSON document. OSIRIS Producers are the bridge between proprietary data sources and the OSIRIS Open Standard interchange format. Their quality determines whether downstream consumers (validators, diagram engines, CMDBs, diff tools etc.) can trust the exported snapshot at a point in time.

This guide defines the mapping contract that every producer **MUST** honor. It is intentionally language-agnostic: the rules apply whether the producer is written in Go (recommended for first-party producers), Python, Rust, C or any other high or low-level language. Implementation-level patterns (transport, concurrency, SDK helpers) belong in the Go producer SDK documentation, not here.

> [!NOTE]
> Back-reference: The ecosystem boundaries and canonical truth rule are defined in [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md).
> The validation model (levels, codes, profiles) is defined in [OSIRIS-ADG-VL-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-VALIDATION-LEVELS.md).
> Validation engine internals are defined in [OSIRIS-ADG-TLB-CORE-1.0](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-TOOLBOX-CORE.md).

---

## 1.1 Discovery vs. static definition
Producer implementations fall on a spectrum between two extremes.

| Mode | Description | Typical source | Example |
|---|---|---|---|
| **Live discovery** | The producer queries a live API, CLI, or protocol endpoint and builds the OSIRIS document from the response. | Cloud provider APIs, SNMP/LLDP/NETCONF, device CLIs via SSH | `osirisjson-producer azure`, `osirisjson-producer arista --ssh` |
| **Static ingestion** | The producer reads an existing export file (Terraform state, inventory dump, CMDB extract) and transforms resources it into OSIRIS. | JSON/YAML/CSV files, database exports, IaC state files | Terraform state > OSIRIS, ServiceNow CMDB CSV > OSIRIS |

Most real-world producers combine both: they discover live inventory for some resource types and fall back to static sources for others.

**Guidance:**
- Producers **SHOULD** document which mode they use for each resource type, including known limitations and required permissions.
- Producers **MUST NOT** assume they will discover the complete infrastructure. Partial inventories are valid OSIRIS documents; the scope **SHOULD** describe what was exported and what was not (see `metadata.scope`).
- Discovery failures for individual resources **SHOULD NOT** abort the entire export. Producers **SHOULD** continue collecting other resources, log the failure and reflect limitations in the emitted scope or tags.

---

## 1.2 Scope of responsibility (what NOT to map)
A producer execute a snapshot in time translating existing infrastructure into OSIRIS. It **MUST NOT** orchestrate, provision or mutate anything and:

| **MUST** | **SHOULD** | **MUST NOT** |
|---|---|---|
| Emit structurally valid OSIRIS JSON (passes Level 1 schema validation) | Map to standard OSIRIS types ([OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md#7-resource-type-taxonomy) Chapter 7 and Appendix C) before resorting to custom types | Re-implement validation logic. Canonical validation is `@osirisjson/core`. Producers validate their output by invoking the canonical engine (e.g. `npx @osirisjson/cli validate <file>`) in CI |
| Populate all required fields (`version`, `metadata.timestamp` and required resource/connection/group fields per the [OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md) specification Chapter 3-6) | Provide `metadata.generator` with a stable tool name and version | Invent vendor-specific interpretations of the [OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md#7-resource-type-taxonomy) specification or core [osiris.schema.json](https://github.com/osirisjson/osiris/blob/main/schema/v1.0/osiris.schema.json) |
| Generate stable, deterministic resource IDs | Describe export boundaries in `metadata.scope` | Emit data intended to provision, modify, or delete infrastructure. OSIRIS is a read-only snapshot format |
| Exclude credentials, secrets and authentication material (see Chapter 3) | Produce deterministic output for the same input | Guess or produce unknown values for unknown fields. If data is not available, omit the optional field entirely ([OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md#1115-partial-data-and-unknowns) specification section 11.1.5) |
