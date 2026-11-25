# ODR-0001 Resource Type Reconciliation<!-- omit in toc -->

| Field | Value |
|---|---|
| Authors | Tia Zanella [@skhell](https://github.com/skhell) |
| Revision | `1.0.0-DRAFT` |
| Creation date | 25 November 2026 |
| Last revision date | 25 November 2026 |
| Status | Draft |
| Record ID | `ODR-0001` |
| Specification baseline | [OSIRIS JSON v1.0](../../../specification/v1.0/OSIRIS-JSON-v1.0.md) |
| Core schema | [OSIRIS v1.0 schema](../../../schema/v1.0/osiris.schema.json) |
| License | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |

> **Review notice:** This document records a proposed reconciliation of
> *published OSIRIS Specification v1.0 claims*. It does not itself amend
> the specification, approve a taxonomy release, deprecate an identifier
> or make an unresolved semantic relationship authoritative.
> See the [taxonomy decision-record process](README.md).

- [1. Problem and evidence](#1-problem-and-evidence)
- [2. Scope and invariants](#2-scope-and-invariants)
- [3. Proposed disposition by decision group](#3-proposed-disposition-by-decision-group)
- [4. Complete published-claim register](#4-complete-published-claim-register)
- [5. Rationale and unresolved semantic questions](#5-rationale-and-unresolved-semantic-questions)
  - [5.1 Publication evidence is not semantic approval](#51-publication-evidence-is-not-semantic-approval)
  - [5.2 Equivalent names require evidence](#52-equivalent-names-require-evidence)
  - [5.3 Function semantics require a version-aware decision](#53-function-semantics-require-a-version-aware-decision)
- [References](#references)


## 1. Problem and evidence

The published OSIRIS JSON v1.0 specification contains two inventories of 
standard resource identifiers:

- Chapter 7 defines **41** primary resource-type strings.
- Appendix C lists **41** resource-type strings.
- **19** strings occur in both inventories.
- Each inventory contains **22** strings absent from the other, together
  they contain **63 distinct published claims** (`41+41-19 = 63`).

These counts describe **textual publication evidence**, not 63
approved or semantically distinct resource concepts.

Chapter 4, section 4.2.5, identifies Chapter 7 as the source of standard
types, while Appendix C calls itself a comprehensive standard-type
registry. That inconsistency MUST be addressed explicitly as neither the
intersection nor the union can be substituted for a reviewed
authoritative catalogue without a decision.

The differences include incomplete indexes, spelling variants, possible
semantic overlaps and concepts present only in Appendix C. They cannot
all be resolved by string matching or namespace-prefix substitution.

## 2. Scope and invariants

This record addresses resource-type identifiers published in Chapter 7
or Appendix C of the original OSIRIS Specification v1.0.
The original v1.0 baseline is used for the claim register below.
It is distinct from the current [OSIRIS Specification v1.1 draft](../../../specification/v1.1/OSIRIS-JSON-v1.1.md).
The 63-row register is historical evidence and MUST NOT be replaced with
the fixes and growing v1.1 type inventory.

The proposed reconciliation preserves the following existing rules:

- All structurally valid resource type strings remain valid, whether or
  not they appear in a known catalogue.
- Unknown valid types, free-form resource `properties` and recognized or
  unrecognized extension namespaces remain accepted under the core
  schema and applicable compatibility rules.
- Type identifiers are preserved verbatim in existing documents.
  A preferred identifier for discovery or synonym **does not** authorize
  rewriting OSIRIS JSON Producer output.
- Identifier appearance in a source document is **publication evidence**
  its lifecycle, preferred-name status, equivalence and eligibility for
  generated canonical exports require governed decisions.
- Taxonomy identity is `(kind, type)`; resource, connection and group
  vocabularies remain separate even when strings overlap.
- IT and OT are classification facets, not automatic instructions to
  rename existing `ot.*`, `power.*`, `security.*`, or `industrial.*`
  identifiers.
- This OSIRIS Decision Record **does not** impose new required fields or
  close the set of accepted resource types.

**Out of scope:** newly proposed OSIRIS JSON v1.1 types, approval of
deprecations, provider-specific property profiles, registry file layout,
connection/group reconciliations and type strings found only in examples
or provider-mapping guidance.
References to these topics below are handoffs to separate release
decisions, not approvals by this record.

## 3. Proposed disposition by decision group

The following Decisions D1-D10 identifiers are stable references within
**this draft**. Their proposed outcomes require maintainer review before
incorporation into a next normative release.

| Group | Published evidence | Proposed action | Review concern |
|---|---|---|---|
| **D1** | 19 strings appear in both lists. | We preserve the strings; inventory agreement does not itself authorize changes to their meaning. | None for their inventory identity. |
| **D2** | 9 strings have Chapter 7 definitions but no Appendix C row. | We preserve their existing Chapter 7 identity, synchronize the index. | Confirm index generation from approved definitions. |
| **D3** | 7 non-OT strings appear only in Appendix C. | We retain as published claims and define their scope before approval as canonical records. | Especially `container.*` and the `storage.disk`/`storage.volume` boundary. |
| **D4** | `network.security.group` vs `network.securitygroup`. | We prefer the Chapter 7 spelling for discovery, evaluate exact synonymy. | Confirm the scope is identical, preserve both strings. |
| **D5** | `compute.function` vs `compute.function.serverless`. | We evaluate a broader/narrower relationship, not presumed exact equivalence. | Original Appendix C explicitly describes `compute.function` as “Serverless function”; broadening its meaning is a semantic change that may require v1.1 review. |
| **D6** | `network.vpc` vs `network.vnet`. | We prefer `network.vpc` as a standard among hyperscalers naming convention; eventually we evaluate exact synonymy. | Verify provider-neutral semantics and retain original strings. |
| **D7** | 5 Chapter 7 / `ot.*` pairs. | We prefer Chapter 7 terms for discovery where appropriate, review each possible equivalence individually. | `ot.camera` may be narrower than `security.camera`; `ot.access.controller` and `ot.power` need independent scope checks. Proposed deprecation of the `ot.*` identifiers in v1.1. |
| **D8** | `building.hvac.*` vs `ot.hvac`. | We treat HVAC equipment and the HVAC controller as related but distinct. | Proposed deprecation of the `ot.*` identifiers in v1.1. |
| **D9** | `industrial.scada` vs `ot.controller.dcs`. | We treat as related but distinct system classes. | Proposed deprecation of the `ot.*` identifiers in v1.1. |
| **D10** | 5 OT types appear only in Appendix C. | We preserve the exact published strings of OSIRIS Specification v1.0 as claims and add reviewed definitions. | Proposed deprecation of the `ot.*` identifiers in v1.1. |

**Proposed OT deprecation:** Because OT, IT and IoT describe domains
rather than resource kinds, we propose deprecating the existing `ot.*`
resource-type identifiers in OSIRIS v1.1 in favor of domain-neutral
classifications, subject to individual semantic review. Each affected
type will have documented migration guidance, including a replacement
where an equivalent exists or an explicit statement that none exists.
If adopted, deprecated identifiers will remain valid and supported
throughout v1.x; existing documents require no automatic rewriting.

**Proposed compatibility outcome:** None of the 63 published strings is
automatically renamed, deprecated, removed, or silently normalized.
Synonyms and relations, where approved, are **discovery metadata**
rather than validation aliases or data transformations.

## 4. Complete published-claim register

Chapter 7 is the section of the *OSIRIS Specification v1.0* in which a
type is primarily defined. Appendix C is the original Appendix C
subsection. A dash (`-`) means the identifier is not listed there.

**Review** describes the remaining reconciliation work; it is not a
lifecycle or approval status of the resource type.

The table retains all 63 unique strings.

| # | Type | Chapter 7 | Appendix C | Group | Relationship (proposed where indicated) | Review | Follow-up |
|---|---|---|---|---|---|---|---|
| 1 | `application.database` | 7.2.1 | C.4 | D1 | no relationship recorded | Aligned | - |
| 2 | `application.queue` | 7.2.2 | C.4 | D1 | no relationship recorded | Aligned | - |
| 3 | `application.eventstream` | 7.2.2 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 4 | `application.cache` | 7.2.3 | C.4 | D1 | no relationship recorded | Aligned | - |
| 5 | `application.repository` | 7.2.4 | C.4 | D1 | no relationship recorded | Aligned | - |
| 6 | `application.service` | 7.2.5 | C.4 | D1 | no relationship recorded | Aligned | - |
| 7 | `compute.server` | 7.3.1 | C.1 | D1 | no relationship recorded | Aligned | - |
| 8 | `compute.vm` | 7.3.2 | C.1 | D1 | no relationship recorded | Aligned | - |
| 9 | `compute.container` | 7.3.3 | C.1 | D1 | no relationship recorded | Aligned | - |
| 10 | `compute.cluster` | 7.3.4 | C.1 | D1 | no relationship recorded | Aligned | - |
| 11 | `compute.function.serverless` | 7.3.5 | - | D5 | narrower than (proposed) `compute.function` | Semantic review | Resolve broad/narrow interpretation |
| 12 | `storage.volume` | 7.4.1 | C.3 | D1 | no relationship recorded | Aligned | - |
| 13 | `storage.bucket` | 7.4.2 | C.3 | D1 | no relationship recorded | Aligned | - |
| 14 | `storage.filesystem` | 7.4.3 | C.3 | D1 | no relationship recorded | Aligned | - |
| 15 | `storage.array` | 7.4.4 | C.3 | D1 | no relationship recorded | Aligned | - |
| 16 | `network.vpc` | 7.5.1 | - | D6 | preferred (proposed) | Synonym review | Verify exact equivalence, document preferred identifier for discovery |
| 17 | `network.subnet` | 7.5.1 | C.2 | D1 | no relationship recorded | Aligned | - |
| 18 | `network.vlan` | 7.5.1 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 19 | `network.router` | 7.5.2 | C.2 | D1 | no relationship recorded | Aligned | - |
| 20 | `network.router.port` | 7.5.2 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 21 | `network.switch` | 7.5.2 | C.2 | D1 | no relationship recorded | Aligned | - |
| 22 | `network.switch.port` | 7.5.2 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 23 | `network.interface` | 7.5.3 | C.2 | D1 | no relationship recorded | Aligned | - |
| 24 | `network.loadbalancer` | 7.5.4 | C.2 | D1 | no relationship recorded | Aligned | - |
| 25 | `network.firewall` | 7.5.5 | C.2 | D1 | no relationship recorded | Aligned | - |
| 26 | `network.security.group` | 7.5.5 | - | D4 | preferred (proposed) | Synonym review | Verify exact equivalence, document preferred identifier for discovery |
| 27 | `building.hvac.ahu` | 7.6.1 | - | D8 | related to `ot.hvac` | Distinct-type review | Specify HVAC controller vs equipment. |
| 28 | `building.hvac.vav` | 7.6.1 | - | D8 | related to `ot.hvac` | Distinct-type review | Specify HVAC controller vs equipment. |
| 29 | `building.hvac.boiler` | 7.6.1 | - | D8 | related to `ot.hvac` | Distinct-type review | Specify HVAC controller vs equipment. |
| 30 | `building.hvac.chiller` | 7.6.1 | - | D8 | related to `ot.hvac` | Distinct-type review | Specify HVAC controller vs equipment. |
| 31 | `security.access.panel` | 7.6.2 | - | D7 | preferred (proposed) | Semantic review | Verify scope for each OT pair. |
| 32 | `security.camera` | 7.6.2 | - | D7 | preferred (proposed) | Semantic review | Verify scope for each OT pair. |
| 33 | `power.ups` | 7.6.3 | - | D7 | preferred (proposed) | Semantic review | Verify scope for each OT pair. |
| 34 | `power.pdu` | 7.6.3 | - | D7 | preferred (proposed) | Semantic review | Verify scope for each OT pair. |
| 35 | `industrial.plc` | 7.6.4 | - | D7 | preferred (proposed) | Semantic review | Verify scope for each OT pair. |
| 36 | `industrial.scada` | 7.6.4 | - | D9 | related to `ot.controller.dcs` | Distinct-type review | Specify SCADA vs DCS distinction. |
| 37 | `physical.datacenter` | 7.6.5 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 38 | `physical.building` | 7.6.5 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 39 | `physical.floor` | 7.6.5 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 40 | `physical.room` | 7.6.5 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 41 | `physical.rack` | 7.6.5 | - | D2 | no relationship recorded | Index update | Include in consolidated index |
| 42 | `compute.function` | - | C.1 | D5 | broader than (proposed) `compute.function.serverless` | Semantic review | Resolve broad/narrow interpretation |
| 43 | `network.securitygroup` | - | C.2 | D4 | exact synonym (proposed) of `network.security.group` | Synonym review | Verify exact equivalence, document preferred identifier for discovery |
| 44 | `network.vnet` | - | C.2 | D6 | exact synonym (proposed) of `network.vpc` | Synonym review | Verify exact equivalence, document preferred identifier for discovery |
| 45 | `network.endpoint` | - | C.2 | D3 | no relationship recorded | Definition needed | Define semantics in registry |
| 46 | `network.gateway` | - | C.2 | D3 | no relationship recorded | Definition needed | Define semantics in registry |
| 47 | `network.vpn` | - | C.2 | D3 | no relationship recorded | Definition needed | Define semantics in registry |
| 48 | `storage.disk` | - | C.3 | D3 | no relationship recorded | Definition needed | Define semantics in registry |
| 49 | `container.account` | - | C.5 | D3 | no relationship recorded | Definition review; v1.1 deprecation proposed separately | Review administrative-scope migration; retain historical identifier and producer compatibility |
| 50 | `container.project` | - | C.5 | D3 | no relationship recorded | Definition review; v1.1 deprecation proposed separately | Review administrative-scope migration; retain historical identifier and producer compatibility |
| 51 | `container.resourcegroup` | - | C.5 | D3 | no relationship recorded | Definition review; v1.1 deprecation proposed separately | Review administrative-scope migration; retain historical identifier and producer compatibility |
| 52 | `ot.controller.plc` | - | C.6 | D7 | exact synonym (proposed) of `industrial.plc` | Equivalence review; v1.1 deprecation proposed separately | Review PLC scope before any replacement or deprecation; domain facets alone do not prove equivalence. Proposed deprecation in v1.1. |
| 53 | `ot.access.controller` | - | C.6 | D7 | exact synonym (proposed) of `security.access.panel` | Semantic review | Verify scope for each OT pair. Proposed deprecation in v1.1. |
| 54 | `ot.camera` | - | C.6 | D7 | exact synonym (proposed) of `security.camera` | Semantic review | Verify scope for each OT pair. Proposed deprecation in v1.1. |
| 55 | `ot.ups` | - | C.6 | D7 | exact synonym (proposed) of `power.ups` | Semantic review | Verify scope for each OT pair. Proposed deprecation in v1.1. |
| 56 | `ot.power` | - | C.6 | D7 | exact synonym (proposed) of `power.pdu` | Semantic review | Verify scope for each OT pair. Proposed deprecation in v1.1. |
| 57 | `ot.hvac` | - | C.6 | D8 | related to (proposed) `building.hvac.ahu`, `building.hvac.vav`, `building.hvac.boiler`, `building.hvac.chiller` | Distinct-type review | Specify HVAC controller vs equipment. Proposed deprecation in v1.1. |
| 58 | `ot.controller.dcs` | - | C.6 | D9 | related to (proposed) `industrial.scada` | Distinct-type review | Specify SCADA vs DCS distinction. Proposed deprecation in v1.1. |
| 59 | `ot.access.lock` | - | C.6 | D10 | no relationship recorded | Definition needed | Proposed deprecation in v1.1. |
| 60 | `ot.access.reader` | - | C.6 | D10 | no relationship recorded | Definition needed | Proposed deprecation in v1.1. |
| 61 | `ot.printer` | - | C.6 | D10 | no relationship recorded | Definition needed | Proposed deprecation in v1.1. |
| 62 | `ot.sensor.environmental` | - | C.6 | D10 | no relationship recorded | Definition needed | Proposed deprecation in v1.1. |
| 63 | `ot.sensor.industrial` | - | C.6 | D10 | no relationship recorded | Definition needed | Proposed deprecation in v1.1. |

## 5. Rationale and unresolved semantic questions

### 5.1 Publication evidence is not semantic approval

The 19 D1 matches require no name-level reconciliation.
The Chapter 7 only D2 entries already have primary definitions, so
missing Appendix C rows SHOULD be corrected rather than interpreted as
removals. Appendix C-only D3 and D10 entries are also part of the
historical published record, but Chapter 4's definition of standard
types makes their normative status ambiguous until the specification is
reconciled. **D3 and D10 MUST follow the same approval procedure.**

### 5.2 Equivalent names require evidence

D4 (`network.security.group`/`network.securitygroup`) and D6
(`network.vpc`/`network.vnet`) are plausible exact-synonym candidates.
An exact synonym means the same resource concept and scope, not merely
similar names or compatible providers. Approval MUST cite the intended
definitions and both strings MUST remain discoverable without
mutating documents.

### 5.3 Function semantics require a version-aware decision

The original Chapter 7 describes `compute.function.serverless` as
serverless, while original Appendix C also labels `compute.function`
**“Serverless function.”**
The proposed broader meaning of `compute.function` in the later revised
Chapter 7 may be a useful improvement, but it is not established by the
original v1.0 evidence.

## References

- [OSIRIS JSON v1.0 specification](../../../specification/v1.0/OSIRIS-JSON-v1.0.md), especially Chapter 4 section 4.2, Chapter 7, Appendix C and Chapter 12.
- [OSIRIS v1.0 core JSON Schema](../../../schema/v1.0/osiris.schema.json).
- [OSIRIS JSON Decision Records](README.md).
