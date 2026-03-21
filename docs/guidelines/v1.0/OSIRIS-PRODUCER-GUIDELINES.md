# OSIRIS JSON Producer Guidelines<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 08 February 2026 |
| Last revision date | 16 February 2026 |
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
- [Table of Content](#table-of-content)
- [1 The producer mindset](#1-the-producer-mindset)
  - [1.1 Discovery vs. static definition](#11-discovery-vs-static-definition)
  - [1.2 Scope of responsibility (what NOT to map)](#12-scope-of-responsibility-what-not-to-map)
- [2 The ingestion workflow](#2-the-ingestion-workflow)
  - [2.1 Discovery normalization redaction emission](#21-discovery-normalization-redaction-emission)
  - [2.2 Identity strategy (deterministic IDs and naming patterns)](#22-identity-strategy-deterministic-ids-and-naming-patterns)
    - [2.2.1 Resource IDs](#221-resource-ids)
    - [2.2.2 Connection IDs](#222-connection-ids)
    - [2.2.3 Group IDs](#223-group-ids)
  - [2.3 Schema compliance essentials](#23-schema-compliance-essentials)
    - [2.3.1 The `$schema` field](#231-the-schema-field)
    - [2.3.2 Top-level field rules](#232-top-level-field-rules)
    - [2.3.3 The `provider.name = "custom"` namespace requirement](#233-the-providername--custom-namespace-requirement)
    - [2.3.4 Properties vs. extensions (quick rule)](#234-properties-vs-extensions-quick-rule)
  - [2.4 Handling relationships (Physical vs. logical links)](#24-handling-relationships-physical-vs-logical-links)
    - [2.4.1 When to use connections](#241-when-to-use-connections)
    - [2.4.2 When to use groups](#242-when-to-use-groups)
    - [2.4.3 Avoiding duplication](#243-avoiding-duplication)
    - [2.4.4 Inferred relationships](#244-inferred-relationships)
  - [2.5 Data normalization (units, timestamps, casing standards)](#25-data-normalization-units-timestamps-casing-standards)
    - [2.5.1 Units](#251-units)
    - [2.5.2 Timestamps](#252-timestamps)
    - [2.5.3 Casing standards](#253-casing-standards)
    - [2.5.4 Vendor-native type preservation](#254-vendor-native-type-preservation)
- [3 Security and redaction (deep dive)](#3-security-and-redaction-deep-dive)
  - [3.1 Secret stripping (non-negotiable patterns)](#31-secret-stripping-non-negotiable-patterns)
    - [3.1.1 Prohibited content categories](#311-prohibited-content-categories)
    - [3.1.2 Detection patterns](#312-detection-patterns)
    - [3.1.3 Sanitization strategy for connection strings](#313-sanitization-strategy-for-connection-strings)
  - [3.2 Filtering irrelevant vendor metadata](#32-filtering-irrelevant-vendor-metadata)
    - [3.2.1 Producers configurable detail levels](#321-producers-configurable-detail-levels)
  - [3.3 Safe failure behavior modes](#33-safe-failure-behavior-modes)
    - [3.3.1 Fail-closed (default for regulated environments)](#331-fail-closed-default-for-regulated-environments)
    - [3.3.2 Log-and-redact (opt-in for tolerant environments)](#332-log-and-redact-opt-in-for-tolerant-environments)
- [4 Quality assurance](#4-quality-assurance)
  - [4.1 Golden files (Standardizing test fixtures)](#41-golden-files-standardizing-test-fixtures)
    - [4.1.1 Structure](#411-structure)
    - [4.1.2 Requirements for golden files](#412-requirements-for-golden-files)
    - [4.1.3 Golden file maintenance workflow](#413-golden-file-maintenance-workflow)
  - [4.2 Regression testing against schema and @osirisjson/core](#42-regression-testing-against-schema-and-osirisjsoncore)
    - [4.2.1 CI validation pipeline](#421-ci-validation-pipeline)
    - [4.2.2 What to test](#422-what-to-test)
    - [4.2.3 Version alignment](#423-version-alignment)
    - [4.2.4 Snapshot comparison strategy](#424-snapshot-comparison-strategy)


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


---

# 2 The ingestion workflow
The ingestion workflow is the lifecycle a producer follows to transform vendor data into an OSIRIS snapshot.

```mermaid
flowchart LR
  D["**Discovery**
  (vendor APIs, SNMP/LLDP/NETCONF, device CLIs via SSH etc.)"] --> N["**Normalization**
  (types, IDs, units etc.)"]
  N --> R["**Redaction**
  (secrets, PII, noise etc.)"]
  R --> E["**Emission**
  (JSON serialization + validation)"]
```

---

## 2.1 Discovery normalization redaction emission
Each phase has a clear contract and failure mode.

**Discovery:** Acquire raw vendor data (API responses, CLI output, file contents). Log what was fetched, what was skipped and why. Establish the export scope (`metadata.scope`).

**Normalization:** Transform vendor-native representations into OSIRIS specification compliant structures. This is where type mapping, ID generation, unit conversion, timestamp normalization and relationship extraction happen. Normalization standardizes **representation**, not meaning; it **MUST NOT** introduce artifacts or invent data altering the source of truth.

**Redaction:** Strip credentials, secrets, PII and irrelevant vendor noise before the document leaves the producer boundary. Redaction **MUST NOT** be negotiable (see Chapter 3 of the current document).

**Emission:** Serialize the document to JSON and validate it against the OSIRIS schema. Producers **MUST** validate before publishing. Emit structured logs summarizing the run (resource counts, warnings, errors).

**Failure propagation rules:**
- Discovery failures for individual resources: log, skip and continue.
- Normalization errors for individual resources: log, skip the resource (or emit with `status: "unknown"`) and continue.
- Redaction detection of secrets: **MUST** halt emission for the affected resource or fail the entire export, depending on configuration (see section 3.3 of the current document).
- Validation failure at Level 1: **MUST** fail the export pipeline.
- Validation failure at Level 2: **MUST** fail the export pipeline under the `strict` profile. Under `default`, producers **MAY** continue only if explicitly configured, but **MUST** emit the diagnostics and mark limitations in `metadata.scope`.


---

## 2.2 Identity strategy (deterministic IDs and naming patterns)
Stable identity is what makes OSIRIS useful for topology diffs, snapshot correlation and downstream automation. If IDs drift across exports, consumers cannot detect changes reliably.


### 2.2.1 Resource IDs
Producers **MUST** ensure resource `id` values are unique within the document and stable across exports when the underlying entity is the same.

**Recommended patterns:**

| Domain | Pattern | Examples |
|---|---|---|
| Hyperscalers and Public Cloud providers | `provider::native-id` | `aws::i-0abc123def456`, `azure::/subscriptions/sub-123/.../vm01` |
| On-premise | `site::identifier` | `mxp::spine-sw-01`, `mxp::srv-r770-010` |
| OT | `site::identifier` | `mxp::sensor-temp-01`, `mxp::rfid-reader-01` |

**Rules:**
- If the source provides a stable unique identifier, producers **SHOULD** build `id` from it.
- Producers **SHOULD NOT** generate random IDs (UUIDs, timestamps) for real resources.
- When no stable native identifier exists, producers **MAY** derive a deterministic ID from a stable tuple (e.g. `{site, name, serial}`) and **SHOULD** document the derivation strategy.
- IDs are opaque to consumers. Consumers **SHOULD NOT** parse ID structure for meaning.


#### 2.2.1.1 Provider attribution contract (native identity and origin)
Producers **MUST** separate **OSIRIS identity** (`resource.id`) from **provider-native identity** (`resource.provider.native_id`).

**Rules:**
- If the source provides a stable vendor identifier, producers **MUST** set `resource.provider.native_id` to that value.
- Producers **SHOULD** derive `resource.id` from stable inputs (often including the native id), but `resource.id` **MUST NOT** be the only place where the vendor identifier exists.
- Producers **SHOULD** preserve the vendor’s original type/class string in `resource.provider.type` when available.
- Origin/context fields (tenant/account/subscription/project/region/site/zone) **MUST** live in the `provider` object when OSIRIS defines a corresponding field. They **MUST NOT** be buried in `extensions` as ad-hoc conventions.
- Vendor-only payloads or deep raw objects **MAY** be preserved under `extensions` using a namespaced key, but only when they are not representable in OSIRIS standard fields.

**Rationale:** `provider.*` enables round-trip correlation and cross-producer consistency even when `resource.id` patterns differ.


### 2.2.2 Connection IDs
Connection IDs **SHOULD** be deterministic and derived from the relationship they represent.

**Recommended algorithm:**
Producers **SHOULD** build a canonical key from stable parts:

- `type`
- `direction` (if omitted, treat as `"bidirectional"`)
- canonicalized `(source, target)` using **resource IDs**
  - if `direction = "bidirectional"`, sort `(source, target)` lexicographically to prevent flips across exports
- stable qualifiers when needed (examples: `properties.port`, `properties.protocol`, interface IDs, provider-native link ID)

**Canonical key serialization (normative):**
Serialize as:
`v1|{type}|{direction}|{sourceId}|{targetId}|{qualifiers}`

Where:
- `sourceId`/`targetId` are the full resource IDs (not hints)
- `qualifiers` is a comma-joined list of `k=v` pairs sorted by key (omit absent qualifiers)

Compute:
- `hash16 = first 16 chars of lowercase hex(SHA-256(canonical_key))`

**Emit this recommended pattern:**
`conn-{type}-{sourceHint}-to-{targetHint}-{hash16}`

**Collision rule:**
- If an ID collision is detected within the same document, producers **MUST** extend the hash length (e.g. take 24 or 32 chars) rather than renumbering or adding randomness.

**Example:**
```json
{
  "id": "conn-dataflow.tcp-plc-line-01-to-printer-label-01-7f3a91c22f4c0a1b",
  "type": "dataflow.tcp",
  "source": "mxp::plc-line-01",
  "target": "mxp::printer-label-01",
  "direction": "forward",
  "properties": {
    "protocol": "tcp",
    "port": 9100,
    "application": "zpl_over_tcp"
  }
}
```

**Hint derivation (normative):**
- `sourceHint` and `targetHint` **MUST** be derived from `source`/`target` **resource IDs** (never from `name`).
- Extraction rule:
  1) take the substring after the last `::` if present, else after the last `/`
  2) lowercase
  3) replace any sequence of non `[a-z0-9]` with `-`
  4) trim leading/trailing `-`
  5) truncate to max `24` chars
- If the result is empty, use the first `8` chars of `hash16`.


> [!NOTE]
> - `sourceHint`, `targetHint`, and `boundaryHint` are **display-only** slugs and **MUST NOT** be used as inputs to hashing (use full IDs/stable scope fields).



### 2.2.3 Group IDs
Group IDs **SHOULD** be derived from stable boundaries (region, site, rack, environment, cloud organizational unit).

**Recommended algorithm:**
- Producers **MUST** select a stable `boundaryToken` representing the group boundary
  (examples: site code `mxp`, region `eu-west-1`, rack id `r42`, subscription id, project id).
- The canonical key **MUST NOT** include `members` or `children`
  (membership and hierarchy may change over time and would destabilize the group ID).

**Canonical key serialization (normative):**
Serialize as:
`v1|{type}|boundary={boundaryToken}|{scopePairs}`

Where:
- `boundaryToken` is the stable machine token (hash input).
- `scopePairs` is a pipe-joined list of `k=v` pairs sorted by key name.
- Eligible keys (use if present):
  - `provider.name`
  - `provider.namespace`
  - `provider.account`
  - `provider.subscription`
  - `provider.project`
  - `provider.region`
  - `provider.site`
  - `provider.zone`
- Omit absent keys entirely (do not emit empty placeholders).

**Compute:**
- `hash16 = first 16 chars of lowercase hex(SHA-256(canonical_key))`

**Input rules:**
- The canonical key **MUST NOT** include `members` or `children`
  (membership and hierarchy may change over time and would destabilize the group ID).
- When applicable, include provider scope inputs
  (e.g. provider name/namespace, account/subscription, region/site).

**Emit this recommended pattern:**
`group-{type}-{boundaryHint}-{hash16}`

**Example:**
```json
{
  "id": "group-physical.site-mxp-2c10d4a93d7a5b82",
  "type": "physical.site",
  "name": "MXP Datacenter",
  "description": "All on-premise resources in Milan Datacenter",
  "members": [
    "mxp::srv-r770-001",
    "mxp::srv-r770-002",
    "mxp::storage-dell-me5024"
  ],
  "properties": {
    "address": "Via Malpensa 1, 21010 Vizzola Ticino VA, Italy",
    "coordinates": "45.6301° N, 8.7280° E"
  },
  "tags": {
    "location": "on-premise",
    "datacenter": "mxp"
  }
}
```

**boundaryHint rule:**
- `boundaryHint` **MUST** be derived from `boundaryToken` (display-only slug).
- `boundaryHint` **MUST NOT** be used as an input to hashing.
- If no stable boundary token exists, producers **MAY** omit it and emit: `group-{type}-{hash16}`.

Groups **SHOULD** remain stable across exports even when temporarily empty ([OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md#238-empty-groups) specification section 2.3.8).

---

## 2.3 Schema compliance essentials
Every OSIRIS document **MUST** include the three required top-level fields: `version`, `metadata` and `topology`.


### 2.3.1 The `$schema` field
Producers **MUST** include `$schema` at the top level to enable editor resolution and tooling auto-detection.

```json
{
  "$schema": "https://osirisjson.org/schema/v1.0/osiris.schema.json",
  "version": "1.0.0"
}
```

Producers targeting OSIRIS v1.0 **MUST** emit `"version": "1.0.0"`. Other top-level fields beyond `$schema` **SHOULD NOT** be emitted in v1.0.

> [!NOTE]
> Producers emit the specification baseline version (`1.0.0` for OSIRIS v1.0). Consumers and validators may accept forward-compatible PATCH updates within the same MAJOR.MINOR (e.g. `1.0.x`) without requiring producer changes.


### 2.3.2 Top-level field rules
| Field | Requirement | Notes |
|---|---|---|
| `version` | **MUST** be `"1.0.0"` for v1.0 producers | SemVer string matching the specification version |
| `metadata.timestamp` | **MUST** be present | [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339)/ISO 8601 date-time with timezone (e.g. `"2026-02-14T10:30:00Z"`) |
| `metadata.generator.name` | **MUST** be present | Stable producer name (e.g. `"osirisjson-producer-azure"`) |
| `metadata.generator.version` | **MUST** be present | Producer SemVer string, not OSIRIS specification version |
| `metadata.scope` | **SHOULD** be present | Describes export boundaries (providers, regions, accounts, sites, environments) |
| `topology.resources` | **MUST** be present (array, may be empty) | Minimum: `[]` |
| `topology.connections` | **SHOULD** be present when relationships are known | Defaults to `[]` |
| `topology.groups` | **SHOULD** be present when groupings are known | Defaults to `[]` |


### 2.3.3 The `provider.name = "custom"` namespace requirement
When `provider.name` is `"custom"`, the `provider.namespace` field becomes **required** by the schema. This is enforced at Level 1 (structural validation).

The namespace **MUST** follow the `osiris.<identifier>` pattern using reverse-domain notation.

```json
{
  "provider": {
    "name": "custom",
    "namespace": "osiris.com.acme",
    "native_id": "asset-12345",
    "site": "mxp"
  }
}
```

**Guidance:**
- Use `"custom"` for on-premise equipment, internal systems, or any private resource. You **MUST NOT** use it from a well-known public vendor device or platform.
- Prefer well-known canonical provider names (`aws`, `azure`, `gcp`, `cisco`, `arista`, `nokia`, etc.) when applicable as documented in OSIRIS Specification.
- Do not use `"custom"` as a catch-all when a standard provider name exists.


### 2.3.4 Properties vs. extensions (quick rule)
- Use `properties` for **generic, broadly useful** attributes that many producers could emit consistently.
- Use `extensions` for **vendor/org-specific** payloads, nested objects, or fields whose semantics are not standardized by OSIRIS.
- Extension keys **MUST** be namespaced (e.g. `osiris.aws.*`) and **MUST NOT** be used to store data that OSIRIS already models in standard fields.

---

## 2.4 Handling relationships (Physical vs. logical links)
Producers **SHOULD** emit explicit relationships whenever they are discoverable. OSIRIS provides two mechanisms: **connections** (graph edges) and **groups** (membership sets).

### 2.4.1 When to use connections
Connections represent relationships that consumers traverse as graph edges. 

Use connections for:

- Network connectivity (physical links, logical paths, tunnels)
- Application dependencies (service-to-database, frontend-to-API)
- Data flows (producer-to-consumer, source-to-sink)
- Containment when traversal semantics are required (VM inside a host, disk attached to a server)
- Attachment (NIC-to-switch port, volume-to-VM)

**Required fields:** `id`, `type`, `source`, `target`.
**Default direction:** `bidirectional` when `direction` is omitted.

### 2.4.2 When to use groups
Groups represent classification, organization and boundaries without graph traversal semantics. Use groups for:
- Organizational boundaries (resource groups, projects, accounts, subscriptions)
- Physical boundaries (datacenter site, rack, floor, building)
- Logical boundaries (environment, availability zone, security zone, cost center)
- Hierarchical nesting (parent group > child group via `children`)

### 2.4.3 Avoiding duplication
Producers **SHOULD NOT** model the same relationship as both a `contains` connection and a group membership.

Choose one representation:

```mermaid
flowchart TD
  Q["Does this relationship need
  to be traversed as topology?"]

  Q -- "Yes" --> CONN["Use a **connection**
  (graph edge)"]

  Q -- "No" --> GRP["Use a **group**
  (membership set)"]

  CONN --> CONN_EX["Network links, dependencies,
  data flows, containment,
  attachments"]

  GRP --> GRP_EX["Inventory, reporting,
  classification, organizational
  and physical boundaries"]
```


### 2.4.4 Inferred relationships
When producers infer relationships (e.g. from naming conventions, subnet overlap, or LLDP data), they **SHOULD** mark inferred relationships using `tags` to distinguish them from explicitly discovered relationships:

```json
{
  "tags": {
    "osiris.inferred": "true",
    "osiris.inference_source": "lldp"
  }
}
```

---

## 2.5 Data normalization (units, timestamps, casing standards)
Normalization ensures documents are comparable across producers and over time. Every producer **SHOULD** apply these conventions consistently.

### 2.5.1 Units
OSIRIS does not enforce specific units, but producers **SHOULD** follow consistent conventions to maximize interoperability.

| Measurement | Recommended unit | Property name convention | Example |
|---|---|---|---|
| Memory/RAM | Gigabytes | `memory_gb` | `"memory_gb": 64` |
| Storage capacity | Gigabytes (or Terabytes for large volumes) | `capacity_gb`/`capacity_tb` | `"capacity_tb": 1.92` |
| Network bandwidth | Gigabits per second | `bandwidth_gbps` | `"bandwidth_gbps": 100` |
| CPU count | Virtual CPUs | `vcpus` | `"vcpus": 8` |
| CPU frequency | Gigahertz | `cpu_frequency_ghz` | `"cpu_frequency_ghz": 2.4` |
| Latency | Milliseconds | `latency_ms` | `"latency_ms": 5` |
| Power | Watts | `power_watts` | `"power_watts": 750` |

Producers **MUST** include the unit suffix in the property name. Bare names like `"memory": 64` are ambiguous and **SHOULD** be avoided.

### 2.5.2 Timestamps
All timestamps **MUST** be [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339)/ISO 8601 format with timezone. UTC is recommended.

| Good | Bad |
|---|---|
| `2026-02-14T10:30:00Z` | `2026-02-14 10:30:00` (missing T separator and timezone) |
| `2026-02-14T11:30:00+01:00` | `1708000200` (Unix epoch, not RFC 3339) |
| - | `Feb 14, 2026` (ambiguous locale-dependent format) |

Producers **SHOULD** normalize vendor timestamps to UTC when the source timezone is known.

### 2.5.3 Casing standards

| Field | Casing rule | Example |
|---|---|---|
| `provider.name` | Lowercase, dots allowed | `"aws"`, `"cisco"`, `"custom"` |
| Resource/connection/group `type` | Lowercase, dot-separated segments | `"compute.vm"`, `"network.switch"` |
| Extension namespace keys | `osiris.<lowercase.segments>` | `"osiris.aws"`, `"osiris.com.acme"` |
| Property keys | `snake_case` (recommended) | `"memory_gb"`, `"serial_number"` |
| Tag keys | `snake_case` or `dotted.path` | `"env"`, `"cost_center"`, `"osiris.inferred"` |

Producers **MUST NOT** emit uppercase characters in `provider.name`, `type`, or extension namespace keys. This is enforced by schema patterns at Level 1.

### 2.5.4 Vendor-native type preservation
While the OSIRIS `type` field is normalized to the standard taxonomy, the original vendor type string **SHOULD** be preserved in `provider.type`:

```json
{
  "type": "compute.vm",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123"
  }
}
```

This enables round-trip correlation: consumers can identify the OSIRIS-normalized type (`compute.vm`) and the vendor's original classification (`AWS::EC2::Instance`).

---

# 3 Security and redaction (deep dive)
OSIRIS documents describe infrastructure topology. They **MUST** be safe to share under controlled policies. Producers are the first and most critical line of defense against accidental secret disclosure.

> [!NOTE]
> Back-reference: Normative security requirements are defined in the OSIRIS specification Chapter 13.
> Cross-cutting security constraints are summarized in [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) section 5.2.

---

## 3.1 Secret stripping (non-negotiable patterns)
OSIRIS documents **MUST NOT** contain credentials, secrets, or authentication material. Producers **MUST** scan document content for common credential patterns before emission and **MUST** refuse to emit fields known to contain secrets.

### 3.1.1 Prohibited content categories
- Passwords (plaintext, hashed, or encoded)
- API keys and access tokens
- SSH private keys or certificates (private material)
- Database connection strings with embedded credentials
- OAuth client secrets
- Cloud access keys/secret keys (e.g. AWS `AKIA*`, GCP service account JSON keys)
- Encryption keys or private certificates
- Bearer tokens, JWTs with secrets, session tokens
- PII that is not required for topology (employee names, phone numbers, email addresses in non-generator contexts)

### 3.1.2 Detection patterns
Producers **MUST** implement heuristic scanning before emission. The following patterns are non-exhaustive but represent the minimum coverage.

**Key-name matching (case-insensitive):**
Any property key containing: `password`, `secret`, `token`, `credential`, `private_key`, `api_key`, `access_key`, `client_secret`, `auth`.

**Examples of value-pattern matching:**

| Pattern | What it catches |
|---|---|
| `AKIA[0-9A-Z]{16}` | AWS access key IDs |
| `[A-Za-z0-9/+=]{40}` adjacent to `AKIA` | AWS secret access keys |
| `ghp_[A-Za-z0-9]{36}` | GitHub personal access tokens |
| `gho_[A-Za-z0-9]{36}` | GitHub OAuth tokens |
| `-----BEGIN .* PRIVATE KEY-----` | PEM-encoded private keys |
| `Bearer [A-Za-z0-9\-._~+/]+=*` | Bearer tokens |
| `Basic [A-Za-z0-9+/]+=*` | Base64-encoded basic auth |
| `eyJ[A-Za-z0-9_-]*\.eyJ[A-Za-z0-9_-]*\.` | JWT tokens |
| `xox[boaprs]-[A-Za-z0-9-]+` | Slack tokens |
| Connection strings with `://user:pass@` | Embedded credentials in URIs |

### 3.1.3 Sanitization strategy for connection strings
Producers **MUST** decompose connection strings rather than emitting them whole.

**Prohibited:**
```json
{ "connection_string": "postgresql://admin:s3cret@db.prod.internal:5432/app" }
```

**Required:**
```json
{
  "endpoint": "db.prod.internal:5432",
  "database": "app",
  "protocol": "postgresql"
}
```

Credentials are omitted entirely, not replaced with placeholders.

---

## 3.2 Filtering irrelevant vendor metadata
Not every field returned by a vendor API belongs in an OSIRIS document. Producers **SHOULD** apply data minimization ([OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md#1313-data-minimization) specification section 13.1.3) and emit only what serves the intended use case.

| Filter out | Keep |
|---|---|
| Internal vendor request/response metadata (request IDs, pagination tokens, HTTP headers, rate-limit counters) | Resource identity and classification (IDs, types, names, descriptions) |
| Volatile operational telemetry that changes every second (real-time CPU load, instantaneous packet counters, live session counts) | Provider attribution (native IDs, regions, accounts, zones, sites) |
| Redundant or derived fields that consumers can compute from existing data | Stable configuration properties (instance type, memory, CPU, firmware version) |
| Vendor marketing or billing metadata unrelated to topology (pricing tier names, promotional flags) | Network addressing when required for topology (IPs, MAC addresses, VLANs, subnets) |
| Debugging artifacts (stack traces, internal error codes from the vendor API) | Physical characteristics for on-premise resources (serial numbers, rack positions, hardware models) |
| - | Vendor-specific features that affect behavior, placed in `extensions` (e.g. `ebs_optimized`, `fault_tolerance`) |

### 3.2.1 Producers configurable detail levels
Producers **SHOULD** support a configuration option to control emission detail (e.g. `--detail minimal|detailed`). This allows the same producer to serve documentation-focused exports (minimal) and audit-focused exports (detailed) as described in the [OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/blob/main/specification/v1.0/OSIRIS-JSON-v1.0.md#1313-data-minimization) specification section 13.1.3.

---

## 3.3 Safe failure behavior modes
When a producer detects potential secrets or encounters an ambiguous field, it **MUST** choose a safe failure mode.

### 3.3.1 Fail-closed (default for regulated environments)
The producer **MUST** halt emission for the affected resource or fail the entire export pipeline when credentials are detected. This is the safest posture and **SHOULD** be the default.

```mermaid
flowchart LR
  SCAN["Scan field value"] --> DETECT{"Credential
  pattern found?"}

  DETECT -- "no" --> PASS["Continue
  emission"]

  DETECT -- "yes" --> LOG["Log error
  (field path only,
  **never** the value)"]

  LOG --> MODE{"Producer
  config?"}

  MODE -- "fail-closed
  (default)" --> ABORT["Skip resource
  or abort run"]

  MODE -- "log-and-redact
  (opt-in)" --> REDACT["Replace value
  with REDACTED"]

  ABORT --> EXIT["Exit non-zero
  (fail CI)"]

  REDACT --> META["Set metadata.redacted: true
  + redaction_policy"]
```

### 3.3.2 Log-and-redact (opt-in for tolerant environments)
When explicitly configured, the producer **MAY** replace the detected value with a `"REDACTED"` placeholder and continue emission. This mode trades safety for completeness when operators accept the risk.

```mermaid
flowchart LR
  DETECT["Credential
  pattern detected"] --> REPLACE["Replace value
  with REDACTED"]

  REPLACE --> LOG["Log warning
  (field path only,
  **never** the value)"]

  LOG --> META["Set metadata.redacted: true
  + metadata.redaction_policy"]

  META --> CONTINUE["Continue
  emission"]
```

**Rules for both safe failure behavior modes:**
- Producers **MUST NOT** log, print, or include the actual secret value in any output (logs, error messages, diagnostics, stack traces).
- Producers **MUST NOT** silently pass through detected secrets.
- The choice between fail-closed and log-and-redact **MUST** be an explicit producer configuration option, never implicit.

---

# 4 Quality assurance
Producers **MUST** be testable and their output **MUST** be reproducible. The OSIRIS ecosystem relies on canonical validation via `@osirisjson/core`; producers do not implement their own validation but **MUST** ensure their output passes it.

> [!NOTE]
> Back-reference: The canonical truth rule (validation behavior is never re-implemented) is defined in [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md)  section 1.2.1.
> Validation levels and profiles are defined in [OSIRIS-ADG-VL-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-VALIDATION-LEVELS.md).

---

## 4.1 Golden files (Standardizing test fixtures)
A **golden file** is a known-good OSIRIS document that represents the expected output for a given input. Golden files are the primary regression defense for producers.

### 4.1.1 Structure
Each producer **SHOULD** maintain a `testdata/` (or `fixtures/`) directory containing paired files:

```text
testdata/
├── vendor_scenario_a/
│   ├── input.json # Mocked vendor API response or static source
│   ├── golden.json # Expected OSIRIS output
│   └── README.md # Scenario description and coverage notes
├── vendor_scenario_b/
│   ├── input.json
│   ├── golden.json
│   └── README.md
├── README.md # Test suite overview and run instructions
└── ...
```

### 4.1.2 Requirements for golden files
- Golden files **MUST** pass `@osirisjson/core` validation at the `strict` profile with zero errors.
- Golden files **MUST** be committed to version control and updated only through deliberate, reviewed changes.
- Golden files **SHOULD** include `$schema` for editor support.
- Golden files **SHOULD NOT** contain synthetic data that looks like real production infrastructure (real-looking IPs, real hostnames, real serial numbers). Use obviously fictional values documented in the OSIRIS specification examples (e.g. IPv4 Address Blocks Reserved for Documentation like `203.0.113.x` adhering to [RFC 5737](https://datatracker.ietf.org/doc/html/rfc5737)).
- Golden files **SHOULD** cover edge cases: empty topologies, resources with minimal fields, resources with full properties and extensions, custom provider namespaces, multiple connection types, nested group hierarchies.

### 4.1.3 Golden file maintenance workflow
1. Developer makes a mapping change in the producer.
2. Run the producer against the mocked input.
3. Diff the output against the golden file.
4. If the diff is intentional: update the golden file, document the change reason in the commit message.
5. If the diff is unintentional: investigate and fix the regression.
6. CI validates all golden files against `@osirisjson/core` on every commit.

---

## 4.2 Regression testing against schema and @osirisjson/core
Producers **MUST** integrate canonical validation into their CI pipeline. This ensures that no mapping change silently producing invalid OSIRIS output.

### 4.2.1 CI validation pipeline

```mermaid
flowchart LR
  BUILD["Producer
  build"] --> RUN["Run against
  test fixtures"]

  RUN --> DIFF["Diff against
  golden files"]

  DIFF --> VALIDATE["Validate with
  @osirisjson/core"]

  VALIDATE --> GATE{errors?}

  GATE -- "none" --> PASS["Pass"]
  GATE -- "any" --> FAIL["Fail"]
```

**Implementation:**
Producers invoke the canonical TypeScript validator via the CLI. Producers **MUST NOT** embed `@osirisjson/core` as a library; they call it as an external tool.

```bash
# Validate a single golden file at the strict profile
npx @osirisjson/cli validate --profile strict testdata/vendor_scenario_a/golden.json

# Validate all golden files in a directory
npx @osirisjson/cli validate --profile strict testdata/**/golden.json
```

### 4.2.2 What to test

| Test category | What to verify | Failure means |
|---|---|---|
| **Schema compliance** | All golden files pass Level 1 (structural) | Producer emits malformed OSIRIS |
| **Semantic integrity** | All golden files pass Level 2 (referential integrity, uniqueness) | Broken references, duplicate IDs |
| **Domain best practices** | Golden files pass Level 3 with no errors under `strict` | Non-standard types, missing recommended fields |
| **Determinism** | Running the producer twice with the same input produces identical output (byte-for-byte after normalization) | Non-deterministic ID generation, unstable ordering |
| **Redaction** | Golden files contain no credential patterns (run secret scanner on output) | Secret leakage in test fixtures |
| **Snapshot stability** | Golden file diffs are empty when input has not changed | Unintended mapping drift |

### 4.2.3 Version alignment
- Producer CI **SHOULD** pin the `@osirisjson/cli` version and update deliberately.
- When `@osirisjson/core` introduces new validation rules (e.g. in a MINOR release), producers **SHOULD** update their golden files to address any new warnings before tagging a release.
- Producers **SHOULD** declare which OSIRIS specification `MAJOR` version they target in their documentation and package metadata.

### 4.2.4 Snapshot comparison strategy
Producers **SHOULD** normalize JSON output before comparison to avoid false-positive diffs from insignificant formatting changes:
- Sort top-level arrays (`resources`, `connections`, `groups`) by `id`.
- Use consistent 2-space indentation.
- Emit a trailing newline.

This produces clean, reviewable diffs when golden files change intentionally.

**Go implementation note:**
- Do not build output arrays by iterating over maps. Collect into slices and **sort explicitly** (e.g. by `id`) before emission.
- Always sort `resources`, `connections`, and `groups` deterministically (and any nested arrays with semantic meaning).