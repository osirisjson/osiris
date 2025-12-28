# OSIRIS JSON Format Specification

| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 14 December 2025 |
| Last revision date | 28 December 2025 |
| Status    | Draft |
| Specification ID | OSIRIS-1.0 |
| Schema URI | tbd later |
| License   | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Repository | [github.com/osirisjson/osiris](https://github.com/osirisjson/osiris) |


## Table of Content
1. [Introduction](#1-intro)
    1.1 [Problem statement](#11-intro-problem-statement)
    1.2 [Solution overview](#12-intro-solution-overview)
    1.3 [Scope](#13-intro-scope)
    1.4 [Design principles](#14-intro-design-principles)
    1.5 [Terminology](#15-intro-terminology)

2. [Core concepts](#2-coreconc)
	2.1 [Resources](#21-coreconc-resources)
	2.2 [Connections](#22-coreconc-connections)
	2.3 [Groups](#23-coreconc-groups)
	2.4 [Metadata](#24-coreconc-metadata)

3. [Schema structure](#3-schstr)
	3.1 [Top-Level object](#31-schstr-toplevelobj)
	3.2 [Version](#32-schstr-version)
	3.3 [Metadata object](#33-schstr-metadataobj)
	3.4 [Topology object](#34-schstr-topologyobj)

4. [Resource model](#4-resmod)
	4.1 [Resource object structure](#41-resmod-resobjstructure)
	4.2 [Resource types](#42-resmod-restypes)
	4.3 [Provider information](#43-resmod-providerinfo)
	4.4 [Properties and extensions](#44-resmod-propertiesext)
	4.5 [Status and state](#45-resmod-statustate)

5. [Connection model](#5-conmod)
	5.1 [Connection object structure](#51-conmod-connobjstr)
	5.2 [Connection types](#52-conmod-conntype)
	5.3 [Directionality](#53-conmod-directionality)
	5.4 [Connection properties](#54-conmod-connprop)


## Preface
This document specifies the Open Standard for Infrastructure Resource Interchange Schema (OSIRIS), a JSON-based data format for describing infrastructure resources, their properties and their topological relationships in a vendor-neutral manner.

OSIRIS is intended to define a unified comprehensible schema to normalize data exported from diverse IT environments. OSIRIS is also designed to be extended to Operational Technology (OT) systems to support the inclusion of industrial infrastructure resources.

By decoupling the resource identity from its provider-specific implementation, OSIRIS enables a unified language format for cross-platform visibility, automated diagramming and standardized auditing without requiring closed-source programs or vendor-specific parsers for every consumption tool.


##### Intended Audience
This specification is intended for:

- **Developers** building visualization, documentation, inventory, CMDB or management tools.
- **Platform Engineers** designing automation and infrastructure-as-code workflows.
- **Solution Architects** documenting multi-cloud and hybrid environments.
- **Compliance or Risk Teams** requiring consistent infrastructure evidence and auditing inputs.

---

# 1 Introduction
OSIRIS (Open Standard for Infrastructure Resource Interchange Schema) defines a vendor-neutral JSON format for describing infrastructure resources, their properties and their topological relationships across heterogeneous IT and OT environments.

OSIRIS is designed to normalize infrastructure data exported from diverse domains, including public hyperscalers, cloud platforms, private Data Centers, network devices, virtualization platforms and where applicable Operational Technology (OT) assets. OSIRIS focuses on describing what exists and how it relates, enabling cross-platform visibility and portable consumption by tools without requiring consumers to implement vendor-specific parsers.

OSIRIS is an interchange schema. It is not intended to replace vendor APIs or infrastructure-as-code models, but viceversa it aims to provide a stable and consistent schema suitable for diagramming, documentation, audit evidence, inventories and integrations.

---

## 1.1 Problem statement
The modern infrastructure landscape span multiple technology stacks and providers, including hyperscalers and continuously evolving private and public cloud platforms, on-premises data centers featuring compute virtualization platforms, storage and network systems and continuously increasing OT integration. While many systems started offerin a comprehensible export format for resource inventories and configurations, the exported representations differ significantly across vendors and products, even when JSON is used. In OT environments the gap is even bigger due to legacy systems and vendor specific protocols.


### 1.1.1 Heterogeneous exports and inconsistent semantics:
Infrastructure data is tipically exposed through vendor-specific formats and models with inconsistent semantics for similar concepts:

- **Major hyperscalers:** Different providers expose fundamentally similar concepts (compute, storage, networking) using different export structures and naming conventions (e.g. differing resource identifiers, property models and relationship representations).

- **Network devices:** Device inventories and topologies data are often obtained via CLI output, NETCONF/YANG, vendor APIs or telemetry streams, each requiring specialized parsing and normalization.

- **Virtualization platforms:** APIs and exports vary widely (JSON, XML, proprietary models), with distinct field naming, resource hierarchies and relationship models.

- **Operational technology:** Building automation systems (BACnet, Modbus etc.) and industrial control systems use binary protocols or vendor-specific data representations with no standardized topology export.

As a result, producing a comprehensible, cross-platform view of infrastructure typically requires bespoke translation layers that are costly to build and difficult to sustain.


### 1.1.2 Consequences
This fragmentation creates recurring challenges:

- **Integration complexity:** Tools that consume infrastructure data must implement vendor-specific parsing and mapping logic and often maintain separate adapters for each source system and each consuming application’s internal model.

- **High maintenance cost:** Vendor formats and APIs evolve over time, requiring continuous updates to parsers and mappers.

- **Inconsistent topology semantics:** Relationships such as attached-to, connected-to, member-of, routes-through or depends-on are represented inconsistently or only implicitly, reducing reliability for diagram generation and analysis.

- **Duplicate effort across the ecosystem:** CMDBs, diagramming tools, monitoring platforms and compliance systems repeatedly re-implement similar vendor integrations, multiplying cost and slowing interoperability.

- **Documentation drift:** Without automated, format-agnostic interchange, documentation becomes outdated quickly and manual translation introduces errors.

- **Audit and compliance:** Producing consistent evidence across heterogeneous systems requires custom extraction and normalization per vendor, increasing time, complexity and risk.

- **Lock-in risk:** Organizations may depend on closed-source or vendor specific normalization solutions, limiting portability, extensibility and auditability.

The absence of a vendor-neutral interchange format means the infrastructure management ecosystem lacks a common language and producing a comprehensible hybrid cross-platform view typically requires bespoke translation layers that are costly to build and difficult to sustain.

---

## 1.2 Solution overview
OSIRIS addresses the fragmentation described in Section 1.1 by defining a canonical JSON schema that serves as a neutral interchange format between infrastructure data sources and consuming applications.

Rather than requiring each consuming application to implement and maintain vendor-specific parsers, OSIRIS adopts a translation layer approach:

- **Producers** (exporters, translators or discovery agents) transform vendor-specific representations into OSIRIS format.
- **Consumers** (diagramming tools, CMDBs, IPAMs, DCIMs, monitoring systems, documentation pipelines) read a single, Open Standard well-defined schema.

This decouples data sources from consuming applications, reducing integration complexity from S×C (S sources × C consumers) to P+C (P producers + C consumers)

This decouples data sources from consuming applications. In environments with **S** source systems and **C** consumer applications, the number of required integrations is reduced from potentially **S×C** vendor-to-tool mappings to **S** producer mappings (source > OSIRIS) plus **C** consumer mappings (OSIRIS > application).


### 1.2.1 Core capabilities
OSIRIS provides:

1. **Unified resource model:** A common structure for representing infrastructure resources (compute, network, storage, applications) with consistent field semantics regardless of origin.

2. **Explicit relationship model:** First-class representation of connections, dependencies, containment and other topological relationships that are often implicit or vendor-specific.

3. **Grouping constructs:** Support for logical and physical grouping that reflects organizational and architectural structure without mandating a single taxonomy.

4. **Provider attribution:** Resources retain traceability to their originating platform (e.g. AWS, Azure, GCP, Arista Networks, Nokia, Cisco, Proxmox) while being described in standardized terms enabling mixed-source topologies.

5. **Extensibility:** A defined mechanism for vendor-specific properties and custom resource types without breaking schema compatibility.


### 1.2.2 Design Approach
OSIRIS is designed as a **static snapshot format** describing infrastructure state at a point in time.

OSIRIS is not intended to be:
- A real-time monitoring or telemetry protocol
- An infrastructure-as-code deployment language
- A configuration management system

Instead, OSIRIS is optimized for interchange scenarios where infrastructure topology must be communicated between systems: 

- generating high quality documentation
- producing accurate diagrams
- feeding CMDBs, IPAMs or DCIMs
- supporting audit workflows
- enabling cross-platform analytics

By providing a stable interchange schema, OSIRIS allows the ecosystem to develop reusable parsers (vendor > OSIRIS) and consumers (OSIRIS > application-specific models) independently as an Open Standard, fostering interoperability and reducing duplicate and complexity effort.

---

## 1.3 Scope
OSIRIS defines an **Open Standard JSON schema** for describing infrastructure resources and their topological relationships across heterogeneous environments.


### 1.3.1 In Scope
**Infrastructure domains covered by OSIRIS v1.0 include:**

- **Hyperscalers and public clouds:** Virtual networks, storage resources, compute instances and platform services exported from hyperscalers and public cloud providers.

- **Network infrastructure:** Physical and virtual network devices including routers, switches, firewalls, load balancers and their interconnections.

- **Virtualization platforms:** Hypervisors, virtual networks, virtual machines and storage constructs from virtualization systems.

- **Storage systems:** Storage arrays, volumes, file systems, object storage and storage network topology elements.

- **Application resources:** Application components, services, containers and repositories relevant to infrastructure topology.

- **Operational technology (OT initial support):** Building automation systems (BAS), industrial control systems and physical infrastructure components where integration with IT systems is relevant with the goal of providing an holistic end-to-end interchange visibility.


#### OSIRIS represents the following topology elements:
- Resource properties and attributes
- Connections and relationships between resources (e.g. network connectivity, dependencies, containment, data flow)
- Logical and physical grouping structures (e.g. VPCs, subnets, resource groups, availability zones, racks, data centers)
- Provider attribution and source metadata
- Status and state information


### 1.3.2 Out of Scope
The following are explicitly **out of scope** for OSIRIS v1.0:

- **Real-time telemetry and monitoring:** OSIRIS **describes** topology and inventory state, not time-series metrics, logs or observability data. Projects like OpenTelemetry or Prometheus, Grafana address telemetry concerns.

- **Infrastructure deployment and orchestration:** OSIRIS is not an infrastructure-as-code language or a deployment specification. Projects like Terraform, CloudFormation or TOSCA Oasis address deployment workflows.

- **Configuration management:** OSIRIS does not define or enforce desired state configuration for resources. Configuration management systems like Ansible, Puppet or Chef serve this purpose.

- **Change tracking and history:** OSIRIS represents a point-in-time snapshot. Tracking changes over time is the responsibility of producing and consuming systems.

- **Authentication, authorization and access policies:** OSIRIS may carry metadata describing boundaries or ownership, but it does not define access control mechanisms or policies.

- **Cost and billing information:** Detailed billing and financial modeling are outside the core schema scope, although cost-related metadata may be included by extension.


### 1.3.3 Version specific considerations
OSIRIS v1.0 prioritizes:

1. **Core IT infrastructure:** Network, storage and compute resources across hyperscalers, public cloud providers and on-premises environments.

2. **Common resource types:** Resource categories and relationships found across multiple platforms receive first-class support.

3. **Practical extensibility:** The extension mechanism enables representation of vendor-specific or specialized resources without requiring immediate schema updates.

4. **OT integration as secondary objective:** While OT resources are architecturally feseable and supported conceptually, comprehensive OT taxonomy and relationship modeling may evolve in subsequent versions based on community feedback.


### 1.3.4 Future Extensibility
OSIRIS is designed to accommodate:

- Additional resource types through the extension mechanism
- Domain-specific taxonomies (e.g. expanded OT support, edge computing, IoT)
- Enhanced relationship semantics
- Additional metadata models

The core schema structure and extension mechanisms are designed primarly for stability, allowing implementations to handle future resource types and properties without breaking compatibility during time.

---

## 1.4 Design principles
OSIRIS is guided by the following core principles:

### 1.4.1 Open Standard and driven by Community Governance
OSIRIS is an **Open Standard** intended for broad adoption across vendors, platforms and communities. The specification and evolution of the schema are intended to be community-driven and openly reviewable.

### 1.4.2  Simplicity
OSIRIS prioritizes ease of understanding and implementation. The schema uses straightforward JSON structures with clear field semantics. Complexity is introduced only where necessary to represent real-world infrastructure. Producers and consumers **SHOULD** be able to implement basic OSIRIS support without extensive specialized knowledge.

### 1.4.3  Vendor neutrality
OSIRIS does not favor any particular vendor, platform or technology. Infrastructure sources including hyperscalers, cloud providers, on-premises systems and OT environments are represented using consistent patterns. Provider specific details are accommodated through well defined extension mechanisms rather than privileged positions in the core schema.

### 1.4.4  Extensibility without fragmentation
OSIRIS supports vendor-specific properties, custom resource types and domain-specific extensions without breaking compatibility. Extensions follow structured conventions to preserve interoperability: consumers that do not recognize extensions **MUST** be able to safely ignore them while still processing core data.

### 1.4.5  Explicit over implicit
Relationships, dependencies and topological connections are represented explicitly. Resource properties are clearly named with defined semantics. The schema avoids ambiguous or context-dependent interpretations that would require out-of-band knowledge to resolve.

### 1.4.6  Stability and compatibility
The core schema structure is designed for long-term stability. Version changes follow semantic versioning principles. Backwards compatibility **SHOULD** be maintained across minor versions and breaking changes **MUST** be introduced only in major versions with clear migration guidance and deprecation policies.

### 1.4.7  Practicality and parial data
OSIRIS is designed for real-world infrastructure conditions, including incomplete inventories, mixed-vendor environments and partial topology exports. The schema supports scenarios where full resource details or relationships are unavailable, enabling incremental adoption.

#### Interoperability
OSIRIS uses the widely supported JSON format and aligns with established conventions, including JSON Schema for structural validation and [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119.html) keywords for normative requirements. OSIRIS is designed to integrate with existing toolchains, programming languages and data processing pipelines without requiring specialized runtimes.

#### Human readability
While OSIRIS is machine-readable by design, the JSON structure remains human-readable for debugging, auditing and manual inspection. Field names use clear, descriptive terminology and the schema avoids unnecessary encoding that would obscure content.

#### Semantic richness
OSIRIS represents not only resource existence but also meaningful relationships and hierarchies. The schema distinguishes between different connection types (e.g. network connectivity, dependency, containment). It supports grouping constructs and preserves provider attribution to enable accurate visualization and analysis.

#### Separation of concerns
OSIRIS focuses on **Infrastructure Resources Interchange**. It does not define deployment orchestration, monitoring or telemetry, configuration management or access control. This focused scope allows OSIRIS to serve as a stable interchange layer that complements specialized systems.

---

## 1.5 Terminology
This section defines key terms used throughout this specification.


### 1.5.1 Normative keywords
The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**,  **MAY** and **OPTIONAL** in this document are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119.html).


### 1.5.2 Core terms
**OSIRIS document:** A JSON document conforming to the OSIRIS specification, representing an infrastructure documentation at a specific point in time.

**OSIRIS topology:** The complete set of resources, connections, groups and metadata describing an infrastructure environment and its relationships at a specific point in time conforming to the OSIRIS specification.

**Resource:** A uniquely identifiable infrastructure entity with properties and relationships (e.g. compute instances, network devices, storage systems, applications).

**Connection:** An explicit relationship between two resources (e.g. network connectivity, dependency, containment or data flow).

**Group:** A logical or physical collection used to organize resources (e.g. VPC, subnet, resource group, availability zone, hypervisor, physical server, rack, data center).

**Producer:** A system or component that generates OSIRIS documents by exporting, translating or discovering infrastructure data from source systems.

**Consumer:** A system or component that reads and processes OSIRIS documents (e.g. diagramming tools, CMDBs, monitoring systems, documentation generators).

**Provider:** The originating platform, vendor or system from which a resource was sourced (e.g. AWS, Azure, Arista Networks, Cisco, Proxmox).

**Resource type:** A classification that defines what kind of infrastructure entity a resource represents (e.g. compute.vm, network.switch, storage.volume).

**Connection type:** A classification that defines the semantic meaning of a relationship between resources (e.g. network, attached_to, contains, dependency, routes_through).

**Extension:** Namespaced vendor-specific or custom properties and types that extend OSIRIS without modifying the core schema structure.

**Topology:** The complete set of resources, connections and groups that describe an infrastructure environment's structure and relationships.


### 1.5.2 Domain specific terms
**Hyperscaler:** A large-scale provider offering a broad portfolio of infrastructure services (e.g. AWS, Azure, GCP oracle Cloud, IBM Cloud, Alibaba Cloud, Tencent Cloud).

**Cloud provider:** A minor provider offering cloud services such as IaaS, PaaS, SaaS or NaaS.

**On-Premise infrastructure:** Computing, networking and storage resources operated within an organization’s facilities rather than in a cloud provider environment.

**Operational Technology (OT):** Hardware and software systems that monitor and control physical devices, processes and infrastructure, including building automation systems (BAS), industrial control systems (ICS) and supervisory control and data acquisition (SCADA) systems.

**IT-OT convergence:** The integration of IT systems with OT systems to enable unified end-to-end visibility and management.

**Interchange format:** A standardized data representation designed for exchange between heterogeneous systems and not tied to any single tool’s internal model.


### 1.5.3 JSON and schema terms
**JSON (JavaScript Object Notation):** The data serialization format used by OSIRIS, as defined in [RFC 8259](https://www.rfc-editor.org/rfc/rfc8259.html).

**JSON Schema:** A vocabulary for validating the structure of JSON documents, used to define the formal structure of OSIRIS documents.

**Required field:** A field that **MUST** be present in a valid OSIRIS document or object.

**Optional field:** A field that **MAY** be present but is not requird for validity.

**Additional properties:** Fields beyond those explicitly defined in the schema, typically used for extensions or vendor-specific data.

---

# 2 Core concepts
This section introduces the fundamental building blocks of OSIRIS: **Resources**, **Connections**, **Groups** and **Metadata**. Understanding these core concepts is essential before examining the detailed schema structure in subsequent sections.

OSIRIS models infrastructure as a graph composed of:

- **Resources (nodes):** Individual infrastructure components with identity, properties and state.
- **Connections (edges):** Explicit relationships between resources representing dependencies, network links or containment.
- **Groups:** Collections of resources organized by logical or physical boundaries.
- **Metadata:** Contextual information describing the topology’s origin, scope and provenance.

Together these elements enable OSIRIS to represent complex, heterogeneous infrastructure topologies in a structured, vendor-neutral format.

---

## 2.1 Resources
### 2.1.1 Definition
A **Resource** represents a discrete infrastructure component with identity, properties and lifecycle. Resources are the fundamental baseline units of an OSIRIS topology and **MAY** correspond to (non-exhaustive examples):

- **Compute resources:** virtual machines, containers, physical servers.
- **Network devices:** routers, switches, firewalls, load balancers.
- **Storage systems:** volumes, arrays, buckets, file systems.
- **Platform services:** databases, message queues, managed services.
- **Application components:** services, repositories, deployments.
- **Operational technology elements:** building automation controllers, industrial sensors, SCADA components.


### 2.1.2 Resource identity
Every resource **MUST** have a unique identifier within an OSIRIS document. Resource identifiers enable references from connections, group memberships and other objects.

Resource identity in OSIRIS is **document-scoped**: identifiers **MUST** be unique within a single OSIRIS document but need not be globally unique across other documents or systems. This simplifies producer implementation while preserving referential integrity within each snapshot.

Resources **SHOULD** retain stable identifiers across multiple exports of the same infrastructure when feasible. Stable identifiers enable consumers to correlate resources over time, detect changes and compare snapshots.


### 2.1.3 Resource types
Each resource **MUST** declare a type that classifies what kind of infrastructure component it represents. Resource types follow a hierarchical dot notation convention (e.g. `compute.vm`, `network.switch`, `storage.volume`) that supports both categorization and specificity.

The type system includes:

- **Standard types:** common resource categories defined in chapter 7 (Resource Type Taxonomy).
- **Vendor specific types:** platform specific types using namespaced conventions (see chapter 8).
- **Custom types:** organization defined types for specialized or internal components.

Consumers **MUST** be able to process resources with unknown types by treating them as opaque resources while still preserving identity, properties and relationships.


### 2.1.4 Provider attribution
Resources retain attribution to their originating system through provider information. 

Provider attribution enables:

- traceability to source systems for validation or enrichment.
- mixed-vendor topologies where resources from different platforms coexist.
- platform-specific rendering or behavior in consuming tools (when supported).
- audit trails and source tracking.

Provider information typically includes provider name (e.g. AWS, Azure, Arista, Cisco, Proxmox), native resource identifiers, account/subscription/project context and regional or zonal placement where applicable.


### 2.1.5 Resource properties
Resources carry properties that describe characteristics, configuration and operational attributes. 

Properties **MAY** include:

- **Intrinsic attributes:** size, capacity, network addresses, operating system.
- **Configuration details:** enabled features, policies, attached resources.
- **Operational indicators:** running, stopped, healthy, degraded (as applicable).
- **Vendor-specific metadata:** fields relevant to particular platforms.

The core OSIRIS schema defines a standard property structure. The extension mechanism allows producers to include additional properties without breaking schema validity. Consumers that do not recognize extended properties **MUST** safely ignore them while processing core fields.


### 2.1.6 Resource status and state
Resources **MAY** include high-level status and/or state information to support filtering, visualization and operational awareness.

- **Status** generally refers to lifecycle presence or availability within the snapshot context.
- **State** generally refers to an operational condition (where applicable).

The exact fields and allowed values are defined in the chapter 4 Resource model (see Section 4.5).


### 2.1.7 Resource lifecycle considerations
OSIRIS represents infrastructure at a point in time. Resources reflect their observed state when the OSIRIS document was generated, not ongoing changes. Lifecycle events such as creation, modification or deletion are not represented within a single document but **MAY** be inferred by comparing multiple snapshots.

Resources that no longer exist in source systems **SHOULD NOT** appear in subsequent exports unless producers intentionally emit tombstones or inactive entries for change-tracking purposes (as defined by the schema and producer behavior).

---

## 2.2 Connections
### 2.2.1 Definition
A **Connection** represents an explicit relationship between two resources. Connectios are the edges in the OSIRIS topology graph and describe how resources interact depend on each other or are organized hierarchically.

Connections capture relationships that **MAY** include (non-exhaustive examples):

- **Network connectivity:** Layer 2/Layer 3 links, routing neighbours, firewall rules, VPN tunnels.
- **Dependencies:** Applications depending on databases, services consuming APIs, load balancers distributing to backends.
- **Containment:** Virtual machines within hypervisors, containers within pods, resources within VPCs or subnets.
- **Data flow:** Storage mounted to compute, backup relationships, replication streams.
- **Physical topology:** Cables between switches, power distribution, rack-to-server relationships.
- **Logical associations:** Membership in availability zones, assignment to projects, tagging relationships.


### 2.2.2 Connection identity
Every connection **MUST** have a unique identifier within an OSIRIS document. Connection identifiers enable reference and validation.

Connection identity is **document-scoped**, analogous to resource identity. Identifiers **MUST** be unique within a single OSIRIS document but need not be globally unique.


### 2.2.3 Source and target
Each connection **MUST** identify a source resource and a target resource using their respective resource identifiers.

Both source and target identifiers **MUST** reference resources that exist within the same OSIRIS document. Connections **MUST NOT** reference resources outside the document boundary.

Producers **SHOULD** validate referential integrity before emitting OSIRIS documents. Consumers **MUST** handle connections with invalid references gracefully, either by reporting validation errors or by ignoring malformed connections while processing valid topology data.


### 2.2.4 Connection types
Each connection **MUST** declare a type that classifies the nature of the relationship. Connection types enable consumers to distinguish between different kinds of relationships and apply appropriate logic for visualization, analysis or validation.

The connection type system includes:

- **Standard types:** common relationship types defined in chapter 5 Section 5.2 (Connection Types), such as `network`, `dependency`, `contains`, `dataflow`.
- **Vendor-specific types:** platform specific relationships using namespaced conventions (see Chapter 8).
- **Custom types:** organization defined types for specialized relationships.

Consumers **MUST** be able to process connections with unknown types by treating them as generic relationships while preserving their source, target and properties.


### 2.2.5 Directionality
Connections **MAY** specify directionality to indicate the orientation or flow of the relationship:

- **Bidirectional:** The relationship applies equally in both directions (e.g. a Layer 2 network link, peering relationships).
- **Directed (source > target):** The relationship flows from source to target (e.g. a service depends on a database, traffic flows from client to server).

If directionality is unspecified, consumers **SHOULD** treat the connection as bidirectional unless the connection type definition explicitly specifies otherwise.

Directionality is distinct from the graph structure itself: the source and target fields define graph topology, while directionality provides semantic annotation about how the relationship should be interpreted.


### 2.2.6 Connection properties
Connections **MAY** carry properties that describe relationship characteristics:

- **Protocol or mechanism:** TCP, HTTP, NFS, iSCSI, routing protocol.
- **Capacity or bandwidth:** Link speed, throughput limits.
- **Ports or endpoints:** Source/destination ports, IP addresses.
- **Quality attributes:** Latency, packet loss, reliability indicators.
- **Administrative metadata:** Tags, labels, ownership, creation time.

As with resources, the extension mechanism allows producers to include vendor specific or custom properties without breaking schema validity. Consumers that do not recognize extended properties **MUST** safely ignore them.


### 2.2.7 Connection status
Connections **MAY** include status information indicating operational health or availability to support filtering and visualization.

Common values include `active`, `inactive`, `degraded` and `unknown`. The exact field structure and allowed values are defined in chapter 5 Connection model (see Section 5.4 Connection properties).


### 2.2.8 Implicit vs. explicit relationships
OSIRIS requires connections to be **explicit**. Relationships that could be inferred from resource properties (e.g. a VM referencing a VPC ID in its properties) **SHOULD** also be represented as explicit connection objects.

Explicit representation ensures:

- consuming tools do not require inference logic specific to vendor property conventions.
- relationship semantics are clear and unambiguous.
- topology graphs can be traversed using connection objects alone.

Producers **MAY** emit both property based references and explicit connections for redundancy and compatibility with different consumer implementations.


### 2.2.9 Multiple connections between resources
Two resources **MAY** have multiple connections of different types or with different properties.

For example:

- A virtual machine and a storage volume may have both a **contains** relationship and a **dataflow** relationship.
- Two switches may have multiple **network** connections representing distinct physical links.

Each distinct relationship **MUST** be represented as a separate connection object with a unique identifier.

---

## 2.3 Groups
### 2.3.1 Definition
A **Group** represents a logical or physical collection of resources organized by common characteristics, boundaries or administrative domains. Groups provide structure to OSIRIS topologies and enable representation of organizational, architectural or geographical boundaries.

Groups **MAY** represent (non-exhaustive examples):

- **Network boundaries:** VPCs/virtual networks, subnets, VLANs, VRFs, security zones.
- **Resource organization:** resource groups, projects, namespaces, folders, tenants.
- **Physical location:** data centers, buildings, floors, rooms, racks, rows.
- **Availability constructs:** regions, availability zones, fault domains.
- **Administrative boundaries:** subscriptions, accounts organizational units.
- **Functional grouping:** application tiers (web/app/db), environments (production/staging/development).


### 2.3.2 Group identity
Every group **MUST** have a unique identifier within an OSIRIS document. Group identifiers enable reference from other groups and from metadata contexts.

Group identity is **document-scoped**, consistent with resources and connections. Identifiers **MUST** be unique within a single OSIRIS document but need not be globally unique.


### 2.3.3 Group types
Each group **MUST** declare a type that classifies what kind of collection or boundary it represents. Group types follow a hierarchical dot-notation convention similar to resource types (e.g. `network.vpc`, `logical.environment`, `physical.datacenter`).

The group type system includes:

- **Standard types:** common grouping categories defined in Chapter 6 Group model, Section 6.2 (Group types).
- **Vendor-specific types:** platform specific grouping constructs using namespaced conventions (see Chapter 8 Extensive mechanism).
- **Custom types:** organization defined types for specialized grouping needs.

Consumers **MUST** be able to process groups with unknown types by treating them as generic collections while preserving membership and hierarchy.


### 2.3.4 Membership (resources)
Groups contain **members**, which are resource identifiers representing the resources that belong to the group. Membership is represented as an array of resource IDs.

All member identifiers **MUST** reference resources that exist within the same OSIRIS document. Groups **MUST NOT** reference resources outside the document boundary.

A resource **MAY** be a member of multiple groups simultaneously. For example, a physical server may belong to both a rack (`physical.rack`) and an environment group (`logical.environment.production`), while a VM may belong to both a subnet group (`network.subnet`) and an application tier group (`logical.tier.web`).

Membership is **explicit and direct**: resources listed in a group are members of that group only. Consumers that require transitive membership **SHOULD** implement traversal logic.


### 2.3.5 Hierarchical groups (nesting)
Groups **MAY** define hierarchy by referencing other groups by identifier as children (sub-groups). Group hierarchies **MUST NOT** contain cycles.

This enables nested organizational structures such as:

- A data center group containing building groups.
- A building group containing floor groups.
- A floor group containing room groups.
- A room group containing rack groups.
- A rack group containing server groups.
- A server group containing resources (via membership).

Group nesting **MUST** be explicit (e.g. a `children[]` list of group IDs). Resources that are members of a child group are **not automatically** members of the parent group unless explicitly listed (e.g. no implicit transitive membership).

Producers **MUST** ensure that group hierarchies do not create circular references. Consumers **SHOULD** detect and handle circular hierarchies gracefully, either by reporting validation errors or by ignoring problematic relationships.


### 2.3.6 Group properties
Groups **MAY** carry properties that describe collection characteristics:

- **Boundaries or constraints:** IP ranges (subnets), VLAN IDs, VRF names, geographic coordinates (data centers).
- **Configuration or policies:** security policies, routing constraints, segmentation intent.
- **Capacity or limits:** quotas, available rack units, power capacity.
- **Administrative metadata:** tags, labels, ownership, creation time.

As with resources and connections, the extension mechanism allows producers to include vendor-specific or custom properties without breaking schema validity. Consumers that do not recognize extended properties **MUST** safely ignore them.


### 2.3.7 Groups vs. connections
Groups and connections serve complementary purposes:

- **Groups** represent **containment and organization** (boundaries, membership, hierarchy).
- **Connections** represent **relationships and interactions** (communication, dependency, linkage).

Example (in hybrid environments):
- A VM or server is a **member** of a subnet/VLAN group (structure).
- A load balancer **dependency** or **routes_to** backend instances via **connections** (behavior/interaction).

Producers **SHOULD** choose the representation that most clearly expresses the intended semantics. Consumers **SHOULD** support both patterns and **MUST NOT** assume exclusive use of either mechanism.


### 2.3.8 Empty groups
Groups **MAY** have an empty membership list (zero members). Empty groups are valid and may represent:

- Pre-defined organizational structures awaiting resource assignment.
- Placeholder groups for documentation or planning.
- Structures preserved across exports even when temporarily unused.

Consumers **MUST** handle empty groups without error.

---

## 2.4 Metadata
### 2.4.1 Definition
**Metadata** provides contextual information about an OSIRIS document, including its origin, scope, generation details and provenance. Metadata enables consumers to understand what infrastructure the document represents, when it was produced and how to interpret or validate its contents.

Metadata is distinct from resource properties: metadata describes the **document and topology as a whole** not individual infrastructure components.


### 2.4.2 Purpose of metadata
Metadata serves several critical functions in the OSIRIS interchange model:

- **Provenance tracking:** Identifying the source system, tool or process that generated the document.
- **Temporal context:** Recording when the snapshot was taken to enable time-based correlation or change detection.
- **Scope documentation:** Describing what infrastructure is included (regions, accounts, environments) and what is excluded.
- **Specification context:** Indicating which OSIRIS version the document conforms to.
- **Audit trail:** Supporting compliance and governance workflows by documenting data lineage.
- **Consumer guidance:** Providing advisory hints that help consuming tools interpret or validate the topology.


### 2.4.3 Core metadata elements
OSIRIS metadata **MUST** include at minimum:

- **Timestamp:** An ISO 8601 timestamp indicating when the topology snapshot was generated (e.g. `2025-12-18T14:30:00Z`).

OSIRIS metadata **SHOULD** include:

- **Generator information:** The name and version of the tool or system that produced the document.
- **Scope description:** What providers, regions, accounts/projects/subscriptions or environments are represented.

OSIRIS metadata **MAY** include:

- **Tags or labels:** Custom key-value pairs for organizational categorization.
- **Source references:** References to originating systems, API endpoints or datasets used to construct the topology.
- **Validation hints:** Advisory information to assist consumers in validating or interpreting the topology.

> [!NOTE] 
> The OSIRIS specification version is declared at the document level (see Section 3.2). Metadata **MAY** repeat this information for convenience, but any duplicated version information **MUST** be consistent.


### 2.4.4 Metadata extensibility
Like resources, connections and groups, metadata supports extension through custom properties. Producers **MAY** include vendor-specific or organization-specific metadata fields without breaking schema validity.

Consumers that do not recognize extended metadata properties **MUST** safely ignore them while processing the document.


### 2.4.5 Metadata vs. resource properties
It is important to distinguish metadata from resource level properties:

- **Metadata** describes the OSIRIS document: "This topology was exported from AWS on 2025-12-18 at 14:30 UTC covering us-east-1 and us-west-2a."
- **Resource properties** describe individual components: "This instance is `t3.medium` with 2 vCPUs and 4 GB RAM."

Information that applies to the entire document or export process belongs in metadata. Information that applies to a specific resource belongs in that resource's properties.


### 2.4.6 Metadata and change tracking
While OSIRIS represents infrastructure at a point in time, metadata enables consumers to correlate multiple snapshots over time:

- Timestamps allow chronological ordering of documents.
- Generator information enables filtering by source or tool version.
- Scope descriptions help identify whether documents represent comparable infrastructure sets.

Change detection and versioning logic are the responsibility of consuming systems but metadata provides the context to support these workflows.


### 2.4.7 Metadata stability
Metadata structure is part of the core OSIRIS schema and follows the same versioning and compatibility rules as the rest of the specification.

Consumers **SHOULD** validate metadata before processing topology content to detect version mismatches or unsupported schema features early.

---

# 3 Schema Structure
This section defines the structure of an OSIRIS document at the JSON schema level. It describes the top-level object, required fields and the organization of topology data.

OSIRIS documents are JSON objects that conform to [RFC 8259](https://www.rfc-editor.org/rfc/rfc8259.html). The schema is formally defined using JSON Schema (see Appendix A).

---

## 3.1 Top-Level Object
### 3.1.1 Structure
An OSIRIS document **MUST** be a JSON object containing **at least** the following top-level fields:

```json
{
  "version": "1.0.0",
  "metadata": { ... },
  "topology": { ... }
}
```


### 3.1.2 Required fields
The following fields are **REQUIRED** at the top level:

- **`version`** (string): The OSIRIS specification version to which this document conforms. See Section 3.2 for version string format and semantics.

- **`metadata`** (object): Contextual information about the document's origin, scope and generation. See Section 3.3 for metadata object structure.

- **`topology`** (object): The infrastructure topology data, including resources, connections and groups. See Section 3.4 for topology object structure.


### 3.1.3 Additional top-level fields
OSIRIS v1.0 defines only `version`, `metadata` and `topology` at the top level.

Producers **SHOULD NOT** emit additional top-level fields in v1.0 documents.

Consumers **MUST** ignore unknown top-level fields and **MAY** emit warnings about unrecognized fields. This ensures forward compatibility if future OSIRIS versions introduce additional top-level constructs (e.g. signatures, links, fragments).


### 3.1.4 Document size considerations
OSIRIS does not impose a maximum document size, but producers **SHOULD** consider practical limits:

- Large topologies (thousands of resources) may benefit from splitting across multiple documents.
- Consumers may have memory or processing constraints that limit document size.
- Network transmission and storage efficiency favor reasonably-sized documents.

Producers generating large topologies **MAY** split infrastructure across multiple OSIRIS documents based on logical boundaries (e.g. per region, per environment, per account) and document the scope in metadata.

---

## 3.2 Version
### 3.2.1 Field definition
The `version` field is a **REQUIRED** string at the top level of every OSIRIS document. It declares which version of the OSIRIS specification the document conforms to.


### 3.2.2 Version string format
The version string **MUST** follow semantic versioning principles as defined in [Semantic Versioning 2.0.0](https://semver.org).

Version strings **MUST** use the format: `MAJOR.MINOR.PATCH`

Examples:
- `1.0.0` - OSIRIS version 1.0.0
- `1.1.0` - OSIRIS version 1.1.0 (minor update, backward compatible)
- `2.0.0` - OSIRIS version 2.0.0 (major update, may contain breaking changes)


### 3.2.3 Version semantics
Version numbers convey compatibility guarantees:

- **MAJOR version** increments indicate breaking changes that are not backward compatible. Documents with different MAJOR versions may use incompatible schema structures or semantics.

- **MINOR version** increments indicate backward-compatible additions or enhancements. New optional fields, resource types or connection types may be introduced. Existing fields and semantics remain unchanged.

- **PATCH version** increments indicate backward-compatible fixes or clarifications to the specification that may affect interpretation or validation behavior, but do not change schema structure.


### 3.2.4 Consumer version handling
Consumers **MUST** check the `version` field before processing an OSIRIS document.

Consumers **SHOULD** implement the following compatibility logic:

- **Different MAJOR version**: Document is unsupported. Consumer **SHOULD** reject the document or emit an error.

- **Same MAJOR version**: Document is supported.
  - If the document has a **higher MINOR** version than the consumer supports, the consumer **MUST** ignore unknown fields and **MAY** emit informational warnings.
  - If the document has the **same MINOR** version and a **higher PATCH** version, the consumer **MUST** accept the document.

Example:
- Consumer supports OSIRIS `1.0.0`
- Document version `1.0.5` > **Accept** (same major.minor, higher patch)
- Document version `1.2.0` > **Accept, ignore unknown fields, MAY warn** (same major, higher minor)
- Document version `2.0.0` > **Reject or error** (different major)


### 3.2.5 Version field placement
The `version` field appears at the document's top level only. It does not appear in nested objects (metadata, resources, connections, groups).

Metadata **MAY** include generator or tool version information, but this is distinct from the OSIRIS specification version declared in the top-level `version` field.

---

## 3.3 Metadata Object
### 3.3.1 Structure
The `metadata` field is a **REQUIRED** object at the top level of every OSIRIS document. It provides contextual information about the document's generation, scope and provenance as described in Section 2.4.

A minimal metadata object:
```json
{
  "timestamp": "2025-12-18T14:30:00Z"
}
```


### 3.3.2 Required fields
The following field is **REQUIRED** within the metadata object:

- **`timestamp`** (string): An ISO 8601 formatted timestamp indicating when the topology snapshot was generated. The timestamp **MUST** include date and time and **MUST** include a timezone designator (either `Z` for UTC or an offset such as `+02:00` or `-05:00`).

> [!NOTE] 
> OSIRIS recommends using RFC 3339 compatible timestamps for maximum interoperability.

Examples of valid timestamps:
  - `2025-12-18T14:30:00Z` (UTC)
  - `2025-12-18T14:30:00+00:00` (UTC with offset notation)
  - `2025-12-18T09:30:00-05:00` (Eastern Time with offset)

> [!WARNING] 
> Timestamps without timezone designators (e.g. `2025-12-18T14:30:00`) are **NOT VALID**.


### 3.3.3 Optional fields
The following fields are **OPTIONAL** but **RECOMMENDED**:

- **`generator`** (object): Information about the tool or system that produced the document. If present, the generator object **SHOULD** contain:
  - `name` (string): Name of the generating tool or system
  - `version` (string): Version of the generating tool
  - `url` (string, optional): Reference URL for the generator

Example:
```json
  {
    "name": "osiris-aws-exporter",
    "version": "1.2.3",
    "url": "https://github.com/osirisjson/hyperscaler-parser/aws/osiris-aws-exporter"
  }
```

- **`scope`** (object): Description of what infrastructure is represented in this document. The scope object **MAY** contain:
    - `name` (string): Human readable name for this topology
    - `description` (string): Textual description of the scope
    - `providers` (array of strings): List of infrastructure providers included (e.g. `["aws", "azure"]`)
    - `regions` (array of strings): Geographic regions or zones included
    - `accounts` (array of strings): Account, subscription or project identifiers
    - `environments` (array of strings): Environment designations (e.g. `["production", "staging"]`)
    - `sites` (array of strings): On-premises sites or facility identifiers (e.g. `["MXP-DC-1", "AGP-DC-2"]`)
    - `clusters` (array of strings): Cluster identifiers (e.g. `["proxmox-prod"]`, `["vsphere-cluster-a"]`)


> [!NOTE] 
> Detailed physical hierarchy (building/floor/room/row/rack) **SHOULD** be represented using **groups** in the topology.

Hyperscaler example:
```json
  {
    "name": "Production Infrastructure - US East",
    "providers": ["aws"],
    "regions": ["us-east-1", "us-east-2"],
    "accounts": ["1234567890"],
    "environments": ["production"]
  }
```


### 3.3.4 Extension fields
The metadata object **MAY** contain additional fields beyond those defined above. These extension fields enable producers to include:

- Custom organizational metadata (cost centers, ownership, compliance tags)
- Source system references (API endpoints, query parameters)
- Validation or processing hints
- Vendor-specific context

Consumers **MUST** ignore unrecognized metadata fields. Consumers **MAY** preserve unknown fields when transforming or re-exporting OSIRIS documents, provided such fields do not violate security or redaction requirements.


### 3.3.5 Metadata object size
Metadata objects **SHOULD** be concise. Large amounts of auxiliary data (e.g. full audit logs, detailed cost breakdowns) **SHOULD** be stored externally and referenced via URL or identifier rather than embedded directly in metadata.


### 3.3.6 Complete metadata example
#### Hyperscaler example
```json
{
  "timestamp": "2025-12-18T14:30:00Z",
  "generator": {
    "name": "osiris-multihyperscalers-exporter",
    "version": "2.1.0"
  },
  "scope": {
    "name": "Production multi Hyperscalers topology",
    "description": "Primary production workloads across AWS and Azure",
    "providers": ["aws", "azure"],
    "regions": ["us-east-1", "eastus"],
    "environments": ["production"]
  },
  "tags": {
    "cost-center": "engineering",
    "compliance": "sox"
  }
}

```

#### On-premise example
```json
{
  "timestamp": "2025-12-18T14:30:00Z",
  "generator": {
    "name": "osiris-hybrid-exporter",
    "version": "0.9.0"
  },
  "scope": {
    "name": "Hybrid production topology",
    "description": "AWS workloads including on-prem DCs fabric with Proxmox cluster",
    "providers": ["aws", "arista", "dell", "proxmox"],
    "regions": ["eu-west-1"],
    "sites": ["MXP-DC-1"],
    "clusters": ["proxmox-prod-1"],
    "environments": ["production"]
  },
  "tags": {
    "owner": "it-systems-services",
    "compliance": "iso27001"
  }
}
```


---

## 3.4 Topology Object
### 3.4.1 Structure
The `topology` field is a **REQUIRED** object at the top level of every OSIRIS document. It contains the infrastructure topology data: resources, connections between resources and logical or physical groupings.

A minimal valid topology object:
```json
{
  "resources": []
}
```


### 3.4.2 Required fields
The following field is **REQUIRED** within the topology object:

- **`resources`** (array): An array of resource objects representing infrastructure components. Each resource object is defined in Chapter 4 (Resource Model).

  The resources array **MAY** be empty (representing a topology with no resources), but the field itself **MUST** be present.


### 3.4.3 Optional fields
The following fields are **OPTIONAL**:

- **`connections`** (array): An array of connection objects representing relationships between resources. Each connection object is defined in Chapter 5 (Connection model).

  If omitted, the topology contains resources but no explicit relationships. Consumers **SHOULD** treat an absent `connections` field as equivalent to an empty array.

- **`groups`** (array): An array of group objects representing logical or physical collections of resources. Each group object is defined in Chapter 6 (Group model).

If omitted, the topology contains resources but no explicit grouping structure. Consumers **SHOULD** treat an absent `groups` field as equivalent to an empty array.


### 3.4.4 Field ordering
The order of `resources`, `connections` and `groups` arrays within the topology object is not semantically significant. Producers **MAY** emit these fields in any order.

However, for human readability and diff-friendliness, producers **SHOULD** use a consistent field order. The **RECOMMENDED** order is: `resources`, `connections`, `groups`.


### 3.4.5 Array element ordering
The order of elements within `resources`, `connections` and `groups` arrays is not semantically significant unless otherwise specified by consumer requirements.

Producers **SHOULD** consider stable, deterministic ordering (e.g. sorted by ID) to produce diff-friendly outputs when the same infrastructure is exported multiple times.


### 3.4.6 Referential integrity
Resources, connections and groups reference each other by identifier:

- Connections reference resources via `source` and `target` fields
- Groups reference resources via `members` arrays
- Groups may reference other groups for hierarchical nesting

All identifier references **MUST** point to objects that exist within the same OSIRIS document. Cross-document references are not supported in OSIRIS v1.0.

Producers **SHOULD** validate referential integrity before emitting documents. Consumers **MUST** handle referential integrity violations gracefully (see chapter 9, Section 9.3 for validation rules). Consumers **MAY** ignore invalid connections or groups while still processing valid resources.


### 3.4.7 Topology object example
#### Hyperscaler example
```json
{
  "resources": [
    {
      "id": "vm-001",
      "name": "web-server-1",
      "type": "compute.vm",
      "provider": {
        "name": "aws"
      }
    },
    {
      "id": "db-001",
      "name": "database-1",
      "type": "storage.database",
      "provider": {
        "name": "aws"
      }
    }
  ],
  "connections": [
    {
      "id": "conn-001",
      "source": "vm-001",
      "target": "db-001",
      "type": "dependency"
    }
  ],
  "groups": [
    {
      "id": "vpc-prod",
      "type": "network.vpc",
      "members": ["vm-001", "db-001"]
    }
  ]
}
```
#### On-premise example
```json
{
  "resources": [
    {
      "id": "MXP-F1-R01-SW-001",
      "name": "mxp-sw-leaf-01",
      "type": "network.switch",
      "provider": { "name": "arista" },
      "properties": { "rack_unit_start": 12, "rack_unit_height": 1, "face": "front" }
    },
    {
      "id": "MXP-F1-R98-SRV-001",
      "name": "mxp-f1-srv-r98-001",
      "type": "compute.server",
      "provider": { "name": "dell" },
      "properties": { "rack_unit_start": 30, "rack_unit_height": 2, "face": "front" }
    },
    {
      "id": "MXP-F1-R98-HV-001",
      "name": "mxp-srv-proxmox-01",
      "type": "compute.hypervisor",
      "provider": { "name": "proxmox" }
    }
  ],
  "connections": [
    {
      "id": "MXP-F1-CONN-001",
      "source": "MXP-F1-R01-SW-001",
      "target": "MXP-F1-R98-SRV-001",
      "type": "network"
    },
    {
      "id": "MXP-F1-R98-RUNS-001",
      "source": "MXP-F1-R98-HV-001",
      "target": "MXP-F1-R98-SRV-001",
      "type": "dependency"
    }
  ],
  "groups": [
    { "id": "MXP-DC-1", "type": "facility.datacenter", "members": [], "children": ["MXP-F1"] },
    { "id": "MXP-F1", "type": "facility.floor", "members": [], "children": ["MXP-F1-ROW-A"] },
    { "id": "MXP-F1-ROW-A", "type": "facility.row", "members": [], "children": ["MXP-F1-R01", "MXP-F1-R98"] },
    { "id": "MXP-F1-R01", "type": "facility.rack", "members": ["MXP-F1-R01-SW-001"] },
    { "id": "MXP-F1-R98", "type": "facility.rack", "members": ["MXP-F1-R98-SRV-001"] }
  ]
}
```

> [!NOTE]
> Resource types and group types in this example (e.g. `storage.database`, `network.vpc`) are illustrative. The complete type taxonomy is defined in Chapter 7 (Resource type taxonomy) and Section 6.2 (Group types). Identifiers are opaque strings and producers **MAY** use structured IDs based on specific naming conventions.


### 3.4.8 Empty topologies
A topology with no resources, connections or groups is valid:

```json
{
  "resources": []
}
```

When the `resources` array is empty, producers **SHOULD** either emit `connections` and `groups` as empty arrays or omit them entirely. Producers **MUST NOT** emit connections or groups that reference non-existent resources.

Empty topologies may represent:

- Newly provisioned environments awaiting resource deployment
- Filtered exports where no resources matched selection criteria
- Placeholder documents for testing or validation purposes

Consumers **MUST** handle empty topologies without error.

---

# 4 Resource model
This chapter defines the OSIRIS **Resource** object, the fundamental unit for representing infrastructure components in an OSIRIS topology.

Resources model heterogeneous infrastructure elements, compute instances, network devices, storage systems, platform services, applications and operational technology components in a consistent, vendor neutral format while preserving provider origin and supporting extensibility.

A resource object is designed to:

- Provide stable identity within an OSIRIS document
- Declare a type that enables categorization and consumer-specific behavior
- Retain provider attribution for traceability and enrichment
- Carry properties and extensions without requiring vendor-specific parsers in consuming tools

---

## 4.1 Resource object structure
### 4.1.1 Overview
A resource is represented as a JSON object. At minimum a resource **MUST** include an identifier, a type and `provider` object.

A minimal valid resource object:

```json
{
  "id": "urn:uuid:f81d4fae-7dec-11d0-a765-00a0c91e6bf6",
  "type": "compute.vm",
  "provider": {
    "name": "aws"
  }
}
```

> [!NOTE]
> `id` values are opaque to consumers and **MUST** be unique within the document.


### 4.1.2 Required fields
Each resource object **MUST** include the following fields:

- **`id`** (string): A document scoped unique identifier for the resource. Resource identifiers **MUST** be unique within a single OSIRIS document but need not be globally unique across documents. Identifiers are **case-sensitive** and opaque to consumers. Consumers **MUST NOT** infer semantic meaning from identifier structure or formatting.

    Examples of valid resource IDs:

    - `vm-001`
    - `i-0abc123def456` (an example of AWS instance ID style)
    - `MXP-F1-R01-SW-001` (structured datacenter naming convention)
    - `urn:uuid:f81a4bcd-7efg-11h0-a765-00a0c91e5fg7` (UUID-based)

- **`type`** (string): The resource type classification using hierarchical dot notation (e.g. `compute.vm`, `network.switch`, `storage.volume`). Resource type conventions are defined in Section 4.2 and the complete taxonomy is specified in Chapter 7 (Resource type taxonomy).

- **`provider`** (object): Provider attribution describing the originating platform or system for this resource. The provider object structure is defined in Section 4.3 (Provider information).

> [!NOTE]
> The `provider` field is **REQUIRED** in OSIRIS v1.0 because origin and traceability are core to the interchange model. Resources without provider attribution lose critical context for validation, enrichment and correlation with source systems.
>
> For resources from unknown or offline sources, producers **MAY** use `provider.name = "unknown"` or `provider.name = "custom"` with additional context in `provider.source` or `provider.system` (see Section 4.3).


### 4.1.3 Optional fields
A resource object **MAY** include the following optional fields:

- **`name`** (string): A human-readable name or label for the resource (e.g. hostname, instance name, device name). If present, `name` **SHOULD** remain stable across multiple exports of the same infrastructure when feasible.

- **`description`** (string): A free-text description of the resource's purpose, function or characteristics.

- **`properties`** (object): An object containing resource-specific properties that describe characteristics, configuration and attributes. Properties are free-form and producer-defined. Property conventions and extensibility are detailed in Section 4.4 (Properties and extensions).

  Example:
    ```json
    {
        "properties": {
        "instance_type": "t3.medium",
        "vcpus": 2,
        "memory_gb": 4,
        "private_ip": "10.0.1.2"
        }
    }
    ```

- **`extensions`** (object): Namespaced extension data for vendor-specific or domain-specific fields that extend beyond core OSIRIS semantics. Extensions **MUST** use the `osiris.<namespace>` prefix convention (e.g. `osiris.aws`, `osiris.azure`, `osiris.custom`). Extension mechanisms are defined in Chapter 8 (Extension mechanism) and Section 4.4.

  Example:
    ```json
    {
        "extensions": {
        "osiris.aws": {
            "placement": {
            "tenancy": "default",
            "partition_number": 1
            }
        }
        }
    }
    ```

- **`status`** (string): Optional high-level lifecycle or availability indicator. Status represents a **coarse lifecycle or availability category** (e.g. whether a resource is operational, degraded or offline).

- **`state`** (string): Optional operational condition indicator. State provides **more specific operational detail** beyond status (e.g. running, stopped, starting, stopping).

> [!NOTE]
> The distinction between `status` and `state`, allowed values and usage semantics are defined in Section 4.5 (Status and state).

- **`tags`** (object): Key-value labels used for organizational categorization, filtering or metadata annotation (e.g. environment, owner, cost-center, compliance requirements). Tags are general-purpose and cross-platform.

  Example:
```json
  {
    "tags": {
      "environment": "production",
      "owner": "platform-team",
      "cost-center": "engineering"
    }
  }
```

> [!NOTE]
> Platform native label concepts (e.g. Kubernetes labels, annotations) **SHOULD** be represented in the `extensions` object (e.g. `extensions.osiris.k8s.labels`) to preserve platform-specific semantics while keeping the core schema simple.


### 4.1.4 Field extensibility and unknown fields
Consumers **MUST** ignore unknown fields to ensure forward compatibility. Producers **SHOULD** place non-standard fields under `properties` or `extensions` and **SHOULD NOT** introduce new top-level resource fields in documents conforming to OSIRIS v1.0.

This approach ensures:

- Forward compatibility when new OSIRIS versions introduce fields
- Clear extension boundaries for vendor-specific data
- Simplified validation and schema evolution


### 4.1.5 Resource object validation
Resource objects **MUST** conform to the OSIRIS JSON Schema (Appendix A). Structural validation ensures required fields are present and field types are correct.

Additional semantic validation rules (ID uniqueness, type validity, referential integrity) are defined in Chapter 9 (Validation).

#### Hyperscaler compute resource
```json
{
  "id": "i-0abc123def456",
  "type": "compute.vm",
  "name": "web-server-prod-01",
  "provider": {
    "name": "aws",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "instance_type": "t3.medium",
    "vcpus": 2,
    "memory_gb": 4,
    "private_ip": "10.0.1.22",
    "public_ip": "203.0.113.1",
    "ami_id": "ami-0abcdef1234567890"
  },
  "status": "active",
  "tags": {
    "environment": "production",
    "tier": "web",
    "managed_by": "terraform"
  }
}
```

#### On-premise network device
```json
{
  "id": "MXP-F1-R01-SW-001",
  "type": "network.switch",
  "name": "mxp-sw-leaf-01",
  "provider": {
    "name": "arista",
    "model": "7050X4-48Y-4DF"
  },
  "properties": {
    "management_ip": "10.130.100.12",
    "serial_number": "XXXXXXXXXXXXXX",
    "rack_unit_start": 12,
    "rack_unit_height": 1,
    "face": "front",
    "ports": 48,
    "uplink_ports": ["Ethernet49/1", "Ethernet50/1"]
  },
  "status": "active",
  "tags": {
    "site": "MXP-DC-1",
    "floor": "F1",
    "rack": "R01",
    "role": "leaf",
    "platform": "arista-eos"
  }
}
```

---

## 4.2 Resource types
### 4.2.1 Overview
The `type` field classifies what kind of infrastructure component a resource represents. Resource types enable consumers to:

- Categorize resources for filtering and visualization
- Apply type-specific rendering or behavior
- Understand resource semantics without parsing properties
- Map resources to internal models or taxonomies

Resource types use a hierarchical dot notation that balances human readability with machine categorization.


### 4.2.2 Type field definition
The `type` field is a **REQUIRED** string (see Section 4.1). Type values:

- **MUST NOT** contain whitespace
- **MUST** be lowercase strings
- **MUST** use dot (`.`) as the segment separator
- **MUST** contain at least two segments (see Dot notation structure below)
- **MAY** contain alphanumeric characters (`a-z`, `0-9`) within segments
- **SHOULD** use dots for hierarchy rather than hyphens (e.g. `compute.vm.template` rather than `compute.vm-template`)
- **MUST NOT** start or end with a dot
- **MUST NOT** contain consecutive dots (`..`)
- **SHOULD** be stable across OSIRIS versions for standard types

Examples of valid type strings:
- `compute.vm`
- `network.switch`
- `storage.volume`
- `network.switch.leaf`
- `compute.vm.template`

Examples of invalid type strings:
- `Compute.VM` (uppercase not allowed)
- `compute` (single segment - violates minimum requirement)
- `.compute.vm` (starts with dot)
- `network..switch` (consecutive dots)
- `network.switch.` (ends with dot)


Type strings **MUST** be validated against the pattern rules above.


### 4.2.3 Dot notation structure
Resource types use hierarchical dot notation with segments ordered from general to specific:

```
<category>.<subcategory>[.<variant>][.<detail>]
```

**Minimum structure:** Types **MUST** contain at least two segments (category and subcategory):
- `compute.vm` (category: compute, subcategory: vm)
- `network.switch` (category: network, subcategory: switch)
- `storage.volume` (category: storage, subcategory: volume)

**Extended hierarchy:** Types **MAY** include additional segments for specificity:
- `network.switch.leaf` (leaf switch in network fabric)
- `network.switch.spine` (spine switch in network fabric)
- `storage.volume.block` (block storage volume)
- `storage.volume.file` (file storage volume)

**Hierarchy depth:** Types **SHOULD NOT** exceed 4-5 segments to maintain readability. Highly specific details belong in `properties` rather than type strings. Consumers **MUST** accept types with deeper hierarchies and treat them as normal type strings without error.


### 4.2.4 Namespace reservation
To ensure clear separation between standard types and extensions:

- Standard types **MUST NOT** start with `osiris.`
- Extension types **MUST** start with `osiris.`

This convention enables unambiguous identification of extension types and simplifies validation.


### 4.2.5 Standard types
**Standard types** are resource types defined in the OSIRIS specification and enumerated in Chapter 7 (Resource type taxonomy). Standard types have first-class support and well-defined semantics across implementations.

Examples of standard types (see Chapter 7 for complete list):
- `compute.vm` - Virtual machine
- `compute.container` - Container instance
- `compute.server` - Physical server
- `network.switch` - Network switch
- `network.router` - Network router
- `storage.volume` - Storage volume
- `storage.bucket` - Object storage bucket

Consumers **SHOULD** implement rendering, filtering or behavioral logic for standard types where applicable.


### 4.2.6 Vendor-specific and custom types
Resources representing vendor-specific or organization-specific components that do not map to standard types **SHOULD** use namespaced type strings.

**Namespace convention:** Custom types **MUST** use the `osiris.<namespace>.<type>` pattern, where:
- `osiris.` prefix indicates an extension type
- `<namespace>` identifies the vendor organization or domain
- `<type>` follows standard dot notation rules

Examples of namespaced types:
- `osiris.aws.lambda` (AWS Lambda function)
- `osiris.azure.app.service` (Azure App Service)
- `osiris.arista.vrf` (Arista VRF instance)
- `osiris.proxmox.vm` (Proxmox VM)
- `osiris.acme.custom.device` (organization-specific device type)

**Namespace selection:** There is no central namespace registry for OSIRIS v1.0. To reduce collision risk:

- Well-known vendor namespaces **SHOULD** use simple vendor names (e.g. `osiris.aws`, `osiris.cisco`, `osiris.proxmox`)
- Organization-specific namespaces **SHOULD** use stable identifiers such as reverse domain notation (e.g. `osiris.com-acme`, `osiris.org-example`)

Namespace consistency within a producer or organization improves interoperability and maintainability.

**Fallback behavior:** When vendor-specific types are necessary but a close standard equivalent exists, producers **SHOULD** consider:
- Using the standard type with vendor details in `properties` or `extensions`
- Using the namespaced type only when semantics differ significantly from standard types

Example:
```json
{
  "id": "lambda-001",
  "type": "osiris.aws.lambda",
  "provider": { "name": "aws" },
  "properties": {
    "runtime": "python3.11",
    "memory_mb": 512
  }
}
```

### 4.2.7 Unknown type handling
Consumers **MUST** accept resources with unknown or unrecognized types. This ensures forward compatibility and allows incremental adoption of new types.

When encountering unknown types, consumers **SHOULD**:
- Preserve the resource in the topology graph
- Treat it as a generic infrastructure node
- Display the type string verbatim for debugging or manual interpretation
- Process connections, group memberships and other relationships normally

Consumers **MAY**:
- Apply heuristic categorization based on type prefix (e.g. types starting with `compute.` likely represent compute resources)
- Emit warnings for unrecognized types during validation
- Provide configuration to map unknown types to internal models

Consumers **MUST NOT**:
- Reject documents containing unknown types
- Silently discard resources with unknown types


### 4.2.8 Type evolution and stability
**Standard types** defined in Chapter 7 are considered stable within OSIRIS v1.x releases. New standard types **MAY** be added in minor version updates (e.g. 1.1.0) without breaking compatibility.

Standard types **SHOULD NOT** be removed or have their semantics changed in minor updates. Deprecated types **SHOULD** remain documented and supported for at least one major version cycle.

**Extension types** (`osiris.*`) are not governed by OSIRIS and may evolve independently. Producers using extension types **SHOULD** version their generator tools if type semantics change to help consumers track compatibility.


### 4.2.9 Type selection guidance
When producing OSIRIS documents, use this decision tree for type selection:

1. **Does a standard type match the resource semantics?**  
   - Use the standard type from Chapter 7

2. **Is the resource vendor-specific with no standard equivalent?**  
   - Use a namespaced type: `osiris.<vendor>.<type>`

3. **Is the resource a specialization of a standard type?**  
   - Consider using the standard type with details in `properties` or use extended hierarchy: `<standard-type>.<variant>`

4. **Is the resource highly organization-specific?**  
   - Use namespaced custom type: `osiris.<org>.<type>`

Examples:
- AWS EC2 instance > `compute.vm` (standard type)
- AWS Lambda function > `osiris.aws.lambda` (no standard equivalent)
- Cisco Nexus leaf switch > `network.switch.leaf` or `network.switch` with `properties.role = "leaf"`
- Custom monitoring appliance > `osiris.acme.monitor` (organization-specific)


### 4.2.10 Type field validation
The `type` field value **MUST** conform to the format rules defined in this section. Structural validation (format, allowed characters, minimum segments) is enforced by JSON Schema (Appendix A).

Semantic validation (whether a type is defined in the standard taxonomy) is **OPTIONAL** and implementation-dependent. Consumers **MAY** validate against Chapter 7 but **MUST NOT** reject documents with valid but unrecognized types.

---

## 4.3 Provider information
### 4.3.1 Overview
The `provider` object is a **REQUIRED** field in every resource (see Section 4.1). It describes the originating platform or system from which the resource was sourced, enabling traceability, correlation and enrichment.

The provider object answers: **"Where did this resource come from?"** rather than **"Where is this resource located?"** Regional placement **MAY** be recorded in `provider.region` or `provider.zone` for correlation with provider APIs; broader export boundaries belong in `metadata.scope`.


### 4.3.2 Provider object purpose
Provider attribution serves several critical functions:

- **Provenance tracking**: Identifying the source system for validation and audit purposes
- **Correlation**: Enabling lookups or enrichment by referencing native vendor systems
- **Mixed-vendor topologies**: Distinguishing resources from different platforms in unified views
- **Tool-specific behavior**: Allowing consumers to apply platform-specific rendering or logic
- **Change detection**: Tracking resource origin across multiple snapshot exports


### 4.3.3 Required fields
The provider object **MUST** include:

- **`name`** (string): The vendor, platform or system name (e.g. `aws`, `azure`, `gcp`, `arista`, `proxmox`, `cisco`, `dell`).

    `provider.name` identifies the originating system/vendor for that resource type (e.g. cloud platform for cloud resources; hardware vendor for physical assets; virtualization platform for hypervisors).

  Provider names **SHOULD** be:
  - Lowercase
  - Short and recognizable (vendor name or product name)
  - Consistent across exports from the same producer

  Examples:
  - `aws` (Amazon Web Services)
  - `azure` (Microsoft Azure)
  - `gcp` (Google Cloud Platform)
  - `arista` (Arista Networks)
  - `proxmox` (Proxmox Virtual Environment)
  - `vmware` (VMware vSphere)
  - `cisco` (Cisco systems)


### 4.3.4 Optional fields
The provider object **MAY** include:

- **`native_id`** (string): The resource's native identifier in the source system. This enables correlation with vendor APIs, reconciliation across exports and deduplication.

  Examples:
  - AWS ARN: `arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def456`
  - Azure Resource ID: `/subscriptions/{sub}/resourceGroups/{rg}/providers/Microsoft.Compute/virtualMachines/{vm}`
  - GCP full resource name: `//compute.googleapis.com/projects/{project}/zones/{zone}/instances/{instance}`
  - Device serial number: `JPE19350123`

- **`region`** (string): The geographic region, zone or location where the resource is deployed (e.g. `us-east-1`, `westeurope`, `asia-southeast1`).

  > [!NOTE]
  > `provider.region` describes where the resource exists within the provider's infrastructure. Document wide regional scope is represented in `metadata.scope.regions` (see chapter 3, section 3.3).

- **`account`** (string): The account, subscription, project or tenant identifier within the provider's platform.

  Examples:
  - AWS account ID: `123456789012`
  - Azure subscription ID: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`
  - GCP project ID: `my-project-12345`

- **`zone`** (string): Availability zone, fault domain or sub-regional placement (e.g. `us-east-1a`, `eastus`).

- **`model`** (string): Hardware or software model identifier for physical devices or appliances.

  Examples:
  - `DCS-7050SX3-48YC12` (Arista switch model)
  - `PowerEdge R770` (Dell server model)
  - `5132` (Ciena router model)

- **`version`** (string): Software version, firmware version or platform version (e.g. `EOS-4.28.3F`, `ESXi 7.0.3`).

- **`namespace`** (string): An organization or domain identifier for custom or internal providers (see Unknown and custom providers below).

- **`system`** (string): The producing system name or source inventory system identifier (useful for custom and multi-source exporters).

- **`source`** (string): a short identifier of the data source path/method, e.g. cloudtrail, cloud_asset_inventory, eapi, netconf, snmp, cmdb_export.


### 4.3.5 Unknown and custom providers
For resources from unknown, offline or organization-specific sources, producers **MAY** use:

- **`provider.name = "unknown"`**: When the source system cannot be determined or is intentionally omitted.

- **`provider.name = "custom"`** with **`provider.namespace`**: For organization-specific or internal systems that do not map to recognized vendors.

  When `provider.name` is set to `"custom"`, the `namespace` field **MUST** be provided to uniquely identify the organization or system.

  Example:
```json
  {
    "provider": {
      "name": "custom",
      "namespace": "acme-internal-cmdb",
      "system": "legacy-inventory-v2"
    }
  }
```

Consumers **MUST** accept resources with `provider.name = "unknown"` or `provider.name = "custom"` and **SHOULD** treat them as generic resources while preserving all provider metadata.


### 4.3.6 Provider vs metadata scope
The `provider` object and `metadata.scope` serve distinct but complementary purposes:

**`resource.provider`** (per-resource attribution):
- Identifies the source system for an individual resource
- Enables correlation with native vendor identifiers
- Supports mixed-vendor topologies where resources from different providers coexist

**`metadata.scope`** (document-wide context):
- Describes what infrastructure the entire document represents
- Lists which providers, regions, accounts or environments are included in the export
- Documents the boundaries of the topology snapshot

**Example of overlap without duplication:**

Metadata scope (document-wide):
```json
{
  "metadata": {
    "scope": {
      "providers": ["aws", "azure"],
      "regions": ["us-east-1", "eastus"],
      "environments": ["production"]
    }
  }
}
```

Resource provider (per-resource):
```json
{
  "id": "vm-001",
  "provider": {
    "name": "aws",
    "region": "us-east-1",
    "account": "123456789012",
    "native_id": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123"
  }
}
```

The metadata establishes document scope. The provider provides specific attribution for each resource. Both are valuable and non-redundant.


### 4.3.7 Provider object stability
Provider object fields **SHOULD** remain stable across multiple exports of the same infrastructure. Stable provider attribution enables:

- Resource identity correlation over time
- Change detection by comparing snapshots
- Reliable mapping to external systems

Producers **SHOULD** preserve `provider.native_id` when available, as it provides the strongest correlation anchor.
`native_id` **MAY** be the provider’s primary identifier (e.g. instance ID) or a fully-qualified identifier (e.g. ARN/Azure resource ID).

### 4.3.8 Provider object examples
#### Hyperscaler resource
```json
{
  "provider": {
    "name": "aws",
    "region": "us-east-1",
    "account": "123456789012",
    "native_id": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def456"
  }
}
```

#### Network device
```json
{
  "provider": {
    "name": "arista",
    "model": "DCS-7050SX3-48YC12",
    "version": "EOS-4.28.3F",
    "native_id": "JPEXXXXXXXX"
  }
}
```

#### Virtualization platform
```json
{
  "provider": {
    "name": "proxmox",
    "version": "8.1.3",
    "native_id": "qemu/101"
  }
}
```

#### Custom internal system
```json
{
  "provider": {
    "name": "custom",
    "namespace": "acme-corp",
    "system": "asset-management-db",
    "native_id": "ASSET-EU-04567"
  }
}
```


### 4.3.9 Provider field validation
The provider object **MUST** conform to the structural requirements defined in this section. At minimum, `provider.name` is required. All other fields are optional and producer-defined based on available source data.

Consumers **MUST** accept provider objects with unrecognized fields and preserve them when re-exporting or transforming documents.

---

## 4.4 Properties and extensions
### 4.4.1 Overview
Resources in OSIRIS use two complementary mechanisms to represent data beyond the standard fields defined in Section 4.1:

- **`properties`**: A free-form object for resource attributes and configuration details
- **`extensions`**: A namespaced object for vendor-specific or organization-specific structures that must not collide with the core schema

Both fields are **OPTIONAL**, but they provide essential flexibility for representing heterogeneous infrastructure while preserving interoperability.


### 4.4.2 Properties object
#### Purpose
The `properties` object captures resource-specific attributes that vary by resource type or implementation, such as:

- Configuration parameters (e.g. instance size, disk size, VLAN IDs)
- Addressing and identifiers (e.g. IPs, MACs, serial numbers)
- Physical placement details (e.g. rack units for hardware)

Properties enable producers to export rich details without requiring all consumers to understand every attribute.

#### Structure
If present, `properties` **MUST** be a JSON object.
```json
{
  "properties": {
    "key1": "value1",
    "key2": 123,
    "key3": ["array", "of", "values"],
    "nested": { "object": "allowed" }
  }
}
```

Properties **MAY** contain primitive values, arrays and nested objects of arbitrary depth.


### 4.4.3 Naming conventions
Property keys **SHOULD** follow these conventions:

- Lowercase with underscores for multi-word keys (e.g. `instance_type`, `private_ip`, `disk_size_gb`)
- Include units in the key name where applicable (e.g. `memory_gb`, `throughput_mbps`)
- Remain stable across exports from the same producer

Property keys **SHOULD NOT** embed vendor prefixes (e.g. avoid `awsInstanceType`). Vendor-specific structures belong in `extensions`.


### 4.4.4 Properties vs standard fields
Producers **SHOULD** use standard OSIRIS fields when an attribute maps cleanly to OSIRIS semantics (e.g. `id`, `type`, `provider`, `status`, `name`).

Producers **SHOULD** use `properties` for intrinsic resource details that do not have a standardized OSIRIS field.


### 4.4.5 Sensitive data
Producers **MUST NOT** include secrets in `properties`, including tokens, passwords, private keys or connection strings containing credentials.


### 4.4.6 Extensions object
#### Purpose
The `extensions` object provides a namespacing mechanism for:

- Vendor-native structures (e.g. provider-specific metadata)
- Tool-specific annotations (e.g. Terraform/diagram hints)
- Organization-specific tracking fields (e.g. internal ownership/compliance attributes)

This allows rich producer-specific data without polluting the core OSIRIS model.

#### Structure and namespacing rules
If present, `extensions` **MUST** be a JSON object.

Each extension entry **MUST** be keyed by a namespace using the reserved `osiris.` prefix:
```
osiris.<namespace>
```

The value associated with each `osiris.<namespace>` key **MUST** be a JSON object.

Example:
```json
{
  "extensions": {
    "osiris.aws": { "security_groups": ["sg-123"] },
    "osiris.acme": { "owner_team": "platform" }
  }
}
```

Namespaces **SHOULD** be stable and collision resistant:

- Well-known vendors **MAY** use simple namespaces (e.g. `osiris.aws`, `osiris.azure`, `osiris.gcp`, `osiris.arista`)
- Organizations **SHOULD** use a stable identifier (e.g. `osiris.com.acme`)

> [!NOTE]
> Some data stores and document databases restrict `.` in object keys. Consumers storing OSIRIS documents in such systems may need to escape or transform extension keys (e.g. `osiris.<namespace>` > `osiris_<namespace>`) while preserving semantics.

#### Consumer behavior
Consumers **MUST** ignore unrecognized extension namespaces and fields.

Consumers that transform or re-export OSIRIS documents **SHOULD** preserve extensions data when feasible (some consumers are intentionally lossy).


### 4.4.7 Properties vs extensions
Producers **SHOULD** use:

- **`properties`** for broadly useful, portable attributes that generic consumers might filter/visualize
- **`extensions`** for vendor/tool/org-specific structures that most generic consumers will ignore

If the same concept appears in both:
- `properties` is the portable summary
- `extensions` is the authoritative vendor/tool structure


### 4.4.8 Tags (resource labeling)
#### Definition
The `tags` field **MAY** be present. If present, it **MUST** be a JSON object mapping string keys to string values.
Tags support categorization and filtering (e.g. environment, owner, cost center).

#### Rules
Producers **MUST NOT** store secrets in `tags`.
Producers **SHOULD** keep tag keys stable across exports.

If a platform has richer label/annotation concepts (e.g. Kubernetes), producers **SHOULD** represent those under `extensions` (e.g. `extensions.osiris.k8s.labels`).


### 4.4.9 Forward compatibility rules
#### Consumers MUST:
- Accept resources with unknown `properties` keys
- Accept resources with unknown `extensions` namespaces/fields
- Ignore unrecognized fields without error

#### Producers SHOULD:
- Keep property keys stable across exports
- Keep extension namespaces stable across exports
- Avoid excessive redundancy between `metadata`, `provider`, `properties` and `extensions`


### 4.4.10 Examples
#### Compute resource with portable properties + vendor extension
```json
{
  "id": "vm-web-001",
  "name": "web-server-01",
  "type": "compute.vm",
  "provider": {
    "name": "aws",
    "region": "us-east-1",
    "account": "123456789012",
    "native_id": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def456"
  },
  "properties": {
    "instance_type": "t3.large",
    "vcpus": 2,
    "memory_gb": 8,
    "private_ip": "10.0.1.10",
    "public_ip": "203.0.113.10",
    "root_volume_size_gb": 100
  },
  "extensions": {
    "osiris.aws": {
      "subnet_id": "subnet-0abc1234def567",
      "security_groups": ["sg-0123456789abcdef0"]
    }
  },
  "tags": {
    "environment": "production",
    "managed_by": "terraform"
  }
}
```

#### Network device with properties + vendor and org extensions
```json
{
  "id": "switch-dc1-leaf-01",
  "name": "dc1-leaf-01",
  "type": "network.switch.leaf",
  "provider": {
    "name": "arista",
    "model": "DCS-7050SX3-48YC12",
    "version": "EOS-4.28.3F",
    "native_id": "SERIAL:JPE19350123"
  },
  "properties": {
    "management_ip": "10.0.0.101",
    "port_count": 48,
    "uplink_ports": ["Ethernet49/1", "Ethernet50/1"],
    "rack_unit_start": 42,
    "rack_unit_height": 1,
    "face": "front"
  },
  "extensions": {
    "osiris.arista": {
      "system_mac": "00:1c:73:aa:bb:cc",
      "mlag": { 
        "domain_id": "MLAG1", 
        "peer_link": "Port-Channel1" 
      }
    },
    "osiris.acme": {
      "maintenance_window": "sunday-02:00-04:00"
    }
  }
}
```

#### Kubernetes pod with platform-native labels/annotations in extensions
```json
{
  "id": "pod-nginx-7d8f9c",
  "name": "nginx-7d8f9c-abcde",
  "type": "compute.container",
  "provider": {
    "name": "k8s",
    "system": "prod-cluster-1",
    "native_id": "pod/nginx-7d8f9c-abcde"
  },
  "properties": {
    "namespace": "production",
    "image": "nginx:1.21",
    "restart_count": 0,
    "node": "worker-03"
  },
  "extensions": {
    "osiris.k8s": {
      "labels": {
        "app": "nginx",
        "version": "1.21",
        "environment": "production"
      },
      "annotations": {
        "prometheus.io/scrape": "true",
        "prometheus.io/port": "9113"
      },
      "owner_references": [
        { 
          "kind": "ReplicaSet", 
          "name": "nginx-7d8f9c" 
        }
      ]
    }
  }
}
```

#### Custom resource with organization extensions (namespaced type)
```json
{
  "id": "custom-monitor-01",
  "name": "APM-Monitor-NYC",
  "type": "osiris.acme.apm.monitor",
  "provider": {
    "name": "custom",
    "namespace": "acme-monitoring",
    "system": "observability-platform",
    "native_id": "monitor/01"
  },
  "properties": {
    "monitoring_url": "https://apm.acme.com/monitor/01",
    "health": "healthy",
    "targets": ["web-01", "web-02", "api-01"]
  },
  "extensions": {
    "osiris.acme": {
      "owner_team": "sre-observability",
      "cost_center": "CC-1234",
      "compliance_required": true
    }
  }
}
```


### 4.4.11 Validation
If present, `properties` and `extensions` **MUST** be JSON objects.

Structural validation is enforced by JSON Schema (Appendix A). Semantic validation of property keys and extension contents is **OPTIONAL** and consumer-dependent. 

Consumers **MUST NOT** reject documents solely due to unrecognized `properties` keys or unknown `extensions` namespaces.

---

## 4.5 Status and state
### 4.5.1 Overview
The `status` and `state` fields are **OPTIONAL** fields that provide snapshot-time indicators of resource operational condition. 

These fields enable:

- **Filtering** resources by operational status in visualizations or queries
- **Correlation** with resource relationships (e.g. identifying degraded dependencies)
- **Change detection** when comparing snapshots over time
- **Visual differentiation** in diagrams or dashboards

> [!NOTE]
> `status` and `state` represent point-in-time observations at the moment of export. They are **NOT** intended for real-time monitoring, alerting or telemetry. For runtime observability, use dedicated monitoring systems (e.g. OpenTelemetry, Prometheus).


### 4.5.2 Status field
#### Definition
The `status` field provides a normalized snapshot indicator of resource operational condition. If present, it **MUST** be a string.

#### Recommended values
Producers **SHOULD** use one of the following standard status values when applicable:

- **`active`**: Resource is operational and serving its intended function
- **`inactive`**: Resource exists but is not currently operational (e.g. stopped VM, disabled interface)
- **`degraded`**: Resource is operational but experiencing issues (e.g. reduced capacity, partial failure)
- **`retired`**: Resource no longer exists or is permanently removed from service (e.g. terminated VM, decommissioned device)
- **`unknown`**: Status cannot be determined or is unavailable

#### Semantics
Status values represent **high-level operational condition at snapshot time**, combining lifecycle phase and health indicators:

- `active` indicates the resource is functioning as designed
- `inactive` indicates intentional or controlled non-operation
- `degraded` indicates operational with impaired functionality
- `retired` indicates permanent removal or termination
- `unknown` is a fallback when status cannot be reliably determined

#### Unknown status values
Consumers **MUST** accept resources with unrecognized status values. When encountering unknown status values, consumers **SHOULD**:

- Preserve the status value when re-exporting (when feasible)
- Treat the resource as having `unknown` status for filtering or visualization
- Display the literal status string for manual interpretation

Consumers **MUST NOT** reject documents containing unrecognized status values.

#### Omitting status
If `status` is omitted, consumers **SHOULD** assume the resource status is unknown or not applicable. Producers **MAY** omit `status` when:

- Status information is unavailable from the source system
- Status is not meaningful for the resource type (e.g. static configuration objects)
- The export represents historical or planned infrastructure (not current state)


### 4.5.3 State field
#### Definition
The `state` field provides detailed, provider-specific operational state information. If present, it **MUST** be a string.
Producers **SHOULD** normalize state to lowercase when the provider uses a known canonical lowercase set.

#### Purpose
While `status` offers broad categorization, `state` captures vendor-specific lifecycle phases or operational modes:

- AWS EC2: `running`, `stopped`, `stopping`, `pending`, `shutting-down`, `terminated`
- Kubernetes pod: `pending`, `running`, `succeeded`, `failed`, `unknown`
- Network interface: `up`, `down`, `admin-down`, `testing`

#### State vs status
The relationship between `state` and `status`:

- **`status`** is normalized across vendors (active/inactive/degraded/retired/unknown)
- **`state`** is vendor-specific and granular

Producers **MAY** include both fields. When both are present:

- `status` provides vendor-neutral categorization for generic consumers
- `state` provides detailed information for vendor-aware tools

**Example mapping:**
```json
{
  "status": "active",
  "state": "running"
}
```
```json
{
  "status": "inactive",
  "state": "stopped"
}
```
```json
{
  "status": "degraded",
  "state": "impaired"
}
```
```json
{
  "status": "retired",
  "state": "terminated"
}
```

#### Unknown state values
Consumers **MUST** accept resources with any `state` value. State values are provider-defined and not governed by OSIRIS.

Consumers **SHOULD**:
- Preserve state values when re-exporting (when feasible)
- Display state verbatim for manual interpretation
- Not attempt to normalize or validate state values


### 4.5.4 Producer rules
#### When to set status
Producers **SHOULD** set `status` when:

- The source system provides operational status information
- Status can be reliably mapped to one of the standard values (`active`, `inactive`, `degraded`, `retired`)
- The export represents current infrastructure state (not historical or planned)

Producers **MAY** use `"unknown"` when status information is unavailable but the field is being populated for consistency.

#### When to set state
Producers **MAY** set `state` to capture vendor-specific lifecycle information. State is entirely optional and provider-defined.

Producers **SHOULD**:
- Use state values directly from the source system without normalization
- Keep state values stable across exports for the same resource
- Avoid inventing custom state values when vendor-native values exist

#### When to omit status/state
Producers **MAY** omit both `status` and `state` when:

- Operating on static inventory data (e.g. CMDB exports without live status)
- Exporting planned or historical infrastructure
- Source systems do not provide status information

#### Sensitive information
Producers **MUST NOT** include credentials, tokens or secrets in `status` or `state`. Producers **SHOULD** avoid embedding verbose diagnostic logs or incident identifiers in these fields.


### 4.5.5 Consumer rules
#### Accepting unknown values
Consumers **MUST**:
- Accept resources with any `status` or `state` value (including unrecognized values)
- Not reject or discard resources based on unknown status/state values

Consumers **SHOULD**:
- Preserve status/state when re-exporting or transforming documents (when feasible)

#### Display and filtering
When encountering unknown status values, consumers **SHOULD**:
- Display the literal value for debugging
- Treat unknown status as equivalent to `"unknown"` for filtering
- Provide user configuration to map custom status values to standard categories

#### Validation
Consumers **MAY**:
- Emits warnings for unrecognized status values (but **MUST NOT** fail parsing)
- Validate status against the recommended vocabulary in non-strict mode
- Apply heuristic to infer status category from state (e.g. "stopped" state > "inactive" status)


### 4.5.6 Examples
#### Cloud VM with status and state
```json
{
  "id": "vm-web-001",
  "name": "web-server-01",
  "type": "compute.vm",
  "provider": {
    "name": "aws",
    "region": "us-east-1",
    "native_id": "i-0abc123def456"
  },
  "status": "active",
  "state": "running",
  "properties": {
    "instance_type": "t3.medium",
    "private_ip": "10.0.1.10"
  }
}
```

#### Network switch with degraded status
```json
{
  "id": "switch-dc1-leaf-01",
  "name": "dc1-leaf-01",
  "type": "network.switch.leaf",
  "provider": {
    "name": "arista",
    "model": "DCS-7050SX3-48YC12",
    "native_id": "JPEXXXXXXXX"
  },
  "status": "degraded",
  "state": "fan-failure-detected",
  "properties": {
    "management_ip": "10.0.0.101",
    "port_count": 48
  }
}
```

#### Kubernetes pod with vendor-specific state
```json
{
  "id": "pod-nginx-abc123",
  "name": "nginx-deployment-abc123",
  "type": "compute.container",
  "provider": {
    "name": "k8s",
    "native_id": "pod/nginx-deployment-abc123"
  },
  "status": "active",
  "state": "Running",
  "properties": {
    "namespace": "production",
    "image": "nginx:1.21",
    "node": "worker-node-03"
  },
  "extensions": {
    "osiris.k8s": {
      "conditions": [
        {
          "type": "Ready",
          "status": "True"
        },
        {
          "type": "ContainersReady",
          "status": "True"
        }
      ]
    }
  }
}
```

#### Stopped VM (inactive status)
```json
{
  "id": "vm-staging-002",
  "name": "staging-app-server",
  "type": "compute.vm",
  "provider": {
    "name": "azure",
    "region": "eastus",
    "native_id": "/subscriptions/.../virtualMachines/staging-app-02"
  },
  "status": "inactive",
  "state": "stopped",
  "properties": {
    "vm_size": "Standard_B2s",
    "os_type": "Linux"
  }
}
```

#### Terminated VM (retired status)
```json
{
  "id": "vm-legacy-001",
  "name": "legacy-web-server",
  "type": "compute.vm",
  "provider": {
    "name": "aws",
    "region": "us-west-2",
    "native_id": "i-0xyz789abc123"
  },
  "status": "retired",
  "state": "terminated",
  "properties": {
    "instance_type": "t2.micro",
    "termination_time": "2025-12-15T10:30:00Z"
  }
}
```

#### Resource with unknown status (source unavailable)
```json
{
  "id": "legacy-db-001",
  "name": "legacy-database",
  "type": "storage.database",
  "provider": {
    "name": "custom",
    "namespace": "legacy-systems",
    "native_id": "DB-001"
  },
  "status": "unknown",
  "properties": {
    "database_engine": "postgresql",
    "version": "9.6"
  }
}
```

---

# 5. Connection model
## Overview
Connections in OSIRIS represent explicit relationships between resources. They form the edges of the topology graph and encode how infrastructure components interact, depend on each other or are linked physically or logically.

This chapter defines:
- The structure and required fields of connection objects
- Standard connection types and their semantics
- Directionality and how it models flows or dependencies
- Connection metadata via properties, extensions, tags and status/state

Connections complement resources (Chapter 4) and groups (Chapter 6) to form a complete, traversable topology graph.


## 5.1 Connection object structure
### 5.1.1 Overview
A connection is a JSON object that links two resources within the same OSIRIS document. Connections are stored in the `topology.connections` array (see chapter 3, section 3.4).


### 5.1.2 Required fields
Every connection **MUST** include:

- **`id`** (string): Unique identifier for this connection within the document
- **`source`** (string): Resource ID of the source endpoint
- **`target`** (string): Resource ID of the target endpoint
- **`type`** (string): Connection type using dot notation (see Section 5.2)


### 5.1.3 Optional fields
Connections **MAY** include:

- **`name`** (string): Human-readable label for the connection
- **`description`** (string): Free-text description of the relationship
- **`direction`** (string): Directionality indicator (see chapter 5, ection 5.3)
- **`status`** (string): Normalized status category (see chapter 4, section 4.5 for vocabulary)
- **`state`** (string): Vendor-specific granular state (analogous to resource `state`)
- **`properties`** (object): Connection-specific attributes
- **`extensions`** (object): Vendor-specific metadata using `osiris.<namespace>` keys (see Chapter 8)
- **`tags`** (object): Key-value labels (`string` > `string`)


### 5.1.4 Minimal connection example
```json
{
  "id": "conn-001",
  "source": "vm-001",
  "target": "db-001",
  "type": "dependency",
  "direction": "forward"
}
```


### 5.1.5 Referential integrity
The `source` and `target` fields **MUST** reference resources that exist in the same document's `topology.resources` array.

Connections **MUST NOT**:
- Reference resources in different OSIRIS documents
- Reference non-existent resource IDs
- Create self-references where `source == target`

Producers **SHOULD** validate referential integrity before exporting. Consumers **MUST** handle invalid references gracefully (e.g. emit warnings, skip invalid connections or reject the document in strict-validation mode).


### 5.1.6 Connection identity and stability
Connection IDs **MUST** be unique within the document. Connection IDs **SHOULD** remain stable across exports when feasible to enable change detection and correlation.

Producers **MAY** generate connection IDs deterministically using a stable fingerprint derived from:
- endpoint identities (`source`, `target`)
- connection `type`
- `direction`
- (optionally) key endpoint metadata that materially distinguishes the link (e.g. `source_port`, `target_port`, VLAN, protocol)

#### Deterministic fingerprint (recommended)
Producers **SHOULD** build a canonical endpoint tuple:
- `A = (resource_id, port)` and `B = (resource_id, port)` where `port` is optional (empty string if unknown)

For `direction = "bidirectional"`, producers **SHOULD** canonicalize endpoint ordering by sorting `(A, B)` before generating the ID, to prevent ID flips between exports.

For `direction = "forward"` and `direction = "reverse"`, producers **SHOULD NOT** reorder endpoints, as the orientation is semantically meaningful.

Example fingerprint:
```
fingerprint = type + "|" + direction + "|" + A.resource_id + "|" + A.port + "|" + B.resource_id + "|" + B.port
id = hash(fingerprint)
```

#### Human-readable IDs (optional)
Instead of hashing, producers **MAY** use a human-readable ID encoding both endpoints, for example:
```
cn-<source_id>-<source_port>-to-<target_id>-<target_port>-<index>
```

When using human-readable IDs for `direction = "bidirectional"`, producers **SHOULD** apply the same canonical endpoint ordering described above (sort `(A, B)` first) to ensure stability across exports.

> [!NOTE]
> A single pair of resources may have multiple distinct connections (e.g. multiple ports or VLANs). If the chosen fingerprint is not sufficient to uniquely identify each connection, producers **MUST** include additional discriminators (such as ports, VLAN, protocol) or append a stable suffix.


### 5.1.7 Multiple connections between the same resources
Two resources **MAY** have multiple connections of different types or different properties. Each distinct relationship **MUST** be represented as a separate connection object with a unique `id`.


### 5.1.8 Field extensibility
Consumers **MUST** accept connections with unrecognized fields for forward compatibility. Producers **SHOULD** place non-standard data under `properties` or `extensions` rather than introducing new top-level fields in OSIRIS v1.0.


## 5.2 Connection types
### 5.2.1 Overview
The `type` field classifies the semantic meaning of a relationship between resources. Connection types use hierarchical dot notation similar to resource types (see chapter 4, section 4.2).


### 5.2.2 Type format rules
Connection type values:

- **MUST NOT** contain whitespace
- **MUST** be lowercase
- **MUST** use dot (`.`) as the segment separator when multiple segments are present
- **MUST NOT** start or end with a dot
- **MUST NOT** contain consecutive dots (`..`)

Examples of valid connection types:
- `network`
- `dependency`
- `contains`
- `dataflow`
- `physical.ethernet`
- `network.bgp`


### 5.2.3 Standard connection types (v1.0)
OSIRIS defines the following standard connection types:

#### network
**Meaning**: Network-layer connectivity or reachability between resources (L2/L3 or higher-level network linkage)

**Direction**: Typically bidirectional unless modeling asymmetric flows

**Common subtypes** (optional):
- `network.l2`
- `network.l3`
- `network.bgp`
- `network.ospf`
- `network.vpn`

#### dependency
**Meaning**: One resource requires another to function (application/service dependency, runtime requirement)

**Direction**: Typically forward (source depends on target)

**Common subtypes** (optional):
- `dependency.api`
- `dependency.database`
- `dependency.storage`

#### contains
**Meaning**: Hierarchical containment (source contains target)

**Direction**: Typically forward

> [!NOTE]
> Containment can be represented via `contains` connections or via groups (Chapter 6). Use whichever yields the clearest model; avoid duplicating the same containment semantics in both unless you have a specific consumer need.

#### dataflow
**Meaning**: Data transfer, message passing or write/read flow between resources

**Direction**: Typically forward (data flows from source to target)

**Common subtypes** (optional):
- `dataflow.http`
- `dataflow.kafka`
- `dataflow.mqtt`

#### physical.*
**Meaning**: Physical connectivity (cables, fiber, power)

**Direction**: Typically bidirectional

**Common subtypes**:
- `physical.ethernet`
- `physical.fiber`
- `physical.power`


### 5.2.4 Vendor-specific and custom connection types
Vendor-specific or organization-specific relationships **SHOULD** use the namespaced pattern:
```
osiris.<namespace>.<type>
```

Examples:
- `osiris.aws.vpc-peering`
- `osiris.k8s.service-selector`
- `osiris.arista.mlag`
- `osiris.com-acme.workflow`


### 5.2.5 Unknown connection types
Consumers **MUST** accept connections with unknown types. When encountering unknown types, consumers **SHOULD**:
- Preserve the connection in the graph
- Treat it as a generic relationship
- Display the type string verbatim (useful for debugging)
- Traverse it normally


## 5.3 Directionality
### 5.3.1 Overview
The `direction` field indicates whether a connection represents a symmetric relationship or a directed relationship with a specific orientation.


### 5.3.2 Direction values
If present, `direction` **MUST** be one of:

- **`bidirectional`**: Symmetric relationship (default if omitted)
- **`forward`**: Flows from source to target
- **`reverse`**: Flows from target to source


### 5.3.3 Default behavior
If `direction` is omitted, consumers **SHOULD** treat the connection as `bidirectional` unless the connection type definition explicitly states a directional default.


### 5.3.4 Graph traversal semantics
- **`bidirectional`**: edges in both directions
- **`forward`**: edge source > target
- **`reverse`**: edge target > source


## 5.4 Connection properties
### 5.4.1 Overview
Connections use the same metadata pattern as resources:

- **`properties`**: Portable, connection-specific attributes
- **`extensions`**: Vendor/tool/org-specific structures using `osiris.<namespace>`
- **`tags`**: Simple labels (`string`  > `string`)
- **`status`**: Normalized high-level category (`active`|`inactive`|`degraded`|`retired`|`unknown`)
- **`state`**: Optional granular vendor-defined state


### 5.4.2 Common properties
**Network connections:**
- `protocol` (e.g. tcp, udp, icmp, bgp)
- `ports` (array of numbers)
- `vlan` (number)
- `mtu` (number)
- `bandwidth_mbps` / `speed_gbps`
- `latency_ms`

**Physical connections:**
- `source_port` (string)
- `target_port` (string)
- `cable_type` (string; e.g. `cat6a`, `om4`, `os2`, `dac`)
- `length_meters` (number)

- `source_transceiver` (object; optional): transceiver/module installed on the source port
- `target_transceiver` (object; optional): transceiver/module installed on the target port

`source_transceiver` and `target_transceiver` are intended for pluggable modules (SFP/SFP+/SFP28/QSFP family, SFP-10G-T, etc.). For fixed copper ports (integrated BASE-T NIC ports), producers **SHOULD** omit `*_transceiver` and instead describe the port/NIC at the resource level (e.g. server NIC model in resource properties or `provider.model`). If producers still choose to represent fixed-port endpoints, they **MUST** not imply optical DOM availability.

Each `*_transceiver` object **SHOULD** include when known:
- `vendor` (string)
- `model` (string; e.g. "SFP-10G-T", "QSFP-100G-SR4")
- `part_number` (string; optional)
- `serial_number` (string; optional)
- `form_factor` (string; e.g. `sfp`, `sfp+`, `sfp28`, `qsfp28`, `qsfp-dd`)
- `dom` (object; optional): Digital Optical Monitoring - Digital Diagnostic Monitoring (DOM/DDM) telemetry for transceivers that support diagnostics.
  - DOM/DDM commonly includes module temperature, supply voltage, Tx/Rx optical power and often Tx bias current.
  - Availability, precision and supported metrics are module/vendor dependent; producers **SHOULD** omit keys that are not supported (equivalent to "N/A" on many platforms).
  - Consumers **SHOULD** treat DOM values as indicative operational telemetry, not metrology-grade measurements.

  **Recommended DOM keys (portable core):**
  - `module_temperature_c` (number)
  - `supply_voltage_v` (number)
  - `tx_power_dbm` (number)
  - `rx_power_dbm` (number)
  - `tx_bias_ma` (number; if supported)

  **Lane-aware DOM (optional, for multi-lane optics):**
  - `lanes` (array of objects): per-lane diagnostics when available
    - `lane` (number; 1-based index)
    - `tx_power_dbm` (number; optional)
    - `rx_power_dbm` (number; optional)
    - `tx_bias_ma` (number; optional)
  - For multi-lane optics, producers **SHOULD** use `lanes`. For single-lane optics, producers **MAY** use only the top-level keys. If both are present, consumers **SHOULD** prefer `lanes`.

  **Advanced diagnostics (optional, often vendor/platform dependent):**
  - `pre_fec_ber` (number; optional)
  - `uncorrected_ber` (number; optional)
  - `snr_db` (number; optional)
  - `laser_temperature_c` (number; optional)
  - `laser_frequency_ghz` (number; optional)

  Producers **SHOULD** place highly vendor-specific optics telemetry, proprietary counters or fields with vendor-dependent semantics in `extensions.osiris.<namespace>`.

**Dependency connections:**
- `required` (boolean)
- `endpoint` (string; URL/host)
- `timeout_seconds` (number)
- `api_version` (string)

**Containment connections:**
- `allocation` (object; producer-defined)
- `capacity` (object; producer-defined)


### 5.4.3 Extensions
Extensions follow the same `osiris.<namespace>` convention as resources:
```json
{
  "extensions": {
    "osiris.cisco": {
      "neighbor_address": "192.0.2.1",
      "soft_reconfiguration": true
    }
  }
}
```

Consumers **MUST** ignore unknown extension namespaces and fields.


### 5.4.4 Examples
#### Network connection (bidirectional)
```json
{
  "id": "conn-net-web-db",
  "source": "vm-web-001",
  "target": "vm-db-001",
  "type": "network",
  "direction": "bidirectional",
  "status": "active",
  "properties": {
    "protocol": "tcp",
    "ports": [5432],
    "vlan": 100
  },
  "tags": {
    "environment": "production"
  }
}
```

#### Dependency (forward)
```json
{
  "id": "conn-dep-api-auth",
  "source": "service-api-gateway",
  "target": "service-auth",
  "type": "dependency.api",
  "direction": "forward",
  "properties": {
    "required": true,
    "api_version": "v2",
    "timeout_seconds": 5
  }
}
```

#### Physical copper ethernet link (bidirectional)
```json
{
  "id": "cn-MXP-F1-R01-SW-001-48-to-MXP-F1-R01-SRV-042-eno1-1",
  "source": "MXP-F1-R01-SW-001",
  "target": "MXP-F1-R01-SRV-042",
  "type": "physical.ethernet",
  "direction": "bidirectional",
  "status": "active",
  "properties": {
    "source_port": "Ethernet48",
    "target_port": "eno1",
    "speed_gbps": 10,
    "cable_type": "cat6a",
    "length_meters": 5,
    "source_transceiver": {
      "vendor": "arista",
      "model": "SFP-10G-T",
      "form_factor": "sfp+",
      "serial_number": "SFP10GT-XXXXXXXX"
    },
    "target_transceiver": {
      "vendor": "broadcom",
      "model": "57412 Quad Port 10GbE BASE-T (OCP 3.0)",
      "form_factor": "base-t",
      "serial_number": "BCM57412-YYYYYYYY"
    }
  }
}
```

#### Physical fiber ethernet link (bidirectional)
```json
{
  "id": "cn-MXP-F1-R01-SW-001-49-1-to-MXP-F1-R01-SRV-042-ens2np0-1",
  "source": "MXP-F1-R01-SW-001",
  "target": "MXP-F1-R01-SRV-042",
  "type": "physical.fiber",
  "direction": "bidirectional",
  "status": "active",
  "properties": {
    "source_port": "Ethernet49/1",
    "target_port": "ens2np0",
    "speed_gbps": 100,
    "cable_type": "om4",
    "length_meters": 30,
    "source_transceiver": {
      "vendor": "arista",
      "model": "100GBASE-SR4",
      "form_factor": "qsfp28",
      "serial_number": "QSFP100SR4-XXXXXXXX",
      "dom": {
        "module_temperature_c": 42.1,
        "supply_voltage_v": 3.28,
        "lanes": [
          { "lane": 1, "tx_power_dbm": -1.3, "rx_power_dbm": -2.1, "tx_bias_ma": 44.2 },
          { "lane": 2, "tx_power_dbm": -1.2, "rx_power_dbm": -2.3, "tx_bias_ma": 44.0 },
          { "lane": 3, "tx_power_dbm": -1.4, "rx_power_dbm": -2.2, "tx_bias_ma": 44.4 },
          { "lane": 4, "tx_power_dbm": -1.1, "rx_power_dbm": -2.0, "tx_bias_ma": 43.8 }
        ]
      }
    },
    "target_transceiver": {
      "vendor": "nvidia",
      "model": "100GBASE-SR4",
      "form_factor": "qsfp56",
      "serial_number": "QSFP56SR4-YYYYYYYY",
      "dom": {
        "module_temperature_c": 39.7,
        "supply_voltage_v": 3.30,
        "lanes": [
          { "lane": 1, "tx_power_dbm": -1.0, "rx_power_dbm": -2.4, "tx_bias_ma": 41.9 },
          { "lane": 2, "tx_power_dbm": -1.1, "rx_power_dbm": -2.5, "tx_bias_ma": 42.1 },
          { "lane": 3, "tx_power_dbm": -1.2, "rx_power_dbm": -2.6, "tx_bias_ma": 42.0 },
          { "lane": 4, "tx_power_dbm": -1.0, "rx_power_dbm": -2.3, "tx_bias_ma": 41.8 }
        ]
      }
    }
  }
}
```

#### Vendor-specific connection type
```json
{
  "id": "conn-vpc-peer-prod-dev",
  "source": "vpc-prod-001",
  "target": "vpc-dev-001",
  "type": "osiris.aws.vpc-peering",
  "direction": "bidirectional",
  "status": "active",
  "properties": {
    "cidr_blocks": ["10.0.0.0/16", "10.1.0.0/16"]
  },
  "extensions": {
    "osiris.aws": {
      "peering_connection_id": "pcx-0abc123def456"
    }
  }
}
```

### 5.4.5 Validation
If present, `properties` and `extensions` **MUST** be JSON objects. `tags` (if present) **MUST** be a JSON object mapping strings to strings.

Consumers **MUST** accept connections with unrecognized property keys or extension namespaces and **MUST NOT** reject documents solely due to unknown connection metadata.