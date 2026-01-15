# OSIRIS JSON Format Specification
OSIRIS (Open Standard for Infrastructure Resource Interchange Schema) defines a vendor-neutral JSON format for describing infrastructure resources, their properties and their topological relationships across heterogeneous IT and OT environments.

OSIRIS is designed to normalize infrastructure data exported from diverse domains, including public hyperscalers, cloud platforms, private Data Centers, network devices, virtualization platforms and where applicable Operational Technology (OT) assets. OSIRIS focuses on describing what exists and how it relates, enabling cross-platform visibility and portable consumption by tools without requiring consumers to implement vendor-specific parsers.

OSIRIS is an interchange schema and is not intended to be:
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


## What problem this solves
The modern infrastructure landscape span multiple technology stacks and providers, including hyperscalers and continuously evolving private and public cloud platforms, on-premises data centers featuring compute virtualization platforms, storage and network systems and continuously increasing OT integration. While many systems started offering a comprehensible export format for resource inventories and configurations, the exported representations differ significantly across vendors and products, even when JSON is used. In OT environments the gap is even bigger due to legacy systems and vendor specific protocols.

The absence of a vendor-neutral interchange format means the infrastructure management ecosystem lacks a common language and producing a comprehensible hybrid cross-platform view typically requires bespoke translation layers that are costly to build and difficult to sustain.


## Get started
Visit https://osirisjson.org/en/get-started for more informations


## Contribute
Visit https://osirisjson.org/en/get-involved for more informations