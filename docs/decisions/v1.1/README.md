# OSIRIS JSON Decision Records<!-- omit in toc -->

| Field | Value |
|---|---|
| Created | 25 November 2026 |
| Last revised | 25 November 2026 |
| Author | Tia Zanella ([@skhell](https://github.com/skhell)) |
| Specification baseline | [OSIRIS JSON v1.0](../../../specification/v1.0/OSIRIS-JSON-v1.0.md) |
| Core schema | [OSIRIS v1.0 schema](../../../schema/v1.0/osiris.schema.json) |
| License | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |

- [Purpose](#purpose)
- [Where decisions fit](#where-decisions-fit)
- [Compatibility](#compatibility)
- [How we record decisions](#how-we-record-decisions)
- [Relationship terms](#relationship-terms)
- [Decision records](#decision-records)
- [Topics for future decisions](#topics-for-future-decisions)

## Purpose

We use **OSIRIS Decision Records (ODRs)** to explain significant design
choices: the problem, the options we considered, our proposed solution
and its effect on existing documents and implementations.

We develop OSIRIS as an open community project. We welcome proposals and 
eedback from contributors; the project maintainer reviews and records
the final decision.

An ODR is useful when we reconcile type names, define relationships or
change a modeling convention. Routine wording fixes and documentation
updates do not need a separate record.

The released specification remains the normative authority. Accepting
an ODR records our decision; implementation and publication follow
separately. A draft does not change the specification or deprecate a
type.

## Where decisions fit

| Location | Purpose |
|---|---|
| `specification/v1.0/` | Historical baseline for reconciliation |
| `specification/v1.1/` | Proposed specification changes |
| `schema/v1.0/` | Current document-validation baseline |
| `docs/decisions/v1.1/` | Decisions, rationale and supporting evidence |
| `taxonomy/v1.1/` | Draft machine-readable taxonomy and property guidance |
| `docs/guidelines/v1.0/` | Practical guidance for producers, consumers and tooling |
| `docs/guidelines/v1.1/` | Eventually revision proposal for guidelines in v1.1 |

We keep approved definitions consistent across the specification and
taxonomy. ODRs explain the choices behind those definitions. Category
folders help us organize the taxonomy; moving a file does not rename a
type. We generate shared reference material from maintained definitions
rather than editing duplicate catalogues independently.

## Compatibility

When we change the taxonomy, we preserve these principles:

- We accept unknown types that satisfy the applicable schema pattern.
  Resource and group types require at least two segments; connection
  types may have one. An unknown type does not justify dropping an
  entity or relationship.
- We keep `properties` open and support valid unknown extension
  namespaces and payloads. Common-property guidance remains advisory.
- We identify a type by `(kind, type)`. A resource and a group may share
  the same type string without representing the same concept.
- We use preferred identifiers and synonyms for discovery. They do not
  automatically rewrite existing type strings or resource IDs.
- We treat IT, OT and IoT as domains, and public platforms/hyperscalers,
  on-premise and hybrid as deployment contexts. These classifications
  alone do not prove that two types are equivalent or justify replacing
  one with another.
- We follow the [versioning and compatibility policy](../../guidelines/v1.0/OSIRIS-VERSIONING-AND-RELEASES.md).
  Deprecated identifiers remain valid and supported throughout v1.x;
  we document migration guidance when proposing deprecation.

## How we record decisions

We keep each record focused on the problem, source evidence, proposed
choice, rationale and compatibility consequences. We distinguish what
is already published from what we propose to change, and make unresolved
questions visible.

| Status | Meaning |
|---|---|
| **Draft** | We are developing or reviewing the proposal. |
| **Accepted** | The maintainer has approved the recorded decision; implementation or release may still be pending. |
| **Superseded** | A linked, accepted record replaces or changes the decision. |
| **Withdrawn** | We closed the proposal without adopting it and retain it for context. |

We record acceptance explicitly and update the index below. If only part
of a proposal is ready, we separate that decision from the unresolved
parts before marking it Accepted. Where useful, we link the
implementation commit or release afterward.

We assign stable IDs such as `ODR-0001`. For new records, we use
`ODR-NNNN-short-descriptive-name.md`.

A record's status describes the decision process. A type's lifecycle
(such as active or deprecated) describes the type; these are separate.

## Relationship terms

We use the following terms consistently:

| Term | Meaning |
|---|---|
| **Preferred identifier for discovery** | The identifier we prioritize in references and search, without renaming historical data. |
| **Exact synonym** | Two identifiers with the same documented meaning and scope. |
| **Broader / narrower** | Different concepts with a specialization relationship; they are not interchangeable. |
| **Related** | Associated concepts without implied equivalence or inheritance. |
| **No relationship recorded** | The record makes no relationship claim. |

We establish relationships from definitions and evidence, not spelling
or prefixes alone. A dotted name does not automatically imply schema
inheritance. Proposed relationships remain provisional until accepted.

## Decision records

| ID | Subject | Status |
|---|---|---|
| [ODR-0001](ODR-0001-RESOURCE-TYPE-RECONCILIATION.md) | Reconcile the 63 resource-type identifiers published in OSIRIS Specification v1.0 Chapter 7 and Appendix C | **Draft** |

## Topics for future decisions

We may create further records as these topics develop:

- Connection and group vocabularies, including direction defaults.
- Type grammar, status/state fields and diagnostic consistency.
- Provider fields, extension conventions and mapping-only identifiers.
- Registry format, generation and specification-version binding.
- Advisory property profiles, suggested values and units.
- Deprecation and proposed migration from historical `ot.*` or
  `container.*` types.

These are discussion topics, not approved changes or a release
commitment.