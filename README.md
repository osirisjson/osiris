# OSIRIS JSON format specification
OSIRIS (Open Standard for Infrastructure Resource Interchange Schema) defines a vendor-neutral JSON format for describing infrastructure resources, their properties and their topological relationships across heterogeneous IT and OT environments.

---

## The challenge
Modern infrastructure spans multiple stacks and providers: hyperscalers (AWS, Azure, GCP etc.), public clouds providers, on-prem datacenters, hybrid networks and complex OT environments integration.

While some platforms export inventories (often as JSON), the representations are inconsistent across vendors even for equivalent concepts like identity, properties and relationships.
Accurate topology documentation is expensive and fragile. Teams rely on partial, inaccurate documentation and hand-drawn diagrams. Critical context lives in people's heads.


## The solution
OSIRIS normalizes exports from hyperscalers and cloud providers, on-prem datacenters (compute, storage, network) and supports OT inclusion where applicable (initial v1.0 scope).
Designed for cross-platform visibility and portable consumption by tools (diagramming, inventory, audit) without requiring each consumer to implement vendor-specific parsers.

---

## Design principles
OSIRIS is a static snapshot interchange format. It captures what exists and how it relates at a point in time. It was not designed as a real-time monitoring system, a deployment tool, or an Infrastructure-as-Code engine.

#### What OSIRIS IS
Optimized for scenarios where documentation and topology must be exchanged between systems and teams.

- **Reliable, flexible infrastructure snapshots**
Capture “what exists” and “how it relates” at a point in time.

- **Documentation-ready outputs**
Enable consistent inventories, technical summaries and system context.

- **Diagram-friendly topology**
Provide normalized relationships for automated visualization and diagram generation.

- **Feeding CMDB / IPAM / DCIM workflows**
Export normalized data into systems of record and asset management tools.

- **Audit support**
Assist evidence collection and traceability by standardizing structure and relationships.


## Get started
Visit https://osirisjson.org/en/docs for more informations.


## Contribute
OSIRIS is developed in public and warmly welcomes contributions of any size, visit https://osirisjson.org/en/docs/get-involved/community for more informations.