# OSIRIS JSON Format Specification<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 14 December 2025 |
| Last revision date | 31 December 2025 |
| Status    | Draft |
| Specification ID | OSIRIS-1.0 |
| Schema URI | tbd later |
| License   | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Repository | [github.com/osirisjson/osiris](https://github.com/osirisjson/osiris) |

## Table of Content
- [1 Introduction](#1-introduction)
  - [1.1 Problem statement](#11-problem-statement)
    - [1.1.1 Heterogeneous exports and inconsistent semantics:](#111-heterogeneous-exports-and-inconsistent-semantics)
    - [1.1.2 Consequences](#112-consequences)
  - [1.2 Solution overview](#12-solution-overview)
    - [1.2.1 Core capabilities](#121-core-capabilities)
    - [1.2.2 Design Approach](#122-design-approach)
  - [1.3 Scope](#13-scope)
    - [1.3.1 In Scope](#131-in-scope)
    - [1.3.2 Out of Scope](#132-out-of-scope)
    - [1.3.3 Version specific considerations](#133-version-specific-considerations)
    - [1.3.4 Future Extensibility](#134-future-extensibility)
  - [1.4 Design principles](#14-design-principles)
    - [1.4.1 Open Standard and driven by Community Governance](#141-open-standard-and-driven-by-community-governance)
    - [1.4.2  Simplicity](#142--simplicity)
    - [1.4.3  Vendor neutrality](#143--vendor-neutrality)
    - [1.4.4  Extensibility without fragmentation](#144--extensibility-without-fragmentation)
    - [1.4.5  Explicit over implicit](#145--explicit-over-implicit)
    - [1.4.6  Stability and compatibility](#146--stability-and-compatibility)
    - [1.4.7  Practicality and parial data](#147--practicality-and-parial-data)
  - [1.5 Terminology](#15-terminology)
    - [1.5.1 Normative keywords](#151-normative-keywords)
    - [1.5.2 Core terms](#152-core-terms)
    - [1.5.2 Domain specific terms](#152-domain-specific-terms)
    - [1.5.3 JSON and schema terms](#153-json-and-schema-terms)
- [2 Core concepts](#2-core-concepts)
  - [2.1 Resources](#21-resources)
    - [2.1.1 Definition](#211-definition)
    - [2.1.2 Resource identity](#212-resource-identity)
    - [2.1.3 Resource types](#213-resource-types)
    - [2.1.4 Provider attribution](#214-provider-attribution)
    - [2.1.5 Resource properties](#215-resource-properties)
    - [2.1.6 Resource status and state](#216-resource-status-and-state)
    - [2.1.7 Resource lifecycle considerations](#217-resource-lifecycle-considerations)
  - [2.2 Connections](#22-connections)
    - [2.2.1 Definition](#221-definition)
    - [2.2.2 Connection identity](#222-connection-identity)
    - [2.2.3 Source and target](#223-source-and-target)
    - [2.2.4 Connection types](#224-connection-types)
    - [2.2.5 Directionality](#225-directionality)
    - [2.2.6 Connection properties](#226-connection-properties)
    - [2.2.7 Connection status](#227-connection-status)
    - [2.2.8 Implicit vs. explicit relationships](#228-implicit-vs-explicit-relationships)
    - [2.2.9 Multiple connections between resources](#229-multiple-connections-between-resources)
  - [2.3 Groups](#23-groups)
    - [2.3.1 Definition](#231-definition)
    - [2.3.2 Group identity](#232-group-identity)
    - [2.3.3 Group types](#233-group-types)
    - [2.3.4 Membership (resources)](#234-membership-resources)
    - [2.3.5 Hierarchical groups (nesting)](#235-hierarchical-groups-nesting)
    - [2.3.6 Group properties](#236-group-properties)
    - [2.3.7 Groups vs. connections](#237-groups-vs-connections)
    - [2.3.8 Empty groups](#238-empty-groups)
  - [2.4 Metadata](#24-metadata)
    - [2.4.1 Definition](#241-definition)
    - [2.4.2 Purpose of metadata](#242-purpose-of-metadata)
    - [2.4.3 Core metadata elements](#243-core-metadata-elements)
    - [2.4.4 Metadata extensibility](#244-metadata-extensibility)
    - [2.4.5 Metadata vs. resource properties](#245-metadata-vs-resource-properties)
    - [2.4.6 Metadata and change tracking](#246-metadata-and-change-tracking)
    - [2.4.7 Metadata stability](#247-metadata-stability)
- [3 Schema Structure](#3-schema-structure)
  - [3.1 Top-Level Object](#31-top-level-object)
    - [3.1.1 Structure](#311-structure)
    - [3.1.2 Required fields](#312-required-fields)
    - [3.1.3 Additional top-level fields](#313-additional-top-level-fields)
    - [3.1.4 Document size considerations](#314-document-size-considerations)
  - [3.2 Version](#32-version)
    - [3.2.1 Field definition](#321-field-definition)
    - [3.2.2 Version string format](#322-version-string-format)
    - [3.2.3 Version semantics](#323-version-semantics)
    - [3.2.4 Consumer version handling](#324-consumer-version-handling)
    - [3.2.5 Version field placement](#325-version-field-placement)
  - [3.3 Metadata Object](#33-metadata-object)
    - [3.3.1 Structure](#331-structure)
    - [3.3.2 Required fields](#332-required-fields)
    - [3.3.3 Optional fields](#333-optional-fields)
    - [3.3.4 Extension fields](#334-extension-fields)
    - [3.3.5 Metadata object size](#335-metadata-object-size)
    - [3.3.6 Complete metadata example](#336-complete-metadata-example)
  - [3.4 Topology Object](#34-topology-object)
    - [3.4.1 Structure](#341-structure)
    - [3.4.2 Required fields](#342-required-fields)
    - [3.4.3 Optional fields](#343-optional-fields)
    - [3.4.4 Field ordering](#344-field-ordering)
    - [3.4.5 Array element ordering](#345-array-element-ordering)
    - [3.4.6 Referential integrity](#346-referential-integrity)
    - [3.4.7 Topology object example](#347-topology-object-example)
    - [3.4.8 Empty topologies](#348-empty-topologies)
- [4 Resource model](#4-resource-model)
  - [4.1 Resource object structure](#41-resource-object-structure)
    - [4.1.1 Overview](#411-overview)
    - [4.1.2 Required fields](#412-required-fields)
    - [4.1.3 Optional fields](#413-optional-fields)
    - [4.1.4 Field extensibility and unknown fields](#414-field-extensibility-and-unknown-fields)
    - [4.1.5 Resource object validation](#415-resource-object-validation)
  - [4.2 Resource types](#42-resource-types)
    - [4.2.1 Overview](#421-overview)
    - [4.2.2 Type field definition](#422-type-field-definition)
    - [4.2.3 Dot notation structure](#423-dot-notation-structure)
    - [4.2.4 Namespace reservation](#424-namespace-reservation)
    - [4.2.5 Standard types](#425-standard-types)
    - [4.2.6 Vendor-specific and custom types](#426-vendor-specific-and-custom-types)
    - [4.2.7 Unknown type handling](#427-unknown-type-handling)
    - [4.2.8 Type evolution and stability](#428-type-evolution-and-stability)
    - [4.2.9 Type selection guidance](#429-type-selection-guidance)
    - [4.2.10 Type field validation](#4210-type-field-validation)
  - [4.3 Provider information](#43-provider-information)
    - [4.3.1 Overview](#431-overview)
    - [4.3.2 Provider object purpose](#432-provider-object-purpose)
    - [4.3.3 Required fields](#433-required-fields)
    - [4.3.4 Optional fields](#434-optional-fields)
    - [4.3.5 Unknown and custom providers](#435-unknown-and-custom-providers)
    - [4.3.6 Provider vs metadata scope](#436-provider-vs-metadata-scope)
    - [4.3.7 Provider object stability](#437-provider-object-stability)
    - [4.3.8 Provider object examples](#438-provider-object-examples)
    - [4.3.9 Provider field validation](#439-provider-field-validation)
  - [4.4 Properties and extensions](#44-properties-and-extensions)
    - [4.4.1 Overview](#441-overview)
    - [4.4.2 Properties object](#442-properties-object)
    - [4.4.3 Naming conventions](#443-naming-conventions)
    - [4.4.4 Properties vs standard fields](#444-properties-vs-standard-fields)
    - [4.4.5 Sensitive data](#445-sensitive-data)
    - [4.4.6 Extensions object](#446-extensions-object)
    - [4.4.7 Properties vs extensions](#447-properties-vs-extensions)
    - [4.4.8 Tags (resource labeling)](#448-tags-resource-labeling)
    - [4.4.9 Forward compatibility rules](#449-forward-compatibility-rules)
    - [4.4.10 Examples](#4410-examples)
    - [4.4.11 Validation](#4411-validation)
  - [4.5 Status and state](#45-status-and-state)
    - [4.5.1 Overview](#451-overview)
    - [4.5.2 Status field](#452-status-field)
    - [4.5.3 State field](#453-state-field)
    - [4.5.4 Producer rules](#454-producer-rules)
    - [4.5.5 Consumer rules](#455-consumer-rules)
    - [4.5.6 Examples](#456-examples)
- [5. Connection model](#5-connection-model)
  - [Overview](#overview)
  - [5.1 Connection object structure](#51-connection-object-structure)
    - [5.1.1 Overview](#511-overview)
    - [5.1.2 Required fields](#512-required-fields)
    - [5.1.3 Optional fields](#513-optional-fields)
    - [5.1.4 Minimal connection examples](#514-minimal-connection-examples)
    - [5.1.5 Referential integrity](#515-referential-integrity)
    - [5.1.6 Connection identity and stability](#516-connection-identity-and-stability)
    - [5.1.7 Multiple connections between the same resources](#517-multiple-connections-between-the-same-resources)
    - [5.1.8 Field extensibility](#518-field-extensibility)
  - [5.2 Connection types](#52-connection-types)
    - [5.2.1 Overview](#521-overview)
    - [5.2.2 Type format rules](#522-type-format-rules)
    - [5.2.3 Standard connection types (v1.0)](#523-standard-connection-types-v10)
    - [5.2.4 Vendor-specific and custom connection types](#524-vendor-specific-and-custom-connection-types)
    - [5.2.5 Unknown connection types](#525-unknown-connection-types)
  - [5.3 Directionality](#53-directionality)
    - [5.3.1 Overview](#531-overview)
    - [5.3.2 Direction values](#532-direction-values)
    - [5.3.3 Default behavior](#533-default-behavior)
    - [5.3.4 Graph traversal semantics](#534-graph-traversal-semantics)
  - [5.4 Connection properties](#54-connection-properties)
    - [5.4.1 Overview](#541-overview)
    - [5.4.2 Common properties](#542-common-properties)
    - [5.4.3 Extensions](#543-extensions)
    - [5.4.4 Examples](#544-examples)
    - [5.4.5 Validation](#545-validation)
- [6. Group model](#6-group-model)
  - [Overview](#overview-1)
  - [6.1 Group object structure](#61-group-object-structure)
    - [6.1.1 Overview](#611-overview)
    - [6.1.2 Required fields](#612-required-fields)
    - [6.1.3 Optional fields](#613-optional-fields)
    - [6.1.4 Minimal group example](#614-minimal-group-example)
    - [6.1.5 Referential integrity](#615-referential-integrity)
    - [6.1.6 Group identity and stability](#616-group-identity-and-stability)
    - [6.1.7 Field extensibility](#617-field-extensibility)
  - [6.2 Group types](#62-group-types)
    - [6.2.1 Overview](#621-overview)
    - [6.2.2 Type format rules](#622-type-format-rules)
    - [6.2.3 Standard group types](#623-standard-group-types)
    - [6.2.4 Vendor-specific and custom group types](#624-vendor-specific-and-custom-group-types)
    - [6.2.5 Unknown group types](#625-unknown-group-types)
  - [6.3 Membership](#63-membership)
    - [6.3.1 Members array](#631-members-array)
    - [6.3.2 Multiple memberships](#632-multiple-memberships)
    - [6.3.3 Children array (hierarchical nesting)](#633-children-array-hierarchical-nesting)
    - [6.3.4 Nesting semantics](#634-nesting-semantics)
  - [6.4 Groups vs contains connections](#64-groups-vs-contains-connections)
    - [6.4.1 Prefer groups when](#641-prefer-groups-when)
    - [6.4.2 Prefer contains connections when](#642-prefer-contains-connections-when)
    - [6.4.3 Avoid duplication](#643-avoid-duplication)
  - [6.5 Group metadata](#65-group-metadata)
    - [6.5.1 Examples](#651-examples)
  - [6.6 Validation](#66-validation)
- [7 Resource type taxonomy](#7-resource-type-taxonomy)
  - [7.1 Overview](#71-overview)
    - [7.1.1 Purpose and scope](#711-purpose-and-scope)
    - [7.1.2 Type naming conventions](#712-type-naming-conventions)
    - [7.1.3 Standard types vs custom types](#713-standard-types-vs-custom-types)
    - [7.1.4 Taxonomy evolution](#714-taxonomy-evolution)
  - [7.2 Application resources](#72-application-resources)
    - [7.2.1 Databases](#721-databases)
    - [7.2.2 Message queues and event streams](#722-message-queues-and-event-streams)
    - [7.2.3 Caching services](#723-caching-services)
    - [7.2.4 Code repositories](#724-code-repositories)
    - [7.2.5 Application services](#725-application-services)


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
OSIRIS addresses the fragmentation described in section 1.1 by defining a canonical JSON schema that serves as a neutral interchange format between infrastructure data sources and consuming applications.

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

Resource identity in OSI
RIS is **document-scoped**: identifiers **MUST** be unique within a single OSIRIS document but need not be globally unique across other documents or systems. This simplifies producer implementation while preserving referential integrity within each snapshot.

Resources **SHOULD** retain stable identifiers across multiple exports of the same infrastructure when feasible. Stable identifiers enable consumers to correlate resources over time, detect changes and compare snapshots.

The `id` field uniquely identifies a resource within the topology.

**ID Construction:**

Producers **SHOULD** construct IDs using one of these strategies:

1. **Native ID `<id>` (preferred for unique native identifiers):**
```json
   "id": "i-0abc123def456"
```

2. **Namespaced native ID `<provider>::<id>` (preferred for multi-provider topologies):**
```json
   "id": "aws::i-0abc123def456"
   "id": "vmware::vm-1234"
   "id": "cisco::FOC1234ABCD"
   "id": "dellemc::MXP-SRV-001"
```

3. **Full resource identifier (for providers that support structured resource IDs):**
```json
   "id": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def456"
   "id": "/subscriptions/.../resourceGroups/rg/providers/Microsoft.Compute/virtualMachines/vm-01"
```

4. **Generated ID (only when native ID unavailable or not globally unique):**
```json
   "id": "postgres-prod-01.mxp.internal.example.com"
```

**Rationale:** 
Using native provider IDs maintains traceability to source systems, enables change tracking, and reflects actual infrastructure state.

**ID separator (`::`):**
The double-colon separator distinguishes the provider namespace from the native identifier. This separator was chosen because:
- Does not conflict with AWS ARNs (which use single colons: `arn:aws:ec2:...`)
- Does not conflict with Azure resource IDs (which use slashes: `/subscriptions/...`)
- Provides clear visual distinction between provider and identifier
- Easy to parse programmatically: `id.split('::')` works in all languages

The provider name (left of `::`) follows canonical naming rules from section 4.3.3: lowercase, no hyphens or spaces (e.g., `arista`, `paloalto`). The native identifier (right of `::`) preserves the original vendor format.

**Uniqueness:** 
IDs **MUST** be unique within a topology. When combining resources from multiple providers, use namespaced IDs (approach 2) or full resource identifiers (approach 3).


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

The exact fields and allowed values are defined in the chapter 4 Resource model (see section 4.5).


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

- **Standard types:** common relationship types defined in chapter 5 section 5.2 (Connection Types), such as `network`, `dependency`, `contains`, `dataflow`.
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

Common values include `active`, `inactive`, `degraded` and `unknown`. The exact field structure and allowed values are defined in chapter 5 Connection model (see section 5.4 Connection properties).


### 2.2.8 Implicit vs. explicit relationships
OSIRIS requires connections to be **explicit**. Relationships that could be inferred from resource properties (e.g. a VM referencing a VPC ID in its properties) **SHOULD** also be represented as explicit connection objects.

Explicit representation ensures:

- consuming tools do not require inference logic specific to vendor property conventions.
- relationship semantics are clear and unambiguous.
- topology graphs can be traversed using connection objects alone.

Producers **MAY** emit both property based references and explicit connections for redundancy and compatibility with different consumer implementations.


### 2.2.9 Multiple connections between resources
Two resources **MAY** have multiple connections of different types or with different properties.

**Example:**

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

- **Standard types:** common grouping categories defined in Chapter 6 Group model, section 6.2 (Group types).
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
> The OSIRIS specification version is declared at the document level (see section 3.2). Metadata **MAY** repeat this information for convenience, but any duplicated version information **MUST** be consistent.


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

- **`version`** (string): The OSIRIS specification version to which this document conforms. See section 3.2 for version string format and semantics.

- **`metadata`** (object): Contextual information about the document's origin, scope and generation. See section 3.3 for metadata object structure.

- **`topology`** (object): The infrastructure topology data, including resources, connections and groups. See section 3.4 for topology object structure.


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

**Example:**
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
The `metadata` field is a **REQUIRED** object at the top level of every OSIRIS document. It provides contextual information about the document's generation, scope and provenance as described in section 2.4.

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

**Example:**
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

**Hyperscaler example:**
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
    "cost-center": "sw-development",
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

Producers **SHOULD** validate referential integrity before emitting documents. Consumers **MUST** handle referential integrity violations gracefully (see chapter 9, section 9.3 for validation rules). Consumers **MAY** ignore invalid connections or groups while still processing valid resources.


### 3.4.7 Topology object example
#### Hyperscaler example
```json
{
  "resources": [
    {
      "id": "aws::i-0abc123def4567890",
      "name": "production-web-server-01",
      "type": "compute.vm",
      "provider": {
        "name": "aws",
        "type": "AWS::EC2::Instance",
        "native_id": "i-0abc123def4567890",
        "region": "us-east-1",
        "account_id": "123456789012"
      },
      "status": "active"
    },
    {
      "id": "aws::myapp-prod-db",
      "name": "production-postgresql-primary",
      "type": "application.database",
      "provider": {
        "name": "aws",
        "type": "AWS::RDS::DBInstance",
        "native_id": "myapp-prod-db",
        "region": "us-east-1",
        "account_id": "123456789012"
      },
      "status": "active"
    }
  ],
  "connections": [
    {
      "id": "conn-web-to-db",
      "source": "aws::i-0abc123def4567890",
      "target": "aws::myapp-prod-db",
      "type": "dependency",
      "direction": "forward"
    }
  ],
  "groups": [
    {
      "id": "aws::vpc-0abc123def4567890",
      "type": "network.vpc",
      "members": ["aws::i-0abc123def4567890", "aws::myapp-prod-db"]
    }
  ]
}
```

#### On-premise example
```json
{
  "resources": [
    {
      "id": "arista::MXP-SW-LEAF-01",
      "name": "mxp-leaf-switch-01",
      "type": "network.switch",
      "provider": {
        "name": "arista",
        "type": "DCS-7050SX3-48YC12",
        "native_id": "MXP-SW-LEAF-01"
      },
      "properties": {
        "rack_unit_start": 12,
        "rack_unit_height": 1,
        "face": "front"
      },
      "location": {
        "datacenter": "MXP",
        "building": "01",
        "floor": "F1",
        "room": "105",
        "rack": "R01",
        "unit": 12
      },
      "status": "active"
    },
    {
      "id": "dell::MXP-SRV-R98-001",
      "name": "mxp-proxmox-host-01",
      "type": "compute.server",
      "provider": {
        "name": "dell",
        "type": "PowerEdge R750",
        "native_id": "MXP-SRV-R98-001"
      },
      "properties": {
        "rack_unit_start": 30,
        "rack_unit_height": 2,
        "face": "front",
        "service_tag": "ABC1234"
      },
      "location": {
        "datacenter": "MXP",
        "building": "01",
        "floor": "F1",
        "room": "105",
        "rack": "R98",
        "unit": 30
      },
      "status": "active"
    },
    {
      "id": "proxmox::vm-100",
      "name": "mxp-production-web-01",
      "type": "compute.vm",
      "provider": {
        "name": "proxmox",
        "type": "VirtualMachine",
        "native_id": "vm-100",
        "cluster": "proxmox-prod-cluster"
      },
      "location": {
        "datacenter": "MXP"
      },
      "status": "active"
    }
  ],
  "connections": [
    {
      "id": "conn-switch-to-server",
      "source": "arista::MXP-SW-LEAF-01",
      "target": "dell::MXP-SRV-R98-001",
      "type": "network",
      "properties": {
        "source_interface": "Ethernet48",
        "target_interface": "eno1",
        "speed_gbps": 25
      }
    },
    {
      "id": "conn-vm-runs-on-host",
      "source": "proxmox::vm-100",
      "target": "dell::MXP-SRV-R98-001",
      "type": "runs_on",
      "direction": "forward"
    }
  ],
  "groups": [
    {
      "id": "grp-mxp-dc1",
      "type": "physical.datacenter",
      "members": [],
      "children": ["grp-mxp-f1"]
    },
    {
      "id": "grp-mxp-f1",
      "type": "physical.floor",
      "members": [],
      "children": ["grp-mxp-f1-row-a"]
    },
    {
      "id": "grp-mxp-f1-row-a",
      "type": "physical.row",
      "members": [],
      "children": ["grp-mxp-r01", "grp-mxp-r98"]
    },
    {
      "id": "grp-mxp-r01",
      "type": "physical.rack",
      "members": ["arista::MXP-SW-LEAF-01"]
    },
    {
      "id": "grp-mxp-r98",
      "type": "physical.rack",
      "members": ["dell::MXP-SRV-R98-001", "proxmox::vm-100"]
    }
  ]
}
```

> [!NOTE]
> Resource types and group types in this example (e.g. `storage.database`, `network.vpc`) are illustrative. The complete type taxonomy is defined in Chapter 7 (Resource type taxonomy) and section 6.2 (Group types). Identifiers are opaque strings and producers **MAY** use structured IDs based on specific naming conventions.


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
    - `i-0abc123def4567890` (an example of AWS instance ID style)
    - `MXP-F1-R01-SW-001` (structured datacenter naming convention)
    - `urn:uuid:f81a4bcd-7efg-11h0-a765-00a0c91e5fg7` (UUID-based)

- **`type`** (string): The resource type classification using hierarchical dot notation (e.g. `compute.vm`, `network.switch`, `storage.volume`). Resource type conventions are defined in section 4.2 and the complete taxonomy is specified in Chapter 7 (Resource type taxonomy).

- **`provider`** (object): Provider attribution describing the originating platform or system for this resource. The provider object structure is defined in section 4.3 (Provider information).

> [!NOTE]
> The `provider` field is **REQUIRED** in OSIRIS v1.0 because origin and traceability are core to the interchange model. Resources without provider attribution lose critical context for validation, enrichment and correlation with source systems.
>
> For resources from unknown or offline sources, producers **MAY** use `provider.name = "unknown"` or `provider.name = "custom"` with additional context in `provider.source` or `provider.system` (see section 4.3).


### 4.1.3 Optional fields
A resource object **MAY** include the following optional fields:

- **`name`** (string): A human-readable name or label for the resource (e.g. hostname, instance name, device name). If present, `name` **SHOULD** remain stable across multiple exports of the same infrastructure when feasible.

- **`description`** (string): A free-text description of the resource's purpose, function or characteristics.

- **`properties`** (object): An object containing resource-specific properties that describe characteristics, configuration and attributes. Properties are free-form and producer-defined. Property conventions and extensibility are detailed in section 4.4 (Properties and extensions).

**Example:**
```json
{
    "properties": {
    "instance_type": "t3.medium",
    "vcpus": 2,
    "memory_gb": 4,
    "ip_addresses": {
        "private_ip": ["10.0.1.10", "10.0.1.11"],
        "public_ip": "203.0.113.10"
        },
    }
}
```

- **`extensions`** (object): Namespaced extension data for vendor-specific or domain-specific fields that extend beyond core OSIRIS semantics. Extensions **MUST** use the `osiris.<namespace>` prefix convention (e.g. `osiris.aws`, `osiris.azure`, `osiris.custom`). Extension mechanisms are defined in Chapter 8 (Extension mechanism) and section 4.4.

**Example:**
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
> The distinction between `status` and `state`, allowed values and usage semantics are defined in section 4.5 (Status and state).

- **`tags`** (object): Key-value labels used for organizational categorization, filtering or metadata annotation (e.g. environment, owner, cost-center, compliance requirements). Tags are general-purpose and cross-platform.

**Example:**
```json
  {
    "tags": {
      "environment": "production",
      "owner": "software-development-team",
      "cost-center": "sw-development"
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
  "id": "aws::i-0abc123def4567890",
  "type": "compute.vm",
  "name": "web-server-prod-01",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123def4567890",
    "region": "us-east-1",
    "account_id": "123456789012"
  },
  "properties": {
    "instance_type": "t3.medium",
    "vcpus": 2,
    "memory_gb": 4,
    "ip_addresses": {
      "private_ip": ["10.0.1.10", "10.0.1.11"],
      "public_ip": "203.0.113.10"
    }
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
  "id": "arista::MXP-SW-LEAF-01",
  "type": "network.switch",
  "name": "mxp-leaf-switch-01",
  "provider": {
    "name": "arista",
    "type": "DCS-7050SX3-48YC12",
    "native_id": "MXP-SW-LEAF-01"
  },
  "properties": {
    "management_ip": "10.130.100.12",
    "serial_number": "JPE12345678",
    "rack_unit_start": 12,
    "rack_unit_height": 1,
    "face": "front",
    "ports": 48,
    "uplink_ports": ["Ethernet49/1", "Ethernet50/1"]
  },
  "location": {
    "datacenter": "MXP",
    "building": "01",
    "floor": "F1",
    "room": "105",
    "rack": "R01",
    "unit": 12
  },
  "status": "active",
  "tags": {
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
The `type` field is a **REQUIRED** string (see section 4.1). Type values:

- **MUST NOT** contain whitespace
- **MUST** be lowercase strings
- **MUST** use dot (`.`) as the segment separator
- **MUST** contain at least two segments (see Dot notation structure below)
- **MAY** contain alphanumeric characters (`a-z`, `0-9`) within segments
- **MUST** use lowercase letters, digits and dots only (no hyphens, underscores or spaces) (e.g. `compute.vm.template` rather than `compute.vm-template`)
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
- `<namespace>` identifies the vendor, organization or domain
- `<type>` follows standard dot notation rules (lowercase, dots only, no hyphens, no spaces)

Examples of namespaced types:
- `osiris.aws.lambda.edge` (AWS Lambda @Edge - distinct from standard Lambda)
- `osiris.azure.cosmosdb` (Azure Cosmos DB with multi-model API)
- `osiris.vmware.vsan` (VMware vSAN - specific storage technology)
- `osiris.cisco.aci` (Cisco ACI fabric - vendor-specific SDN)
- `osiris.acme.widget` (organization-specific device type)

**Namespace selection:**
There is no central namespace registry for OSIRIS v1.0. To reduce collision risk:

- Well-known vendor namespaces **SHOULD** use simple vendor names (e.g. `osiris.aws`, `osiris.cisco`, `osiris.vmware`)
- Organization-specific namespaces **SHOULD** use stable identifiers such as reverse domain notation (e.g. `osiris.com.acme`, `osiris.org.example`)

Namespace consistency within a producer or organization improves interoperability and maintainability.

**Fallback behavior:**
When vendor-specific types are necessary but a close standard equivalent exists, producers **SHOULD** consider:
- Using the standard type with vendor details in `properties` or `extensions` (preferred)
- Using the namespaced type only when semantics differ significantly from standard types

**Example (standard type preferred):**
```json
{
  "id": "aws::payment-processor-function",
  "type": "compute.function.serverless",
  "name": "payment-processing-lambda",
  "provider": {
    "name": "aws",
    "type": "AWS::Lambda::Function",
    "native_id": "payment-processor-function",
    "region": "us-east-1"
  },
  "properties": {
    "runtime": "python3.11",
    "memory_mb": 512,
    "timeout_seconds": 30
  },
  "status": "active"
}
```

**Example (custom type when semantics differ):**
```json
{
  "id": "aws::cdn-edge-function",
  "type": "osiris.aws.lambda.edge",
  "name": "cloudfront-edge-auth",
  "provider": {
    "name": "aws",
    "type": "AWS::Lambda::Function",
    "native_id": "cdn-edge-function",
    "region": "us-east-1"
  },
  "properties": {
    "runtime": "nodejs18.x",
    "memory_mb": 128,
    "cloudfront_distribution": "E1ABCDEFGHIJK"
  },
  "status": "active"
}
```

> **Note:** The first example uses the standard type `compute.function.serverless` (defined in Chapter 7) because AWS Lambda is a standard serverless function. The second example uses a custom type `osiris.aws.lambda.edge` because Lambda @Edge has distinct execution semantics (runs at CloudFront edge locations) that differ from standard serverless functions.


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

**Examples:**
- AWS EC2 instance > `compute.vm` (standard type)
- AWS Lambda function > `compute.function.serverless` (standard type from Chapter 7.1)
- AWS Lambda@Edge > `osiris.aws.lambda.edge` (edge semantics differ from standard)
- Cisco Nexus leaf switch > `network.switch.leaf` or `network.switch` with `properties.role = "leaf"`
- Custom monitoring appliance > `osiris.acme.monitor` (organization-specific)


### 4.2.10 Type field validation
The `type` field value **MUST** conform to the format rules defined in this section. Structural validation (format, allowed characters, minimum segments) is enforced by JSON Schema (Appendix A).

Semantic validation (whether a type is defined in the standard taxonomy) is **OPTIONAL** and implementation-dependent. Consumers **MAY** validate against Chapter 7 but **MUST NOT** reject documents with valid but unrecognized types.

---

## 4.3 Provider information
### 4.3.1 Overview
The `provider` object is a **REQUIRED** field in every resource (see section 4.1). It describes the originating platform or system from which the resource was sourced, enabling traceability, correlation and enrichment.

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

- **`name` (string, required)**
  The provider name identifies the vendor, platform or system that manages the resource.

    `provider.name` identifies the originating system/vendor for that resource type (e.g. cloud platform for cloud resources; hardware vendor for physical assets; virtualization platform for hypervisors).

  Provider names **MUST** be:
  - Lowercase
  - Short and recognizable (vendor name or product name)
  - Consistent across exports from the same producer
  - No hyphens, underscores or spaces

**Canonical provider names:**

Producers **MUST** use canonical lowercase identifiers for well-known providers:

| Provider | OSIRIS Canonical name | Examples to avoid |
|----------|----------------------|-------------------|
| Amazon Web Services | `aws` | `AWS`, `Amazon`, `amazon-web-services` |
| Microsoft Azure | `azure` | `Azure`, `Microsoft Azure`, `msazure` |
| Google Cloud Platform | `gcp` | `GCP`, `Google`, `googlecloud` |
| Oracle Cloud | `oci` | `OCI`, `Oracle`, `oraclecloud` |
| IBM Cloud | `ibm` | `IBM`, `ibmcloud` |
| Tencent Cloud | `tencent` | `Tencent`, `tencentcloud` |
| Alibaba Cloud | `alibaba` | `Alibaba`, `alibabacloud`, `aliyun` |
| VMware | `vmware` | `VMware`, `vmwareesxi` |
| Proxmox | `proxmox` | `Proxmox`, `proxmoxve` |
| Cisco | `cisco` | `Cisco Systems`, `CiscoSystems` |
| Arista | `arista` | `Arista Networks`, `aristanetworks` |
| Juniper | `juniper` | `Juniper Networks`, `junipernetworks` |
| Dell | `dell` | `Dell EMC`, `dellemc`, `DellEMC` |
| HPE | `hpe` | `HPE`, `Hewlett Packard Enterprise`, `hewlettpackard` |
| Palo Alto | `paloalto` | `Palo Alto Networks`, `palo-alto`, `PaloAlto` |
| GitHub | `github` | `GitHub`, `git-hub` |
| GitLab | `gitlab` | `GitLab`, `git-lab` |
| Kubernetes | `kubernetes` | `k8s`, `K8s`, `Kubernetes` |
| Redis | `redis` | `Redis Labs`, `redislabs` |
| PostgreSQL | `postgresql` | `Postgres`, `postgres`, `PostgreSQL` |
| MongoDB | `mongodb` | `Mongo`, `MongoDB` |
| Kafka | `kafka` | `Apache Kafka`, `apachekafka` |

For unlisted providers, use lowercase, no hyphens, underscores or spaces (e.g. `arista`, `ciena`, `paloalto`, `checkpoint`, `f5networks`).

**Rationale:** Canonical names enable reliable provider-based filtering and matching across tools.


### 4.3.4 Optional fields
The provider object **MAY** include:

- **`native_id`** (string): The resource's native identifier in the source system. This enables correlation with vendor APIs, reconciliation across exports and deduplication.

  Examples:
  - AWS ARN: `arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def4567890`
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

#### Unknown provider example
```json
{
  "id": "unknown::device-basement-rack2-u15",
  "type": "network.switch",
  "name": "unidentified-switch",
  "provider": {
    "name": "unknown"
  },
  "properties": {
    "management_ip": "192.168.1.47",
    "discovered_via": "network-scan"
  },
  "status": "active"
}
```

#### Custom provider example
```json
{
  "id": "custom::legacy-app-server-03",
  "type": "compute.physical.server",
  "name": "legacy-payroll-server",
  "provider": {
    "name": "custom",
    "namespace": "acme-internal-cmdb",
    "system": "legacy-inventory-v2",
    "native_id": "legacy-app-server-03"
  },
  "properties": {
    "serial_number": "JDX0001",
    "install_date": "2020-03-12"
  },
  "location": {
    "datacenter": "MXP",
    "building": "01",
    "room": "server-legacy"
  },
  "status": "active"
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
  "id": "aws::i-0abc123def4567890",
  "type": "compute.vm",
  "name": "web-server-prod-01",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123def4567890",
    "region": "us-east-1",
    "account_id": "123456789012",
    "arn": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def4567890"
  },
  "status": "active"
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
#### Hyperscaler resource (AWS EC2)
```json
{
  "id": "aws::i-0abc123def4567890",
  "type": "compute.vm",
  "name": "web-server-prod-01",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123def4567890",
    "region": "us-east-1",
    "account_id": "123456789012",
    "arn": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def4567890"
  },
  "status": "active"
}
```

#### Network device (Arista switch)
```json
{
  "id": "arista::MXP-SW-LEAF-01",
  "type": "network.switch",
  "name": "mxp-leaf-switch-01",
  "provider": {
    "name": "arista",
    "type": "DCS-7050SX3-48YC12",
    "native_id": "MXP-SW-LEAF-01",
    "serial_number": "JPE19350287",
    "software_version": "EOS-4.28.3F"
  },
  "location": {
    "datacenter": "MXP",
    "building": "01",
    "floor": "F1",
    "room": "105",
    "rack": "R01",
    "unit": 12
  },
  "status": "active"
}
```

#### Virtualization platform (Proxmox VM)
```json
{
  "id": "proxmox::vm-101",
  "type": "compute.vm",
  "name": "database-server",
  "provider": {
    "name": "proxmox",
    "type": "QEMU",
    "native_id": "vm-101",
    "cluster": "pve-cluster-01",
    "node": "pve-node-02",
    "version": "8.1.3"
  },
  "properties": {
    "vmid": 101,
    "vcpus": 4,
    "memory_gb": 8
  },
  "status": "active"
}
```

#### Custom internal system
```json
{
  "id": "custom::ASSET-EU-04567",
  "type": "compute.physical.server",
  "name": "legacy-app-server",
  "provider": {
    "name": "custom",
    "namespace": "acme-corp",
    "system": "asset-management-db",
    "native_id": "ASSET-EU-04567"
  },
  "properties": {
    "purchase_date": "2018-06-15",
    "warranty_expires": "2023-06-15"
  },
  "location": {
    "datacenter": "MXP",
    "building": "01",
    "room": "legacy-storage"
  },
  "status": "active"
}
```


### 4.3.9 Provider field validation
The provider object **MUST** conform to the structural requirements defined in this section. At minimum, `provider.name` is required. All other fields are optional and producer-defined based on available source data.

Consumers **MUST** accept provider objects with unrecognized fields and preserve them when re-exporting or transforming documents.

---

## 4.4 Properties and extensions
### 4.4.1 Overview
Resources in OSIRIS use two complementary mechanisms to represent data beyond the standard fields defined in section 4.1:

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

**Example Hyperscaler resource (AWS EC2):**
```json
{
  "properties": {
    "instance_type": "t3.medium",
    "memory_gb": 8,
    "cpu_cores": 4,
    "ip_addresses": {
      "private_ip": ["10.0.1.10", "10.0.1.11"],
      "public_ip": "203.0.113.10"
    },
    "storage": {
      "disk_size_gb": 100,
      "disk_type": "gp3"
    }
  }
}
```

Properties **MAY** contain primitive values, arrays and nested objects of arbitrary depth.


### 4.4.3 Naming conventions
Property keys **SHOULD** follow these conventions:

- Lowercase with underscores for multi-word keys (e.g. `instance_type`, `private_ip`, `disk_size_gb`)
  - Underscores enable direct property access in most programming languages (unlike hyphens which are operators)
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

**Generic example:**
```json
{
  "extensions": {
    "osiris.aws": { "security_groups": ["sg-0abc123"] },
    "osiris.acme": { "owner_team": "software-development" }
  }
}
```

#### Complete resource example
```json
{
  "id": "aws::i-0abc123def4567890",
  "type": "compute.vm",
  "name": "web-server-prod-01",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123def4567890",
    "region": "us-east-1",
    "account_id": "123456789012"
  },
  "properties": {
    "instance_type": "t3.medium",
    "memory_gb": 4,
    "ip_addresses": {
        "private_ip": ["10.0.1.10", "10.0.1.11"],
        "public_ip": "203.0.113.10"
    }
  },
  "extensions": {
    "osiris.aws": {
      "security_groups": [
        {
          "id": "sg-0abc123",
          "name": "web-tier"
        }
      ],
      "iam_role": "arn:aws:iam::123456789012:role/web-server-role",
      "placement": {
        "availability_zone": "us-east-1a",
        "tenancy": "default"
      }
    },
    "osiris.terraform": {
      "workspace": "production",
      "module": "web_servers",
      "resource_address": "module.web_servers.aws_instance.main[0]"
    },
    "osiris.acme": {
      "owner_team": "software-development",
      "cost_center": "informationtechnology",
      "compliance_tags": {
        "pci_scope": false,
        "data_classification": "internal"
      }
    }
  },
  "status": "active"
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


#### Properties vs extensions guidance
Use `properties` for attributes intrinsic to the resource (configuration, capacity, addressing).

Use `extensions` for contextual metadata specific to a vendor, tool or organization:
- **Vendor extensions** (`osiris.aws`, `osiris.cisco`): Provider-native structures not in core OSIRIS model
- **Tool extensions** (`osiris.terraform`, `osiris.ansible`): Automation/management tool metadata
- **Organization extensions** (`osiris.acme`): Internal tracking, ownership, compliance data

**Example decision:**
- `properties.instance_type` - intrinsic AWS attribute
- `extensions.osiris.aws.iam_role` - AWS-specific IAM construct
- `extensions.osiris.acme.owner_team` - organization-specific tracking


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
  "id": "aws::i-0abc123def4567890",
  "type": "compute.vm",
  "name": "web-server-prod-01",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123def4567890",
    "region": "us-east-1",
    "account_id": "123456789012",
    "arn": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def4567890"
  },
  "properties": {
    "instance_type": "t3.large",
    "vcpus": 2,
    "memory_gb": 8,
    "ip_addresses": {
      "private_ip": ["10.0.1.10", "10.0.1.11"],
      "public_ip": "203.0.113.10"
    },
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
  },
  "status": "active"
}
```

#### Network device with properties + vendor and org extensions
```json
{
  "id": "arista::MXP-SW-LEAF-01",
  "type": "network.switch.leaf",
  "name": "mxp-leaf-switch-01",
  "provider": {
    "name": "arista",
    "type": "DCS-7050SX3-48YC12",
    "native_id": "MXP-SW-LEAF-01",
    "serial_number": "JPE19350123",
    "software_version": "EOS-4.28.3F"
  },
  "properties": {
    "management_ip": "10.130.100.101",
    "port_count": 48,
    "uplink_ports": ["Ethernet49/1", "Ethernet50/1"],
    "rack_unit_start": 42,
    "rack_unit_height": 1,
    "face": "front"
  },
  "location": {
    "datacenter": "MXP",
    "building": "01",
    "floor": "F1",
    "room": "105",
    "rack": "R01",
    "unit": 42
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
      "maintenance_window": "sunday-02:00-04:00",
      "owner_team": "network-infrastructure"
    }
  },
  "status": "active"
}
```

#### Kubernetes pod with platform-native labels/annotations in extensions
```json
{
  "id": "k8s::nginx-7d8f9c-abcde",
  "type": "compute.container",
  "name": "nginx-7d8f9c-abcde",
  "provider": {
    "name": "k8s",
    "type": "Pod",
    "native_id": "nginx-7d8f9c-abcde",
    "cluster": "prod-cluster-1",
    "namespace": "production"
  },
  "properties": {
    "image": "nginx:1.21",
    "restart_count": 0,
    "node": "worker-03",
    "phase": "Running"
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
  },
  "status": "active"
}
```

#### Custom resource with organization extensions (namespaced type)
```json
{
  "id": "custom::monitor-01",
  "type": "osiris.acme.apm.monitor",
  "name": "apm-monitor-mxp",
  "provider": {
    "name": "custom",
    "namespace": "acme-monitoring",
    "system": "observability-platform",
    "native_id": "monitor-01"
  },
  "properties": {
    "monitoring_url": "https://apm.acme.com/monitor/01",
    "health": "healthy",
    "targets": ["web-01", "web-02", "api-01"]
  },
  "extensions": {
    "osiris.acme": {
      "owner_team": "sre-observability",
      "cost_center": "informationtechnology",
      "compliance_required": true
    }
  },
  "status": "active"
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
#### Hyperscaler (AWS) VM with status and state
```json
{
  "id": "aws::i-0abc123def4567890",
  "type": "compute.vm",
  "name": "web-server-prod-01",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123def4567890",
    "region": "us-east-1",
    "account_id": "123456789012"
  },
  "status": "active",
  "state": "running",
  "properties": {
    "instance_type": "t3.medium",
    "ip_addresses": {
      "private_ip": "10.0.1.10",
      "public_ip": "203.0.113.50"
    }
  }
}
```

#### Hyperscaler (Azure) Stopped VM (inactive status)
```json
{
  "id": "azure::staging-app-02",
  "type": "compute.vm",
  "name": "staging-app-server",
  "provider": {
    "name": "azure",
    "type": "Microsoft.Compute/virtualMachines",
    "native_id": "staging-app-02",
    "region": "eastus",
    "subscription_id": "12345678-1234-1234-1234-123456789012",
    "resource_id": "/subscriptions/12345678-1234-1234-1234-123456789012/resourceGroups/staging/providers/Microsoft.Compute/virtualMachines/staging-app-02"
  },
  "status": "inactive",
  "state": "stopped",
  "properties": {
    "vm_size": "Standard_B2s",
    "os_type": "Linux",
    "ip_addresses": {
      "private_ip": "10.1.0.20"
    }
  }
}
```

#### Hyperscaler (AWS) Terminated VM (retired status)
```json
{
  "id": "aws::i-0xyz789abc123def",
  "type": "compute.vm",
  "name": "legacy-web-server",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0xyz789abc123def",
    "region": "us-west-2",
    "account_id": "123456789012"
  },
  "status": "retired",
  "state": "terminated",
  "properties": {
    "instance_type": "t2.micro",
    "termination_time": "2025-12-15T10:30:00Z"
  }
}
```

#### On-premise Network switch with degraded status
```json
{
  "id": "arista::MXP-SW-LEAF-01",
  "type": "network.switch.leaf",
  "name": "mxp-leaf-switch-01",
  "provider": {
    "name": "arista",
    "type": "DCS-7050SX3-48YC12",
    "native_id": "MXP-SW-LEAF-01",
    "serial_number": "JPE19350287",
    "software_version": "EOS-4.28.3F"
  },
  "status": "degraded",
  "state": "fan-failure-detected",
  "properties": {
    "management_ip": "10.130.100.101",
    "port_count": 48,
    "uplink_ports": ["Ethernet49/1", "Ethernet50/1"]
  },
  "location": {
    "datacenter": "MXP",
    "building": "01",
    "floor": "F1",
    "room": "105",
    "rack": "R01",
    "unit": 42
  }
}
```

#### Kubernetes pod with vendor-specific state
```json
{
  "id": "k8s::nginx-deployment-abc123",
  "type": "compute.container",
  "name": "nginx-deployment-abc123",
  "provider": {
    "name": "k8s",
    "type": "Pod",
    "native_id": "nginx-deployment-abc123",
    "cluster": "prod-cluster-1",
    "namespace": "production"
  },
  "status": "active",
  "state": "Running",
  "properties": {
    "image": "nginx:1.21",
    "node": "worker-node-03",
    "restart_count": 0
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

#### Resource with unknown status (source unavailable)
```json
{
  "id": "custom::DB-001",
  "type": "storage.database",
  "name": "legacy-database",
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

---

## 5.1 Connection object structure
### 5.1.1 Overview
A connection is a JSON object that links two resources within the same OSIRIS document. Connections are stored in the `topology.connections` array (see chapter 3, section 3.4).


### 5.1.2 Required fields
Every connection **MUST** have a unique identifier within an OSIRIS document via the `id` field.

Connection identifiers are **document-scoped**: they **MUST** be unique within a single OSIRIS document but need not be globally unique across other documents or systems.

Unlike resources which represent vendor-managed objects with native identifiers, connections represent relationships discovered or inferred by parsers. Therefore, connection IDs do not reference external vendor systems and do not use the `provider::native-id` namespace pattern.

Every connection **MUST** include:

- **`id`** (string): Unique identifier for this connection within the document
- **`source`** (string): Resource ID of the source endpoint
- **`target`** (string): Resource ID of the target endpoint
- **`type`** (string): Connection type using dot notation (see section 5.2)

**ID Construction:**
Producers **SHOULD** construct connection IDs using one of these strategies:

1. **Sequential numbering:**
```json
   "id": "conn-001"
   "id": "conn-002"
   "id": "conn-003"
```

2. **Descriptive naming (recommended):**
```json
   "id": "conn-aws-vpc-subnet-001"
   "id": "conn-aws-web-db-dependency"
   "id": "conn-mxp-leaf-spine-uplink"
   "id": "conn-mxp-app-to-cache"
```

3. **Type-prefixed descriptive:**
```json
   "id": "network-vpc-to-subnet-001"
   "id": "dependency-web-to-db"
   "id": "contains-subnet-vm-001"
```

4. **Hash-based (for reproducibility):**
```json
   "id": "conn-a1b2c3d4e5f6"
```

5. **UUID (for guaranteed uniqueness on large infrastructures):**
```json
   "id": "550e8400-e29b-41d4-a716-446655440000"
```


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


### 5.1.4 Minimal connection examples
Following examples show how OSIRIS can at minimum allow you to represent objects within the infrastructure.

Hyperscaler (AWS) EC2 dependency to Postgres DB:
```json
{
  "id": "conn-aws-web-db-dependency",
  "source": "aws::i-0abc123def4567890",
  "target": "aws::db-prod-postgres-01",
  "type": "dependency",
  "direction": "forward"
}
```

**Additional examples:**

Hyperscaler (AWS) network containment:
```json
{
  "id": "conn-aws-ec2-subnet-001",
  "source": "aws::subnet-0abc123",
  "target": "aws::i-0abc123def4567890",
  "type": "contains",
  "direction": "forward"
}
```

Multi-vendor hybrid network connection:
```json
{
  "id": "conn-aws-vpc-subnet-001",
  "source": "aws::vpc-0abc123",
  "target": "vmware::vm-1234",
  "type": "network",
  "direction": "bidirectional"
}
```

On-premise Physical network link between datacenter switches:
```json
{
  "id": "conn-mxp-leaf-spine-uplink-r01-u42",
  "source": "arista::MXP-SW-LEAF-01",
  "target": "arista::MXP-SW-SPINE-01",
  "type": "network",
  "direction": "bidirectional",
  "properties": {
    "link_type": "uplink",
    "protocol": "ethernet",
    "bandwidth_gbps": 100,
    "mtu": 9214,
    "vlan_mode": "trunk",
    "interfaces": {
      "source": {
        "port": "Ethernet49/1",
        "port_type": "uplink",
        "speed_gbps": 100
      },
      "target": {
        "port": "Ethernet1/1",
        "port_type": "downlink",
        "speed_gbps": 100
      }
    },
    "management": {
      "source_ip": "10.130.100.101",
      "target_ip": "10.130.100.1"
    }
  },
  "status": "active",
  "extensions": {
    "osiris.arista": {
      "lacp_mode": "active",
      "port_channel_id": "Port-Channel49"
    }
  }
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

---

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
- `osiris.com.acme.workflow`


### 5.2.5 Unknown connection types
Consumers **MUST** accept connections with unknown types. When encountering unknown types, consumers **SHOULD**:
- Preserve the connection in the graph
- Treat it as a generic relationship
- Display the type string verbatim (useful for debugging)
- Traverse it normally

---

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

---

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
#### Hyperscaler (AWS) network connection (bidirectional)
```json
{
  "id": "conn-aws-web-db-network",
  "source": "aws::i-0abc123def4567890",
  "target": "aws::db-prod-postgres-01",
  "type": "network",
  "direction": "bidirectional",
  "status": "active",
  "properties": {
    "protocol": "tcp",
    "ports": [5432],
    "vlan_id": 100,
    "interfaces": {
      "source": {
        "ip": "10.0.1.10",
        "port": "eth0"
      },
      "target": {
        "ip": "10.0.2.20",
        "port": "eth0"
      }
    }
  },
  "tags": {
    "environment": "production",
    "application": "web-tier"
  }
}
```

#### Application dependency (forward)
```json
{
  "id": "conn-app-api-auth-dependency",
  "source": "k8s::api-gateway-7d8f9c-abcde",
  "target": "k8s::auth-service-5a6b7c-fghij",
  "type": "dependency.api",
  "direction": "forward",
  "status": "active",
  "properties": {
    "required": true,
    "api_version": "v2",
    "timeout_seconds": 5,
    "retry_policy": {
      "max_retries": 3,
      "backoff_seconds": 2
    }
  },
  "tags": {
    "criticality": "high"
  }
}
```

#### On-premise physical copper ethernet link (bidirectional)
```json
{
  "id": "conn-mxp-sw-to-srv-042-port48",
  "source": "arista::MXP-SW-LEAF-01",
  "target": "dell::MXP-SRV-042",
  "type": "physical.ethernet",
  "direction": "bidirectional",
  "status": "active",
  "properties": {
    "link_type": "server-access",
    "speed_gbps": 10,
    "cable": {
      "type": "cat6a",
      "length_meters": 5
    },
    "interfaces": {
      "source": {
        "port": "Ethernet48",
        "port_type": "access",
        "transceiver": {
          "vendor": "arista",
          "model": "SFP-10G-T",
          "form_factor": "sfp+",
          "serial_number": "SFP10GT-AR12345"
        }
      },
      "target": {
        "port": "eno1",
        "port_type": "onboard",
        "transceiver": {
          "vendor": "broadcom",
          "model": "57412 Quad Port 10GbE",
          "form_factor": "base-t",
          "serial_number": "BCM57412-DL98765"
        }
      }
    }
  },
  "tags": {
    "rack": "R01",
    "cable_id": "MXP-CAB-048-042"
  }
}
```

#### On-premise physical fiber ethernet lin (bidirectional)
```json
{
  "id": "conn-mxp-leaf-spine-uplink-r01",
  "source": "arista::MXP-SW-LEAF-01",
  "target": "arista::MXP-SW-SPINE-01",
  "type": "physical.fiber",
  "direction": "bidirectional",
  "status": "active",
  "properties": {
    "link_type": "uplink",
    "speed_gbps": 100,
    "cable": {
      "type": "om4",
      "length_meters": 30
    },
    "interfaces": {
      "source": {
        "port": "Ethernet49/1",
        "port_type": "uplink",
        "admin_status": "up",
        "oper_status": "up",
        "transceiver": {
          "vendor": "arista",
          "model": "100GBASE-SR4",
          "form_factor": "qsfp28",
          "serial_number": "QSFP100SR4-AR12345",
          "wavelength_nm": 850,
          "digital_optical_monitoring": {
            "module_temperature_c": 42.1,
            "supply_voltage_v": 3.28,
            "lanes": [
              {
                "lane": 1,
                "tx_power_dbm": -1.3,
                "rx_power_dbm": -2.1,
                "tx_bias_ma": 44.2
              },
              {
                "lane": 2,
                "tx_power_dbm": -1.2,
                "rx_power_dbm": -2.3,
                "tx_bias_ma": 44.0
              },
              {
                "lane": 3,
                "tx_power_dbm": -1.4,
                "rx_power_dbm": -2.2,
                "tx_bias_ma": 44.4
              },
              {
                "lane": 4,
                "tx_power_dbm": -1.1,
                "rx_power_dbm": -2.0,
                "tx_bias_ma": 43.8
              }
            ]
          }
        }
      },
      "target": {
        "port": "Ethernet1/1",
        "port_type": "downlink",
        "admin_status": "up",
        "oper_status": "up",
        "transceiver": {
          "vendor": "arista",
          "model": "100GBASE-SR4",
          "form_factor": "qsfp28",
          "serial_number": "QSFP100SR4-AR67890",
          "wavelength_nm": 850,
          "digital_optical_monitoring": {
            "module_temperature_c": 39.7,
            "supply_voltage_v": 3.30,
            "lanes": [
              {
                "lane": 1,
                "tx_power_dbm": -1.0,
                "rx_power_dbm": -2.4,
                "tx_bias_ma": 41.9
              },
              {
                "lane": 2,
                "tx_power_dbm": -1.1,
                "rx_power_dbm": -2.5,
                "tx_bias_ma": 42.1
              },
              {
                "lane": 3,
                "tx_power_dbm": -1.2,
                "rx_power_dbm": -2.6,
                "tx_bias_ma": 42.0
              },
              {
                "lane": 4,
                "tx_power_dbm": -1.0,
                "rx_power_dbm": -2.3,
                "tx_bias_ma": 41.8
              }
            ]
          }
        }
      }
    }
  },
  "tags": {
    "rack": "R01",
    "layer": "leaf-spine"
  },
  "extensions": {
    "osiris.arista": {
      "lacp_mode": "active",
      "port_channel_id": "Port-Channel49"
    }
  }
}
```

#### Hyperscaler (AWS) vendor-specific connection type (VPC peering)
```json
{
  "id": "conn-aws-vpc-peering-prod-dev",
  "source": "aws::vpc-0abc123def456",
  "target": "aws::vpc-0def789ghi012",
  "type": "osiris.aws.vpc_peering",
  "direction": "bidirectional",
  "status": "active",
  "properties": {
    "peering_status": "active",
    "cidr_blocks": {
      "source": ["10.0.0.0/16"],
      "target": ["10.1.0.0/16"]
    },
    "dns_resolution": {
      "source_to_target": true,
      "target_to_source": true
    }
  },
  "extensions": {
    "osiris.aws": {
      "peering_connection_id": "pcx-0abc123def4567890",
      "accepter_vpc_info": {
        "owner_id": "123456789012",
        "region": "us-east-1"
      },
      "requester_vpc_info": {
        "owner_id": "123456789012",
        "region": "us-east-1"
      }
    }
  },
  "tags": {
    "purpose": "cross-environment-access",
    "cost_center": "networking"
  }
}
```

### 5.4.5 Validation
If present, `properties` and `extensions` **MUST** be JSON objects. `tags` (if present) **MUST** be a JSON object mapping strings to strings.

Consumers **MUST** accept connections with unrecognized property keys or extension namespaces and **MUST NOT** reject documents solely due to unknown connection metadata.

---

# 6. Group model
## Overview
Groups in OSIRIS provide a mechanism to organize and classify resources into meaningful collections without altering the underlying topology graph. Groups are primarily used for:

- **Aggregation:** Collecting resources by shared attributes (e.g. "all production workloads", "all devices in on-premise Data Center DC-MXP", "all resources owned by Software Development Team")
- **Presentation and navigation:** Building hierarchical trees, filtered views and organized inventories
- **Boundary modeling:** Representing organizational, physical, logical, network or security boundaries

Groups complement resources (nodes) and connections (edges) by providing *structure and categorization*. Groups do not create topological edges; they provide categorization and hierarchical organization. While connections represent explicit relationships between individual resources, groups provide organizational context and multi-dimensional classification.

This chapter defines:
- Group object structure and required fields
- Standard group types and their semantics
- Membership models including hierarchical nesting
- When to use groups versus `contains` connections
- Group metadata via properties, extensions, tags and status/state

---

## 6.1 Group object structure
### 6.1.1 Overview
A group is a JSON object stored in the `topology.groups` array (see chapter 3, section 3.4). A group may reference:
- **Resources** via the `members` array
- **Other groups** via the `children` array (hierarchical nesting)

Groups enable multi-dimensional classification where a single resource can belong to multiple groups simultaneously (e.g. a VM can be in both "production environment" and "web tier" groups).


### 6.1.2 Required fields
Every group **MUST** include:

- **`id`** (string): Unique identifier for this group within the document
- **`type`** (string): Group type using dot notation (see section 6.2)

**ID Construction:**

Group identifiers are **document-scoped**: they **MUST** be unique within a single OSIRIS document but need not be globally unique across other documents or systems.

Groups do not have vendor-assigned "native IDs" since they represent logical or physical collections defined by parsers or infrastructure operators. Producers **SHOULD** construct group IDs using one of these strategies:

1. **Sequential numbering:**
```json
   "id": "group-001"
   "id": "group-002"
```

2. **Descriptive naming (recommended):**
```json
   "id": "group-aws-vpc-production"
   "id": "group-mxp-datacenter"
   "id": "group-web-tier"
   "id": "group-k8s-prod-cluster"
```

3. **Type-prefixed descriptive:**
```json
   "id": "environment-production"
   "id": "datacenter-mxp"
   "id": "vnet-azure-prod"
```

4. **Hash-based (for reproducibility):**
```json
   "id": "group-a1b2c3d4e5f6"
```

5. **UUID (for guaranteed uniqueness on large infrastructures):**
```json
   "id": "550e8400-e29b-41d4-a716-446655440000"
```

Descriptive naming (strategies 2-3) is **RECOMMENDED** as it improves human readability and debugging while maintaining uniqueness within the document scope.

**Note:** Unlike resource IDs which use `provider::native-id` format to reference vendor systems, group IDs do not require namespace prefixes since groups represent logical or physical collections rather than vendor-managed objects.


### 6.1.3 Optional fields
Groups **MAY** include:

- **`name`** (string): Human-readable label for the group
- **`description`** (string): Free-text description of the group's purpose or contents
- **`members`** (array of strings): Resource IDs that belong to this group (see section 6.3.1)
- **`children`** (array of strings): Group IDs nested under this group (see section 6.3.3)
- **`status`** (string): Normalized operational status (`active`|`inactive`|`degraded`|`retired`|`unknown`)
- **`state`** (string): Vendor-specific granular state
- **`properties`** (object): Portable group attributes
- **`extensions`** (object): Vendor-specific metadata using `osiris.<namespace>` keys
- **`tags`** (object): Key-value labels (`string` > `string`)


### 6.1.4 Minimal group example
Logical environment grouping:
```json
{
  "id": "group-aws-production-web-app",
  "type": "logical.environment",
  "name": "Production environment",
  "members": [
    "aws::i-0abc123def4567890",
    "aws::db-prod-postgres-01",
    "aws::vpc-0abc123"
  ]
}
```

**Additional examples:**

Physical datacenter grouping:
```json
{
  "id": "group-mxp-datacenter",
  "type": "physical.datacenter",
  "name": "Milan Datacenter",
  "members": [
    "arista::MXP-SW-LEAF-01",
    "arista::MXP-SW-SPINE-01",
    "dell::MXP-SRV-042"
  ]
}
```

Hyperscaler (AWS) Network VPC grouping:
```json
{
  "id": "group-aws-vpc-prod",
  "type": "network.vpc",
  "name": "Production VPC",
  "members": [
    "aws::vpc-0abc123",
    "aws::subnet-0abc123",
    "aws::i-0abc123def4567890"
  ]
}
```


### 6.1.5 Referential integrity
Each entry in `members` **MUST** reference a resource ID present in `topology.resources`.
Each entry in `children` **MUST** reference a group ID present in `topology.groups`.

A group **MUST NOT**:
- List itself in `children` (direct self-reference)
- Create cycles in the group hierarchy (indirect self-reference through descendants)

Producers **SHOULD** validate referential integrity and cycle detection before export.

Consumers **MUST** handle invalid references gracefully by:
- Emitting warnings for broken references
- Skipping invalid members or children entries
- Rejecting the document in strict validation mode (optional)


### 6.1.6 Group identity and stability
Group IDs **MUST** be unique within the document. Group IDs **SHOULD** remain stable across exports when feasible to enable:
- Change detection when comparing snapshots
- Correlation across multiple topology exports
- Reliable external references from consuming systems

Producers **MAY** use deterministic ID generation based on:
- Group type
- Group name
- Stable contextual identifiers (e.g. datacenter code, environment name, VPC ID)

**Example deterministic ID patterns:**
- On-prem site: grp-<site_code> (e.g. grp-MXP)
- Physical datacenter: `grp-MXP` (datacenter code)
- Physical rack: `grp-MXP-F1-R01` (datacenter-floor-rack)
- Logical environment: `grp-prod` or `grp-env-production`

- Hyperscalers and Cloud provider netwrok groups: `grp-<provider.name>-net-<id>`
  - AWS VPC: `grp-aws-net-vpc-0abc123def4567890`
  - Azure VNet: `grp-az-net-vnet-prod-weu-01`
  - GCP VPC: `grp-gcp-net-vpc-prod-eu-01`
  - DigitalOcean VPC: `grp-digitalocean-net-vpc-<uuid>`
  - Leaseweb Private Network: `grp-leaseweb-net-private-<id>`

> [!NOTE]
> Providers use different names for private networking constructs (e.g. VPC/VNet/Networks/Private Network). OSIRIS treats these uniformly as “Hyperscalers and Cloud provider network groups”; the concrete provider object IDs remain provider-specific.


> [!NOTE]
> Some producers embed a short provider code in deterministic IDs for readability (e.g. `grp-aws-net-...`).
> OSIRIS does not require this. If used, the provider code SHOULD match `provider.name` where applicable.

**Recommended provider codes (non-normative):**
- `aws` (Amazon Web Services)
- `az` (Microsoft Azure)
- `gcp` (Google Cloud Platform)
- `oci` (Oracle Cloud Infrastructure)
- `ibm` (IBM Cloud)
- `tc` (Tencent Cloud)
- `ali` (Alibaba Cloud)

Producers **MAY** use additional provider codes as needed. Consumers **MUST NOT** treat this list as exhaustive and **MUST** accept unknown provider codes.


### 6.1.7 Field extensibility
Consumers **MUST** accept groups with unrecognized fields for forward compatibility.

Producers **SHOULD** place non-standard data under `properties` or `extensions` rather than introducing new top-level fields in OSIRIS v1.0.

---

## 6.2 Group types
### 6.2.1 Overview
The `type` field classifies what the group represents and how it should be interpreted by consumers. Group types use hierarchical dot notation to organize categories and subcategories.

### 6.2.2 Type format rules
Group type values:

- **MUST NOT** contain whitespace
- **MUST** be lowercase
- **MUST** use dot (`.`) as the segment separator
- **MUST** contain at least two segments
- **MUST NOT** start or end with a dot
- **MUST NOT** contain consecutive dots (`..`)

**Examples of valid group types:**
- `logical.environment`
- `physical.datacenter`
- `network.vpc`
- `security.zone`
- `org.team`

**Examples of invalid group types:**
- `Environment` (uppercase)
- `datacenter` (single segment)
- `data center` (space)
- `.datacenter` (starts with dot)
- `network..vpc` (consecutive dots)


### 6.2.3 Standard group types
OSIRIS defines the following standard group type families. Producers **SHOULD** use these standard types when applicable rather than creating custom types for common infrastructure concepts.

#### logical.*
**Purpose:** Conceptual groupings that reflect application architecture, deployment organization or operational intent.

**Recommended types:**
- **`logical.environment`**: Deployment environments (e.g. production, staging, development, QA)
- **`logical.application`**: Application or system groupings (e.g. billing-system, crm, identity-platform)
- **`logical.service`**: Individual services or microservices (e.g. api-gateway, auth-service, payments-processor)
- **`logical.tier`**: Architectural tiers (e.g. web, application, database, cache)
- **`logical.workload`**: Workload classifications (e.g. batch-processing, real-time, analytics)

**Example use cases:**
- Grouping all production resources for filtering and access control
- Organizing resources by application for cost allocation
- Separating frontend, backend and database tiers in diagrams

#### physical.*
**Purpose:** Location-based and facility-based grouping reflecting physical infrastructure organization.

**Recommended types:**
- **`physical.datacenter`**: Data center facilities
- **`physical.building`**: Buildings within a campus or multi-building facility
- **`physical.floor`**: Floors within a building
- **`physical.room`**: Rooms or caged areas
- **`physical.rack`**: Equipment racks
- **`physical.pod`**: Modular datacenter pods or availability zones

**Example use cases:**
- Modeling datacenter physical topology
- Organizing resources by geographic location
- Planning capacity and power distribution by rack

#### network.*
**Purpose:** Network segmentation and network construct organization.

**Recommended types:**
- **`network.vpc`** or **`network.vnet`**: Virtual private clouds or virtual networks
- **`network.subnet`**: Network subnets or segments
- **`network.vlan`**: VLAN groupings
- **`network.segment`**: Generic network segments or security zones
- **`network.asn`**: Autonomous System Number groupings (for BGP topology)

> [!NOTE]
> Provider-specific terminology (e.g. AWS VPC vs Azure VNet) can be expressed as:
> - Standard type `network.vpc` with provider details in `properties` or `extensions` (recommended)
> - Namespaced type `osiris.aws.vpc` or `osiris.azure.vnet` (when provider-specific semantics are required)

**Common properties for network.asn groups:**
When using `network.asn` groups, producers **SHOULD** include these properties when available:

- `asn_number` (integer): The AS number (e.g. 3356)
- `organization_name` (string): Legal name of the ASN organization
- `domain` (string): Organization's domain name
- `asn_type` (string): ASN classification (`ISP`, `Hosting`, `Education`, `Government`, `Business`)
- `country_code` (string): ISO 3166-1 alpha-2 country code from WHOIS records
- `registry` (string): Regional Internet Registry (`ARIN`, `RIPE`, `APNIC`, `LACNIC`, `AFRINIC`)
- `cidr_blocks` (array of strings): CIDR blocks announced by this ASN
- `peering_policy` (string): BGP peering policy (`open`, `selective`, `restrictive`, `no`)

**Example use cases:**
- Organizing resources by network boundary
- Modeling network segmentation for security analysis
- Grouping resources within the same broadcast domain
- Documenting BGP topology and AS path relationships

#### security.*
**Purpose:** Security boundaries, trust zones and compliance groupings.

**Recommended types:**
- **`security.zone`**: Security or trust zones (e.g. DMZ, trusted, untrusted, quarantine)
- **`security.trust-boundary`**: Trust domain boundaries
- **`security.compliance-scope`**: Compliance boundary groupings (e.g. PCI-DSS scope, HIPAA scope)

**Example use cases:**
- Modeling zero-trust network architecture
- Documenting compliance boundaries for audit
- Visualizing security zone transitions

#### org.*
**Purpose:** Organizational ownership, responsibility and business structure groupings.

**Recommended types:**
- **`org.team`**: Team ownership (e.g. platform-team, sme-team, security-team)
- **`org.owner`**: Individual or group ownership
- **`org.cost-center`**: Financial cost center allocations
- **`org.business-unit`**: Business unit or department groupings
- **`org.project`**: Project-based groupings

**Example use cases:**
- Allocating infrastructure costs by team or cost center
- Identifying ownership for incident response
- Organizing resources by business unit for governance


### 6.2.4 Vendor-specific and custom group types
Vendor-specific or organization-specific group types **SHOULD** use the namespaced pattern:
```
osiris.<namespace>.<type>
```

**Examples:**
- `osiris.aws.account` (AWS account boundary)
- `osiris.azure.subscription` (Azure subscription)
- `osiris.gcp.project` (GCP project)
- `osiris.k8s.namespace` (Kubernetes namespace)
- `osiris.vmware.cluster` (VMware vSphere cluster)
- `osiris.com.acme.product-line` (organization-specific product grouping)

**Namespace selection guidance:**
- Well-known vendors **MAY** use simple namespaces (e.g. `osiris.aws`, `osiris.arista`)
- Organizations **SHOULD** use stable identifiers such as reverse domain notation without hyphens, underscores or spaces (e.g. `osiris.com.acme`, `osiris.org.example`)


### 6.2.5 Unknown group types
Consumers **MUST** accept groups with unknown or unrecognized types. When encountering unknown group types, consumers **SHOULD**:

- Preserve the group object when re-exporting or transforming documents
- Display the type string verbatim for debugging and manual interpretation
- Allow normal membership resolution and hierarchy traversal
- Treat the group as a generic collection for filtering and presentation

Consumers **MUST NOT** reject documents containing unknown group types.

---

## 6.3 Membership
### 6.3.1 Members array
The `members` field is an array of resource IDs:
```json
"members": [
  "aws::i-0abc123def4567890",
  "aws::db-prod-postgres-01",
  "aws::lb-prod-web-001"
]
```

All member IDs **MUST** reference valid resource IDs defined elsewhere in the document. Member IDs **SHOULD** use the same format as resource IDs (typically `provider::native-id` for resources from vendor systems).

**Example in context:**
```json
{
  "id": "group-aws-production-web-app",
  "type": "logical.application",
  "name": "Production Web APP Lollipop of Acme Corp",
  "members": [
    "aws::i-0abc123def4567890",
    "aws::i-0def456ghi7890ab",
    "aws::lb-prod-web-001",
    "aws::db-prod-postgres-01"
  ],
  "properties": {
    "tier": "web",
    "availability_zones": ["us-east-1a", "us-east-1b"]
  }
}
```

**Rules:**
- `members` entries **MUST** be strings
- Each entry **MUST** reference a valid resource ID from `topology.resources`
- Producers **SHOULD** avoid duplicate entries
- Consumers **SHOULD** treat duplicate entries as a single membership
- Order of the `members` array **MUST NOT** be considered semantically meaningful
- Empty `members` array is valid (group may have children but no direct members)
- Omitting `members` field is equivalent to an empty array

**Membership semantics:**
Resources listed in `members` are considered **direct members** of the group. Membership does not imply any particular relationship between resources within the group (e.g. resources in the same group are not automatically connected).


### 6.3.2 Multiple memberships
A resource **MAY** belong to multiple groups simultaneously. This is expected and recommended for multi-dimensional classification.

**Example: Multi-dimensional membership**

Resource `aws::i-0abc123def4567890` can simultaneously belong to:
- `group-aws-production-web-app` (logical.environment)
- `group-web-tier` (logical.tier)
- `group-billing-application` (logical.application)
- `group-team-software-development` (org.team)
- `group-mxp-rack-r01` (physical.rack)

This enables flexible filtering and navigation:
- "Show all production resources" > Filter by `group-aws-production-web-app`
- "Show all web tier resources" > Filter by `group-web-tier`
- "Show all resources owned by software development team" > Filter by `group-team-software-development`
- "Show all resources in rack R01" > Filter by `group-mxp-rack-r01`

**Complete example:**
```json
{
  "resources": [
    {
      "id": "aws::i-0abc123def4567890",
      "type": "compute.vm",
      "name": "billing-api-prod-01"
    }
  ],
  "groups": [
    {
      "id": "group-aws-production-web-app",
      "type": "logical.environment",
      "name": "Production Environment",
      "members": ["aws::i-0abc123def4567890", "..."]
    },
    {
      "id": "group-web-tier",
      "type": "logical.tier",
      "name": "Web Tier",
      "members": ["aws::i-0abc123def4567890", "..."]
    },
    {
      "id": "group-billing-application",
      "type": "logical.application",
      "name": "Billing Application",
      "members": ["aws::i-0abc123def4567890", "..."]
    },
    {
      "id": "group-team-software-development",
      "type": "org.team",
      "name": "Software Development Team",
      "members": ["aws::i-0abc123def4567890", "..."]
    },
    {
      "id": "group-mxp-rack-r01",
      "type": "physical.rack",
      "name": "MXP Datacenter Rack R01",
      "members": ["aws::i-0abc123def4567890", "..."]
    }
  ]
}
```


### 6.3.3 Children array (hierarchical nesting)
The `children` field is an array of group IDs that are nested under this group, enabling hierarchical organization:
```json
"children": ["group-mxp-building-01", "group-mxp-building-02"]
```

**Hierarchical nesting example:**

Complete physical datacenter hierarchy:
```json
{
  "groups": [
    {
      "id": "group-mxp-datacenter",
      "type": "physical.datacenter",
      "name": "Milan Datacenter",
      "children": [
        "group-mxp-building-01"
      ]
    },
    {
      "id": "group-mxp-building-01",
      "type": "physical.building",
      "name": "MXP Building 01",
      "children": [
        "group-mxp-floor-f1",
        "group-mxp-floor-f2"
      ]
    },
    {
      "id": "group-mxp-floor-f1",
      "type": "physical.floor",
      "name": "MXP Floor F1",
      "children": [
        "group-mxp-room-105",
        "group-mxp-room-106"
      ]
    },
    {
      "id": "group-mxp-room-105",
      "type": "physical.room",
      "name": "MXP Server Room 105",
      "children": [
        "group-mxp-rack-r01",
        "group-mxp-rack-r02"
      ]
    },
    {
      "id": "group-mxp-rack-r01",
      "type": "physical.rack",
      "name": "MXP Rack R01",
      "members": [
        "arista::MXP-SW-LEAF-01",
        "dell::MXP-SRV-042",
        "dell::MXP-SRV-043"
      ]
    }
  ]
}
```

**Hierarchy visualization:**
```
group-mxp-datacenter (physical.datacenter)
|   +-- group-mxp-building-01 (physical.building)
|       +-- group-mxp-floor-f1 (physical.floor)
|           +-- group-mxp-room-105 (physical.room)
|               +-- group-mxp-rack-r01 (physical.rack)
|                   +-- arista::MXP-SW-LEAF-01
|                   +-- dell::MXP-SRV-042
|                   +-- dell::MXP-SRV-043
|               +-- group-mxp-rack-r02 (physical.rack)
|                   +-- arista::MXP-SW-LEAF-02
|                   +-- dell::MXP-SRV-044
|           +-- group-mxp-room-106 (physical.room)
|               +-- group-mxp-rack-r03 (physical.rack)
|                   +-- arista::MXP-SW-SPINE-01
|       +-- group-mxp-floor-f2 (physical.floor)
|           +-- group-mxp-room-201 (physical.room)
|               +-- group-mxp-rack-f2-r01 (physical.rack)
|                   +-- dell::MXP-SRV-201
```

This hierarchical structure enables queries like:
- "Show all resources in datacenter MXP" > Traverse from `group-mxp-datacenter`
- "Show all resources on floor F1" > Traverse from `group-mxp-floor-f1`
- "Show all resources in rack R01" > Direct members of `group-mxp-rack-r01`

**Rules:**
- `children` entries **MUST** be strings
- Each entry **MUST** reference a valid group ID from `topology.groups`
- Producers **SHOULD** avoid duplicate entries
- Consumers **SHOULD** treat duplicate entries as a single child relationship
- Order of the `children` array **MUST NOT** be considered semantically meaningful
- Empty `children` array is valid (leaf groups with no subgroups)
- Omitting `children` field is equivalent to an empty array


### 6.3.4 Nesting semantics
Groups **MAY** form tree structures or directed acyclic graphs (DAGs):
- **Tree:** Each group has at most one parent
- **DAG:** Groups may have multiple parents (a rack might belong to both a physical room and a logical availability zone)

A group **MAY** appear as a child of multiple parent groups (DAG). Consumers computing transitive membership **SHOULD** deduplicate resource IDs in the resulting effective membership set.

Group nesting **MUST NOT** contain cycles (directly or indirectly). Producers **MUST** validate cycle-freedom before export.

**Transitive membership:**
Consumers **MAY** compute "expanded membership" (transitive closure) when useful:
```
effective_members(group) = union of members(group) and members(all_descendant_groups)
```

This enables queries like "all resources in datacenter MXP" to include resources in nested rooms and racks.

**Depth guidance:**
Producers **SHOULD** keep nesting depth reasonable for human comprehension:
- Typical datacenter: datacenter > floor > room > rack (4 levels)
- Typical cloud hierarchy (4–6 levels): tenant/account/project > region/zone > container/resource-group > network (VPC/VNet) > subnet
- Excessive depth (>6-7 levels) may reduce usability in visualization tools

---

## 6.4 Groups vs contains connections
Containment relationships can be represented either via groups or via `contains` connections (see chapter 5 section 5.2.3). Understanding when to use each mechanism is important for clear topology modeling.


### 6.4.1 Prefer groups when
Use groups when containment is primarily:
- A **view or categorization** (e.g. "all resources in VPC X", "all production resources")
- **Multi-dimensional** (resources belong to multiple "containers" such as team + environment + application)
- **Not required as an explicit graph edge** for dependency analysis or path traversal
- **Organizational or logical** rather than structural

**Examples:**
- "Production environment" > group
- "Platform team ownership" > group
- "Web tier" > group
- Resources in a VPC > group (usually)


### 6.4.2 Prefer contains connections when
Use `contains` connections when containment is:
- A **strict structural relationship** that must behave like a graph edge
- Used for **topology traversals** such as "what runs on what", "what is physically inside what"
- A **single-parent hierarchy** where each resource has exactly one container
- **Physical or architectural** rather than organizational

**Examples:**
- "Chassis contains linecard" > connection
- "Host contains VMs" > connection
- "Kubernetes node contains pods" > connection
- "Rack contains servers" > connection (may also use groups for inventory)


### 6.4.3 Avoid duplication
Producers **SHOULD** avoid representing the same containment relationship in both groups and `contains` connections unless there is a clear consumer requirement for both representations.

If duplication is necessary (e.g. both graph-based and inventory-based consumers):
- Groups should be treated as the **presentation layer** (for filtering, navigation, reporting)
- `contains` connections should be treated as the **authoritative graph edge** (for topology analysis, dependency tracing)

**Example: Hyperscaler or Cloud VPC containment**
**Using groups (recommended for most VPC scenarios):**
```json
{
  "id": "grp-aws-net-vpc-0abc123def4567890",
  "type": "network.vpc",
  "name": "Production VPC",
  "members": ["subnet-aws-web-001", "subnet-aws-db-001", "aws::i-0abc123def4567890", "vm-aws-db-001"]
}
```

**Using contains connections (if VPC is a topology node):**
```json
{
  "id": "vpc-0abc123def4567890",
  "type": "network.vpc",
  "name": "Production VPC",
  "provider": {
    "name": "aws",
    "native_id": "vpc-0abc123def4567890",
    "region": "eu-west-1",
    "account": "123456789012"
  }
}
{
  "id": "cn-vpc-0abc123def4567890-contains-subnet-web-001",
  "source": "vpc-0abc123def4567890",
  "target": "subnet-web-001",
  "type": "contains",
  "direction": "forward"
}
```

IDs shown are human-readable and derived from stable provider identifiers; hashing is optional and not required by OSIRIS.
Choose based on consumer needs: groups for inventory/presentation, connections for topology traversal.


**Example: Host contains VMs (on-prem virtualization)**
**Using groups (recommended for inventory or reporting):**
```json
{
  "id": "grp-mxp-esxi-cluster-prod",
  "type": "logical.cluster",
  "name": "MXP Production ESXi Cluster",
  "members": ["host-mxp-f1-r22-esxi-001", "host-mxp-f1-r50-esxi-002", "vm-mxp-pr-web-001", "vm-mxp-pr-db-001"],
  "tags": {
    "site": "MXP",
    "environment": "prod",
    "org": "acme"
  }
}
```

**Using contains connections (recommended when containment must be traversed as topology):**
```json
{
  "id": "host-mxp-f1-r22-esxi-001",
  "type": "compute.host",
  "name": "mxp-esxi-001",
  "provider": {
    "name": "vmware",
    "native_id": "host-mxp-12345",
    "version": "ESXi 8.0"
  }
}
```
```json
{
  "id": "vm-mxp-pr-web-001",
  "type": "compute.vm",
  "name": "vm-mxp-pr-web-001",
  "provider": {
    "name": "vmware",
    "native_id": "vm-67890"
  }
}
```
```json
{
  "id": "cn-host-mxp-f1-r22-esxi-001-contains-vm-mxp-pr-web-001",
  "source": "host-mxp-f1-r22-esxi-001",
  "target": "vm-mxp-pr-web-001",
  "type": "contains",
  "direction": "forward"
}
```

> [!NOTE]
> Groups can classify VMs or pods by cluster, environment or ownership, while `contains` edges model the runtime placement relationship required for topology traversal (e.g. impact analysis).

---

## 6.5 Group metadata
Groups use the same metadata pattern as resources and connections:

- **`properties`**: Portable attributes specific to the group (e.g. capacity, quota, region_code, cost_center)
- **`extensions`**: Vendor/tool/org-specific metadata using `osiris.<namespace>` keys
- **`tags`**: Simple key-value labels (`string` > `string`)
- **`status`**: Normalized operational status (`active`|`inactive`|`degraded`|`retired`|`unknown`)
- **`state`**: Optional vendor-specific granular state

**Common group properties (non-exhaustive):**
- Capacity or quota information (e.g. `max_members`, `allocated_capacity`)
- Geographic or regional metadata (e.g. `region`, `availability_zone`, `geo_location`)
- Ownership or responsibility (e.g. `owner_email`, `oncall_team`, `budget_code`)
- Configuration (e.g. `maintenance_window`, `backup_policy`)

> [!NOTE]
> `properties` **SHOULD** remain portable across producers. Vendor or platform-derived fields with non-portable semantics **SHOULD** be placed under `extensions.osiris.<namespace>`.


### 6.5.1 Examples

> [!NOTE]
> **Members vs children:**
> - `members` = Direct resource membership (flat set for inventory, filtering, reporting)
> - `children` = Hierarchical nesting (tree/DAG structure for navigation and containment traversal)
> A group may have both `members` (leaf resources) and `children` (nested subgroups).

> [!NOTE]
> **Resource IDs:** Resource and group IDs in these examples are producer-defined and may follow different conventions (human-readable names, provider native IDs, deterministic hashes). OSIRIS does not mandate a specific ID format; producers **SHOUL** prioritize stability and uniqueness within each document.


#### 6.5.1.1 On-premise infrastructure groups
##### Physical datacenter hierarchy
```json
{
  "id": "grp-MXP",
  "type": "physical.datacenter",
  "name": "Datacenter MXP",
  "description": "Primary datacenter facility of Acme Corp. located in Milan.",
  "children": ["grp-MXP-F1", "grp-MXP-F2"],
  "properties": {
    "total_racks": 120,
    "power_capacity_kw": 3200,
    "geo_location": {
      "latitude": null,
      "longitude": null
    }
  },
  "tags": {
    "site": "MXP",
    "tier": "tier-3",
    "region": "emea"
  }
}
```

> [!NOTE]
> Geographic coordinates are optional and may be omitted for privacy or security considerations. If included, coordinates should use WGS84 decimal degrees format. Producers **MAY** apply precision reduction (e.g. rounding to 2-3 decimal places) or geographic jitter to avoid exposing exact facility locations while maintaining sufficient accuracy for regional grouping and visualization.

```json
{
  "id": "grp-MXP-F1",
  "type": "physical.floor",
  "name": "Floor 1",
  "description": "Main floor housing production infrastructure",
  "children": ["grp-MXP-F1-R01", "grp-MXP-F1-R02", "grp-MXP-F1-R03"],
  "properties": {
    "floor_number": 1,
    "total_racks": 40
  }
}
```
```json
{
  "id": "grp-MXP-F1-R01",
  "type": "physical.rack",
  "name": "Rack 01",
  "description": "Network fabric and compute rack",
  "members": ["MXP-F1-R01-SW-001", "MXP-F1-R01-SRV-042"],
  "properties": {
    "rack_units": 42,
    "power_capacity_watts": 5000,
    "cooling_type": "hot-aisle"
  },
  "tags": {
    "site": "MXP",
    "floor": "1",
    "row": "A"
  }
}
```

##### VMware vSphere cluster grouping
```json
{
  "id": "grp-vmware-cluster-mxp-prod",
  "type": "compute.cluster",
  "name": "MXP Production Cluster",
  "description": "VMware vSphere production cluster in datacenter MXP",
  "members": [
    "MXP-F1-R01-SRV-042",
    "MXP-F1-R22-SRV-002",
    "MXP-F1-R50-SRV-003",
    "vm-mxp-pr-web-001",
    "vm-mxp-pr-db-001"
  ],
  "properties": {
    "cluster_name": "MXP-PROD-CLUSTER-01",
    "hypervisor_version": "ESXi 8.0 U3",
    "ha_enabled": true,
    "drs_enabled": true,
    "total_hosts": 3,
    "total_cpu_ghz": 96,
    "total_memory_gb": 768
  },
  "tags": {
    "site": "MXP",
    "environment": "prod",
    "hypervisor": "vmware"
  },
  "extensions": {
    "osiris.vmware": {
      "vcenter_instance": "vcenter-mxp.acmecorp.internal",
      "cluster_moref": "domain-c7",
      "ha_admission_control": "failover-hosts",
      "drs_automation_level": "fullyAutomated",
      "evc_mode": "intel-cascadelake"
    }
  }
}
```

##### Proxmox VE cluster grouping
```json
{
  "id": "grp-proxmox-cluster-mxp-dev",
  "type": "compute.cluster",
  "name": "MXP Development Cluster",
  "description": "Proxmox VE development cluster in datacenter MXP",
  "members": [
    "MXP-F2-R05-SRV-001",
    "MXP-F2-R05-SRV-002",
    "MXP-F2-R06-SRV-003",
    "vm-mxp-dev-web-001",
    "vm-mxp-dev-db-001",
    "ct-mxp-dev-cache-001"
  ],
  "properties": {
    "cluster_name": "mxp-dev-pve",
    "hypervisor_version": "Proxmox VE 9.1",
    "quorum_expected_votes": 3,
    "total_nodes": 3,
    "total_cpu_cores": 72,
    "total_memory_gb": 384,
    "ha_enabled": true,
    "shared_storage": ["ceph-mxp-dev", "nfs-mxp-dev-backup"]
  },
  "tags": {
    "site": "MXP",
    "environment": "dev",
    "hypervisor": "proxmox"
  },
  "extensions": {
    "osiris.proxmox": {
      "cluster_id": "pve-cluster-mxp-dev",
      "corosync_version": "3.1.7",
      "ceph_version": "17.2.7",
      "backup_retention": {
        "daily": 7,
        "weekly": 4,
        "monthly": 3
      },
      "migration_network": "10.100.50.0/24"
    }
  }
}
```

> [!NOTE]
> Both VMware vSphere and Proxmox VE clusters use the generic `compute.cluster` type. Vendor-specific details (vCenter instance, Corosync configuration, Ceph integration) are placed in `extensions.osiris.<vendor>`. Proxmox VE supports both KVM virtual machines and LXC containers; the `members` array can include both `compute.vm` and `compute.container` resources.


##### On-premise network VLAN grouping
```json
{
  "id": "grp-mxp-vlan-100-prod",
  "type": "network.vlan",
  "name": "VLAN 100 - Production",
  "description": "Production VLAN spanning MXP datacenter fabric",
  "members": [
    "MXP-F1-R01-SW-001",
    "MXP-F1-R01-SRV-042",
    "vm-mxp-pr-web-001",
    "vm-mxp-pr-db-001"
  ],
  "properties": {
    "vlan_id": 100,
    "subnet": "10.100.0.0/24",
    "gateway": "10.100.0.1"
  },
  "tags": {
    "site": "MXP",
    "environment": "prod",
    "segment": "application"
  },
  "extensions": {
    "osiris.network": {
      "spanning_tree_priority": 4096,
      "spanning_tree_mode": "rapid-pvst"
    }
  }
}
```

---

#### 6.5.1.2 Hyperscalers and cloud provider hybrid groups
##### VPC Network group (AWS)
```json
{
  "id": "grp-aws-vpc-prod-use1",
  "type": "network.vpc",
  "name": "Production VPC",
  "description": "Primary production VPC in us-east-1",
  "members": ["subnet-aws-web-001", "subnet-aws-db-001", "aws::i-0abc123def4567890", "vm-aws-db-001"],
  "properties": {
    "cidr_block": "10.0.0.0/16",
    "region": "us-east-1"
  },
  "tags": {
    "environment": "prod",
    "managed_by": "terraform"
  },
  "extensions": {
    "osiris.aws": {
      "vpc_id": "vpc-0abc123def4567890",
      "dhcp_options_set": "dopt-0123456789abcdef",
      "default_security_group": "sg-0abc123def4567890"
    }
  }
}
```

##### VPC Network group (GCP)
```json
{
  "id": "grp-gcp-vpc-prod-euw1",
  "type": "network.vpc",
  "name": "Production VPC Network",
  "description": "Primary production VPC in europe-west1",
  "members": ["subnet-gcp-web-002", "subnet-gcp-db-002", "vm-gcp-web-002", "vm-gcp-db-002"],
  "properties": {
    "cidr_block": "10.1.0.0/16",
    "region": "europe-west1"
  },
  "tags": {
    "environment": "prod",
    "managed_by": "terraform"
  },
  "extensions": {
    "osiris.gcp": {
      "vpc_network_id": "projects/acme-prod/global/networks/vpc-prod-euw1",
      "auto_create_subnetworks": false,
      "routing_mode": "REGIONAL"
    }
  }
}
```

##### VNET group (Azure)
```json
{
  "id": "grp-az-vnet-prod-weu",
  "type": "network.vnet",
  "name": "Production VNet",
  "description": "Primary production virtual network in West Europe",
  "members": ["subnet-az-web-003", "subnet-az-db-003", "vm-az-web-003", "vm-az-db-003"],
  "properties": {
    "cidr_block": "10.2.0.0/16",
    "region": "westeurope"
  },
  "tags": {
    "environment": "prod",
    "managed_by": "terraform"
  },
  "extensions": {
    "osiris.azure": {
      "vnet_id": "/subscriptions/12345678-1234-1234-1234-123456789012/resourceGroups/rg-prod-weu/providers/Microsoft.Network/virtualNetworks/vnet-prod-weu",
      "provisioning_state": "Succeeded",
      "enable_ddos_protection": false
    }
  }
}
```

##### Resource group (Azure)
```json
{
  "id": "grp-az-rg-prod-weu-01",
  "type": "cloud.resource_group",
  "name": "rg-prod-weu-01",
  "description": "Production resource group in West Europe",
  "members": [
    "vm-az-web-003",
    "vm-az-db-003",
    "storage-prod-001"
  ],
  "children": [
    "grp-az-vnet-prod-weu"
  ],
  "properties": {
    "region": "westeurope"
  },
  "tags": {
    "environment": "prod",
    "cost_center": "CC-1234",
    "managed_by": "terraform"
  },
  "extensions": {
    "osiris.azure": {
      "subscription_id": "12345678-1234-1234-1234-123456789012",
      "resource_group_id": "/subscriptions/12345678-1234-1234-1234-123456789012/resourceGroups/rg-prod-weu-01",
      "provisioning_state": "Succeeded"
    }
  }
}
```

> [!NOTE]
> Cloud network constructs (AWS VPC, GCP VPC Network, Azure VNet) use generic types (`network.vpc`, `network.vnet`) with provider-specific details in `extensions.osiris.<provider>`. This approach enables consistent cross-cloud querying while preserving vendor-specific metadata for specialized tooling.

##### Hybrid environment grouping
```json
{
  "id": "grp-prod-global",
  "type": "logical.environment",
  "name": "Production - Global Hybrid",
  "description": "Production resources across AWS, Azure, GCP and on-premise datacenter MXP",
  "members": [
    "vm-mxp-pr-web-001",
    "MXP-F1-R01-SW-001",
    "MXP-F1-R01-SRV-042",
    "aws::i-0abc123def4567890",
    "vm-gcp-web-002",
    "vm-az-web-003",
    "vm-aws-db-001",
    "vm-gcp-db-002",
    "vm-az-db-003"
  ],
  "children": [
    "grp-aws-vpc-prod-use1",
    "grp-gcp-vpc-prod-euw1",
    "grp-az-vnet-prod-weu",
    "grp-MXP-F1-R01"
  ],
  "properties": {
    "oncall_team": "platform-sme",
    "maintenance_window": "Sun 02:00-04:00 UTC",
    "backup_retention_days": 90,
    "deployment_strategy": "multi-cloud"
  },
  "tags": {
    "environment": "prod",
    "scope": "global",
    "criticality": "high"
  }
}
```

> [!NOTE]
> Multi-cloud groups enable unified filtering and reporting across heterogeneous infrastructure. Members can include resources from AWS, Azure, GCP and on-premise simultaneously. The `children` array references both cloud network groups and on-premise physical groups, demonstrating hierarchical organization across infrastructure domains.

---

#### 6.5.1.3 Generic organizational and security groups
##### Logical environment with multi-dimensional membership
```json
{
  "id": "grp-prod",
  "type": "logical.environment",
  "name": "Production",
  "description": "All production workloads and infrastructure",
  "members": ["vm-mxp-pr-web-001", "vm-mxp-pr-db-001", "MXP-F1-R01-SW-001"],
  "properties": {
    "oncall_team": "platform-sme",
    "maintenance_window": "Sun 02:00-04:00 UTC",
    "backup_retention_days": 90
  },
  "tags": {
    "environment": "prod",
    "criticality": "high",
    "compliance": "soc2"
  },
  "extensions": {
    "osiris.com.acme": {
      "service_tier": "tier-1",
      "pagerduty_schedule": "PD-PROD-PRIMARY",
      "cost_center": "CC-PLATFORM-001"
    }
  }
}
```

##### Organizational team ownership
```json
{
  "id": "grp-sw-development",
  "type": "org.team",
  "name": "Software Development Team",
  "description": "Infrastructure and platform services development ownership",
  "members": ["vm-mxp-pr-web-001", "vm-mxp-pr-db-001", "MXP-F1-R01-SW-001", "MXP-F1-R01-SRV-042"],
  "properties": {
    "team_email": "sw-development@acmecorp.com",
    "oncall_rotation": "follow-the-sun",
    "slack_channel": "#sw-development",
    "teams_channel": "#sw-development"
  },
  "tags": {
    "department": "sw-development",
    "cost_center": "CC-1234"
  }
}
```

##### DMZ Security zone grouping
```json
{
  "id": "grp-mxp-pr-dmz",
  "type": "security.zone",
  "name": "DMZ",
  "description": "Demilitarized zone for public-facing services",
  "members": ["vm-mxp-pr-web-001", "lb-mxp-pr-web-001"],
  "properties": {
    "trust_level": "untrusted",
    "allowed_protocols": ["http", "https"],
    "egress_restricted": true
  },
  "tags": {
    "security_zone": "dmz",
    "compliance_scope": "pci-dss"
  }
}
```

##### Vendor-specific group type (Kubernetes namespace)
```json
{
  "id": "grp-k8s-production",
  "type": "osiris.k8s.namespace",
  "name": "production",
  "description": "Kubernetes production namespace",
  "members": ["pod-nginx-abc123", "service-nginx", "deployment-nginx"],
  "properties": {
    "cluster": "prod-cluster-1"
  },
  "extensions": {
    "osiris.k8s": {
      "resource_quotas": {
        "cpu": "100",
        "memory": "200Gi",
        "pods": "100"
      },
      "limit_ranges": {
        "max_cpu_per_pod": "4",
        "max_memory_per_pod": "8Gi"
      }
    }
  },
  "tags": {
    "environment": "prod",
    "team": "platform"
  }
}
```

---

## 6.6 Validation
If present, the following fields **MUST** conform to their specified types:

- `properties` **MUST** be a JSON object
- `extensions` **MUST** be a JSON object
- `tags` **MUST** be a JSON object mapping strings to strings
- `members` **MUST** be an array of strings
- `children` **MUST** be an array of strings
- `status` **MUST** be one of the defined status values (if present)

Consumers **MUST**:
- Accept groups with unknown fields and preserve them when re-exporting (when feasible)
- Accept groups with unknown types
- Accept groups with unknown extension namespaces
- Handle invalid member or children references gracefully

Consumers **MUST NOT**:
- Reject documents solely due to unrecognized group types
- Reject documents solely due to unknown group metadata fields
- Reject documents solely due to unknown extension namespaces

---

# 7 Resource type taxonomy
This chapter defines the **standard resource types** for OSIRIS v1.0.
Resource types categorize infrastructure components using a hierarchical dot-notation taxonomy (see Chapter 4, section 4.2.2 Type field definition).

## 7.1 Overview
### 7.1.1 Purpose and scope
 The taxonomy provides:

- **Common vocabulary** for infrastructure resources across IT and OT domains
- **Semantic consistency** enabling tools to recognize and process standard resource categories
- **Extensibility** through namespaced custom types when standard types do not apply
- **Interoperability** by establishing shared type strings that producers and consumers understand

The taxonomy is **descriptive, not prescriptive**: OSIRIS does not mandate that producers use only standard types. Producers **MAY** define custom types for vendor-specific or domain-specific resources. However, producers **SHOULD** use standard types when applicable to maximize interoperability.

**Scope of this chapter:**
- **Standard types** defined by OSIRIS v1.0 (sections 7.2–7.6)
- **Type naming conventions** and hierarchy rules (section 7.1.2)
- **Selection guidance** for choosing appropriate types (section 7.7)

This chapter does **not** define:
- Connection types (see Chapter 8)
- Group types (see Chapter 6, section 6.2)
- Validation rules (see Chapter 9)

**Coverage:**
The taxonomy spans both **IT infrastructure** (network, compute, storage, application) and **Operational Technology** (building automation, physical security, power distribution, industrial control). This unified approach reflects the convergence of IT and OT systems in modern infrastructure, particularly in data centers, smart buildings and industrial IoT environments.


### 7.1.2 Type naming conventions
Resource types follow the dot-notation hierarchy rules defined in Chapter 4, section 4.2.3. This section provides additional guidance specific to the standard taxonomy.


**General conventions:**
- Types **MUST** use lowercase letters, digits and dots only (no hyphens, underscores or spaces)
- Types **SHOULD** use dots to separate hierarchy levels (e.g. `compute.vm.template`)
- Types **SHOULD** use singular nouns (e.g. `storage.volume`, not `storage.volumes`)
- Types **SHOULD** be concise yet descriptive (e.g. `network.firewall` rather than `network.security.firewall.device`)


**Hierarchy depth:**
Standard types in this taxonomy use **2-3 segments** for most resources to avoid overloading the schema:

Two segments:
```
compute.vm
```

Three segments:
```
compute.vm.template
```

Producers **MAY** use deeper hierarchies only when necessary to increase the level of details (e.g. `building.hvac.ahu` or `power.datacenter.pdu`) but **SHOULD** consider whether additional specificity belongs in `properties` rather than the type string itself.


**Standard type families:**
This taxonomy organizes resources into **five top-level families**:

| Family | Prefix | Coverage |
|--------|--------|----------|
| Application | `application.*` | Databases, message queues, event streams, caches, code repositories, services |
| Compute | `compute.*` | Virtual machines, physical servers, containers, serverless functions, clusters |
| Storage | `storage.*` | Block volumes, object buckets, file systems, storage arrays |
| Network | `network.*` | VPCs, subnets, VLANs, routers, switches, firewalls, load balancers, security groups |
| Operational Technology | `building.*`, `security.*`, `power.*`, `industrial.*` | HVAC (AHU, VAV, chillers), access control, cameras, UPS, PDU, PLCs, SCADA, racks |


**Namespace reservation:**
As defined in Chapter 4, section 4.2.4, the `osiris.*` namespace is **reserved** for vendor-specific and organization-specific extensions. Standard types in this chapter do **not** use the `osiris.*` prefix.


### 7.1.3 Standard types vs custom types

**Standard types** are defined in this chapter and recognized across the OSIRIS ecosystem. Producers **SHOULD** use standard types when:

- The resource maps clearly to a standard type definition
- Semantic alignment exists (the standard type accurately describes the resource's role)
- Interoperability is desired (consumers can apply special handling for standard types)

Extension (namespaced) types (`osiris.*`) are defined by producers for vendor-specific or domain-specific resources. 

Producers **SHOULD** use custom types when:
- No standard type exists that accurately describes the resource
- Vendor-specific semantics are critical to preserve
- The resource is organization-specific and not applicable to other environments

**Coexistence:**
Standard and custom types coexist within the same OSIRIS document.

A topology **MAY** contain:
```json
{
  "topology": {
    "resources": [
      {
        "id": "aws::i-0abc123def4567890",
        "type": "compute.vm",
        "provider": {"name": "aws"}
      },
      {
        "id": "custom-aws-widget-001",
        "type": "osiris.aws.lambda.edge",
        "provider": {"name": "aws"}
      }
    ]
  }
}
```

Consumers **MUST** accept resources with both standard and custom types. Consumers **SHOULD** apply special handling (rendering, filtering, behavioral logic) for recognized standard types while treating custom types as generic resources.

**Fallback behavior:**
When a vendor-specific resource has a close standard equivalent, producers **SHOULD** consider:

1. Using the standard type with vendor details in `provider` or `properties`
2. Using a namespaced custom type if semantics differ significantly
3. Documenting the mapping decision for parser consumers


### 7.1.4 Taxonomy evolution
**Stability:**
Standard types defined in this chapter are **stable** for OSIRIS v1.x. Additions, modifications or deprecations follow semantic versioning principles:

- **Minor versions** (e.g. v1.1, v1.2) **MAY** add new standard types without breaking compatibility
- **Minor versions** **SHOULD NOT** remove or change the semantics of existing standard types
- **Major versions** (e.g. v2.0) **MAY** remove deprecated types or change type semantics with migration guidance

**Type deprecation:**
If a standard type becomes obsolete or is replaced by a better alternative, OSIRIS will:

1. Mark the type as **deprecated** in a minor release
2. Document the recommended replacement type
3. Maintain the deprecated type for at least one major version cycle
4. Remove the type in a subsequent major release

**Community contributions:**
Proposals for new standard types **MAY** be submitted to the OSIRIS governance process. Accepted proposals will be incorporated in minor releases.

Criteria for inclusion include:
- Clear, unambiguous definition
- Broad applicability across multiple providers or environments
- Avoidance of vendor-specific semantics
- Alignment with existing taxonomy structure

**Extension types:**
Types in the `osiris.*` namespace are **not governed by OSIRIS** and may evolve independently. Producers using extension types **SHOULD** version their generator tools to help consumers track compatibility.

---

## 7.2 Application resources
Application resources represent software services, databases, message queues, caches and code repositories that support application workloads.


### 7.2.1 Databases

**Type:** `application.database`

**Definition:**
A database is a managed service or self-hosted system that stores, organizes and provides access to structured or semi-structured data. Databases support various data models (relational, document, key-value, graph) and query interfaces.

**When to use:**
Use `application.database` for but not limited to:
- Managed cloud databases (CockroachDB, Neon DB, AWS RDS, Azure SQL Database, GCP Cloud SQL)
- NoSQL databases (DynamoDB, CosmosDB, MongoDB)
- Self-hosted databases (PostgreSQL, MySQL, MariaDB, SQL Server instances)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `engine` | string | Database engine | `"postgres"`, `"mysql"`, `"mongodb"` |
| `engine_version` | string | Engine version | `"15.4"` |
| `database_type` | string | Data model | `"relational"`, `"document"`, `"key-value"` |
| `size_gb` | integer | Storage allocation | `100` |
| `multi_az` | boolean | Multi-AZ deployment | `true` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | RDS Instance / DynamoDB Table | `application.database` |
| Azure | SQL Database / CosmosDB | `application.database` |
| GCP | Cloud SQL / Firestore | `application.database` |
| Self-hosted | PostgreSQL / MySQL | `application.database` |

**Example (AWS RDS):**
```json
{
  "id": "aws::myapp-prod-db",
  "type": "application.database",
  "name": "production-postgresql-primary",
  "provider": {
    "name": "aws",
    "type": "AWS::RDS::DBInstance",
    "native_id": "myapp-prod-db",
    "region": "us-east-1",
    "account_id": "123456789012"
  },
  "properties": {
    "engine": "postgres",
    "engine_version": "15.4",
    "database_type": "relational",
    "instance_class": "db.r6g.xlarge",
    "size_gb": 500,
    "multi_az": true,
    "encrypted": true,
    "endpoint": "myapp-prod-db.abc123.us-east-1.rds.amazonaws.com"
  },
  "status": "active"
}
```

**Example (Self-hosted PostgreSQL):**
```json
{
  "id": "postgres-mxp-prod-01.internal.example.com",
  "type": "application.database",
  "name": "production-postgresql-primary",
  "provider": {
    "name": "postgresql",
    "type": "PostgreSQL",
    "native_id": "postgres-mxp-prod-01.internal.example.com"
  },
  "properties": {
    "engine": "postgres",
    "engine_version": "15.4",
    "database_type": "relational",
    "hostname": "postgres-mxp-prod-01.internal.example.com",
    "port": 5432,
    "size_gb": 2000,
    "replication_mode": "synchronous",
    "replica_count": 2
  },
  "location": {
    "datacenter": "MXP",
    "rack": "R15-A",
    "unit": 12
  },
  "status": "active"
}
```


### 7.2.2 Message queues and event streams

**Type:** `application.queue`

**Definition:**
A message queue is a service that enables asynchronous communication between application components by storing messages in a queue until they are processed. Queues decouple producers and consumers and provide buffering for variable processing rates.

**When to use:**
Use `application.queue` for but not limited to:
- AWS SQS
- Azure Service Bus Queues
- RabbitMQ queues
- Apache Kafka topics (when modeled as message queues)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `queue_type` | string | Queue model | `"standard"`, `"fifo"`, `"stream"` |
| `retention_hours` | integer | Message retention | `168` |
| `max_message_size` | integer | Size limit in bytes | `262144` |

**Example (AWS SQS):**
```json
{
  "id": "aws::order-processing-queue",
  "type": "application.queue",
  "name": "order-processing-fifo-queue",
  "provider": {
    "name": "aws",
    "type": "AWS::SQS::Queue",
    "native_id": "https://sqs.us-east-1.amazonaws.com/123456789012/order-processing-queue.fifo",
    "region": "us-east-1",
    "account_id": "123456789012"
  },
  "properties": {
    "queue_type": "fifo",
    "retention_hours": 336,
    "max_message_size": 262144,
    "encrypted": true
  },
  "status": "active"
}
```


**Type:** `application.eventstream`

**Definition:**
An event stream is a distributed, partitioned log of events that enables publish-subscribe messaging and event replay. Streams support high-throughput data pipelines and real-time processing.

**When to use:**
Use `application.eventstream` for but not limited to:
- Apache Kafka topics
- AWS Kinesis streams
- Azure Event Hubs
- Pub/Sub topics (when emphasizing streaming characteristics)

**Example (AWS Kinesis):**
```json
{
  "id": "aws::user-clickstream-prod",
  "type": "application.eventstream",
  "name": "user-clickstream-analytics",
  "provider": {
    "name": "aws",
    "type": "AWS::Kinesis::Stream",
    "native_id": "user-clickstream-prod",
    "region": "us-east-1",
    "account_id": "123456789012"
  },
  "properties": {
    "shard_count": 4,
    "retention_hours": 168,
    "encrypted": true,
    "enhanced_monitoring": true
  },
  "status": "active"
}
```

**Example (Apache Kafka - on-premise):**
```json
{
  "id": "kafka::order-events-topic",
  "type": "application.eventstream",
  "name": "order-events-stream",
  "provider": {
    "name": "kafka",
    "type": "KafkaTopic",
    "native_id": "order-events-topic"
  },
  "properties": {
    "partition_count": 12,
    "replication_factor": 3,
    "retention_hours": 168,
    "cluster": "kafka-mxp-prod-cluster",
    "compression_type": "lz4"
  },
  "location": {
    "datacenter": "MXP"
  },
  "status": "active"
}
```


### 7.2.3 Caching services

**Type:** `application.cache`

**Definition:**
A caching service provides in-memory data storage for fast retrieval of frequently accessed data. Caches reduce latency and database load by storing hot data in RAM.

**When to use:**
Use `application.cache` for but not limited to:
- AWS ElastiCache (Redis, Memcached)
- Azure Cache for Redis
- GCP Memorystore
- Self-hosted Redis/Memcached instances

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `engine` | string | Cache engine | `"redis"`, `"memcached"` |
| `engine_version` | string | Engine version | `"7.0"` |
| `memory_gb` | integer | Cache size | `16` |
| `node_count` | integer | Number of nodes | `2` |

**Example (AWS ElastiCache):**
```json
{
  "id": "aws::session-cache-prod",
  "type": "application.cache",
  "name": "session-cache-cluster",
  "provider": {
    "name": "aws",
    "type": "AWS::ElastiCache::ReplicationGroup",
    "native_id": "session-cache-prod",
    "region": "us-east-1",
    "account_id": "123456789012"
  },
  "properties": {
    "engine": "redis",
    "engine_version": "7.0",
    "memory_gb": 32,
    "node_count": 3,
    "encrypted_in_transit": true,
    "encrypted_at_rest": true,
    "automatic_failover": true
  },
  "status": "active"
}
```

**Example (Self-hosted Redis):**
```json
{
  "id": "redis::mxp-session-cluster",
  "type": "application.cache",
  "name": "session-cache-cluster",
  "provider": {
    "name": "redis",
    "type": "RedisCluster",
    "native_id": "mxp-session-cluster"
  },
  "properties": {
    "engine": "redis",
    "engine_version": "7.2.3",
    "memory_gb": 64,
    "node_count": 6,
    "cluster_mode": true,
    "persistence": "aof",
    "nodes": [
      "redis-mxp-01.internal.example.com:6379",
      "redis-mxp-02.internal.example.com:6379",
      "redis-mxp-03.internal.example.com:6379"
    ]
  },
  "location": {
    "datacenter": "MXP"
  },
  "status": "active"
}
```


### 7.2.4 Code repositories

**Type:** `application.repository`

**Definition:**
A code repository is a version control system that stores source code, tracks changes and enables collaboration. Repositories use version control systems (Git, SVN) to manage codebase history.

**When to use:**
Use `application.repository` for but not limited to:
- GitHub repositories
- GitLab projects
- Bitbucket repositories
- AWS CodeCommit repositories
- Azure Repos

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `vcs_type` | string | Version control system | `"git"`, `"svn"` |
| `default_branch` | string | Main branch name | `"main"`, `"master"` |
| `visibility` | string | Access level | `"private"`, `"public"` |
| `url` | string | Repository URL | `"https://github.com/org/repo"` |

**Example:**
```json
{
  "id": "github::12345678",
  "type": "application.repository",
  "name": "web-application-frontend",
  "provider": {
    "name": "github",
    "type": "Repository",
    "native_id": "12345678"
  },
  "properties": {
    "vcs_type": "git",
    "default_branch": "main",
    "visibility": "private",
    "url": "https://github.com/myorg/web-app-frontend",
    "language": "TypeScript",
    "size_kb": 45632
  },
  "status": "active"
}
```


### 7.2.5 Application services

**Type:** `application.service`

**Definition:**
An application service is a generic deployed application or microservice that does not fit more specific application resource types. Services represent running applications, APIs or workloads.

**When to use:**
Use `application.service` for but not limited to:
- Microservices
- Web applications
- API services
- Background workers
- Any application component not covered by specific types (database, cache, queue, etc.)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `service_type` | string | Service category | `"api"`, `"web"`, `"worker"` |
| `language` | string | Programming language | `"python"`, `"java"` |
| `framework` | string | Application framework | `"fastapi"`, `"spring-boot"` |
| `endpoints` | array | Service endpoints | `["https://api.example.com"]` |

**Example:**
```json
{
  "id": "kubernetes::payment-api-deployment",
  "type": "application.service",
  "name": "payment-processing-api",
  "provider": {
    "name": "kubernetes",
    "type": "Deployment",
    "native_id": "payment-api-deployment",
    "namespace": "production"
  },
  "properties": {
    "service_type": "api",
    "language": "python",
    "framework": "fastapi",
    "version": "2.5.1",
    "endpoints": ["https://api.example.com/payments"],
    "replicas": 3,
    "container_image": "registry.example.com/payment-api:2.5.1"
  },
  "status": "active"
}
```

---