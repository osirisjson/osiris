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

By decoupling the resource identity from its provider-specific implementation, OSIRIS enables a unified language format for cross-platform visibility, automated diagramming, and standardized auditing without requiring closed-source programs or vendor-specific parsers for every consumption tool.


#### Intended Audience
This specification is intended for:

- **Developers** building visualization, documentation, inventory, CMDB, or management tools.
- **Platform Engineers** designing automation and infrastructure-as-code workflows.
- **Solution Architects** documenting multi-cloud and hybrid environments.
- **Compliance or Risk Teams** requiring consistent infrastructure evidence and auditing inputs.


# Introduction
OSIRIS (Open Standard for Infrastructure Resource Interchange Schema) defines a vendor-neutral JSON format for describing infrastructure resources, their properties, and their topological relationships across heterogeneous IT and OT environments.

OSIRIS is designed to normalize infrastructure data exported from diverse domains, including public hyperscalers, cloud platforms, private Data Centers, network devices, virtualization platforms, and where applicable Operational Technology (OT) assets. OSIRIS focuses on describing what exists and how it relates, enabling cross-platform visibility and portable consumption by tools without requiring consumers to implement vendor-specific parsers.

OSIRIS is an interchange schema. It is not intended to replace vendor APIs or infrastructure-as-code models, but viceversa it aims to provide a stable and consistent schema suitable for diagramming, documentation, audit evidence, inventories, and integrations.


## 1.1 Problem statement
The modern infrastructure landscape span multiple technology stacks and providers, including hyperscalers and continuously evolving private and public cloud platforms, on-premises data centers featuring compute virtualization platforms, storage and network systems and continuously increasing OT integration. While many systems started offerin a comprehensible export format for resource inventories and configurations, the exported representations differ significantly across vendors and products, even when JSON is used. In OT environments the gap is even bigger due to legacy systems and vendor specific protocols.


#### Heterogeneous exports and inconsistent semantics:
Infrastructure data is tipically exposed through vendor-specific formats and models with inconsistent semantics for similar concepts:

- **Major hyperscalers:** Different providers expose fundamentally similar concepts (compute, storage, networking) using different export structures and naming conventions (e.g., differing resource identifiers, property models and relationship representations).

- **Network devices:** Device inventories and topologies data are often obtained via CLI output, NETCONF/YANG, vendor APIs, or telemetry streams, each requiring specialized parsing and normalization.

- **Virtualization platforms:** APIs and exports vary widely (JSON, XML, proprietary models), with distinct field naming, resource hierarchies, and relationship models.

- **Operational technology:** Building automation systems (BACnet, Modbus etc.) and industrial control systems use binary protocols or vendor-specific data representations with no standardized topology export.

As a result, producing a comprehensible, cross-platform view of infrastructure typically requires bespoke translation layers that are costly to build and difficult to sustain.


#### Consequences
This fragmentation creates recurring challenges:

- **Integration complexity:** Tools that consume infrastructure data must implement vendor-specific parsing and mapping logic, and often maintain separate adapters for each source system and each consuming application’s internal model.

- **High maintenance cost:** Vendor formats and APIs evolve over time, requiring continuous updates to parsers and mappers.

- **Inconsistent topology semantics:** Relationships such as attached-to, connected-to, member-of, routes-through, or depends-on are represented inconsistently or only implicitly, reducing reliability for diagram generation and analysis.

- **Duplicate effort across the ecosystem:** CMDBs, diagramming tools, monitoring platforms and compliance systems repeatedly re-implement similar vendor integrations, multiplying cost and slowing interoperability.

- **Documentation drift:** Without automated, format-agnostic interchange, documentation becomes outdated quickly and manual translation introduces errors.

- **Audit and compliance:** Producing consistent evidence across heterogeneous systems requires custom extraction and normalization per vendor, increasing time, complexity, and risk.

- **Lock-in risk:** Organizations may depend on closed-source or vendor specific normalization solutions, limiting portability, extensibility, and auditability.

The absence of a vendor-neutral interchange format means the infrastructure management ecosystem lacks a common language, and producing a comprehensible hybrid cross-platform view typically requires bespoke translation layers that are costly to build and difficult to sustain.


## 1.2 Solution overview
OSIRIS addresses the fragmentation described in Section 1.1 by defining a canonical JSON schema that serves as a neutral interchange format between infrastructure data sources and consuming applications.

Rather than requiring each consuming application to implement and maintain vendor-specific parsers, OSIRIS adopts a translation layer approach:

- **Producers** (exporters, translators, or discovery agents) transform vendor-specific representations into OSIRIS format.
- **Consumers** (diagramming tools, CMDBs, IPAMs, DCIMs, monitoring systems, documentation pipelines) read a single, Open Standard well-defined schema.

This decouples data sources from consuming applications, reducing integration complexity from S×C (S sources × C consumers) to P+C (P producers + C consumers)

This decouples data sources from consuming applications. In environments with **S** source systems and **C** consumer applications, the number of required integrations is reduced from potentially **S×C** vendor-to-tool mappings to **S** producer mappings (source > OSIRIS) plus **C** consumer mappings (OSIRIS > application).


### Core capabilities
OSIRIS provides:

1. **Unified resource model**: A common structure for representing infrastructure resources (compute, network, storage, applications) with consistent field semantics regardless of origin.

2. **Explicit relationship model**: First-class representation of connections, dependencies, containment and other topological relationships that are often implicit or vendor-specific.

3. **Grouping constructs**: Support for logical and physical grouping that reflects organizational and architectural structure without mandating a single taxonomy.

4. **Provider attribution**: Resources retain traceability to their originating platform (e.g., AWS, Azure, GCP, Arista Networks, Nokia, Cisco, Proxmox) while being described in standardized terms enabling mixed-source topologies.

5. **Extensibility**: A defined mechanism for vendor-specific properties and custom resource types without breaking schema compatibility.


### Design Approach
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