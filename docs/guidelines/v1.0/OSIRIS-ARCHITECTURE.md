# OSIRIS JSON Architecture Development Guidelines<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 04 February 2026 |
| Last revision date | 04 February 2026 |
| Status    | Draft |
| Specification ID | OSIRIS-1.0 |
| Specification URI | [OSIRIS-1.0](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) |
| Schema URI | [OSIRIS-1.0](https://osirisjson.org/schema/v1.0/osiris.schema.json) |
| License   | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Repository | [github.com/osirisjson/osiris](https://github.com/osirisjson/osiris) |


# Table of Content
<!-- wip will be done later -->


# 1 Introduction
This document defines the architectural foundation for the OSIRIS ecosystem. It establishes the patterns, principles and repository structures required to build and evolve the OSIRIS core **toolbox**, **producers** and **extensions** in a sustainable and scalable way.

These Architecture Development Guidelines prioritize:

| Priority | Description |
|---|---|
| Modularity | Components are independent and composable, enabling incremental adoption and isolated evolution |
| Consistency | Shared conventions and interfaces across producers and tools reduce ecosystem fragmentation |
| Quality | Robust validation, reproducible fixtures and CI-driven regression coverage |
| Developer experience | Clear APIs, predictable workflows and documentation that makes contributors productive quickly |
| Extensibility | A stable baseline for adding producers, integrations and validation capabilities without breaking existing workflows |

---

## 1.1 Developer navigation matrix
This document is intentionally lean and focuses on ecosystem **contracts**, **boundaries** and **architectural rules**.  
Implementation details live in focused guides and repository READMEs close to the code.

Use the matrix below to find the right entry point.

| If you are | Reference URI | Will help you |
|---|---|---|
| **New to OSIRIS** | [OSIRIS-ARCHITECTURE](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) | Understand the “why”, ecosystem boundaries and non-negotiable rules |
| Implementing validation logic (rules) | [OSIRIS-VALIDATION-LEVELS](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-VALIDATION-LEVELS.md) | Understand structural/semantic/domain validation and how to add or evolve rules safely |
| Working on a producer (vendor integration) | [OSIRIS-PRODUCER-GUIDELINES](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-PRODUCER-GUIDELINES.md) | Map vendor/platform data into OSIRIS consistently (IDs, resources, relationships, fixtures) |
| Working on editor features (VS Code/VSCodium) | [OSIRIS-EXTENSION-GUIDELINES](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-EXTENSION-GUIDELINES.md) | Consume `@osirisjson/core` diagnostics, implement UX patterns and keep editor performance predictable |
| Working on versioning/releases | [OSIRIS-VERSIONING-AND-RELEASES](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-VERSIONING-AND-RELEASES.md) | Understand compatibility rules, publishing workflow and release alignment with schema versions |
| Working on the CLI | [OSIRIS-TOOLBOX-CLI](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE-CLI.md) | Understand command orchestration, output formats and CI-friendly behaviors |
| Working on validation engine internals | [OSIRIS-TOOLBOX-CORE](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE-CORE.md) | Understand schema loading, rule execution pipeline, diagnostics model and performance constraints |
| Working on producer SDK internals | [OSIRIS-TOOLBOX-SDK](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE-SDK.md) | Understand producer base classes, mapping helpers, ID strategy and shared utilities |
| Unsure where your change belongs | Section “Repository boundaries” in this document | Choose the correct repo/package to avoid duplication and dependency violations |
| Working on the spec/schema itself | [OSIRIS specification](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) | Understand the OSIRIS spec, schema and normative examples |
| Contributing rules & governance | [OSIRIS community](https://osirisjson.org/docs/en/get-involved/community) | Understand how you can contribute to OSIRIS |

---

## 1.2 Architectural principles
### 1.2.1 Single source of truth
OSIRIS is designed to avoid fragmentation. The ecosystem must agree on **one** canonical definition of the format and **one** canonical implementation of validation behavior.

| Single source of truth sources | Non-negotiable rules |
|---|---|
| **OSIRIS specification** in the [OSIRIS](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) repository and served through [osirisjson.org](https://osirisjson.org/docs/en/spec/v10/00-preface) are the authoritative definition of OSIRIS v1.0 | Producers **MUST NOT** store manipulated copies or add incompatible interpretations of the specification |
| **OSIRIS core schema** in the [OSIRIS](https://github.com/osirisjson/osiris/tree/main/schema/v1.0/osiris.schema.json) repository and served through [osirisjson.org](https://osirisjson.org/schema/v1.0/osiris.schema.json) are the authoritative schema for OSIRIS v1.0 | Producers **MUST NOT** store manipulated copies or add incompatible interpretations of the core schema |
| **OSIRIS schema endpoints** (e.g. `/schema/v1.0/`) are canonical for tooling resolution and editor integration | Tooling **SHOULD** prefer `$schema` for resolution when available |
| **OSIRIS validation engine** (`@osirisjson/core`) is the canonical implementation of validation behavior and diagnostic formatting | CLI and editor integrations **MUST NOT** re-implement validation logic that belongs to `@osirisjson/core` |

**Rationale:** a document validated in CI should behave the same in VS Code, in the CLI and in any consumer embedding `@osirisjson/core`. Tooling should work offline (schema may be bundled), but the endpoint remains canonical for versioned resolution.


### 1.2.2 Separation of concerns
OSIRIS splits the ecosystem into clear responsibilities to keep scaling sustainable.

| Roles | Boundaries |
|---|---|
| **Producers** translate source inventories (cloud APIs, on-prem discovery, OT platforms) into **OSIRIS documents** | Producers focus on mapping and data hygiene (identity stability, normalization, redaction) |
| **Core validation** checks documents and emits **diagnostics** (structural, semantic, domain) | `@osirisjson/core` focuses on validation and diagnostics (no vendor APIs, no editor UI, no network calls) |
| **Consumers** (CLI, editors, other tooling) present or act on diagnostics and transform/visualize documents | Consumers focus on UX and automation while delegating validation behavior to `@osirisjson/core` |

**Rationale:** when each layer only does its job, the ecosystem remains predictable and contributors know where changes belong.

---