# OSIRIS JSON Versioning and Release lifecycle<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 08 February 2026 |
| Last revision date | 20 February 2026 |
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