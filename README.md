# OSIRIS JSON Specification

[![GitHub License](https://img.shields.io/github/license/osirisjson/osiris?style=flat-square&color=142C49&labelColor=23355F)](https://github.com/osirisjson/osiris-producers/blob/main/LICENSE)
[![Listed on JSON Schema Landscape](https://img.shields.io/badge/JSON%20Schema%20Landscape-Listed-142C49?style=flat-square&labelColor=23355F)](https://landscape.json-schema.org/)

The vendor-neutral interchange format to describe infrastructure
resources, connections and groups.

**OSIRIS** stands for
**Open Specification for Infrastructure Resource Interchange Schema**.

The specification defines a common, vendor-neutral way to describe what
infrastructure exists and how its resources relate to one another, using
a portable JSON document representing a point-in-time snapshot.

This repository is the
**authoritative source for the OSIRIS Specification and core schema**.
It defines the interchange format independently of the softwares used to
discover, validate, visualize or consume infrastructure data.

## An OSIRIS document at a glance

An OSIRIS document represents infrastructure as a collection of
resources, explicit relationships and logical or physical groups,
enriched with provider provenance, properties and namespaced extensions.

The following example is a curated subset of a discovery performed
against the [Cisco DevNet ACI sandbox](https://developer.cisco.com/sandbox/detail/aci-simulator-always-on) using the [OSIRIS JSON Producer for Cisco APIC (v0.4.0)](https://github.com/osirisjson/osiris-producers/).

It illustrates how a leaf-to-spine connection and its physical grouping
can be represented using the vendor-neutral OSIRIS JSON Core Schema
while preserving provider-specific context.

The snapshot has been edited and reduced for readability while
preserving the original identifiers and referential integrity.

```json
{
  "$schema": "https://osirisjson.org/schema/v1.0/osiris.schema.json",
  "version": "1.0.0",
  "metadata": {
    "timestamp": "2026-11-24T12:16:57Z",
    "generator": {
      "name": "osirisjson-producer-cisco-apic",
      "version": "0.4.0",
      "url": "https://docs.osirisjson.org/osiris-producers/network/cisco"
    },
    "scope": {
      "name": "sandboxapicdc.cisco.com",
      "description": "Curated Cisco ACI Fabric1 topology subset; controller IP addresses replaced for documentation.",
      "purpose": "documentation",
      "providers": ["cisco.apic"]
    }
  },
  "topology": {
    "resources": [
      {
        "id": "cisco.apic::topology/pod-1/node-1",
        "type": "compute.server",
        "name": "apic1",
        "provider": {
          "name": "cisco.apic",
          "native_id": "topology/pod-1/node-1",
          "type": "fabricNode",
          "version": "6.1(4h)"
        },
        "status": "active",
        "properties": {
          "address": "192.168.2.1",
          "fabric_domain": "ACI Fabric1",
          "inb_mgmt_addr": "192.168.2.11",
          "manufacturer": "Cisco",
          "node_id": "1",
          "oob_mgmt_addr": "192.168.2.14",
          "pod": "1",
          "role": "controller",
          "serial": "TEP-1-1",
          "system_state": "in-service",
          "uptime": "01:02:20:19.000"
        }
      },
      {
        "id": "cisco.apic::topology/pod-1/node-101",
        "type": "network.switch",
        "name": "leaf-1",
        "provider": {
          "name": "cisco.apic",
          "native_id": "topology/pod-1/node-101",
          "type": "fabricNode",
          "version": "simsw-6.1(4h)"
        },
        "status": "active",
        "properties": {
          "manufacturer": "Cisco",
          "model": "N9K-C9396PX",
          "role": "leaf"
        },
        "extensions": {
          "osiris.cisco": {
            "fabric_id": 1
          }
        }
      },
      {
        "id": "cisco.apic::topology/pod-1/node-102",
        "type": "network.switch",
        "name": "leaf-2",
        "provider": {
          "name": "cisco.apic",
          "native_id": "topology/pod-1/node-102",
          "type": "fabricNode",
          "version": "simsw-6.1(4h)"
        },
        "status": "active",
        "properties": {
          "manufacturer": "Cisco",
          "model": "N9K-C9396PX",
          "role": "leaf"
        }
      },
      {
        "id": "cisco.apic::topology/pod-1/node-201",
        "type": "network.switch",
        "name": "spine-1",
        "provider": {
          "name": "cisco.apic",
          "native_id": "topology/pod-1/node-201",
          "type": "fabricNode",
          "version": "simsw-6.1(4h)"
        },
        "status": "active",
        "properties": {
          "manufacturer": "Cisco",
          "model": "N9K-C9508",
          "role": "spine"
        }
      }
    ],
    "connections": [
      {
        "id": "conn-physical.ethernet-node-1-to-node-101-17900110216ffe24",
        "type": "physical.ethernet",
        "source": "cisco.apic::topology/pod-1/node-1",
        "target": "cisco.apic::topology/pod-1/node-101",
        "direction": "bidirectional",
        "properties": {
          "discovered_by": "fabricLink,lldp",
          "link_state": "ok",
          "source_port": "eth1/2",
          "target_port": "eth1/41"
        }
      },
      {
        "id": "conn-physical.ethernet-node-101-to-node-201-8884842b6db9def5",
        "type": "physical.ethernet",
        "source": "cisco.apic::topology/pod-1/node-101",
        "target": "cisco.apic::topology/pod-1/node-201",
        "direction": "bidirectional",
        "properties": {
          "discovered_by": "fabricLink,lldp",
          "link_state": "ok",
          "source_port": "eth1/49",
          "target_port": "eth5/1"
        }
      },
      {
        "id": "conn-physical.ethernet-node-102-to-node-201-a3fdf9460be4531a",
        "type": "physical.ethernet",
        "source": "cisco.apic::topology/pod-1/node-102",
        "target": "cisco.apic::topology/pod-1/node-201",
        "direction": "bidirectional",
        "properties": {
          "discovered_by": "fabricLink,lldp",
          "link_state": "ok",
          "source_port": "eth1/49",
          "target_port": "eth5/2"
        }
      }
    ],
    "groups": [
      {
        "id": "group-physical.pod-pod-1-24727155ff28b9b8",
        "type": "physical.pod",
        "name": "pod-1",
        "members": [
          "cisco.apic::topology/pod-1/node-1",
          "cisco.apic::topology/pod-1/node-101",
          "cisco.apic::topology/pod-1/node-102",
          "cisco.apic::topology/pod-1/node-201"
        ],
        "properties": {
          "pod_id": "1",
          "pod_type": "physical"
        }
      }
    ]
  }
}
```

### Explore complete OSIRIS JSON snapshots

For more representative infrastructure models, including resource
properties, provider provenance, physical and logical relationships,
and namespaced extensions, explore:

- [IT infrastructure examples](https://docs.osirisjson.org/examples/it-infrastructure/)
- [OT infrastructure examples](https://docs.osirisjson.org/examples/ot-infrastructure/)
- [OSIRIS JSON Producers](https://github.com/osirisjson/osiris-producers/tree/dev/testdata)


## What OSIRIS is and is not

**OSIRIS is:** a vendor-neutral, portable, point-in-time infrastructure
interchange format designed for automation, documentation, architecture
visualization, inventory and CMDB integration, audit evidence, offline
analysis, AI-assisted reasoning and agentic workflows, including
integrations through the Model Context Protocol (MCP).

By providing *structured, portable infrastructure context*, OSIRIS
enables AI-driven applications and agents to reason about your
infrastructure without requiring direct access to live systems, vendor
APIs or infrastructure credentials.

It decouples infrastructure data sources from consuming applications
through a common, extensible JSON schema, providing structured
infrastructure context for both traditional software and AI-driven
systems.

**OSIRIS is not:** a real-time monitoring or telemetry protocol, an
Infrastructure-as-Code deployment language, a configuration management
system, a change-tracking system or a substitute for native discovery
mechanisms and protocols.

Partial infrastructure snapshots are valid and useful when their capture
boundaries are accurately represented.

The format is designed for forward compatibility. OSIRIS JSON Consumers
MUST tolerate structurally valid unknown types, properties and
namespaced extensions without breaking the processing of recognized
content.


## Specification and developer references

| Reference | Purpose |
| --- | --- |
| [JSON Schema Landscape](https://landscape.json-schema.org/) | OSIRIS is listed under **Standards** in the JSON Schema ecosystem directory |
| [OSIRIS JSON Specification v1.0](specification/v1.0/OSIRIS-JSON-v1.0.md) | Normative definitions and behavior |
| [OSIRIS JSON Core Schema v1.0](schema/v1.0/osiris.schema.json) | Structural validation contract |
| [OSIRIS JSON Published schema endpoint](https://osirisjson.org/schema/v1.0/osiris.schema.json) | Canonical schema URI for tooling |
| [OSIRIS JSON Resource type taxonomy](specification/v1.0/OSIRIS-JSON-v1.0.md#7-resource-type-taxonomy) | Standard type naming and selection guidance |
| [OSIRIS JSON Appendix C: Resource type registry](specification/v1.0/OSIRIS-JSON-v1.0.md#appendix-c-resource-type-registry) | Enumerated standard v1.0 resource types |
| [OSIRIS JSON Connection types](specification/v1.0/OSIRIS-JSON-v1.0.md#52-connection-types) | Relationship semantics |
| [OSIRIS JSON Group types](specification/v1.0/OSIRIS-JSON-v1.0.md#62-group-types) | Logical and physical grouping semantics |
| [OSIRIS JSON Architecture development guidelines](docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) | Ecosystem boundaries and implementation roles |
| [OSIRIS JSON IT examples](https://docs.osirisjson.org/examples/it-infrastructure/) and [OSIRIS JSON OT examples](https://docs.osirisjson.org/examples/ot-infrastructure/) | Domain-specific starting points |

**Taxonomy note:** Standard resource types are a shared vocabulary, not
a closed list enforced by the core schema. OSIRIS JSON Producers SHOULD
select standard types when appropriate, but valid custom types remain
supported. A future machine-readable taxonomy catalog can improve
discoverability without turning the core type grammar into a restrictive
enum.

## Build on OSIRIS

| Build a | Go to |
| --- | --- |
| Validation or CI integration | [OSIRIS Toolbox](https://github.com/osirisjson/osiris-toolbox) |
| Source adapter or discovery tool | [OSIRIS JSON Producers](https://github.com/osirisjson/osiris-producers) |
| Documentation or topology tool | [OSIRIS JSON Consumers](https://github.com/osirisjson/osiris-consumers) (**in development**) |

## Contributing and governance

OSIRIS is developed in public. Issues, documentation improvements,
interoperability tests, and proposals for additional resource types are
welcome. Please read the [community contribution guide](https://docs.osirisjson.org/community/contributing/)
before proposing changes to normative content.

The specification is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
consult repository license files for code and other assets.