# OSIRIS JSON Architecture Development Guidelines<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 04 February 2026 |
| Last revision date | 07 February 2026 |
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

# 2 Physical architecture
## 2.1 Ecosystem structure
### 2.1.1 Repository layout (Polyrepo vs monorepo)
OSIRIS is developed across multiple repositories under the `osirisjson` GitHub organization. This structure separates **canonical specification content** from **tooling**, **vendor producers** and **editor integrations**, keeping responsibilities clear and enabling the ecosystem to scale as more producers and tools are added.

OSIRIS generated documents are standard **JSON** files and are expected to use the `.json` extension; consumers and tooling detect OSIRIS content via schema metadata and/or document structure rather than a custom file extension.

```text
osirisjson/
├── .github
│   ├── ISSUE_TEMPLATE/
│   ├── workflows/
│   ├── PULL_REQUEST_TEMPLATE.md
│   ├── CODE_OF_CONDUCT.md
│   ├── COMMUNITY_GUIDE.md
│   ├── FUNDING.yml
│   ├── GOVERNANCE.md
│   ├── MEMBERS.md
│   ├── README.md
│   ├── SECURITY.md
│   └── SUPPORT.md
├── osiris                  # Canonical specification, schema sources, guidelines and normative examples
├── osiris-toolbox          # NPM packages: core validation, SDK and CLI
│   ├── core
│   ├── sdk
│   └── cli
├── osiris-producers        # Vendor-specific producers (parsers/exporters)
├── osiris-extensions       # VS Code/VSCodium extension and future integrations
└── osiris-mcp              # Future MCP server
```

### 2.1.2 Repository responsibilities

| osiris | osiris-toolbox | osiris-producers | osiris-extensions | osirisjson.org | .github | osiris-mcp |
|---|---|---|---|---|---|---|
| Canonical OSIRIS JSON specification aligned to released versions | OSIRIS JSON Core validation engine (structural, semantic and domain validation) | Vendor-specific producer implementations (e.g. Cisco NX-OS, Azure) | Real-time OSIRIS JSON validation powered by the shared validation engine | Documentation website and developer resources | Community health files, contribution templates and security/support policies | MCP server implementation for automation and ecosystem integrations |
| Canonical OSIRIS JSON core schema aligned to released versions | OSIRIS JSON Schema validation with enhanced, user-friendly error reporting | Shared producer utilities and fixtures where vendor-specific reuse is needed | Schema-aware autocomplete and navigation | Hosted schema endpoints and downloadable references aligned to released versions | Standardized issue and PR workflows across all repositories | - |
| Docs and guidelines to support development and project grow | Shared error model and diagnostics format consumed by CLI and editor integrations | Integration and regression test suites per producer | Rich diagnostics with actionable messages and quick fixes | - | - | - |
| Normative examples referenced by the specification | OSIRIS JSON TypeScript SDK for producer development (base classes, helpers, builders, ID strategy) | Producer documentation (configuration, permissions, examples) inside each producer package | Future integrations (e.g. draw.io export/preview workflows) | - | - | - |
| Change log and release notes | OSIRIS JSON CLI tool for validation and conversion workflows | - | - | - | - | - |
| - | Utility functions for OSIRIS JSON document manipulation and analysis | - | - | - | - | - |


#### Repository boundaries
Use these rules to avoid duplication and circular dependencies:


- Format definition changes (fields, semantics, namespaces, examples) belong in `osiris`
- Validation behavior changes (rule logic, diagnostics shape) belong in `@osirisjson/core`
- Producer ergonomics (helpers/builders/base classes, fixtures utilities) belong in `@osirisjson/sdk`
- Command UX / CI behavior (flags, exit codes, output formats, orchestration) belong in `@osirisjson/cli`
- Vendor integrations (API calls, auth, inventory mapping, vendor-specific quirks) belong in `osiris-producers`
- IDE UX (language features, UI performance, quick fixes) belong in `osiris-extensions`

---

## 2.2 The toolbox monorepo (@osirisjson/*)
The toolbox monorepo is the shared foundation of the OSIRIS ecosystem. It provides reusable libraries so that each consumer does not implement its own OSIRIS logic.

### 2.2.1 @osirisjson/core: The validation engine
The purpose is to validate OSIRIS documents and emit deterministic, tool-friendly diagnostics.

| Responsibilities | Non-goals | Typical usage pattern |
|---|---|---|
| Load and apply OSIRIS JSON Schema validation (Level 1 / structural) | No vendor API integrations or inventory fetching | `@osirisjson/cli` |
| Execute semantic integrity checks (Level 2 / referential integrity, uniqueness, format constraints) | No editor UI logic, no CLI orchestration | VS Code/VSCodium extension (directly or through an LSP layer) |
| Execute optional best-practice checks (Level 3 / domain rules) | No network calls during validation (schema resolution must be local/cached) | Third-party tools embedding validation in pipelines |
| Emit diagnostics in a stable data model consumed by CLI and editor integrations | - | - |
| Provide validation profiles (e.g. default vs strict) without changing rule meaning | - | - |


### 2.2.2 @osirisjson/sdk: Producer development kit
The purpose is to provide reusable building blocks to help producers generate high-quality OSIRIS documents consistently.

| Responsibilities | Non-goals | Typical usage pattern |
|---|---|---|
| Base producer patterns (context, logging, scope tracking) | No embedded vendor-specific mapping logic (that belongs in `osiris-producers`) | Producers depend on `@osirisjson/sdk` at runtime |
| Deterministic ID helpers and canonicalization utilities | No “hidden validation fork”: SDK may provide optional preflight checks, but canonical validation is `@osirisjson/core` | Producers may depend on `@osirisjson/core` in dev/test to validate fixtures in CI |
| Builders/helpers to construct resources, connections and groups consistently | - | - |
| Test utilities (fixtures, snapshot testing helpers, regression harness patterns) | - | - |


### 2.2.3 @osirisjson/cli: Command Line Interface
The purpose is to provide a CI-friendly and developer-friendly interface to validate and process OSIRIS documents.

| Responsibilities | Non-goals | Typical usage pattern |
|---|---|---|
| Command orchestration and filesystem IO | No validation re-implementation (always delegate to `@osirisjson/core`) | Local: `npx @osirisjson/cli validate file.json` |
| Invoking `@osirisjson/core` and presenting results | No editor-specific UX assumptions | CI: exit codes + --format json |
| Stable exit codes for CI/CD | - | - |
| Machine-readable output (JSON) and human output (text) | - | - |
| (Optional) conversions that do not change OSIRIS semantics (e.g. formatting, normalization modes) when explicitly requested | - | - |

---

## 2.3 Dependency graph and stability rules
### 2.3.1 The "forbidden direction" rule
OSIRIS packages are layered to prevent circular dependencies and to keep the validation engine portable.
Dependencies **MUST** only flow “down” towards more foundational layers.

- `@osirisjson/core` **MUST NOT** depend on `@osirisjson/sdk`, `@osirisjson/cli`, `osiris-producers`, or any editor integration.
- `@osirisjson/cli` **MAY** depend on `@osirisjson/core`.
- `@osirisjson/sdk` **MUST** remain usable without requiring @osirisjson/cli or editor code.
- osiris-extensions **MUST** delegate validation to `@osirisjson/core` and **MUST NOT** ship incompatible validation logic.
- osiris-producers **MAY** use `@osirisjson/sdk` at runtime and **MAY** use `@osirisjson/core` in tests/CI.

A typical dependency direction looks like this:

```mermaid
flowchart LR
  subgraph OSIRIS Specification
    SPEC["osiris
    (spec + schema)"]
  end
  subgraph OSIRIS Toolbox
    CORE["@osirisjson/core"]
    SDK["@osirisjson/sdk"]
    CLI["@osirisjson/cli"]
  end
  subgraph OSIRIS Producers
    PROD["osiris-producers
    (vendor integrations)"]
  end
  subgraph OSIRIS Editors
    EXT["osiris-extensions
    (VS Code/VS Codium)"]
  end
  CORE --> SPEC
  SDK --> SPEC
  CLI --> CORE
  EXT --> CORE
  PROD --> SDK
  PROD -. dev/test .-> CORE
  %% forbidden edges
  CORE -. MUST NOT .-> CLI
  CORE -. MUST NOT .-> SDK
  CORE -. MUST NOT .-> PROD
  CORE -. MUST NOT .-> EXT
```


### 2.3.2 Version alignment strategy
OSIRIS versioning is designed to keep the ecosystem compatible while allowing evolution.

**Document and schema rules (OSIRIS v1.0)**
- Documents declare `version` as **MAJOR.MINOR.PATCH** (SemVer semantics)
- The v1.0 schema endpoint accepts any `1.x.y` document version
- Producers **MAY** include `$schema`. Other top-level fields **SHOULD NOT** be emitted
- Consumers **MUST** ignore unknown fields for forward compatibility

**Toolbox alignment principles**
- Toolbox packages **MUST** share the same MAJOR version across `@osirisjson/core`, `@osirisjson/sdk` and `@osirisjson/cli`
- `@osirisjson/cli` **MUST** be compatible with the corresponding major of `@osirisjson/core`
- Producers and extensions **SHOULD** declare which OSIRIS major they support (documentation and/or package metadata)
- Breaking changes to diagnostic shapes, rule identifiers, or CLI contract **MUST** be a toolbox MAJOR bump

**Rationale:** a stable major alignment prevents ecosystem “split brain” where producers, editors and CI disagree on what “valid” means.

---

## 2.4 Implementation platform rationale (TypeScript + NPM) (Non-normative)
OSIRIS is language-agnostic vendor-neutral at the document level: any producer in any language is valid if it emits OSIRIS JSON that validates against the core schema and passes the canonical validation engine in CI.

That said, the OSIRIS **reference implementation** (toolbox: `@osirisjson/core`, `@osirisjson/cli`, `@osirisjson/sdk`) is built on **TypeScript** and distributed as **NPM packages**.

### Decision summary
- **Reference toolbox platform:** Node.js + TypeScript
- **Distribution:** NPM packages (SemVer-aligned)
- **Producers:** Any language is supported. OSIRIS producers are language-agnostic; TS SDK is the **recommended** reference path

### Why TypeScript/NPM for the toolbox
The main drivers are **ecosystem alignment** and **code reuse**:
- VS Code/VSCodium extensions runs on Node: sharing diagnostics/schema/validation logic is direct.
- One runtime for CLI + validator + SDK reduces “split brain” behavior and duplication.
- NPM makes SemVer alignment and dependency composition straightforward for an ecosystem.
- TypeScript provides strong typing for diagnostics, rule APIs and SDK builders without sacrificing iteration speed.

### Comparison matrix (toolbox reference implementation)
| Criteria | TypeScript (Node/NPM) | Go | Python | Rust |
|---|---|---|---|---|
| VS Code extension runtime alignment | Excellent (native) | Medium (bridge needed) | Medium (bridge needed) | Medium (bridge needed) |
| Sharing code between CLI/core/extension | Excellent | Medium | Medium | Medium |
| Distribution to devs | Excellent (`npm i`, `npx`) | Good (binaries) | Good (pip) | Medium (cargo/binaries) |
| Type safety for contracts (diagnostics/rules) | Strong | Strong | Weak/Optional | Very strong |
| Performance for large graphs | Good | Very good | Medium | **Very good |
| Build complexity for contributors | Low | Medium | Low | High |
| Ecosystem reach for infra scripting | High | High | **Very high | Medium |
| Cross-platform UX | High | High | Medium (env friction) | Medium/High |
| Best fit for “canonical shared validator” | Yes | Possible | Possible | Possible |

**Conclusion:** TypeScript/NPM is the most pragmatic choice for the **canonical toolbox** because it maximizes reuse across CLI + core + editor integrations with the lowest contributor friction. Go/Rust/Python producers remain first-class citizens because OSIRIS is ultimately a JSON interchange format.

### Guidance for non-TypeScript producers
- Emit OSIRIS JSON with `$schema` when possible.
- Validate in CI using `@osirisjson/core` (via CLI) to match canonical behavior.
- Use golden fixtures to prevent drift across releases.

---

# 3 Logical architecture (Data Flow pipelines)
## 3.1 The ingestion pipeline (Producer lifecycle)
The ingestion pipeline describes how producers create an OSIRIS snapshot from source systems.
It is intentionally described as a **contract-level lifecycle**; implementation details belong in `OSIRIS-PRODUCER-GUIDELINES.md` and `@osirisjson/sdk`.

### 3.1.1 Context and scope
Producers **MUST** establish explicit export context before emitting an OSIRIS document.

- `metadata.timestamp` reflects when the snapshot was generated (ISO 8601 with timezone)
- `metadata.generator` identifies the producer/tool name and version
- `metadata.scope` describes what the snapshot represents (e.g. providers, accounts/subscriptions, regions, sites, environments) and any known limitations

**Guidance**
- If inventory is incomplete, producers **SHOULD** still export what they know and document limitations in `metadata.scope.description` (or equivalent) rather than failing silently.
- Very large infrastructures **MAY** be split into multiple documents (e.g. per region/environment/account). Scope **MUST** make boundaries explicit.


### 3.1.2 Normalization
Normalization makes documents comparable over time and across producers. It standardizes **representation**, not meaning (normalization must not invent facts).

Producers **SHOULD**:
- Normalize provider naming and stable identifiers under `provider`
- Prefer stable field representations (e.g. consistent casing and units)
- Avoid embedding volatile, noise-heavy values unless they are essential for the intended use case
- Produce diff-friendly output where feasible (e.g. stable ordering of arrays such as sorting by `id`)

---

## 3.2 The validation pipeline (Core engine)
Validation is executed by `@osirisjson/core` and emits diagnostics. Validation is defined in terms of **levels**; see `OSIRIS-VALIDATION-LEVELS.md` for the full model and catalog.

Validation **profiles** (e.g. default vs strict) may change which checks run and how results are reported, but **MUST NOT** change the meaning of a rule.

### 3.2.1 Stage 1: Structural (Schema)
| Goal | Check examples | Outcome |
|---|---|---|
| Verify JSON Schema compliance | Required fields present (`version`, `metadata`, `topology`) | Diagnostics that point to precise locations and schema expectations |
|  | Field types and formats (strings, objects, arrays, enums) |  |
|  | Schema constraints (e.g. `direction` enum) |  |

### 3.2.2 Stage 2: Semantic (Integrity)
| Goal | Check examples | Outcome |
|---|---|---|
| Verify internal consistency beyond what schema can express | Referential integrity: connections and groups reference existing IDs in the same document | Deterministic diagnostics that do not depend on runtime environment |
|  | ID uniqueness within each array |  |
|  | Cycle detection where applicable (e.g. group hierarchy) |  |
|  | Basic canonical constraints (e.g. invalid self-references) |  |

### 3.2.3 Stage 3: Domain (Best practices)
| Goal | Check examples | Outcome |
|---|---|---|
| Optional, opinionated checks that improve quality without changing structural validity | Modeling recommendations (e.g. prefer `properties`/`extensions` over new fields) | Domain rules are not a replacement for a CMDB, a scanner or vendor tooling. They exist to improve interoperability and quality |
|  | Quality hints (e.g. missing `provider.native_id` where expected) |  |
|  | Security posture hints when safe and non-invasive |  |

---

## 3.3 The consumption pipeline (CLI and Editors)
Consumers are responsible for presenting and acting on validation outcomes.
CLI and editors **MUST** delegate validation behavior to `@osirisjson/core` (no duplicated validators).

**CLI pipeline:**
- Read files and resolve schema (offline/local).
- Run `@osirisjson/core` validation with a chosen profile.
- Emit:
  - stable exit codes for CI
  - machine-readable results (JSON)
  - human-readable summaries

**Editor pipeline (VS Code/VSCodium):**
- Detect OSIRIS documents (via `$schema` and/or document structure)
- Validate via `@osirisjson/core` (directly or via LSP)
- Render diagnostics (squiggles, hovers, quick fixes)
- Enforce performance constraints (debounce, incremental parsing, avoid UI blocking)


---

# 4 Core architectural patterns
This chapter defines the ecosystem patterns that keep OSIRIS toolbox consistent across CLI, editors, producers and third-party consumers.

---

## 4.1 Diagnostics and error handling
Diagnostics are the common language across the OSIRIS ecosystem. They **MUST** be stable, structured and tool-friendly.

```mermaid
flowchart TD
  DOC["OSIRIS JSON document"]
  PROFILE["Validation profile (default/strict)"]

  subgraph CORE["OSIRIS @osirisjson/core"]
    direction TB

    L1["Level 1 - Structural JSON schema (2020-12)
    - required fields
    - types, formats, enums
    - pattern constraints"]

    L2["Level 2 - Semantic referential integrity
    - connection source/target > resource id
    - group members/children > resource/group id
    - ID uniqueness per array
    - cycle detection (groups)"]

    L3["Level 3 - Domain best-practice checks
    - modeling hints
    - quality warnings
    - security posture hints"]

    L1 --> L2 --> L3
  end

  DOC --> CORE
  PROFILE -.-> CORE

  subgraph DIAG["OSIRIS diagnostic"]
    direction LR
    CODE["code"]
    SEV["severity"]
    MSG["message"]
    PATH["path"]
    RANGE["range"]
  end

  CORE --> DIAG

  subgraph CONSUMERS["OSIRIS consumers"]
    direction LR
    CLI["CLI
    exit codes
    JSON / text"]
    EDITOR["Editor
    squiggles
    quick fixes"]
    THIRDPARTY["Third-party
    pipelines
    dashboards"]
  end

  DIAG --> CLI
  DIAG --> EDITOR
  DIAG --> THIRDPARTY
```

### 4.1.1 The diagnostic model
A **diagnostic** represents a single validation finding emitted by `@osirisjson/core`.

**Minimum fields**
| Field | Meaning |
|---|---|
| `code` | Stable identifier for the finding (treat as opaque string; stable across a `MAJOR` toolbox version) |
| `severity` | One of `error`, `warning`, `info`, `hint` |
| `message` | Human-readable explanation |
| `path` | JSON Pointer (RFC 6901) **or equivalent locator** (implementation-defined but stable) |
| `range` | Document range `{start,end}` with `{line,character}` when available (0-based, LSP-style recommended) |

**Optional fields**
| Field | Meaning |
|---|---|
| `rule` | Rule identifier (useful for domain rules / best-practice checks) |
| `related` | Related locations for cross-reference issues (e.g. “also referenced here”) |
| `fix` | Structured quick-fix metadata (only when safe, deterministic and non-destructive) |

**Example (illustrative, not normative):**

```json
{
  "code": "OSIRIS-ERR-REF-001",
  "severity": "error",
  "message": "Connection target references a resource id that does not exist.",
  "path": "/topology/connections/12/target",
  "range": {
    "start": { "line": 220, "character": 18 },
    "end":   { "line": 220, "character": 44 }
  }
}
```


### 4.1.2 Severity guidance and profiles
Diagnostics separate what failed (code) from how serious it is (severity). Severity may depend on the active validation profile (e.g. default vs strict), but codes must not change meaning.

**General guidance:**

- **Structural (Level 1)** violations are typically error.
- **Semantic (Level 2)** violations are typically error (broken references, duplicate IDs), but some may be downgraded under non-strict profiles.
- **Domain (Level 3)** findings are typically warning, info, or hint.

The authoritative rule catalog and mapping live in `OSIRIS-VALIDATION-LEVELS.md` and are implemented in `@osirisjson/core`

---

# 4.2 Identity strategy (deterministic IDs)
Stable identity is what makes OSIRIS useful for diffs, correlation and long-lived tooling.

**Core rules:**
Resource, connection and group IDs **MUST** be unique within a document.

- IDs **SHOULD** remain stable across exports when feasible.
- IDs are opaque identifiers: consumers should not require parsing ID structure.

**Recommended producer strategy:**
| Entity | Recommended approach |
|---|---|
| Resources | Prefer stable provider identifiers and preserve them under `provider.native_id` when available. Use deterministic `id` generation when no stable native identifier exists. |
| Connections | Generate deterministic connection IDs from stable endpoint fingerprints. For `direction = "bidirectional"`, canonicalize endpoint ordering to prevent ID flips. |
| Groups | Generate stable IDs derived from stable boundaries (region/site/datacenter/rack/env, hyperscaler/cloud object IDs, etc.). Groups should remain stable across exports even when temporarily empty. |

**Rationale:** stable IDs enable meaningful topology diffs and downstream correlation without vendor-specific logic.

---

# 4.3 Extensibility mechanism
OSIRIS is designed to evolve without breaking consumers.

**Forward-compatibility rules:**

- Consumers **MUST** ignore unknown fields (including unknown extension payloads).
- Producers **SHOULD NOT** introduce new top-level fields in v1.0 beyond optional `$schema`.

**Where to put additional data:**
| Need | Use |
|---|---|
| Generic, broadly useful extra attributes | `properties` |
| Vendor/org-specific payloads, schemas, or deep objects | `extensions` (namespaced keys) |

**Namespace governance:**
- Extension keys **MUST** follow `osiris.<identifier>` naming rules (lowercase, dot-separated).
- The `osiris.` prefix is reserved to prevent collisions and support registry conventions.
- OSIRIS governs namespace rules (and optionally a registry), not the internal semantics of each extension payload.
- Consumers **MUST** treat unknown extension fields as opaque.

---

# 4.4 JSON file conventions
These conventions keep OSIRIS documents portable, editor-friendly and diff-friendly.

### 4.4.1 File format
- Documents **MUST** be valid JSON format RFC8259.
- Canonical OSIRIS documents **MUST NOT** use comments (no JSONC).
- Character encoding **MUST** be in `UTF-8` adhering to RFC8259.
- Documents are expected to use the `.json` extension (tools detect OSIRIS via `$schema` and/or structure).

### 4.4.2 Top-level fields
- Producers **MAY** include `$schema` (recommended for tooling/editor resolution).
- Other top-level fields **SHOULD NOT** be emitted in OSIRIS v1.0.

### 4.4.3 Recommended ordering and diffs
- Prefer consistent key ordering for common objects (at least at the top level).
- Prefer stable ordering for arrays when it does not change semantics:
  - `topology.resources` sorted by `id`
  - `topology.connections` sorted by `id`
  - `topology.groups` sorted by `id`
- Avoid nondeterministic iteration order that creates noisy diffs.

### 4.4.4 Recommended formatting
- 2-space indentation
- newline at end of file

---

# 4.5 Terminology
| Term | Description |
|---|---|
| Document | A single OSIRIS JSON file representing a point-in-time snapshot. |
| Producer | A tool that exports source inventory into an OSIRIS document. |
| Consumer | A tool that reads OSIRIS documents (validator, diagrammer, diff engine, inventory, etc.). |
| Topology | The `topology` object containing resources, connections and groups. |
| Resource | An entity described in `topology.resources`. |
| Connection | A relationship/edge described in `topology.connections`. |
| Group | A container/hierarchy described in `topology.groups`. |
| Properties | Freeform attributes for broadly useful non-standard data. |
| Extensions | Namespaced payloads for vendor/org-specific data. |
| Diagnostics | Structured validation findings emitted by `@osirisjson/core`. |
| Structural validation | Schema-based validation (Level 1). |
| Semantic validation | Referential integrity and consistency checks (Level 2). |
| Domain validation | Optional best-practice checks (Level 3). |

---

# 4.6 Stability tiers
This table defines which ecosystem surfaces are expected to remain stable within a `MAJOR` version.

| Artifact/Surface | Stability within MAJOR | What is stable | What may change |
|---|---|---|---|
| OSIRIS specification (`osiris`) | High | Field meanings, normative rules, namespace rules | Clarifications, additional guidance, new optional fields/features in `MINOR` (non-breaking) |
| Schema endpoint (`/schema/v1.0/...`) | High | Validators **MAY** accept `1.x.y` at the `v1.0` endpoint; producers targeting `v1.0` **SHOULD** emit `1.0.0`. | Adds new optional fields / clarifications; Schema **MUST NOT** tighten constraints for existing fields within v1.x. |
| `@osirisjson/core` public API | High | Validator entry points, diagnostic shape, severity model | Internal pipeline, performance optimizations, new rules |
| Diagnostic codes catalog | Medium/High | Existing codes retain meaning once published | New codes added; deprecations announced before `MAJOR` removal |
| `@osirisjson/cli` contract | High | Command names, exit codes, JSON output format promises | New flags/options, improved formatting |
| `@osirisjson/sdk` contract | Medium | Core helpers and base patterns | New helpers, refactors, optional utilities (avoid breaks without `MAJOR`) |
| Producers (`osiris-producers`) | Medium | Supported vendors/scopes per producer (documented) | Coverage changes as vendor APIs evolve |
| Extensions (`osiris-extensions`) | Medium | Delegation to `@osirisjson/core`, editor performance constraints | UI/UX improvements, new features |