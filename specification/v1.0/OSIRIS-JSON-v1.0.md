# OSIRIS JSON Format Specification

| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 14 December 2025 |
| Last revision date | 18 December 2025 |
| Status    | Draft |
| Specification ID | OSIRIS-1.0 |
| Schema URI | tbd later |
| License   | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Repository | [github.com/osirisjson/osiris](https://github.com/osirisjson/osiris) |


## Table of Content
1. [Introduction](#introduction)
    1.1 [Problem statement](#problem)
    1.2 [Solution overview](#problem)
    1.3 [Scope](#problem)
    1.4 [Design principles](#problem)
    1.5 [Terminology](#problem)


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

# Introduction
OSIRIS (Open Standard for Infrastructure Resource Interchange Schema) defines a vendor-neutral JSON format for describing infrastructure resources, their properties and their topological relationships across heterogeneous IT and OT environments.

OSIRIS is designed to normalize infrastructure data exported from diverse domains, including public hyperscalers, cloud platforms, private Data Centers, network devices, virtualization platforms and where applicable Operational Technology (OT) assets. OSIRIS focuses on describing what exists and how it relates, enabling cross-platform visibility and portable consumption by tools without requiring consumers to implement vendor-specific parsers.

OSIRIS is an interchange schema. It is not intended to replace vendor APIs or infrastructure-as-code models, but viceversa it aims to provide a stable and consistent schema suitable for diagramming, documentation, audit evidence, inventories and integrations.

---

## 1.1 Problem statement
The modern infrastructure landscape span multiple technology stacks and providers, including hyperscalers and continuously evolving private and public cloud platforms, on-premises data centers featuring compute virtualization platforms, storage and network systems and continuously increasing OT integration. While many systems started offerin a comprehensible export format for resource inventories and configurations, the exported representations differ significantly across vendors and products, even when JSON is used. In OT environments the gap is even bigger due to legacy systems and vendor specific protocols.


##### Heterogeneous exports and inconsistent semantics:
Infrastructure data is tipically exposed through vendor-specific formats and models with inconsistent semantics for similar concepts:

- **Major hyperscalers:** Different providers expose fundamentally similar concepts (compute, storage, networking) using different export structures and naming conventions (e.g. differing resource identifiers, property models and relationship representations).

- **Network devices:** Device inventories and topologies data are often obtained via CLI output, NETCONF/YANG, vendor APIs or telemetry streams, each requiring specialized parsing and normalization.

- **Virtualization platforms:** APIs and exports vary widely (JSON, XML, proprietary models), with distinct field naming, resource hierarchies and relationship models.

- **Operational technology:** Building automation systems (BACnet, Modbus etc.) and industrial control systems use binary protocols or vendor-specific data representations with no standardized topology export.

As a result, producing a comprehensible, cross-platform view of infrastructure typically requires bespoke translation layers that are costly to build and difficult to sustain.


##### Consequences
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


#### Core capabilities
OSIRIS provides:

1. **Unified resource model:** A common structure for representing infrastructure resources (compute, network, storage, applications) with consistent field semantics regardless of origin.

2. **Explicit relationship model:** First-class representation of connections, dependencies, containment and other topological relationships that are often implicit or vendor-specific.

3. **Grouping constructs:** Support for logical and physical grouping that reflects organizational and architectural structure without mandating a single taxonomy.

4. **Provider attribution:** Resources retain traceability to their originating platform (e.g. AWS, Azure, GCP, Arista Networks, Nokia, Cisco, Proxmox) while being described in standardized terms enabling mixed-source topologies.

5. **Extensibility:** A defined mechanism for vendor-specific properties and custom resource types without breaking schema compatibility.


#### Design Approach
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


#### In Scope
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


#### Out of Scope
The following are explicitly **out of scope** for OSIRIS v1.0:

- **Real-time telemetry and monitoring:** OSIRIS **describes** topology and inventory state, not time-series metrics, logs or observability data. Projects like OpenTelemetry or Prometheus, Grafana address telemetry concerns.

- **Infrastructure deployment and orchestration:** OSIRIS is not an infrastructure-as-code language or a deployment specification. Projects like Terraform, CloudFormation or TOSCA Oasis address deployment workflows.

- **Configuration management:** OSIRIS does not define or enforce desired state configuration for resources. Configuration management systems like Ansible, Puppet or Chef serve this purpose.

- **Change tracking and history:** OSIRIS represents a point-in-time snapshot. Tracking changes over time is the responsibility of producing and consuming systems.

- **Authentication, authorization and access policies:** OSIRIS may carry metadata describing boundaries or ownership, but it does not define access control mechanisms or policies.

- **Cost and billing information:** Detailed billing and financial modeling are outside the core schema scope, although cost-related metadata may be included by extension.


#### Version specific considerations
OSIRIS v1.0 prioritizes:

1. **Core IT infrastructure:** Network, storage and compute resources across hyperscalers, public cloud providers and on-premises environments.

2. **Common resource types:** Resource categories and relationships found across multiple platforms receive first-class support.

3. **Practical extensibility:** The extension mechanism enables representation of vendor-specific or specialized resources without requiring immediate schema updates.

4. **OT integration as secondary objective:** While OT resources are architecturally feseable and supported conceptually, comprehensive OT taxonomy and relationship modeling may evolve in subsequent versions based on community feedback.


#### Future Extensibility
OSIRIS is designed to accommodate:

- Additional resource types through the extension mechanism
- Domain-specific taxonomies (e.g. expanded OT support, edge computing, IoT)
- Enhanced relationship semantics
- Additional metadata models

The core schema structure and extension mechanisms are designed primarly for stability, allowing implementations to handle future resource types and properties without breaking compatibility during time.

---

## 1.4 Design principles
OSIRIS is guided by the following core principles:

#### Open Standard and driven by Community Governance
OSIRIS is an **Open Standard** intended for broad adoption across vendors, platforms and communities. The specification and evolution of the schema are intended to be community-driven and openly reviewable.

#### Simplicity
OSIRIS prioritizes ease of understanding and implementation. The schema uses straightforward JSON structures with clear field semantics. Complexity is introduced only where necessary to represent real-world infrastructure. Producers and consumers **SHOULD** be able to implement basic OSIRIS support without extensive specialized knowledge.

#### Vendor neutrality
OSIRIS does not favor any particular vendor, platform or technology. Infrastructure sources including hyperscalers, cloud providers, on-premises systems and OT environments are represented using consistent patterns. Provider specific details are accommodated through well defined extension mechanisms rather than privileged positions in the core schema.

#### Extensibility without fragmentation
OSIRIS supports vendor-specific properties, custom resource types and domain-specific extensions without breaking compatibility. Extensions follow structured conventions to preserve interoperability: consumers that do not recognize extensions **MUST** be able to safely ignore them while still processing core data.

#### Explicit over implicit
Relationships, dependencies and topological connections are represented explicitly. Resource properties are clearly named with defined semantics. The schema avoids ambiguous or context-dependent interpretations that would require out-of-band knowledge to resolve.

#### Stability and compatibility
The core schema structure is designed for long-term stability. Version changes follow semantic versioning principles. Backwards compatibility **SHOULD** be maintained across minor versions and breaking changes **MUST** be introduced only in major versions with clear migration guidance and deprecation policies.

#### Practicality and parial data
OSIRIS is designed for real-world infrastructure conditions, including incomplete inventories, mixed-vendor environments and partial topology exports. The schema supports scenarios where full resource details or relationships are unavailable, enabling incremental adoption.

##### Interoperability
OSIRIS uses the widely supported JSON format and aligns with established conventions, including JSON Schema for structural validation and [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119.html) keywords for normative requirements. OSIRIS is designed to integrate with existing toolchains, programming languages and data processing pipelines without requiring specialized runtimes.

##### Human readability
While OSIRIS is machine-readable by design, the JSON structure remains human-readable for debugging, auditing and manual inspection. Field names use clear, descriptive terminology and the schema avoids unnecessary encoding that would obscure content.

##### Semantic richness
OSIRIS represents not only resource existence but also meaningful relationships and hierarchies. The schema distinguishes between different connection types (e.g. network connectivity, dependency, containment). It supports grouping constructs and preserves provider attribution to enable accurate visualization and analysis.

##### Separation of concerns
OSIRIS focuses on **Infrastructure Resources Interchange**. It does not define deployment orchestration, monitoring or telemetry, configuration management or access control. This focused scope allows OSIRIS to serve as a stable interchange layer that complements specialized systems.

---

## 1.5 Terminology
This section defines key terms used throughout this specification.


#### Normative keywords
The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**,  **MAY** and **OPTIONAL** in this document are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119.html).


#### Core terms
**OSIRIS document:** A JSON document conforming to the OSIRIS specification, representing an infrastructure documentation at a specific point in time.

**OSIRIS topology:** The complete set of resources, connections, groups and metadata describing an infrastructure environment and its relationships at a specific point in time conforming to the OSIRIS specification.

**Resource:** A uniquely identifiable infrastructure entity with properties and relationships (e.g. compute instances, network devices, storage systems, applications).

**Connection:** An explicit relationship between two resources (e.g. network connectivity, dependency, containment or data flow).

**Group:** A logical or physical collection used to organize resources (e.g. VPC, subnet, resource group, availability zone, hypervisor, physical server, rack, data center).

**Producer:** A system or component that generates OSIRIS documents by exporting, translating or discovering infrastructure data from source systems.

**Consumer:** A system or component that reads and processes OSIRIS documents (e.g. diagramming tools, CMDBs, monitoring systems, documentation generators).

**Provider:** The originating platform, vendor or system from which a resource was sourced (e.g. AWS, Azure, Arista Networks, Cisco, Proxmox).

**Resource type:** A classification that defines what kind of infrastructure entity a resource represents (e.g. compute.vm, network.switch, storage.volume).

**Connection type:** A classification that defines the semantic meaning of a relationship between resources (e.g. connected_to, attached_to, contains, depends_on, routes_through).

**Extension:** Namespaced vendor-specific or custom properties and types that extend OSIRIS without modifying the core schema structure.

**Topology:** The complete set of resources, connections and groups that describe an infrastructure environment's structure and relationships.


#### Domain specific terms
**Hyperscaler:** A large-scale provider offering a broad portfolio of infrastructure services (e.g. AWS, Azure, GCP oracle Cloud, IBM Cloud, Alibaba Cloud, Tencent Cloud).

**Cloud provider:** A minor provider offering cloud services such as IaaS, PaaS, SaaS or NaaS.

**On-Premise infrastructure:** Computing, networking and storage resources operated within an organization’s facilities rather than in a cloud provider environment.

**Operational Technology (OT):** Hardware and software systems that monitor and control physical devices, processes and infrastructure, including building automation systems (BAS), industrial control systems (ICS) and supervisory control and data acquisition (SCADA) systems.

**IT-OT convergence:** The integration of IT systems with OT systems to enable unified end-to-end visibility and management.

**Interchange format:** A standardized data representation designed for exchange between heterogeneous systems and not tied to any single tool’s internal model.


#### JSON and schema terms
**JSON (JavaScript Object Notation):** The data serialization format used by OSIRIS, as defined in [RFC 8259](https://www.rfc-editor.org/rfc/rfc8259.html).

**JSON Schema:** A vocabulary for validating the structure of JSON documents, used to define the formal structure of OSIRIS documents.

**Required field:** A field that **MUST** be present in a valid OSIRIS document or object.

**Optional field:** A field that **MAY** be present but is not requird for validity.

**Additional properties:** Fields beyond those explicitly defined in the schema, typically used for extensions or vendor-specific data.

---

# Core concepts
This section introduces the fundamental building blocks of OSIRIS: **Resources**, **Connections**, **Groups** and **Metadata**. Understanding these core concepts is essential before examining the detailed schema structure in subsequent sections.

OSIRIS models infrastructure as a graph composed of:

- **Resources (nodes):** Individual infrastructure components with identity, properties and state.
- **Connections (edges):** Explicit relationships between resources representing dependencies, network links or containment.
- **Groups:** Collections of resources organized by logical or physical boundaries.
- **Metadata:** Contextual information describing the topology’s origin, scope and provenance.

Together these elements enable OSIRIS to represent complex, heterogeneous infrastructure topologies in a structured, vendor-neutral format.

---

## 2.1 Resources
#### Definition
A **Resource** represents a discrete infrastructure component with identity, properties and lifecycle. Resources are the fundamental baseline units of an OSIRIS topology and **MAY** correspond to (non-exhaustive examples):

- **Compute resources:** virtual machines, containers, physical servers.
- **Network devices:** routers, switches, firewalls, load balancers.
- **Storage systems:** volumes, arrays, buckets, file systems.
- **Platform services:** databases, message queues, managed services.
- **Application components:** services, repositories, deployments.
- **Operational technology elements:** building automation controllers, industrial sensors, SCADA components.


#### Resource identity
Every resource **MUST** have a unique identifier within an OSIRIS document. Resource identifiers enable references from connections, group memberships and other objects.

Resource identity in OSIRIS is **document-scoped**: identifiers **MUST** be unique within a single OSIRIS document but need not be globally unique across other documents or systems. This simplifies producer implementation while preserving referential integrity within each snapshot.

Resources **SHOULD** retain stable identifiers across multiple exports of the same infrastructure when feasible. Stable identifiers enable consumers to correlate resources over time, detect changes and compare snapshots.


#### Resource types
Each resource **MUST** declare a type that classifies what kind of infrastructure component it represents. Resource types follow a hierarchical dot notation convention (e.g. `compute.vm`, `network.switch`, `storage.volume`) that supports both categorization and specificity.

The type system includes:

- **Standard types:** common resource categories defined in chapter 7 (Resource Type Taxonomy).
- **Vendor specific types:** platform specific types using namespaced conventions (see chapter 8).
- **Custom types:** organization defined types for specialized or internal components.

Consumers **MUST** be able to process resources with unknown types by treating them as opaque resources while still preserving identity, properties and relationships.


#### Provider attribution
Resources retain attribution to their originating system through provider information. 

Provider attribution enables:

- traceability to source systems for validation or enrichment.
- mixed-vendor topologies where resources from different platforms coexist.
- platform-specific rendering or behavior in consuming tools (when supported).
- audit trails and source tracking.

Provider information typically includes provider name (e.g. AWS, Azure, Arista, Cisco, Proxmox), native resource identifiers, account/subscription/project context and regional or zonal placement where applicable.


#### Resource properties
Resources carry properties that describe characteristics, configuration and operational attributes. 

Properties **MAY** include:

- **Intrinsic attributes:** size, capacity, network addresses, operating system.
- **Configuration details:** enabled features, policies, attached resources.
- **Operational indicators:** running, stopped, healthy, degraded (as applicable).
- **Vendor-specific metadata:** fields relevant to particular platforms.

The core OSIRIS schema defines a standard property structure. The extension mechanism allows producers to include additional properties without breaking schema validity. Consumers that do not recognize extended properties **MUST** safely ignore them while processing core fields.


#### Resource status and state
Resources **MAY** include high-level status and/or state information to support filtering, visualization and operational awareness.

- **Status** generally refers to lifecycle presence or availability within the snapshot context.
- **State** generally refers to an operational condition (where applicable).

The exact fields and allowed values are defined in the chapter 4 Resource model (see Section 4.5).


#### Resource lifecycle considerations
OSIRIS represents infrastructure at a point in time. Resources reflect their observed state when the OSIRIS document was generated, not ongoing changes. Lifecycle events such as creation, modification or deletion are not represented within a single document but **MAY** be inferred by comparing multiple snapshots.

Resources that no longer exist in source systems **SHOULD NOT** appear in subsequent exports unless producers intentionally emit tombstones or inactive entries for change-tracking purposes (as defined by the schema and producer behavior).

---

## 2.2 Connections
#### Definition
A **Connection** represents an explicit relationship between two resources. Connectios are the edges in the OSIRIS topology graph and describe how resources interact depend on each other or are organized hierarchically.

Connections capture relationships that **MAY** include (non-exhaustive examples):

- **Network connectivity:** Layer 2/Layer 3 links, routing neighbours, firewall rules, VPN tunnels.
- **Dependencies:** Applications depending on databases, services consuming APIs, load balancers distributing to backends.
- **Containment:** Virtual machines within hypervisors, containers within pods, resources within VPCs or subnets.
- **Data flow:** Storage mounted to compute, backup relationships, replication streams.
- **Physical topology:** Cables between switches, power distribution, rack-to-server relationships.
- **Logical associations:** Membership in availability zones, assignment to projects, tagging relationships.


#### Connection identity
Every connection **MUST** have a unique identifier within an OSIRIS document. Connection identifiers enable reference and validation.

Connection identity is **document-scoped**, analogous to resource identity. Identifiers **MUST** be unique within a single OSIRIS document but need not be globally unique.


#### Source and target
Each connection **MUST** identify a source resource and a target resource using their respective resource identifiers.

Both source and target identifiers **MUST** reference resources that exist within the same OSIRIS document. Connections **MUST NOT** reference resources outside the document boundary.

Producers **SHOULD** validate referential integrity before emitting OSIRIS documents. Consumers **MUST** handle connections with invalid references gracefully, either by reporting validation errors or by ignoring malformed connections while processing valid topology data.


#### Connection types
Each connection **MUST** declare a type that classifies the nature of the relationship. Connection types enable consumers to distinguish between different kinds of relationships and apply appropriate logic for visualization, analysis or validation.

The connection type system includes:

- **Standard types:** common relationship types defined in chapter 5 Section 5.2 (Connection Types), such as `network`, `dependency`, `contains`, `dataflow`.
- **Vendor-specific types:** platform specific relationships using namespaced conventions (see Chapter 8).
- **Custom types:** organization defined types for specialized relationships.

Consumers **MUST** be able to process connections with unknown types by treating them as generic relationships while preserving their source, target and properties.


#### Directionality
Connections **MAY** specify directionality to indicate the orientation or flow of the relationship:

- **Bidirectional:** The relationship applies equally in both directions (e.g. a Layer 2 network link, peering relationships).
- **Directed (source > target):** The relationship flows from source to target (e.g. a service depends on a database, traffic flows from client to server).

If directionality is unspecified, consumers **SHOULD** treat the connection as bidirectional unless the connection type definition explicitly specifies otherwise.

Directionality is distinct from the graph structure itself: the source and target fields define graph topology, while directionality provides semantic annotation about how the relationship should be interpreted.


#### Connection properties
Connections **MAY** carry properties that describe relationship characteristics:

- **Protocol or mechanism:** TCP, HTTP, NFS, iSCSI, routing protocol.
- **Capacity or bandwidth:** Link speed, throughput limits.
- **Ports or endpoints:** Source/destination ports, IP addresses.
- **Quality attributes:** Latency, packet loss, reliability indicators.
- **Administrative metadata:** Tags, labels, ownership, creation time.

As with resources, the extension mechanism allows producers to include vendor specific or custom properties without breaking schema validity. Consumers that do not recognize extended properties **MUST** safely ignore them.


#### Connection status
Connections **MAY** include status information indicating operational health or availability to support filtering and visualization.

Common values include `active`, `inactive`, `degraded` and `unknown`. The exact field structure and allowed values are defined in chapter 5 Connection model (see Section 5.4 Connection properties).


#### Implicit vs. explicit relationships
OSIRIS requires connections to be **explicit**. Relationships that could be inferred from resource properties (e.g. a VM referencing a VPC ID in its properties) **SHOULD** also be represented as explicit connection objects.

Explicit representation ensures:

- consuming tools do not require inference logic specific to vendor property conventions.
- relationship semantics are clear and unambiguous.
- topology graphs can be traversed using connection objects alone.

Producers **MAY** emit both property based references and explicit connections for redundancy and compatibility with different consumer implementations.


#### Multiple connections between resources
Two resources **MAY** have multiple connections of different types or with different properties.

For example:

- A virtual machine and a storage volume may have both a **contains** relationship and a **dataflow** relationship.
- Two switches may have multiple **network** connections representing distinct physical links.

Each distinct relationship **MUST** be represented as a separate connection object with a unique identifier.

---

## 2.3 Groups
#### Definition
A **Group** represents a logical or physical collection of resources organized by common characteristics, boundaries, or administrative domains. Groups provide structure to OSIRIS topologies and enable representation of organizational, architectural or geographical boundaries.

Groups **MAY** represent (non-exhaustive examples):

- **Network boundaries:** VPCs/virtual networks, subnets, VLANs, VRFs, security zones.
- **Resource organization:** resource groups, projects, namespaces, folders, tenants.
- **Physical location:** data centers, buildings, floors, rooms, racks, rows.
- **Availability constructs:** regions, availability zones, fault domains.
- **Administrative boundaries:** subscriptions, accounts, organizational units.
- **Functional grouping:** application tiers (web/app/db), environments (production/staging/development).


#### Group identity
Every group **MUST** have a unique identifier within an OSIRIS document. Group identifiers enable reference from other groups and from metadata contexts.

Group identity is **document-scoped**, consistent with resources and connections. Identifiers **MUST** be unique within a single OSIRIS document but need not be globally unique.


#### Group types
Each group **MUST** declare a type that classifies what kind of collection or boundary it represents. Group types follow a hierarchical dot-notation convention similar to resource types (e.g. `network.vpc`, `logical.environment`, `physical.datacenter`).

The group type system includes:

- **Standard types:** common grouping categories defined in Chapter 6 Group model, Section 6.2 (Group types).
- **Vendor-specific types:** platform specific grouping constructs using namespaced conventions (see Chapter 8 Extensive mechanism).
- **Custom types:** organization defined types for specialized grouping needs.

Consumers **MUST** be able to process groups with unknown types by treating them as generic collections while preserving membership and hierarchy.


#### Membership (resources)
Groups contain **members**, which are resource identifiers representing the resources that belong to the group. Membership is represented as an array of resource IDs.

All member identifiers **MUST** reference resources that exist within the same OSIRIS document. Groups **MUST NOT** reference resources outside the document boundary.

A resource **MAY** be a member of multiple groups simultaneously. For example, a physical server may belong to both a rack (`physical.rack`) and an environment group (`logical.environment.production`), while a VM may belong to both a subnet group (`network.subnet`) and an application tier group (`logical.tier.web`).

Membership is **explicit and direct**: resources listed in a group are members of that group only. Consumers that require transitive membership **SHOULD** implement traversal logic.


#### Hierarchical groups (nesting)
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


#### Group properties
Groups **MAY** carry properties that describe collection characteristics:

- **Boundaries or constraints:** IP ranges (subnets), VLAN IDs, VRF names, geographic coordinates (data centers).
- **Configuration or policies:** security policies, routing constraints, segmentation intent.
- **Capacity or limits:** quotas, available rack units, power capacity.
- **Administrative metadata:** tags, labels, ownership, creation time.

As with resources and connections, the extension mechanism allows producers to include vendor-specific or custom properties without breaking schema validity. Consumers that do not recognize extended properties **MUST** safely ignore them.


#### Groups vs. connections
Groups and connections serve complementary purposes:

- **Groups** represent **containment and organization** (boundaries, membership, hierarchy).
- **Connections** represent **relationships and interactions** (communication, dependency, linkage).

Example (in hybrid environments):
- A VM or server is a **member** of a subnet/VLAN group (structure).
- A load balancer **depends_on** or **routes_to** backend instances via **connections** (behavior/interaction).

Producers **SHOULD** choose the representation that most clearly expresses the intended semantics. Consumers **SHOULD** support both patterns and **MUST NOT** assume exclusive use of either mechanism.


#### Empty groups
Groups **MAY** have an empty membership list (zero members). Empty groups are valid and may represent:

- Pre-defined organizational structures awaiting resource assignment.
- Placeholder groups for documentation or planning.
- Structures preserved across exports even when temporarily unused.

Consumers **MUST** handle empty groups without error.

---

## 2.4 Metadata
#### Definition
**Metadata** provides contextual information about an OSIRIS document, including its origin, scope, generation details and provenance. Metadata enables consumers to understand what infrastructure the document represents, when it was produced and how to interpret or validate its contents.

Metadata is distinct from resource properties: metadata describes the **document and topology as a whole** not individual infrastructure components.


#### Purpose of metadata
Metadata serves several critical functions in the OSIRIS interchange model:

- **Provenance tracking:** Identifying the source system, tool or process that generated the document.
- **Temporal context:** Recording when the snapshot was taken to enable time-based correlation or change detection.
- **Scope documentation:** Describing what infrastructure is included (regions, accounts, environments) and what is excluded.
- **Specification context:** Indicating which OSIRIS version the document conforms to.
- **Audit trail:** Supporting compliance and governance workflows by documenting data lineage.
- **Consumer guidance:** Providing advisory hints that help consuming tools interpret or validate the topology.


#### Core metadata elements
OSIRIS metadata **MUST** include at minimum:

- **Timestamp:** An ISO 8601 timestamp indicating when the topology snapshot was generated (e.g., `2025-12-18T14:30:00Z`).

OSIRIS metadata **SHOULD** include:

- **Generator information:** The name and version of the tool or system that produced the document.
- **Scope description:** What providers, regions, accounts/projects/subscriptions or environments are represented.

OSIRIS metadata **MAY** include:

- **Tags or labels:** Custom key-value pairs for organizational categorization.
- **Source references:** References to originating systems, API endpoints or datasets used to construct the topology.
- **Validation hints:** Advisory information to assist consumers in validating or interpreting the topology.

> [!NOTE] 
> The OSIRIS specification version is declared at the document level (see Section 3.2). Metadata **MAY** repeat this information for convenience, but any duplicated version information **MUST** be consistent.


#### Metadata extensibility
Like resources, connections, and groups, metadata supports extension through custom properties. Producers **MAY** include vendor-specific or organization-specific metadata fields without breaking schema validity.

Consumers that do not recognize extended metadata properties **MUST** safely ignore them while processing the document.


#### Metadata vs. resource properties
It is important to distinguish metadata from resource level properties:

- **Metadata** describes the OSIRIS document: "This topology was exported from AWS on 2025-12-18 at 14:30 UTC covering us-east-1 and us-west-2a."
- **Resource properties** describe individual components: "This instance is `t3.medium` with 2 vCPUs and 4 GB RAM."

Information that applies to the entire document or export process belongs in metadata. Information that applies to a specific resource belongs in that resource's properties.


#### Metadata and change tracking
While OSIRIS represents infrastructure at a point in time, metadata enables consumers to correlate multiple snapshots over time:

- Timestamps allow chronological ordering of documents.
- Generator information enables filtering by source or tool version.
- Scope descriptions help identify whether documents represent comparable infrastructure sets.

Change detection and versioning logic are the responsibility of consuming systems but metadata provides the context to support these workflows.


#### Metadata stability
Metadata structure is part of the core OSIRIS schema and follows the same versioning and compatibility rules as the rest of the specification.

Consumers **SHOULD** validate metadata before processing topology content to detect version mismatches or unsupported schema features early.
