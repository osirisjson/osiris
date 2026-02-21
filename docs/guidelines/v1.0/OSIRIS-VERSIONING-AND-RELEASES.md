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
<!-- work in progress -->


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
