# OSIRIS JSON Versioning and Release lifecycle<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 08 February 2026 |
| Last revision date | 21 February 2026 |
| Status    | Draft |
| Document ID | OSIRIS-ADG-VR-1.0 |
| Document URI | [OSIRIS-ADG-VR-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-VERSIONING-AND-RELEASES.md) |
| Document Name | OSIRIS JSON Versioning and Release lifecycle |
| Specification ID | OSIRIS-1.0 |
| Specification URI | [OSIRIS-1.0](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) |
| Schema URI | [OSIRIS-1.0](https://osirisjson.org/schema/v1.0/osiris.schema.json) |
| License   | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Repository | [github.com/osirisjson/osiris](https://github.com/osirisjson/osiris) |

# Table of Content
- [Table of Content](#table-of-content)
- [1 The versioning trinity](#1-the-versioning-trinity)
  - [1.1 Specification vs. schema and packages](#11-specification-vs-schema-and-packages)
    - [1.1.1 The OSIRIS JSON specification](#111-the-osiris-json-specification)
    - [1.1.2 The OSIRIS JSON schema](#112-the-osiris-json-schema)
    - [1.1.3 The OSIRIS JSON toolbox packages](#113-the-osiris-json-toolbox-packages)
    - [1.1.4 The specification leads everything else follows](#114-the-specification-leads-everything-else-follows)
- [2 Compatibility matrix](#2-compatibility-matrix)
  - [2.1 Forward compatibility](#21-forward-compatibility)
  - [2.2 Backward compatibility](#22-backward-compatibility)
  - [2.3 Breaking change definition](#23-breaking-change-definition)
  - [2.4 Cross-package alignment](#24-cross-package-alignment)
- [3 The release lifecycle](#3-the-release-lifecycle)
  - [3.1 Release candidates and beta gates](#31-release-candidates-and-beta-gates)
  - [3.2 Synchronized release windows](#32-synchronized-release-windows)
  - [3.3 Deprecation timelines](#33-deprecation-timelines)


# 1 The versioning trinity
The OSIRIS ecosystem versions three distinct artifacts that evolve on related but independent tracks. Understanding this separation is essential for contributors, producers, consumers and integrators.

> [!NOTE]
> Back-reference: Normative versioning semantics for the specification are defined in [OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) Chapter 12.
> The version alignment strategy and stability tiers are defined in [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) section 2.4.2.

---

## 1.1 Specification vs. schema and packages
All three artifacts use SemVer (`MAJOR.MINOR.PATCH`) but version independently.

### 1.1.1 The OSIRIS JSON specification
(`osiris`) defines the format: fields, semantics, types, validation rules, compatibility guarantees and the normative examples. The specification version is what the document `version` field declares (e.g. `"1.0.0"`). The specification is the authority; schema and packages derive from it.

### 1.1.2 The OSIRIS JSON schema
(`osiris.schema.json`) provides structural validation for the specification. The schema is published per `MAJOR.MINOR` of the specification (e.g. `v1.0/osiris.schema.json`). Schema `PATCH` updates track the specification: a spec `1.0.1` clarification that tightens a regex or fixes an enum omission produces an updated `v1.0` schema. The schema **MUST NOT** introduce constraints that contradict or exceed the specification.

### 1.1.3 The OSIRIS JSON toolbox packages
(`@osirisjson/core`, `@osirisjson/cli`, `@osirisjson/sdk`) version as NPM packages on their own SemVer track. A toolbox `1.3.0` may support specification `1.0.0` through `1.2.0`. Package versioning reflects API surface changes (new flags, new helpers, diagnostic shape evolution), not specification changes directly.

| Artifact | Example version | What triggers MAJOR | What triggers MINOR | What triggers PATCH |
|---|---|---|---|---|
| Specification | `1.1.0` | New required fields, changed semantics, removed features | New optional fields, new types, new domain rules | Clarifications, typo fixes, non-normative text |
| Schema | `v1.0` (published path) | Specification MAJOR | Specification MINOR (if schema changes) | Constraint fixes within MAJOR.MINOR |
| Toolbox packages | `1.3.0` | Diagnostic shape changes, removed APIs, exit code changes | New rules, new CLI flags, new SDK helpers | Bug fixes, performance, message improvements |

### 1.1.4 The specification leads everything else follows
A new specification `MINOR` may or may not require a toolbox `MINOR` (it does if new validation rules are added; it does not if only non-normative text changed). A toolbox `MINOR` may ship without any specification change (e.g. a new CLI flag or performance optimization).

---

# 2 Compatibility matrix
This chapter defines the compatibility contracts that keep the ecosystem stable. These rules apply across all OSIRIS components.

> [!NOTE]
> Back-reference: Forward and backward compatibility rules for documents are defined normatively in [OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) sections 12.2.1 through 12.2.3.
> Version routing behavior in the validation engine is defined in [OSIRIS-ADG-TLB-CORE-1.0](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-TOOLBOX-CORE.md) section 1.1.1.

---

## 2.1 Forward compatibility
Forward compatibility means a consumer built for spec version `X.Y` can accept documents from spec version `X.Z` where `Z > Y`.

| Consumers MUST | Consumers MUST NOT |
|---|---|
|Ignore unknown fields at all levels (top-level, metadata, topology, resources, connections, groups)|Reject a document solely because it contains fields or types the consumer does not recognize|
|Accept unknown resource, connection and group types if structurally valid|Strip unknown fields or namespaces during round-trip processing unless explicitly configured to do so|
|Accept unknown extension namespaces and preserve them when re-exporting|-|

**Validation engine behavior:** when a document's `MINOR` version exceeds the engine's highest supported `MINOR` within the same `MAJOR`, the engine selects the highest supported schema as a fallback and emits `V-DOC-005` (info/warning). Validation proceeds normally. This is defined in [OSIRIS-ADG-TLB-CORE-1.0](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-TOOLBOX-CORE.md) section 1.1.1.

---

## 2.2 Backward compatibility
Backward compatibility means documents valid under spec `1.0.0` remain valid under all `1.x.y` releases.

**Within a MAJOR version, the specification guarantees:**
- Required fields **MUST NOT** be added.
- Existing field semantics **MUST NOT** change incompatibly.
- Validation rules **MUST NOT** become stricter for existing fields.
- New optional fields, types and domain rules **MAY** be added.

**Consumer version handling (decision matrix):**

| Consumer supports | Document version | Action |
|---|---|---|
| `1.0.0` | `1.0.1` | Accept (patch, compatible) |
| `1.0.0` | `1.1.0` | Accept, ignore unknown fields, **MAY** warn |
| `1.0.0` | `2.0.0` | Reject or error (MAJOR mismatch) |

---

## 2.3 Breaking change definition
A breaking change is any modification that causes a previously valid document to become invalid, or that changes the observable behavior of a consumer processing a previously valid document. Breaking changes require a specification `MAJOR` increment.

**Examples of breaking changes:**
- Adding a new required field to resources, connections, groups or metadata.
- Changing the structure of existing required fields (e.g. making `provider` an array).
- Removing or renaming existing fields or types.
- Changing type naming conventions (e.g. `compute.vm` to `compute/vm`).
- Changing namespace prefix rules (e.g. `osiris.*` to `osiris:*`).
- Making previously optional validation rules mandatory at the `default` profile.

**Examples that are NOT breaking changes:**
- Adding new optional fields or new standard types (MINOR).
- Adding new Stage 3 domain rules (MINOR).
- Clarifying ambiguous specification text without changing behavior (PATCH).
- Improving diagnostic messages without changing codes or severity (toolbox PATCH).

When a `MAJOR` increment occurs, the specification **MUST** document all breaking changes, provide a migration guide and **SHOULD** have issued deprecation warnings in prior `MINOR` releases.

---

## 2.4 Cross-package alignment
Toolbox packages **MUST** share the same `MAJOR` version across `@osirisjson/core`, `@osirisjson/cli` and `@osirisjson/sdk`. This is the **major version lock**.

**Rules:**
- A breaking change to any toolbox package's public contract (diagnostic shape, CLI exit codes, SDK type exports) requires a `MAJOR` bump across **all** toolbox packages simultaneously.
- `MINOR` and `PATCH` versions are independent. `@osirisjson/cli@1.4.2` may coexist with `@osirisjson/core@1.3.0` provided they share `MAJOR` `1`.
- The Go producer SDK (`osirisjson-producer`) versions independently because it has no library dependency on the TypeScript toolbox. Producers declare which specification `MAJOR` they support via documentation and package metadata.
- Editor integrations (`osiris-editor-integrations`) version independently but **MUST** declare a compatible `@osirisjson/core` `MAJOR` range in their `package.json` peer dependency.

**Rationale:** the major version lock prevents ecosystem "split brain" where CLI, editor and CI disagree on what constitutes a valid diagnostic, a valid exit code or a valid type export.

---

# 3 The release lifecycle
This chapter defines the policies that govern how OSIRIS artifacts move from development to stable release.

> [!NOTE]
> Back-reference: The deprecation policy for specification features, fields and types is defined normatively in [OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md#1231-deprecation-process) sections 12.3.1 through 12.3.5.

---

## 3.1 Release candidates and beta gates
All `MAJOR` and significant `MINOR` releases **MUST** pass through a release candidate (RC) phase before stable publication.

**RC lifecycle:**
1. **Development branch:** features land on a development branch and are validated against the full test suite (specification examples, golden fixtures, regression tests).
2. **RC publication:** when the release scope is complete, an RC is published (e.g. `1.1.0-rc.1` for NPM packages, `v1.1.0-RC1` tag for the specification). RC artifacts are available for community testing but **MUST NOT** be used in production.
3. **Feedback window:** RCs remain open for a minimum of **14 calendar days** for `MAJOR` releases and **7 calendar days** for `MINOR` releases. Feedback is collected via GitHub issues.
4. **RC iteration:** if changes are required, a new RC is published (e.g. `1.1.0-rc.2`) and the feedback window resets. Once no blocking issues remain, the RC is promoted to stable with no code changes (only version string and metadata updates).

**Beta gates (minimum criteria for RC promotion to stable):**
- All specification examples validate against the updated schema and engine.
- The CLI produces identical exit codes for the reference test suite across the RC and the prior stable release (for backward-compatible releases).
- No unresolved `severity: error` issues in the RC GitHub milestone. Changelog and migration guide (if applicable) are complete and reviewed.

---

## 3.2 Synchronized release windows
Specification and toolbox releases are coordinated to prevent version drift.

**Release sequence for a specification MINOR (e.g. 1.1.0):**
1. Specification text and schema are finalized and tagged.
2. `@osirisjson/core` is updated with new rules/schema support and released.
3. `@osirisjson/cli` and `@osirisjson/sdk` are updated if affected and released.
4. Editor integrations are updated to consume the new `@osirisjson/core` and released.
5. Producer SDK documentation is updated to reflect new types or fields.

Steps 2 through 4 **SHOULD** ship within **5 business days** of the specification tag to minimize the window where the spec is published but tooling lags behind. The specification **MUST NOT** be announced publicly until at least `@osirisjson/core` and `@osirisjson/cli` are published with matching support.

**Release sequence for a toolbox-only MINOR (e.g. new CLI flag):**
Toolbox packages release independently. No specification or schema change is required. The release follows standard NPM publishing and changelog practices.

---

## 3.3 Deprecation timelines
OSIRIS uses an **N-2 support policy** for specification `MAJOR` versions and a structured deprecation lifecycle for features within a `MAJOR`.

**N-2 MAJOR version support:**
When specification `MAJOR` version `N` is released as stable, the project **MUST** continue to maintain `N-1` (security fixes and critical bug fixes) and **SHOULD** accept critical patches for `N-2`. Versions older than `N-2` are end-of-life.

| Current stable | Active support | Security fixes only | End-of-life |
|---|---|---|---|
| `3.x` | `3.x` | `2.x` | `1.x` and older |

**Feature deprecation within a MAJOR:**
Features, fields and types follow the lifecycle defined in the [OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md#123-deprecation-policy) specification (Chapter 12.3):
1. **Deprecation announcement** in a `MINOR` release with warnings, replacement guidance and rationale.
2. **Transition period** of at least one full `MINOR` cycle where the deprecated feature remains valid and functional.
3. **Removal** only in the next `MAJOR` version.

**Toolbox deprecation:**
Deprecated CLI flags, SDK helpers or engine options follow the same pattern: announced in a toolbox `MINOR` with console warnings, functional for at least one additional `MINOR` and removed only in the next toolbox `MAJOR`. Deprecated APIs **MUST** be marked with `@deprecated` JSDoc tags and documented in the changelog.

**Diagnostic code deprecation:**
Published diagnostic codes (`V-*`) **MUST NOT** change meaning within a specification `MAJOR`. Codes **MAY** be deprecated (announced, documented, retained) and removed only in the next `MAJOR`. Deprecated codes **SHOULD** remain in the machine-readable registry with `status: "deprecated"` until removal.