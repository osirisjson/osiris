# OSIRIS JSON Format Specification<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 14 December 2025 |
| Last revision date | 22 January 2026 |
| Status    | Draft |
| Specification ID | OSIRIS-1.0 |
| Schema URI | [OSIRIS-1.0](https://osirisjson.org/schema/v1.0/osiris.schema.json) |
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
    - [1.4.2 Simplicity](#142-simplicity)
    - [1.4.3 Vendor neutrality](#143-vendor-neutrality)
    - [1.4.4 Extensibility without fragmentation](#144-extensibility-without-fragmentation)
    - [1.4.5 Explicit over implicit](#145-explicit-over-implicit)
    - [1.4.6 Stability and compatibility](#146-stability-and-compatibility)
    - [1.4.7 Practicality and partial data](#147-practicality-and-partial-data)
  - [1.5 Terminology](#15-terminology)
    - [1.5.1 Normative keywords](#151-normative-keywords)
    - [1.5.2 Core terms](#152-core-terms)
    - [1.5.3 Domain-specific terms](#153-domain-specific-terms)
    - [1.5.4 JSON and schema terms](#154-json-and-schema-terms)
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
- [5 Connection model](#5-connection-model)
  - [5.0 Overview](#50-overview)
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
- [6 Group model](#6-group-model)
  - [6.0 Overview](#60-overview)
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
  - [7.3 Compute resources](#73-compute-resources)
    - [7.3.1 Physical servers](#731-physical-servers)
    - [7.3.2 Virtual machines](#732-virtual-machines)
    - [7.3.3 Containers](#733-containers)
    - [7.3.4 Compute clusters](#734-compute-clusters)
    - [7.3.5 Serverless functions](#735-serverless-functions)
  - [7.4 Storage resources](#74-storage-resources)
    - [7.4.1 Block storage](#741-block-storage)
    - [7.4.2 Object storage](#742-object-storage)
    - [7.4.3 File storage](#743-file-storage)
    - [7.4.4 Storage systems](#744-storage-systems)
  - [7.5 Network resources](#75-network-resources)
    - [7.5.1 Virtual networks and subnets](#751-virtual-networks-and-subnets)
    - [7.5.2 Network devices](#752-network-devices)
    - [7.5.3 Network interfaces and endpoints](#753-network-interfaces-and-endpoints)
    - [7.5.4 Network Load balancing](#754-network-load-balancing)
    - [7.5.5 Network security](#755-network-security)
  - [7.6 Operational Technology resources](#76-operational-technology-resources)
    - [7.6.1 Building automation](#761-building-automation)
    - [7.6.2 Physical security](#762-physical-security)
    - [7.6.3 Power and environmental](#763-power-and-environmental)
    - [7.6.4 Industrial control systems](#764-industrial-control-systems)
    - [7.6.5 Physical infrastructure](#765-physical-infrastructure)
  - [7.7 Type selection guidance](#77-type-selection-guidance)
    - [7.7.1 Choosing appropriate types](#771-choosing-appropriate-types)
    - [7.7.2 When to use custom types](#772-when-to-use-custom-types)
    - [7.7.3 Type mapping examples from well-known providers](#773-type-mapping-examples-from-well-known-providers)
- [8 Extension mechanism](#8-extension-mechanism)
  - [8.0 Overview](#80-overview)
  - [8.1 Properties vs extensions](#81-properties-vs-extensions)
    - [8.1.1 Properties object](#811-properties-object)
    - [8.1.2 Extensions object](#812-extensions-object)
    - [8.1.3 Decision framework](#813-decision-framework)
    - [8.1.4 Extension field structure](#814-extension-field-structure)
    - [8.1.4.1 Extension object key style](#8141-extension-object-key-style)
    - [8.1.5 Multiple extension namespaces](#815-multiple-extension-namespaces)
    - [8.1.6 Provider-specific metadata vs extensions](#816-provider-specific-metadata-vs-extensions)
    - [8.1.7 Forward compatibility](#817-forward-compatibility)
  - [8.2 Vendor-specific extensions](#82-vendor-specific-extensions)
    - [8.2.1 Vendor namespace structure](#821-vendor-namespace-structure)
    - [8.2.2 AWS specific extensions](#822-aws-specific-extensions)
    - [8.2.3 Azure specific extensions](#823-azure-specific-extensions)
    - [8.2.4 GCP specific extensions](#824-gcp-specific-extensions)
    - [8.2.5 VMware specific extensions](#825-vmware-specific-extensions)
    - [8.2.6 Proxmox specific extensions](#826-proxmox-specific-extensions)
    - [8.2.7 On-premise vendor extensions](#827-on-premise-vendor-extensions)
    - [8.2.8 Extension data types](#828-extension-data-types)
    - [8.2.9 Extension stability](#829-extension-stability)
  - [8.3 Custom resource types](#83-custom-resource-types)
    - [8.3.1 When to create custom types](#831-when-to-create-custom-types)
    - [8.3.2 Custom type naming](#832-custom-type-naming)
    - [8.3.3 Vendor-specific types](#833-vendor-specific-types)
    - [8.3.4 Organization-specific types](#834-organization-specific-types)
    - [8.3.5 Custom type documentation](#835-custom-type-documentation)
    - [8.3.6 Consumer handling of custom types](#836-consumer-handling-of-custom-types)
    - [8.3.7 Type stability and evolution](#837-type-stability-and-evolution)
  - [8.4 Namespacing](#84-namespacing)
    - [8.4.1 Namespace format](#841-namespace-format)
    - [8.4.2 Registered well-known namespaces](#842-registered-well-known-namespaces)
    - [8.4.3 Organization namespaces](#843-organization-namespaces)
    - [8.4.4 Custom namespaces](#844-custom-namespaces)
    - [8.4.5 Namespace registration](#845-namespace-registration)
    - [8.4.6 Namespace collision avoidance](#846-namespace-collision-avoidance)
    - [8.4.7 Namespace versioning](#847-namespace-versioning)
    - [8.4.8 Forward compatibility](#848-forward-compatibility)
  - [8.5 Extension best practices](#85-extension-best-practices)
    - [8.5.1 For producers](#851-for-producers)
    - [8.5.2 For consumers](#852-for-consumers)
    - [8.5.3 Common patterns](#853-common-patterns)
  - [8.6 Summary](#86-summary)
- [9 Validation](#9-validation)
  - [9.0 Overview](#90-overview)
  - [9.1 JSON Schema](#91-json-schema)
    - [9.1.1 Schema purpose](#911-schema-purpose)
    - [9.1.2 Schema location](#912-schema-location)
    - [9.1.3 Schema validation tools (non-normative)](#913-schema-validation-tools-non-normative)
    - [9.1.4 Schema conformance requirements](#914-schema-conformance-requirements)
    - [9.1.5 Schema extensibility](#915-schema-extensibility)
  - [9.2 Minimum required fields (baseline interoperability)](#92-minimum-required-fields-baseline-interoperability)
    - [9.2.1 Top-level required fields](#921-top-level-required-fields)
    - [9.2.2 Metadata required fields](#922-metadata-required-fields)
    - [9.2.3 Topology required fields](#923-topology-required-fields)
    - [9.2.4 Resource required fields](#924-resource-required-fields)
    - [9.2.5 Connection required fields](#925-connection-required-fields)
    - [9.2.6 Group required fields](#926-group-required-fields)
  - [9.3 Validation rules](#93-validation-rules)
    - [9.3.1 Validation levels](#931-validation-levels)
    - [9.3.2 Identity validation rules](#932-identity-validation-rules)
    - [9.3.3 Referential integrity rules](#933-referential-integrity-rules)
    - [9.3.4 Type format rules](#934-type-format-rules)
    - [9.3.5 Provider validation rules](#935-provider-validation-rules)
    - [9.3.6 Extension validation rules](#936-extension-validation-rules)
    - [9.3.7 Domain validation rules](#937-domain-validation-rules)
    - [9.3.8 Validation error levels](#938-validation-error-levels)
    - [9.3.9 Validation implementation guidance](#939-validation-implementation-guidance)
  - [9.4 Validation examples](#94-validation-examples)
    - [9.4.1 Valid minimal document](#941-valid-minimal-document)
    - [9.4.2 Valid document with resources and connections](#942-valid-document-with-resources-and-connections)
    - [9.4.3 Invalid document (missing required field)](#943-invalid-document-missing-required-field)
    - [9.4.4 Invalid document (dangling reference)](#944-invalid-document-dangling-reference)
    - [9.4.5 Invalid document (invalid type format)](#945-invalid-document-invalid-type-format)
  - [9.5 Summary](#95-summary)
- [10 Examples](#10-examples)
  - [10.1 IT Minimal Cloud Provider infrastructure](#101-it-minimal-cloud-provider-infrastructure)
    - [10.1.0 Overview](#1010-overview)
    - [10.1.1 Scenario](#1011-scenario)
    - [10.1.2 Example](#1012-example)
    - [10.1.3 Key features demonstrated](#1013-key-features-demonstrated)
  - [10.2 IT Minimal Hyperscaler infrastructure](#102-it-minimal-hyperscaler-infrastructure)
    - [10.2.0 Overview](#1020-overview)
    - [10.2.1 Scenario](#1021-scenario)
    - [10.2.2 Example](#1022-example)
    - [10.2.3 Key features demonstrated](#1023-key-features-demonstrated)
  - [10.3 IT Simple Hyperscaler infrastructure](#103-it-simple-hyperscaler-infrastructure)
    - [10.3.0 Overview](#1030-overview)
    - [10.3.1 Scenario](#1031-scenario)
    - [10.3.2 Example](#1032-example)
  - [10.4 IT Hyperscaler infrastructure belonging with Resource Group and VNet membership](#104-it-hyperscaler-infrastructure-belonging-with-resource-group-and-vnet-membership)
    - [10.4.0 Overview](#1040-overview)
    - [10.4.1 Scenario](#1041-scenario)
    - [10.4.2 Example](#1042-example)
  - [10.5 IT Multi-Hyperscalers environment](#105-it-multi-hyperscalers-environment)
    - [10.5.0 Overview](#1050-overview)
    - [10.5.1 Scenario](#1051-scenario)
    - [10.5.2 Example](#1052-example)
    - [10.5.3 Key features demonstrated](#1053-key-features-demonstrated)
  - [10.6 IT Hybrid infrastructure on Hyperscaler and On-Premise](#106-it-hybrid-infrastructure-on-hyperscaler-and-on-premise)
    - [10.6.0 Overview](#1060-overview)
    - [10.6.1 Scenario](#1061-scenario)
    - [10.6.2 Example](#1062-example)
    - [10.6.3 Key features demonstrated](#1063-key-features-demonstrated)
  - [10.7 IT Minimal On-Premise infrastructure](#107-it-minimal-on-premise-infrastructure)
    - [10.7.0 Overview](#1070-overview)
    - [10.7.1 Scenario](#1071-scenario)
    - [10.7.2 Example](#1072-example)
    - [10.7.3 Key features demonstrated](#1073-key-features-demonstrated)
  - [10.8 IT On-Premise Network topology](#108-it-on-premise-network-topology)
    - [10.8.0 Overview](#1080-overview)
    - [10.8.1 Scenario](#1081-scenario)
    - [10.8.2 Example](#1082-example)
    - [10.8.3 Key features demonstrated](#1083-key-features-demonstrated)
  - [10.9 OT Minimal infrastructure](#109-ot-minimal-infrastructure)
    - [10.9.0 Overview](#1090-overview)
    - [10.9.1 Scenario](#1091-scenario)
    - [10.9.2 Example](#1092-example)
    - [10.9.3 Key features demonstrated](#1093-key-features-demonstrated)
  - [10.10 OT Cross connection with IT Network](#1010-ot-cross-connection-with-it-network)
    - [10.10.0 Overview](#10100-overview)
    - [10.10.1 Scenario](#10101-scenario)
    - [10.10.2 Example](#10102-example)
    - [10.10.3 Key features demonstrated](#10103-key-features-demonstrated)
  - [10.11 OT Industrial printer](#1011-ot-industrial-printer)
    - [10.11.0 Overview](#10110-overview)
    - [10.11.1 Scenario](#10111-scenario)
    - [10.11.2 Example](#10112-example)
    - [10.11.3 Key features demonstrated](#10113-key-features-demonstrated)
  - [10.12 OT Security camera](#1012-ot-security-camera)
    - [10.12.0 Overview](#10120-overview)
    - [10.12.1 Scenario](#10121-scenario)
    - [10.12.2 Example](#10122-example)
    - [10.12.3 Key features demonstrated](#10123-key-features-demonstrated)
  - [10.13 OT Door access control](#1013-ot-door-access-control)
    - [10.13.0 Overview](#10130-overview)
    - [10.13.1 Scenario](#10131-scenario)
    - [10.13.2 Example](#10132-example)
    - [10.13.3 Key features demonstrated](#10133-key-features-demonstrated)
  - [10.14 Summary](#1014-summary)
    - [About example generators](#about-example-generators)
    - [10.14.1 Common patterns](#10141-common-patterns)
- [11 Implementation guidelines](#11-implementation-guidelines)
  - [11.0 Overview](#110-overview)
  - [11.1 Parser development](#111-parser-development)
    - [11.1.1 Core responsibilities](#1111-core-responsibilities)
    - [11.1.2 Mapping strategy](#1112-mapping-strategy)
    - [11.1.3 Identity and ID stability](#1113-identity-and-id-stability)
    - [11.1.4 Relationship extraction for connections and groups](#1114-relationship-extraction-for-connections-and-groups)
    - [11.1.5 Partial data and unknowns](#1115-partial-data-and-unknowns)
    - [11.1.6 Validation workflow for producers](#1116-validation-workflow-for-producers)
    - [11.1.7 Document splitting and export scope](#1117-document-splitting-and-export-scope)
    - [11.1.8 Logging and telemetry](#1118-logging-and-telemetry)
  - [11.2 Consumer implementation](#112-consumer-implementation)
    - [11.2.1 Version handling and negotiation](#1121-version-handling-and-negotiation)
    - [11.2.2 Consumer validation policy](#1122-consumer-validation-policy)
    - [11.2.3 Graph construction and traversal](#1123-graph-construction-and-traversal)
    - [11.2.4 Unknown types and extensions](#1124-unknown-types-and-extensions)
    - [11.2.5 Merging diff and snapshot correlation](#1125-merging-diff-and-snapshot-correlation)
  - [11.3 Best practices](#113-best-practices)
    - [11.3.1 Best practices for producers](#1131-best-practices-for-producers)
    - [11.3.2 Best practices for consumers](#1132-best-practices-for-consumers)
    - [11.3.3 Common pitfalls](#1133-common-pitfalls)
    - [11.3.4 Interoperability tips and checklist](#1134-interoperability-tips-and-checklist)


## Preface
This document specifies the Open Standard for Infrastructure Resource Interchange Schema (OSIRIS), a JSON-based data format for describing infrastructure resources, their properties and their topological relationships in a vendor-neutral manner.

OSIRIS is intended to define a unified comprehensible schema to normalize data exported from diverse IT environments. OSIRIS is also designed to be extended to Operational Technology (OT) systems to support the inclusion of industrial infrastructure resources.

By decoupling the resource identity from its provider-specific implementation, OSIRIS enables a unified language format for cross-platform visibility, automated diagramming and standardized auditing without requiring closed-source programs or vendor-specific parsers for every consumption tool.


##### Intended Audience
This specification is Intended for:

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

- **Producers** (parsers, translators or discovery agents) transform vendor-specific representations into OSIRIS format.
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
OSIRIS is an **Open Standard** Intended for broad adoption across vendors, platforms and communities. The specification and evolution of the schema are intended to be community-driven and openly reviewable.

### 1.4.2 Simplicity
OSIRIS prioritizes ease of understanding and implementation. The schema uses straightforward JSON structures with clear field semantics. Complexity is introduced only where necessary to represent real-world infrastructure. Producers and consumers **SHOULD** be able to implement basic OSIRIS support without extensive specialized knowledge.

### 1.4.3 Vendor neutrality
OSIRIS does not favor any particular vendor, platform or technology. Infrastructure sources including hyperscalers, cloud providers, on-premises systems and OT environments are represented using consistent patterns. Provider specific details are accommodated through well defined extension mechanisms rather than privileged positions in the core schema.

### 1.4.4 Extensibility without fragmentation
OSIRIS supports vendor-specific properties, custom resource types and domain-specific extensions without breaking compatibility. Extensions follow structured conventions to preserve interoperability: consumers that do not recognize extensions **MUST** be able to safely ignore them while still processing core data.

### 1.4.5 Explicit over implicit
Relationships, dependencies and topological connections are represented explicitly. Resource properties are clearly named with defined semantics. The schema avoids ambiguous or context-dependent interpretations that would require out-of-band knowledge to resolve.

### 1.4.6 Stability and compatibility
The core schema structure is designed for long-term stability. Version changes follow semantic versioning principles. Backwards compatibility **SHOULD** be maintained across minor versions and breaking changes **MUST** be introduced only in major versions with clear migration guidance and deprecation policies.

### 1.4.7 Practicality and partial data
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


### 1.5.3 Domain-specific terms
**Hyperscaler:** A large-scale provider offering a broad portfolio of infrastructure services (e.g. AWS, Azure, GCP oracle Cloud, IBM Cloud, Alibaba Cloud, Tencent Cloud).

**Cloud provider:** A minor provider offering cloud services such as IaaS, PaaS, SaaS or NaaS.

**On-Premise infrastructure:** Computing, networking and storage resources operated within an organization’s facilities rather than in a cloud provider environment.

**Operational Technology (OT):** Hardware and software systems that monitor and control physical devices, processes and infrastructure, including building automation systems (BAS), industrial control systems (ICS) and supervisory control and data acquisition (SCADA) systems.

**IT-OT convergence:** The integration of IT systems with OT systems to enable unified end-to-end visibility and management.

**Interchange format:** A standardized data representation designed for exchange between heterogeneous systems and not tied to any single tool’s internal model.


### 1.5.4 JSON and schema terms
**JSON (JavaScript Object Notation):** The data serialization format used by OSIRIS, as defined in [RFC 8259](https://www.rfc-editor.org/rfc/rfc8259.html).

**JSON Schema:** A vocabulary for validating the structure of JSON documents, used to define the formal structure of OSIRIS documents.

**Required field:** A field that **MUST** be present in a valid OSIRIS document or object.

**Optional field:** A field that **MAY** be present but is not required for validity.

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

The `id` field uniquely identifies a resource within the topology.

**ID Construction:**

Producers **SHOULD** construct IDs using one of these strategies:

1. **Native ID `<id>` (preferred for unique native identifiers):**
```text
   "id": "i-0abc123def456"
```

2. **Namespaced native ID `<provider>::<id>` (preferred for multi-provider topologies):**
```text
   "id": "aws::i-0abc123def456"
   "id": "vmware::vm-1234"
   "id": "cisco::FOC1234ABCD"
   "id": "dell::MXP-SRV-001"
```

3. **Full resource identifier (for providers that support structured resource IDs):**
```text
   "id": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def456"
   "id": "/subscriptions/.../resourceGroups/rg/providers/Microsoft.Compute/virtualMachines/vm-01"
```

4. **Generated ID (only when native ID unavailable or not globally unique):**
```text
   "id": "postgres-prod-01.mxp.internal.osiris.com.acme"
```

**Rationale:** 
Using native provider IDs maintains traceability to source systems, enables change tracking and reflects actual infrastructure state.

**ID separator (`::`):**
The double-colon separator distinguishes the provider namespace from the native identifier. This separator was chosen because:
- Does not conflict with AWS ARNs (which use single colons: `arn:aws:ec2:...`)
- Does not conflict with Azure resource IDs (which use slashes: `/subscriptions/...`)
- Provides clear visual distinction between provider and identifier
- Easy to parse programmatically: `id.split('::')` works in all languages

The provider name (left of `::`) follows canonical naming rules from section 4.3.3: use `segments: [a-z0-9]` lowercase letters, digits and dots separator only (no hyphens, underscores or spaces) (e.g. `arista`, `paloalto`). The native identifier (right of `::`) preserves the original vendor format.

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
    "name": "osiris-aws-parser",
    "version": "1.2.3",
    "url": "https://github.com/osirisjson/hyperscaler-parser/aws/osiris-aws-parser"
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
    "name": "osiris-aws-parser",
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
    "cost_center": "sw-development",
    "compliance": "sox"
  }
}

```

#### On-premise example
```json
{
  "timestamp": "2025-12-18T14:30:00Z",
  "generator": {
    "name": "osiris-aws-parser",
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
        "account": "123456789012"
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
        "account": "123456789012"
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
        "type": "PowerEdge R770",
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
      "type": "compute.runs.on",
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
> Resource types and group types in this example (e.g. `application.database`, `network.vpc`) are illustrative. The complete type taxonomy is defined in Chapter 7 (Resource type taxonomy) and section 6.2 (Group types). Identifiers are opaque strings and producers **MAY** use structured IDs based on specific naming conventions.


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
        }
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

- **`tags`** (object): Key-value labels used for organizational categorization, filtering or metadata annotation (e.g. environment, owner, cost_center, compliance requirements). Tags are general-purpose and cross-platform.

**Example:**
```json
  {
    "tags": {
      "environment": "production",
      "owner": "software-development-team",
      "cost_center": "sw-development"
    }
  }
```

> [!NOTE]
> Platform native label concepts (e.g. Kubernetes labels, annotations) **SHOULD** be represented in the `extensions` object (e.g. `extensions.osiris.kubernetes.labels`) to preserve platform-specific semantics while keeping the core schema simple.


### 4.1.4 Field extensibility and unknown fields
Consumers **MUST** ignore unknown fields to ensure forward compatibility. Producers **SHOULD** place non-standard fields under `properties` or `extensions` and **SHOULD NOT** introduce new top-level resource fields in documents conforming to OSIRIS v1.0.

This approach ensures:

- Forward compatibility when new OSIRIS versions introduce fields
- Clear extension boundaries for vendor-specific data
- Simplified validation and schema evolution


### 4.1.5 Resource object validation
Resource objects **MUST** conform to the [OSIRIS JSON Schema v1.0](https://osirisjson.org/schema/v1.0/osiris.schema.json). Structural validation ensures required fields are present and field types are correct.

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
    "account": "123456789012"
  },
  "properties": {
    "instance_type": "t3.medium",
    "vcpus": 2,
    "memory_gb": 4,
    "ip_addresses": {
      "private_ip": ["10.0.1.10", "10.0.1.11"],
      "public_ip": "203.0.113.10"
    },
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
- **MUST** use `segments: [a-z0-9]` lowercase letters, digits and dots separator only  (no hyphens, underscores or spaces) (e.g. `compute.vm.template` rather than `compute.vm-template` or `compute.vm_template`)
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

```text
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
- `<namespace>` identifies the vendor, organization or domain using reverse-domain
- `<type>` use `segments: [a-z0-9]` lowercase letters, digits and dots separator only (no hyphens, underscores or spaces)

Examples of namespaced types:
- `osiris.aws.lambda.edge` (AWS Lambda @Edge - distinct from standard Lambda)
- `osiris.azure.cosmosdb` (Azure Cosmos DB with multi-model API)
- `osiris.vmware.vsan` (VMware vSAN - specific storage technology)
- `osiris.cisco.aci` (Cisco ACI fabric - vendor-specific SDN)
- `osiris.com.acme.widget` (organization-specific device type)

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

> [!NOTE]
> The first example uses the standard type `compute.function.serverless` (defined in Chapter 7) because AWS Lambda is a standard serverless function. The second example uses a custom type `osiris.aws.lambda.edge` because Lambda @Edge has distinct execution semantics (runs at CloudFront edge locations) that differ from standard serverless functions.


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

**Extension types** under `extensions["osiris.*"]` are **not semantically governed** by OSIRIS and may evolve independently. Producers using vendor/organization extensions **SHOULD** version their generator tools if extension semantics change to help consumers track compatibility.


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
- Custom monitoring appliance > `osiris.com.acme.monitor` (organization-specific)


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
  - Use `segments: ^[a-z0-9]+(\.[a-z0-9]+)*$` lowercase letters, digits and dots separator only (no hyphens, underscores or spaces)

**Canonical provider names:**

Producers **MUST** use canonical lowercase identifiers for well-known providers:

| Provider | OSIRIS Canonical name | Examples to avoid |
|----------|----------------------|-------------------|
| Amazon Web Services | `aws` | `AWS`, `Amazon`, `amazon-web-services` |
| Microsoft Azure | `azure` | `Azure`, `Microsoft Azure`, `msazure` |
| Google Cloud Platform | `gcp` | `GCP`, `Google`, `googlecloud` |
| Oracle Cloud | `oci` | `OCI`, `Oracle`, `oraclecloud` |
| IBM Cloud | `ibm` | `IBM`, `ibmcloud` |
| Tencent Cloud | `tc` | `Tencent`, `tencentcloud` |
| Alibaba Cloud | `ali` | `Alibaba`, `alibabacloud`, `aliyun` |
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

For unlisted providers, use `segments: [a-z0-9]` lowercase letters, digits and dots separator only (no hyphens, underscores or spaces) (e.g. `arista`, `ciena`, `paloalto`, `checkpoint`, `f5networks`).

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

- **`system`** (string): The producing system name or source inventory system identifier (useful for custom and multi-source parsers).

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
  "type": "compute.server",
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

**`provider` (per-resource attribution):**
- Identifies the source system for an individual resource
- Enables correlation with native vendor identifiers
- Supports mixed-vendor topologies where resources from different providers coexist

**`metadata.scope` (document-wide context):**
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
    "account": "123456789012",
    "arn": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abc123def4567890"
  },
  "state": "running"
}
```

The metadata establishes document scope. The provider provides specific attribution for each resource. Both are valuable and non-redundant.

OSIRIS separates **resource provenance/correlation** (provider) from **vendor- or organization-specific semantics** (extensions).

- provider is for **provenance and correlation**: information required to locate, reference, or correlate a resource in its originating system (e.g. native_id, account, region, zone, subscription, project, tenant, site).
- extensions is for **vendor- or organization-specific semantics**: non-portable configuration flags, capabilities, implementation details and settings that are meaningful primarily within that vendor/platform or within an organization.

**Rule of thumb:**
- If it helps you **find/correlate** the resource in the source system, it belongs in provider.
- If it describes a **vendor-only feature or configuration**, it belongs in extensions.

> [!NOTE]
> Vendor namespaces under extensions (e.g. extensions["osiris.aws"]) may be emitted by third-party producers when exporting resources sourced from that vendor/platform. The namespace indicates the resource origin and the meaning of the extension payload, not the identity of the parser.


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
    "account": "123456789012",
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
    "model": "DCS-7050SX3-48YC12",
    "native_id": "MXP-SW-LEAF-01",
    "serial_number": "JPE19350287",
    "version": "EOS-4.28.3F"
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
  "type": "compute.server",
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
```text
osiris.<namespace>
```

The value associated with each `osiris.<namespace>` key **MUST** be a JSON object.

**Generic example:**
```json
{
  "extensions": {
    "osiris.aws": { "security.groups": ["sg-0abc123"] },
    "osiris.com.acme": { "owner_team": "software-development" }
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
    "account": "123456789012"
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
      "security.groups": [
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
    "osiris.com.acme": {
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
- **Organization extensions** (`osiris.com.acme"`): Internal tracking, ownership, compliance data

**Example decision:**
- `properties.instance_type` - intrinsic AWS attribute
- `extensions.osiris.aws.iam_role` - AWS-specific IAM construct
- `extensions.osiris.com.acme.owner_team` - organization-specific tracking


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

If a platform has richer label/annotation concepts (e.g. Kubernetes), producers **SHOULD** represent those under `extensions` (e.g. `extensions.osiris.kubernetes.labels`).


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
    "account": "123456789012",
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
      "security.groups": ["sg-0123456789abcdef0"]
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
    "model": "DCS-7050SX3-48YC12",
    "native_id": "MXP-SW-LEAF-01",
    "serial_number": "JPE19350123",
    "version": "EOS-4.28.3F"
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
    "osiris.com.acme": {
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
  "id": "kubernetes::nginx-7d8f9c-abcde",
  "type": "compute.container",
  "name": "nginx-7d8f9c-abcde",
  "provider": {
    "name": "kubernetes",
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
    "osiris.kubernetes": {
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
  "type": "osiris.com.acme.apm.monitor",
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
    "osiris.com.acme": {
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
> `status` and `state` represent point-in-time observations at the moment of export. They are **NOT** Intended for real-time monitoring, alerting or telemetry. For runtime observability, use dedicated monitoring systems (e.g. OpenTelemetry, Prometheus).


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
    "account": "123456789012"
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
    "account": "123456789012"
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
    "model": "DCS-7050SX3-48YC12",
    "native_id": "MXP-SW-LEAF-01",
    "serial_number": "JPE19350287",
    "version": "EOS-4.28.3F"
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
  "id": "kubernetes::nginx-deployment-abc123",
  "type": "compute.container",
  "name": "nginx-deployment-abc123",
  "provider": {
    "name": "kubernetes",
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
    "osiris.kubernetes": {
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
  "type": "application.database",
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

# 5 Connection model
## 5.0 Overview
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
{
    "id": "conn-001",
    "id": "conn-002",
    "id": "conn-003"
}
```

2. **Descriptive naming (recommended):**
```json
{
    "id": "conn-aws-vpc-subnet-001",
    "id": "conn-aws-web-db-dependency",
    "id": "conn-mxp-leaf-spine-uplink",
    "id": "conn-mxp-app-to-cache"
}
```

3. **Type-prefixed descriptive:**
```json
{
    "id": "network-vpc-to-subnet-001",
    "id": "dependency-web-to-db",
    "id": "contains-subnet-vm-001"
}
```

4. **Hash-based (for reproducibility):**
```json
{
    "id": "conn-a1b2c3d4e5f6"
}
```

5. **UUID (for guaranteed uniqueness on large infrastructures):**
```json
{
    "id": "550e8400-e29b-41d4-a716-446655440000"
}
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
```text
fingerprint = type + "|" + direction + "|" + A.resource_id + "|" + A.port + "|" + B.resource_id + "|" + B.port
id = hash(fingerprint)
```

#### Human-readable IDs (optional)
Instead of hashing, producers **MAY** use a human-readable ID encoding both endpoints, for example:
```text
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
```text
osiris.<namespace>.<type>
```

**Examples:**
- `osiris.aws.vpc.peering`
- `osiris.kubernetes.service.selector`
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

`source_transceiver` and `target_transceiver` are Intended for pluggable modules (SFP/SFP+/SFP28/QSFP family, SFP-10G-T, etc.). For fixed copper ports (integrated BASE-T NIC ports), producers **SHOULD** omit `*_transceiver` and instead describe the port/NIC at the resource level (e.g. server NIC model in resource properties or `provider.model`). If producers still choose to represent fixed-port endpoints, they **MUST** not imply optical DOM availability.

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
  "source": "kubernetes::api-gateway-7d8f9c-abcde",
  "target": "kubernetes::auth-service-5a6b7c-fghij",
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
      "model": "cat6a",
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
      "model": "om4",
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
  "type": "osiris.aws.vpc.peering",
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

# 6 Group model
## 6.0 Overview
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
{
   "id": "group-001",
   "id": "group-002"
}
```

2. **Descriptive naming (recommended):**
```json
{
   "id": "group-aws-vpc-production",
   "id": "group-mxp-datacenter",
   "id": "group-web-tier",
   "id": "group-kubernetes-prod-cluster"
}
```

3. **Type-prefixed descriptive:**
```json
{
   "id": "environment-production",
   "id": "datacenter-mxp",
   "id": "vnet-azure-prod"
}
```

4. **Hash-based (for reproducibility):**
```json
{
   "id": "group-a1b2c3d4e5f6"
}
```

5. **UUID (for guaranteed uniqueness on large infrastructures):**
```json
{
   "id": "550e8400-e29b-41d4-a716-446655440000"
}
```

Descriptive naming (strategies 2-3) is **RECOMMENDED** as it improves human readability and debugging while maintaining uniqueness within the document scope.

> [!NOTE]
> Unlike resource IDs which use `provider::native-id` format to reference vendor systems, group IDs do not require namespace prefixes since groups represent logical or physical collections rather than vendor-managed objects.


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
- **`security.trust.boundary`**: Trust domain boundaries
- **`security.compliance.scope`**: Compliance boundary groupings (e.g. PCI-DSS scope, HIPAA scope)

**Example use cases:**
- Modeling zero-trust network architecture
- Documenting compliance boundaries for audit
- Visualizing security zone transitions

#### org.*
**Purpose:** Organizational ownership, responsibility and business structure groupings.

**Recommended types:**
- **`org.team`**: Team ownership (e.g. platform-team, sme-team, security-team)
- **`org.owner`**: Individual or group ownership
- **`org.costcenter`**: Financial cost center allocations
- **`org.bu`**: Business unit or department groupings
- **`org.dept`**: Business unit or department groupings
- **`org.project`**: Project-based groupings

**Example use cases:**
- Allocating infrastructure costs by team or cost center
- Identifying ownership for incident response
- Organizing resources by business unit for governance


### 6.2.4 Vendor-specific and custom group types
Vendor-specific or organization-specific group types **SHOULD** use the namespaced pattern:
```text
osiris.<namespace>.<type>
```

**Examples:**
- `osiris.aws.account` (AWS account boundary)
- `osiris.azure.subscription` (Azure subscription)
- `osiris.gcp.project` (GCP project)
- `osiris.kubernetes.namespace` (Kubernetes namespace)
- `osiris.vmware.cluster` (VMware vSphere cluster)
- `osiris.com.acme.product.line` (organization-specific product grouping)

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
      "members": ["aws::i-0abc123def4567890", "aws::i-0def456"]
    },
    {
      "id": "group-web-tier",
      "type": "logical.tier",
      "name": "Web Tier",
      "members": ["aws::i-0abc123def4567890", "aws::i-0def456"]
    },
    {
      "id": "group-billing-application",
      "type": "logical.application",
      "name": "Billing Application",
      "members": ["aws::i-0abc123def4567890", "aws::i-0def456"]
    },
    {
      "id": "group-team-software-development",
      "type": "org.team",
      "name": "Software Development Team",
      "members": ["aws::i-0abc123def4567890", "aws::i-0def456"]
    },
    {
      "id": "group-mxp-rack-r01",
      "type": "physical.rack",
      "name": "MXP Datacenter Rack R01",
      "members": ["aws::i-0abc123def4567890", "aws::i-0def456"]
    }
  ]
}
```


### 6.3.3 Children array (hierarchical nesting)
The `children` field is an array of group IDs that are nested under this group, enabling hierarchical organization:
```text
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
```text
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
```text
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

**Resource:**
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
```

**Connection:**
```json
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
  "type": "compute.server",
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
      "default_security.group": "sg-0abc123def4567890"
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
  "type": "cloud.resource.group",
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
  "id": "grp-kubernetes-production",
  "type": "osiris.kubernetes.namespace",
  "name": "production",
  "description": "Kubernetes production namespace",
  "members": ["pod-nginx-abc123", "service-nginx", "deployment-nginx"],
  "properties": {
    "cluster": "prod-cluster-1"
  },
  "extensions": {
    "osiris.kubernetes": {
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
- Connection types (see Chapter 5, section 5.2)
- Group types (see Chapter 6, section 6.2)
- Validation rules (see Chapter 9)

**Coverage:**
The taxonomy spans both **IT infrastructure** (network, compute, storage, application) and **Operational Technology** (building automation, physical security, power distribution, industrial control). This unified approach reflects the convergence of IT and OT systems in modern infrastructure, particularly in data centers, smart buildings and industrial IoT environments.


### 7.1.2 Type naming conventions
Resource types follow the dot-notation hierarchy rules defined in Chapter 4, section 4.2.3. This section provides additional guidance specific to the standard taxonomy.


**General conventions:**
- Types **MUST** use `segments: [a-z0-9]` lowercase letters, digits and dots separator only (no hyphens, underscores or spaces)
- Types **SHOULD** use dots to separate hierarchy levels (e.g. `compute.vm.template`)
- Types **SHOULD** use singular nouns (e.g. `storage.volume`, not `storage.volumes`)
- Types **SHOULD** be concise yet descriptive (e.g. `network.firewall` rather than `network.security.firewall.device`)


**Hierarchy depth:**
Standard types in this taxonomy use **2-3 segments** for most resources to avoid overloading the schema:

Two segments:
```text
compute.vm
```

Three segments:
```text
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

**Extension types** under `extensions["osiris.*"]` are **not semantically governed** by OSIRIS and may evolve independently. Producers using vendor/organization extensions **SHOULD** version their generator tools if extension semantics change to help consumers track compatibility.

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
    "account": "123456789012"
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
  "id": "postgres-mxp-prod-01.internal.osiris.com.acme",
  "type": "application.database",
  "name": "production-postgresql-primary",
  "provider": {
    "name": "postgresql",
    "type": "PostgreSQL",
    "native_id": "postgres-mxp-prod-01.internal.osiris.com.acme"
  },
  "properties": {
    "engine": "postgres",
    "engine_version": "15.4",
    "database_type": "relational",
    "hostname": "postgres-mxp-prod-01.internal.osiris.com.acme",
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
    "account": "123456789012"
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
    "account": "123456789012"
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
    "account": "123456789012"
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
      "redis-mxp-01.internal.osiris.com.acme:6379",
      "redis-mxp-02.internal.osiris.com.acme:6379",
      "redis-mxp-03.internal.osiris.com.acme:6379"
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
| `endpoints` | array | Service endpoints | `["https://api.acme.example"]` |

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
    "endpoints": ["https://api.acme.example/payments"],
    "replicas": 3,
    "container_image": "registry.acme.example/payment-api:2.5.1"
  },
  "status": "active"
}
```

---

## 7.3 Compute resources
Compute resources represent processing capacity: virtual machines, physical servers, containers, serverless functions and compute clusters. These resources execute workloads, run operating systems and provide the computational foundation of infrastructure.


### 7.3.1 Physical servers

**Type:** `compute.server`

**Definition:**
A physical server is a bare-metal compute host: a physical machine with CPU, memory, storage and network hardware. Physical servers may run workloads directly (bare metal) or serve as hypervisor hosts for virtual machines or containers clusters.

**When to use:**
Use `compute.server` for:
- Bare-metal cloud instances (e.g. AWS EC2 Bare Metal, Azure Dedicated Host)
- On-premise physical servers (e.g. Dell PowerEdge, HPE ProLiant, Cisco UCS)
- Hypervisor hosts (e.g. ESXi hosts, Proxmox nodes)
- Any physical compute hardware

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `cpu_model` | string | Processor model | `"Intel Xeon Gold 6248R"` |
| `cpu_cores` | integer | Total physical cores | `40` |
| `memory_mb` | integer | Installed RAM in megabytes | `262144` |
| `manufacturer` | string | Hardware vendor | `"Dell"`, `"HPE"`, `"Cisco"` |
| `model` | string | Server model | `"PowerEdge R770"` |
| `serial_number` | string | Hardware serial | `"ABCD1234"` |
| `role` | string | Server role | `"hypervisor"`, `"bare-metal"` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | EC2 Bare Metal / Dedicated Host | `compute.server` |
| Azure | Dedicated Host | `compute.server` |
| Dell | PowerEdge Server | `compute.server` |
| HPE | ProLiant Server | `compute.server` |
| VMware | ESXi Host | `compute.server` |
| Proxmox | Proxmox Node | `compute.server` |

**Example:**
```json
{
  "id": "dell::SERVICE_TAG_ABC123",
  "type": "compute.server",
  "name": "esx-host-03.acme.example",
  "provider": {
    "name": "dell",
    "model": "PowerEdge R770",
    "native_id": "SERVICE_TAG_ABC123"
  },
  "properties": {
    "manufacturer": "Dell",
    "model": "PowerEdge R770",
    "cpu_model": "Intel Xeon Gold 6348",
    "cpu_cores": 56,
    "memory_mb": 524288,
    "role": "hypervisor",
    "hypervisor": "vmware_esxi_8.0",
    "management_ip": "10.0.10.42"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "floor": "1",
    "rack": "R01",
    "rack_unit": "42"
  }
}
```


### 7.3.2 Virtual machines

**Type:** `compute.vm`

**Definition:**
A virtual machine (VM) is a software-based compute instance that emulates physical hardware. VMs run on hypervisors and provide isolated execution environments with dedicated CPU, memory, storage and network resources.

**When to use:**
Use `compute.vm` for:
- Cloud virtual machine instances (AWS EC2, Azure VMs, GCP Compute Engine)
- On-premise virtualized servers (VMware VMs, Proxmox VMs, Hyper-V VMs)
- Virtual desktop infrastructure (VDI) instances
- Any resource that represents a full virtual machine with OS-level isolation

**Common properties:**
Producers **SHOULD** include these properties when available:

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `vcpus` | integer | Number of virtual CPUs | `4` |
| `memory_mb` | integer | Memory allocation in megabytes | `8192` |
| `os` | string | Operating system | `"Ubuntu 22.04"` |
| `hypervisor` | string | Underlying hypervisor type | `"kvm"`, `"esxi"` |
| `instance_type` | string | Provider-defined size/shape | `"t3.medium"`, `"Standard_D4s_v3"` |
| `state` | string | Runtime state | `"running"`, `"stopped"` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | `AWS::EC2::Instance` | `compute.vm` |
| Azure | `Microsoft.Compute/virtualMachines` | `compute.vm` |
| GCP | `compute#instance` | `compute.vm` |
| Oracle Cloud (OCI) | `oci_core_instance` | `compute.vm` |
| IBM Cloud | `ibm_is_instance` | `compute.vm` |
| Alibaba Cloud | `ALIYUN::ECS::InstanceGroup` | `compute.vm` |
| Tencent Cloud | `tencentcloud_instance` | `compute.vm` |
| VMware | ESXi Virtual Machine | `compute.vm` |
| Proxmox | QEMU/KVM VM | `compute.vm` |
| OpenStack | Nova Instance | `compute.vm` |

> [!NOTE]
> "Native Type" is an example identifier from a common interface (CloudFormation/ARM/API/Terraform/ROS). Parsers **SHOULD** label the source system in documentation (or encode it in provider.source) when the same provider has multiple native type namespaces (e.g. IBM VPC vs Classic, Alibaba ROS vs Terraform).

**Example:**
```json
{
  "id": "aws::i-0abc123def4567890",
  "type": "compute.vm",
  "name": "web-server-01",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123def4567890",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "instance_type": "t3.medium",
    "vcpus": 2,
    "memory_mb": 4096,
    "os": "Ubuntu 22.04 LTS",
    "state": "running",
    "ip_addresses": {
      "private_ip": ["10.0.1.10", "10.0.1.11"],
      "public_ip": "203.0.113.10"
    }
  },
  "status": "active"
}
```

**Related types:**
- `compute.vm.template` - VM templates or images
- `compute.vm.snapshot` - Point-in-time VM snapshots


### 7.3.3 Containers

**Type:** `compute.container`

**Definition:**
A container is a lightweight, isolated execution environment that shares the host OS kernel. Containers package applications with their dependencies and provide process-level isolation without the overhead of full virtualization.

**When to use:**
Use `compute.container` for:
- Docker containers
- Kubernetes pods (when modeling individual containers within pods)
- ECS/EKS task containers
- Any container runtime instance (containerd, CRI-O, etc.)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `image` | string | Container image reference | `"nginx:1.25.3"` |
| `registry` | string | Image registry | `"docker.io"`, `"gcr.io"` |
| `runtime` | string | Container runtime | `"docker"`, `"containerd"` |
| `state` | string | Runtime state | `"running"`, `"stopped"` |
| `cpu_limit` | string | CPU limit | `"500m"`, `"2"` |
| `memory_limit` | string | Memory limit | `"512Mi"`, `"2Gi"` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| Docker | Container | `compute.container` |
| Kubernetes | Container (within Pod) | `compute.container` |
| AWS ECS | Task Container | `compute.container` |
| Azure ACI | Container Instance | `compute.container` |
| GCP Cloud Run | Container | `compute.container` |

**Example:**
```json
{
  "id": "docker::a3b2c1d4e5f6",
  "type": "compute.container",
  "name": "redis-cache-01",
  "provider": {
    "name": "docker",
    "type": "Container",
    "native_id": "a3b2c1d4e5f6"
  },
  "properties": {
    "image": "redis:7.2-alpine",
    "registry": "docker.io",
    "runtime": "docker",
    "state": "running",
    "cpu_limit": "500m",
    "memory_limit": "512Mi",
    "ports": ["6379:6379"]
  },
  "status": "active"
}
```

**Related types:**
- `compute.container.pod` - Kubernetes pods (groups of containers)
- `compute.container.image` - Container images


### 7.3.4 Compute clusters

**Type:** `compute.cluster`

**Definition:**
A compute cluster is a managed group of compute resources that work together to provide scalable processing capacity. Clusters abstract individual nodes and provide unified management, scheduling and scaling.

**When to use:**
Use `compute.cluster` for:
- Kubernetes clusters
- VMware vSphere clusters
- Proxmox clusters
- ECS/EKS clusters
- Auto-scaling groups (when modeled as managed compute pools)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `node_count` | integer | Number of nodes | `5` |
| `orchestrator` | string | Cluster orchestration platform | `"kubernetes"`, `"vmware"` |
| `version` | string | Orchestrator version | `"1.28.5"`, `"8.0 U2"` |
| `capacity` | object | Aggregate capacity | `{"vcpus": 80, "memory_mb": 262144}` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | EKS Cluster / ECS Cluster | `compute.cluster` |
| Azure | AKS Cluster | `compute.cluster` |
| GCP | GKE Cluster | `compute.cluster` |
| VMware | vSphere Cluster | `compute.cluster` |
| Proxmox | Proxmox Cluster | `compute.cluster` |

**Example:**
```json
{
  "id": "vmware::domain-c7",
  "type": "compute.cluster",
  "name": "Production vSphere Cluster",
  "provider": {
    "name": "vmware",
    "type": "vSphere Cluster",
    "native_id": "domain-c7",
    "region": "mxp-dc1"
  },
  "properties": {
    "orchestrator": "vmware_vsphere",
    "version": "8.0 U3",
    "node_count": 6,
    "capacity": {
      "vcpus": 336,
      "memory_mb": 3145728
    },
    "ha_enabled": true,
    "drs_enabled": true
  },
  "status": "active"
}
```


### 7.3.5 Serverless functions

**Type:** `compute.function.serverless`

**Definition:**
A serverless function is an event-driven compute resource that executes code in response to triggers without requiring explicit server or VM management. Functions scale automatically and are billed based on execution time.

**When to use:**
Use `compute.function.serverless` for:
- AWS Lambda functions
- Azure Functions
- Google Cloud Functions
- Cloudflare Workers
- Any function-as-a-service offering

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `runtime` | string | Language runtime | `"python3.11"`, `"nodejs20.x"` |
| `memory_mb` | integer | Allocated memory | `512`, `1024` |
| `timeout_seconds` | integer | Execution timeout | `30`, `900` |
| `handler` | string | Function entry point | `"index.handler"` |
| `trigger_type` | string | Invocation trigger | `"http"`, `"s3"`, `"sqs"` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | `AWS::Lambda::Function` | `compute.function.serverless` |
| Azure | `Microsoft.Web/sites (Function App)` | `compute.function.serverless` |
| GCP | `cloudfunctions#function` | `compute.function.serverless` |
| Cloudflare | Workers Function | `compute.function.serverless` |

**Example:**
```json
{
  "id": "aws::image-processor",
  "type": "compute.function.serverless",
  "name": "image-processor",
  "provider": {
    "name": "aws",
    "type": "AWS::Lambda::Function",
    "native_id": "image-processor",
    "region": "us-east-1",
    "account": "123456789012",
    "arn": "arn:aws:lambda:us-east-1:123456789012:function:image-processor"
  },
  "properties": {
    "runtime": "python3.11",
    "memory_mb": 1024,
    "timeout_seconds": 60,
    "handler": "lambda_function.handler",
    "trigger_type": "s3"
  },
  "status": "active"
}
```

---

## 7.4 Storage resources
Storage resources provide persistent data storage capabilities: block devices, object stores, file systems and storage arrays.


### 7.4.1 Block storage

**Type:** `storage.volume`

**Definition:**
A storage volume is a block-level storage device that can be attached to compute resources. Volumes provide persistent storage with raw block access, typically used for operating systems, databases and applications requiring low-latency I/O.

**When to use:**
Use `storage.volume` for:
- Cloud block storage (AWS EBS, Azure Managed Disks, GCP Persistent Disks)
- SAN volumes
- Virtual machine disks (VMDKs, VHDs)
- LUNs on storage arrays

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `size_gb` | integer | Volume capacity in gigabytes | `100` |
| `volume_type` | string | Storage tier/type | `"ssd"`, `"hdd"`, `"gp3"` |
| `iops` | integer | Provisioned IOPS | `3000` |
| `encrypted` | boolean | Encryption status | `true` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | `AWS::EC2::Volume` | `storage.volume` |
| Azure | `Microsoft.Compute/disks` | `storage.volume` |
| GCP | `compute#disk` | `storage.volume` |
| VMware | VMDK | `storage.volume` |

**Example:**
```json
{
  "id": "aws::vol-0abc123",
  "type": "storage.volume",
  "name": "database-data-volume",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Volume",
    "native_id": "vol-0abc123",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "size_gb": 500,
    "volume_type": "gp3",
    "iops": 5000,
    "throughput_mbps": 250,
    "encrypted": true,
    "availability_zone": "us-east-1a"
  },
  "status": "active"
}
```


### 7.4.2 Object storage

**Type:** `storage.bucket`

**Definition:**
An object storage bucket is a container for storing unstructured data (objects) in the cloud. Buckets provide scalable, durable storage accessed via HTTP APIs, typically used for media files, backups, logs and static website hosting.

**When to use:**
Use `storage.bucket` for:
- AWS S3 buckets
- Azure Blob Storage containers
- GCP Cloud Storage buckets
- MinIO buckets
- Any object storage container

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `versioning_enabled` | boolean | Object versioning | `true` |
| `public_access` | boolean | Public read access | `false` |
| `encryption` | string | Encryption type | `"SSE-S3"`, `"SSE-KMS"` |
| `lifecycle_rules` | boolean | Lifecycle policies configured | `true` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | `AWS::S3::Bucket` | `storage.bucket` |
| Azure | Blob Storage Container | `storage.bucket` |
| GCP | `storage#bucket` | `storage.bucket` |
| MinIO | Bucket | `storage.bucket` |

**Example:**
```json
{
  "id": "aws::my-app-assets-bucket",
  "type": "storage.bucket",
  "name": "application-assets",
  "provider": {
    "name": "aws",
    "type": "AWS::S3::Bucket",
    "native_id": "my-app-assets-bucket",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "versioning_enabled": true,
    "public_access": false,
    "encryption": "SSE-S3",
    "lifecycle_rules": true
  },
  "status": "active"
}
```


### 7.4.3 File storage

**Type:** `storage.filesystem`

**Definition:**
A file storage system provides file-based storage accessible over network protocols (NFS, SMB/CIFS). File systems organize data in hierarchical directories and support file-level operations.

**When to use:**
Use `storage.filesystem` for:
- AWS EFS (Elastic File System)
- Azure Files
- GCP Filestore
- NFS exports
- SMB/CIFS shares

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `protocol` | string or array | Access protocol | `"nfs"`, `["nfs", "smb"]` |
| `size_gb` | integer | Capacity | `1024` |
| `performance_mode` | string | Performance tier | `"general-purpose"`, `"max-io"` |

**Example:**
```json
{
  "id": "aws::fs-0abc123",
  "type": "storage.filesystem",
  "name": "shared-application-data",
  "provider": {
    "name": "aws",
    "type": "AWS::EFS::FileSystem",
    "native_id": "fs-0abc123",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "protocol": "nfs",
    "size_gb": 2048,
    "performance_mode": "general-purpose",
    "encrypted": true
  },
  "status": "active"
}
```


### 7.4.4 Storage systems

**Type:** `storage.array`

**Definition:**
A storage array is a dedicated hardware system that provides centralized storage services. Arrays aggregate multiple disks and provide advanced features like RAID, snapshots, replication and tiered storage.

**When to use:**
Use `storage.array` for:
- SAN arrays (45Drives, NetApp, Pure Storage)
- NAS appliances
- Dedicated storage systems

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `manufacturer` | string | Storage vendor | `"45Drives"`, `"Pure Storage"` |
| `model` | string | Array model | `"storinator-xl60"` |
| `capacity_tb` | integer | Total capacity | `100` |
| `protocols` | array | Supported protocols | `["zfs", "iscsi", "fc"]` |

**Example:**
```json
{
  "id": "45drives::serial-123456",
  "type": "storage.array",
  "name": "storinator-xl60-01",
  "provider": {
    "name": "45drives",
    "model": "Storinator XL60",
    "native_id": "serial-123456"
  },
  "properties": {
    "manufacturer": "45Drives",
    "model": "Storinator XL60",
    "capacity_tb": 120,
    "protocols": ["zfs", "iscsi", "fc"],
    "version": "ceph 10.2.0",
    "management_ip": "10.0.20.10"
  },
  "status": "active"
}
```

---

## 7.5 Network resources
Network resources provide connectivity, routing, security and traffic management. This family includes virtual networks, physical network devices, security constructs, load balancers and network interfaces.


### 7.5.1 Virtual networks and subnets

#### Virtual Private Cloud or Virtual Network

**Type:** `network.vpc`

**Definition:**
A virtual private cloud (VPC) or virtual network (VNet) is an isolated network segment within a cloud or hyperscaler provider's infrastructure. VPCs provide logical network isolation with configurable IP address ranges, subnets, routing tables and security policies.

**When to use:**
Use `network.vpc` for:
- AWS VPCs
- Azure Virtual Networks (VNets)
- GCP VPC Networks
- Any cloud-native virtual network construct

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `cidr` | string or array | IP address range(s) | `"10.0.0.0/16"` |
| `dns_enabled` | boolean | DNS resolution enabled | `true` |
| `ipv6_enabled` | boolean | IPv6 support | `false` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | `AWS::EC2::VPC` | `network.vpc` |
| Azure | `Microsoft.Network/virtualNetworks` | `network.vpc` |
| GCP | `compute#network` | `network.vpc` |

**Example:**
```json
{
  "id": "aws::vpc-0abc123def4567890",
  "type": "network.vpc",
  "name": "production-vpc",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::VPC",
    "native_id": "vpc-0abc123def4567890",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "cidr": "10.0.0.0/16",
    "dns_enabled": true,
    "dns_hostnames": true,
    "ipv6_enabled": false
  },
  "status": "active"
}
```

#### Subnet

**Type:** `network.subnet`

**Definition:**
A subnet is a subdivided IP address range within a larger network. Subnets provide logical segmentation and may be associated with availability zones, routing tables or security policies.

**When to use:**
Use `network.subnet` for:
- Cloud subnets (AWS Subnets, Azure Subnets, GCP Subnetworks)
- VLAN segments (when modeling layer-3 subnets, not layer-2 VLANs)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `cidr` | string | Subnet IP range | `"10.0.1.0/24"` |
| `availability_zone` | string | Availability zone | `"us-east-1a"` |
| `public` | boolean | Internet-routable | `true` |

**Example:**
```json
{
  "id": "aws::subnet-0xyz789",
  "type": "network.subnet",
  "name": "public-subnet-1a",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Subnet",
    "native_id": "subnet-0xyz789",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "cidr": "10.0.1.0/24",
    "availability_zone": "us-east-1a",
    "public": true
  },
  "status": "active"
}
```

#### VLAN

**Type:** `network.vlan`

**Definition:**
A Virtual LAN (VLAN) is a layer-2 network segment that logically partitions a physical network. VLANs isolate broadcast domains and enable network segmentation without physical separation.

**When to use:**
Use `network.vlan` for:
- IEEE 802.1Q VLANs
- Physical switch VLANs
- On-premise network segments (when VLAN ID is the defining characteristic)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `vlan_id` | integer | VLAN identifier (1-4094) | `100` |
| `name` | string | VLAN name | `Office` |
| `subnet` | string | Associated IP subnet | `"10.100.0.0/24"` |

**Example:**
```json
{
  "id": "cisco::100",
  "type": "network.vlan",
  "name": "Production VLAN",
  "provider": {
    "name": "cisco",
    "type": "VLAN",
    "native_id": "100"
  },
  "properties": {
    "vlan_id": 100,
    "vlan_name": "office",
    "subnet": "10.100.0.0/24",
    "description": "Production network segment"
  },
  "status": "active"
}
```


### 7.5.2 Network devices

#### Router

**Type:** `network.router`

**Definition:**
A router is a network device that forwards packets between networks based on IP routing tables. Routers operate at layer 3 and make path decisions based on destination IP addresses.

**When to use:**
Use `network.router` for:
- Physical routers (Cisco, Ciena, Nokia)
- Virtual routers (cloud router gateways, virtual router appliances)
- Layer-3 routing devices

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `manufacturer` | string | Device vendor | `"Cisco"`, `"Ciena"` |
| `model` | string | Device model | `"ASR 1001-X"` |
| `version` | string | OS/firmware version | `"IOS XE 17.9.3"` |
| `routing_protocol` | array | Enabled protocols | `["bgp", "ospf"]` |

**Example:**
```json
{
  "id": "cisco::serial-ABC123",
  "type": "network.router",
  "name": "edge-router-01",
  "provider": {
    "name": "cisco",
    "type": "ASR 1001-X",
    "native_id": "serial-ABC123"
  },
  "properties": {
    "manufacturer": "Cisco",
    "model": "ASR 1001-X",
    "version": "IOS XE 17.9.3",
    "routing_protocol": ["bgp", "ospf"],
    "management_ip": "10.0.10.1"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "floor": "1",
    "rack": "R01"
  }
}
```


#### Router port

**Type:** `network.router.port`

**Definition:**
A router port (also called router interface) is a physical or logical connection point on a router used to connect to networks or other network devices. Router ports operate at Layer 3, have IP addresses assigned and participate in routing protocols.

**When to use:**
Use `network.router.port` for:
- Physical router interfaces (GigabitEthernet, TenGigabitEthernet, Serial)
- Logical interfaces (Loopback, Tunnel, VLAN interfaces)
- WAN interfaces
- Subinterfaces for VLAN trunking

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `interface_name` | string | Port/interface identifier | `"GigabitEthernet0/0/1"` |
| `ip_address` | string | IPv4 address | `"192.168.1.1"` |
| `subnet_mask` | string | Subnet mask or prefix | `"255.255.255.0"`, `"/24"` |
| `admin_status` | string | Administrative state | `"up"`, `"down"` |
| `oper_status` | string | Operational state | `"up"`, `"down"` |
| `speed_mbps` | integer | Interface speed | `1000`, `10000` |
| `duplex` | string | Duplex mode | `"full"`, `"half"`, `"auto"` |
| `encapsulation` | string | Encapsulation type | `"dot1q"`, `"ppp"`, `"hdlc"` |
| `vlan_id` | integer | VLAN ID (if applicable) | `100` |
| `description` | string | Interface description | `"Link to Core Switch"` |

**Example:**
```json
{
  "id": "cisco::Ethernet1",
  "type": "network.router.port",
  "name": "edge-router-uplink",
  "provider": {
    "name": "cisco",
    "type": "Router Interface",
    "native_id": "Ethernet1"
  },
  "properties": {
    "interface_name": "Ethernet1",
    "ip_address": "10.130.100.1",
    "subnet_mask": "255.255.255.252",
    "admin_status": "up",
    "oper_status": "up",
    "speed_mbps": 10000,
    "duplex": "full",
    "mtu": 9214,
    "description": "Uplink to ISP"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "floor": "1",
    "rack": "R01",
    "device": "edge-router-01"
  }
}
```


#### Switch

**Type:** `network.switch`

**Definition:**
A network switch is a device that connects devices within a network segment and forwards frames based on MAC addresses. Switches operate primarily at layer 2, though many modern switches provide layer-3 capabilities.

**When to use:**
Use `network.switch` for:
- Physical switches (Cisco Catalyst, Arista DCS)
- Layer-2 switching devices
- Spine switches
- Leaf switches
- Top-of-rack (ToR) switches

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `manufacturer` | string | Device vendor | `"Arista"`, `"Cisco"`, `"Ciena"`, `"Nokia"` |
| `model` | string | Device model | `"DCS-7050SX3-48YC12"` |
| `version` | string | OS version | `"EOS 4.31.2F"` |
| `port_count` | integer | Number of ports | `48` |
| `layer3_capable` | boolean | Layer-3 routing support | `true` |

**Example:**
```json
{
  "id": "arista::serial-XYZ789",
  "type": "network.switch",
  "name": "leaf-switch-01",
  "provider": {
    "name": "arista",
    "type": "DCS-7050SX3-48YC12",
    "native_id": "serial-XYZ789"
  },
  "properties": {
    "manufacturer": "Arista",
    "model": "DCS-7050SX3-48YC12",
    "version": "EOS 4.31.2F",
    "port_count": 48,
    "layer3_capable": true,
    "management_ip": "10.0.10.15"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "floor": "1",
    "rack": "R01"
  }
}
```


#### Switch port

**Type:** `network.switch.port`

**Definition:**
A switch port is a physical or logical connection point on a network switch used to connect end devices or other network equipment. Switch ports operate primarily at Layer 2, forwarding frames based on MAC addresses, though some ports may support Layer 3 functionality on capable switches.

**When to use:**
Use `network.switch.port` for:
- Physical switch interfaces (Ethernet, GigabitEthernet, TenGigabitEthernet)
- Access ports (connecting end devices)
- Trunk ports (carrying multiple VLANs)
- Port-channel/LAG members
- SFP/QSFP transceiver ports

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `interface_name` | string | Port identifier | `"Ethernet1/1"`, `"GigabitEthernet0/1"` |
| `port_mode` | string | Access or trunk mode | `"access"`, `"trunk"` |
| `vlan` | integer or array | VLAN assignment | `100`, `[10, 20, 30]` |
| `native_vlan` | integer | Native VLAN (trunk mode) | `1` |
| `admin_status` | string | Administrative state | `"up"`, `"down"` |
| `oper_status` | string | Operational state | `"up"`, `"down"` |
| `speed_mbps` | integer | Port speed | `1000`, `10000`, `100000` |
| `duplex` | string | Duplex mode | `"full"`, `"half"`, `"auto"` |
| `spanning_tree_state` | string | STP state | `"forwarding"`, `"blocking"` |
| `poe_enabled` | boolean | Power over Ethernet | `true`, `false` |
| `description` | string | Port description | `"Server-01 NIC1"` |

**Example:**
```json
{
  "id": "arista::Ethernet48",
  "type": "network.switch.port",
  "name": "server-connection-r01-u42",
  "provider": {
    "name": "arista",
    "type": "Switch Port",
    "native_id": "Ethernet48"
  },
  "properties": {
    "interface_name": "Ethernet48",
    "port_mode": "access",
    "vlan": 100,
    "admin_status": "up",
    "oper_status": "up",
    "speed_mbps": 10000,
    "duplex": "full",
    "mtu": 9214,
    "spanning_tree_state": "forwarding",
    "poe_enabled": false,
    "description": "Connection to Dell PowerEdge R770"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "floor": "1",
    "rack": "R01",
    "device": "leaf-switch-01"
  }
}
```


### 7.5.3 Network interfaces and endpoints

**Type:** `network.interface`

**Definition:**
A network interface is a connection point between a resource and a network. Interfaces have IP addresses, MAC addresses and network configuration properties.

**When to use:**
Use `network.interface` for:
- Virtual machine NICs (AWS ENI, Azure vNIC)
- Physical server network adapters
- Container network interfaces

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `mac_address` | string | Hardware address | `"00:1A:2B:3C:4D:5E"` |
| `ip_addresses` | array | Assigned IPs | `["10.0.1.15", "10.0.1.16"]` |
| `interface_type` | string | NIC type | `"primary"`, `"secondary"` |

**Example:**
```json
{
  "id": "aws::eni-0abc123",
  "type": "network.interface",
  "name": "web-server-primary-nic",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::NetworkInterface",
    "native_id": "eni-0abc123",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "mac_address": "0a:1b:2c:3d:4e:5f",
    "ip_addresses": ["10.0.1.15"],
    "interface_type": "primary"
  },
  "status": "active"
}
```


### 7.5.4 Network Load balancing

**Type:** `network.loadbalancer`

**Definition:**
A load balancer distributes incoming network traffic across multiple backend targets (servers, containers, functions) to ensure availability, scalability and fault tolerance.

**When to use:**
Use `network.loadbalancer` for:
- Cloud load balancers (AWS ALB/NLB, Azure Load Balancer, GCP Load Balancer)
- Hardware load balancers (F5, Citrix ADC, Kemp)
- Software load balancers (HAProxy, NGINX)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `load_balancer_type` | string | LB type | `"application"`, `"network"`, `"gateway"` |
| `scheme` | string | Accessibility | `"internet-facing"`, `"internal"` |
| `protocol` | string or array | Supported protocols | `"http"`, `["http", "https"]` |
| `algorithm` | string | Distribution algorithm | `"round-robin"`, `"least-connections"` |

**Provider mappings:**
| Provider | Native Type | Maps to OSIRIS |
|----------|-------------|----------------|
| AWS | ALB / NLB / GLB | `network.loadbalancer` |
| Azure | Azure Load Balancer | `network.loadbalancer` |
| GCP | Cloud Load Balancing | `network.loadbalancer` |
| F5 | BIG-IP | `network.loadbalancer` |

**Example:**
```json
{
  "id": "aws::web-application-lb",
  "type": "network.loadbalancer",
  "name": "web-application-lb",
  "provider": {
    "name": "aws",
    "type": "AWS::ElasticLoadBalancingV2::LoadBalancer",
    "native_id": "web-application-lb",
    "region": "us-east-1",
    "account": "123456789012",
    "arn": "arn:aws:elasticloadbalancing:us-east-1:123456789012:loadbalancer/app/web-application-lb/1234567890abcdef"
  },
  "properties": {
    "load_balancer_type": "application",
    "scheme": "internet-facing",
    "protocol": ["http", "https"],
    "dns_name": "web-lb-123456789.us-east-1.elb.amazonaws.com"
  },
  "status": "active"
}
```


### 7.5.5 Network security

#### Firewalls

**Type:** `network.firewall`

**Definition:**
A firewall is a security device or service that filters network traffic based on rules. Firewalls inspect packets and enforce access control policies to protect networks from unauthorized access.

**When to use:**
Use `network.firewall` for:
- Physical firewall appliances (Palo Alto, Checkpoint, Cisco ASA)
- Cloud firewalls (AWS Network Firewall, Azure Firewall)
- Virtual firewall appliances

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `firewall_type` | string | Deployment model | `"hardware"`, `"virtual"`, `"cloud-native"` |
| `manufacturer` | string | Device vendor | `"Palo Alto"`, `"Check Point"`, `"Cisco"` |
| `model` | string | Device model | `"PA-5220"` |
| `version` | string | Firmware/OS version | `"PAN-OS 11.0.3"` |

**Example:**
```json
{
  "id": "paloalto::serial-PAN5220-001",
  "type": "network.firewall",
  "name": "edge-firewall-primary",
  "provider": {
    "name": "paloalto",
    "type": "PA-5220",
    "native_id": "serial-PAN5220-001"
  },
  "properties": {
    "firewall_type": "hardware",
    "manufacturer": "Palo Alto Networks",
    "model": "PA-5220",
    "version": "PAN-OS 11.0.3",
    "ha_enabled": true,
    "management_ip": "10.0.10.20"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "floor": "1",
    "rack": "R05"
  }
}
```

#### Security group

**Type:** `network.security.group`

**Definition:**
A security group is a classic Hyperscaler virtual firewall that controls inbound and outbound traffic for cloud resources. Security groups use rule-based policies to permit or deny traffic based on protocol, port and source/destination.

**When to use:**
Use `network.security.group` for:
- AWS Security Groups
- Azure Network Security Groups (NSGs)
- GCP Firewall Rules (when modeled as rule groups)

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `direction` | string | Traffic direction | `"ingress"`, `"egress"`, `"both"` |
| `rule_count` | integer | Number of rules | `5` |

**Example:**
```json
{
  "id": "aws::sg-0abc123",
  "type": "network.security.group",
  "name": "web-tier-sg",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::SecurityGroup",
    "native_id": "sg-0abc123",
    "region": "us-east-1",
    "account": "123456789012"
  },
  "properties": {
    "direction": "both",
    "rule_count": 3,
    "description": "Security group for web tier"
  },
  "status": "active"
}
```

---

## 7.6 Operational Technology resources
Operational Technology (OT) resources represent physical infrastructure systems: building automation, physical security, power distribution, industrial control and physical infrastructure. This section start drafting the bridges between IT and OT domains.


### 7.6.1 Building automation

#### Air Handling Unit

**Type:** `building.hvac.ahu`

**Definition:**
An Air Handling Unit (AHU) is a large HVAC device that conditions and circulates air as part of a building's heating, ventilation and air conditioning system. AHUs typically contain fans, filters, heating/cooling coils and dampers.

**When to use:**
Use `building.hvac.ahu` for:
- Commercial building air handlers
- Data center air handling systems
- Large-scale HVAC units with integrated controls

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `manufacturer` | string | Equipment vendor | `"Carrier"`, `"Trane"` |
| `model` | string | Equipment model | `"39M"` |
| `capacity_cfm` | integer | Airflow capacity (cubic feet/minute) | `12000` |
| `protocol` | string | Control protocol | `"bacnet"`, `"modbus"` |
| `bacnet_device_id` | integer | BACnet device identifier | `100201` |

**Example:**
```json
{
  "id": "carrier::AHU-F1-M01",
  "type": "building.hvac.ahu",
  "name": "Floor 1 Main AHU",
  "provider": {
    "name": "carrier",
    "type": "39M-120",
    "native_id": "AHU-F1-M01"
  },
  "properties": {
    "manufacturer": "Carrier",
    "model": "39M-120",
    "capacity_cfm": 12000,
    "protocol": "bacnet",
    "bacnet_device_id": 100201,
    "bacnet_network": 1,
    "ip_address": "10.20.10.30"
  },
  "status": "active",
  "location": {
    "building": "MXP",
    "floor": "1",
    "room": "Mechanical room"
  }
}
```

#### HVAC Zone/VAV

**Type:** `building.hvac.vav`

**Definition:**
A Variable Air Volume (VAV) box is a terminal device that controls airflow to individual zones within a building. VAVs modulate air volume based on zone temperature demands.

**When to use:**
Use `building.hvac.vav` for:
- VAV terminal units
- Zone damper controllers
- Individual zone HVAC controls

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `zone_name` | string | Controlled zone | `"Conference Room A"` |
| `min_cfm` | integer | Minimum airflow | `100` |
| `max_cfm` | integer | Maximum airflow | `800` |

**Example:**
```json
{
  "id": "trane::VAV-F2-201",
  "type": "building.hvac.vav",
  "name": "Room 201 VAV",
  "provider": {
    "name": "trane",
    "type": "VAV Box",
    "native_id": "VAV-F2-201"
  },
  "properties": {
    "zone_name": "Office 201",
    "min_cfm": 150,
    "max_cfm": 1000,
    "protocol": "bacnet",
    "bacnet_device_id": 100301
  },
  "status": "active",
  "location": {
    "building": "MXP",
    "floor": "2",
    "room": "201"
  }
}
```

#### Chiller/Boiler

**Type:** `building.hvac.chiller` / `building.hvac.boiler`

**Definition:**
A chiller produces chilled water for cooling systems. A boiler produces hot water or steam for heating systems. Both are central plant equipment that serve multiple zones or buildings.

**When to use:**
Use these types for:
- Central chiller plants
- Boiler systems
- District heating/cooling equipment

**Example (Chiller):**
```json
{
  "id": "trane::CHLR-01",
  "type": "building.hvac.chiller",
  "name": "Central Chiller #1",
  "provider": {
    "name": "trane",
    "type": "CVHE-400",
    "native_id": "CHLR-01"
  },
  "properties": {
    "manufacturer": "Trane",
    "model": "CVHE-400",
    "capacity_tons": 400,
    "refrigerant": "R-134a",
    "protocol": "bacnet",
    "bacnet_device_id": 100001
  },
  "status": "active",
  "location": {
    "building": "MXP",
    "floor": "Basement",
    "room": "Central Plant"
  }
}
```


### 7.6.2 Physical security

#### Access control panel

**Type:** `security.access.panel`

**Definition:**
An access control panel is a hardware device that manages electronic door locks and reader devices. Panels authenticate credentials (cards, biometrics, PINs) and grant/deny physical access.

**When to use:**
Use `security.access.panel` for:
- Electronic access control systems (HID, Honeywell, Lenel)
- Badge reader controllers
- Door control panels

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `manufacturer` | string | Equipment vendor | `"HID"`, `"Honeywell"` |
| `model` | string | Panel model | `"VertX V100"` |
| `door_count` | integer | Controlled doors | `8` |
| `protocol` | string | Communication protocol | `"wiegand"`, `"osdp"` |

**Example:**
```json
{
  "id": "hid::ACP-F1-001",
  "type": "security.access.panel",
  "name": "Floor 1 Main Access Panel",
  "provider": {
    "name": "hid",
    "type": "VertX V100",
    "native_id": "ACP-F1-001"
  },
  "properties": {
    "manufacturer": "HID",
    "model": "VertX V100",
    "door_count": 8,
    "protocol": "wiegand",
    "ip_address": "10.30.1.10"
  },
  "status": "active",
  "location": {
    "building": "MXP",
    "floor": "1"
  }
}
```

#### Camera

**Type:** `security.camera`

**Definition:**
A security camera is a video surveillance device that captures and records visual information for monitoring, incident response and forensic analysis.

**When to use:**
Use `security.camera` for:
- IP cameras
- Analog cameras (when modeled in infrastructure)
- PTZ (pan-tilt-zoom) cameras

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `camera_type` | string | Camera category | `"fixed"`, `"ptz"`, `"dome"` |
| `resolution` | string | Video resolution | `"1920x1080"`, `"3840x2160"` |
| `stream_url` | string | Video stream endpoint | `"rtsp://10.30.2.15/stream"` |

**Example:**
```json
{
  "id": "axis::ACCC8E123456",
  "type": "security.camera",
  "name": "Main entrance camera",
  "provider": {
    "name": "axis",
    "type": "P3245-LVE",
    "native_id": "ACCC8E123456"
  },
  "properties": {
    "camera_type": "dome",
    "resolution": "1920x1080",
    "firmware_version": "11.0",
    "stream_url": "rtsp://10.30.2.15/axis-media/media.amp",
    "management_ip": "10.30.2.18",
    "poe_powered": true
  },
  "status": "active",
  "location": {
    "building": "MXP",
    "floor": "Ground",
    "description": "Main entrance"
  }
}
```


### 7.6.3 Power and environmental

#### UPS (Uninterruptible Power Supply)

**Type:** `power.ups`

**Definition:**
A UPS provides backup battery power to maintain uptime during electrical outages. UPS systems protect equipment from power disturbances and enable graceful shutdowns.

**When to use:**
Use `power.ups` for:
- Data center UPS systems
- Rack-mounted UPS units
- Building backup power systems

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `manufacturer` | string | Equipment vendor | `"APC"`, `"Eaton"` |
| `model` | string | UPS model | `"Smart-UPS SRT 10kVA"` |
| `capacity_kva` | integer | Power capacity | `10` |
| `runtime_minutes` | integer | Battery runtime at full load | `15` |
| `protocol` | string | Monitoring protocol | `"snmp"`, `"modbus"` |

**Example:**
```json
{
  "id": "apc::AS0123456789",
  "type": "power.ups",
  "name": "Rack 01 UPS",
  "provider": {
    "name": "apc",
    "type": "Smart-UPS SRT 10kVA",
    "native_id": "AS0123456789"
  },
  "properties": {
    "manufacturer": "APC",
    "model": "Smart-UPS SRT 10kVA",
    "capacity_kva": 10,
    "runtime_minutes": 20,
    "protocol": "snmp",
    "management_ip": "10.40.1.15"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "floor": "1",
    "rack": "R01"
  }
}
```

#### PDU (Power Distribution Unit)

**Type:** `power.pdu`

**Definition:**
A PDU distributes electrical power to multiple devices within a rack or equipment area. PDUs provide metering, monitoring and if supported remote outlet control and diagnostic.

**When to use:**
Use `power.pdu` for:
- Rack-mounted PDUs
- Intelligent PDUs with network monitoring and control
- Metered power strips

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `manufacturer` | string | Equipment vendor | `"APC"`, `"Vertiv"` |
| `model` | string | PDU model | `"AP8959"` |
| `outlet_count` | integer | Number of outlets | `16` |
| `max_amperage` | integer | Maximum current (amps) | `30` |
| `metered` | boolean | Power metering capability | `true` |

**Example:**
```json
{
  "id": "apc::PDU-R01-A",
  "type": "power.pdu",
  "name": "Rack 01 PDU A",
  "provider": {
    "name": "apc",
    "type": "AP8959",
    "native_id": "PDU-R01-A"
  },
  "properties": {
    "manufacturer": "APC",
    "model": "AP8959",
    "outlet_count": 16,
    "max_amperage": 30,
    "voltage": 208,
    "metered": true,
    "managed": true,
    "management_ip": "10.40.1.20"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "floor": "1",
    "rack": "R01",
    "position": "A"
  }
}
```


### 7.6.4 Industrial control systems

#### PLC (Programmable Logic Controller)

**Type:** `industrial.plc`

**Definition:**
A PLC is a ruggedized industrial computer that controls manufacturing processes, machinery and automation systems. PLCs execute ladder logic programs and interface with sensors and actuators.

**When to use:**
Use `industrial.plc` for:
- Factory automation controllers
- Process control systems
- Industrial automation equipment

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `manufacturer` | string | Equipment vendor | `"Siemens"`, `"Allen-Bradley"` |
| `model` | string | PLC model | `"S7-1500"` |
| `protocol` | string | Communication protocol | `"modbus"`, `"profinet"` |
| `io_points` | integer | Total I/O points | `128` |

**Example:**
```json
{
  "id": "siemens::PLC-LINE01",
  "type": "industrial.plc",
  "name": "Production Line 1 PLC",
  "provider": {
    "name": "siemens",
    "type": "S7-1500",
    "native_id": "PLC-LINE01"
  },
  "properties": {
    "manufacturer": "Siemens",
    "model": "S7-1500",
    "protocol": "profinet",
    "io_points": 256,
    "firmware_version": "V2.9.3",
    "ip_address": "192.168.100.10"
  },
  "status": "active",
  "location": {
    "facility": "Manufacturing Plant",
    "line": "Production Line 1"
  }
}
```

#### SCADA System

**Type:** `industrial.scada`

**Definition:**
A SCADA (Supervisory Control and Data Acquisition) system monitors and controls industrial processes at scale. SCADA systems aggregate data from sensors and PLCs and provide centralized visualization and control.

**When to use:**
Use `industrial.scada` for:
- Industrial control systems
- Utility monitoring systems (water, power, gas)
- Process visualization and control platforms

**Example:**
```json
{
  "id": "wonderware::SCADA-MASTER",
  "type": "industrial.scada",
  "name": "Plant Master SCADA",
  "provider": {
    "name": "wonderware",
    "type": "System Platform",
    "native_id": "SCADA-MASTER"
  },
  "properties": {
    "software": "Wonderware System Platform",
    "version": "2023",
    "node_count": 12,
    "tag_count": 5000,
    "redundancy": "hot-standby"
  },
  "status": "active",
  "location": {
    "facility": "Manufacturing Plant",
    "room": "Control Room"
  }
}
```


### 7.6.5 Physical infrastructure
Physical infrastructure resources represent the physical locations and structures that house IT and OT equipment. These resources form the foundation of the location hierarchy used throughout OSIRIS.

#### Data center

**Type:** `physical.datacenter`

**Definition:**
A data center (or datacenter) is a facility used to house computer systems, networking equipment, storage systems and associated components. Data centers provide power distribution, cooling, physical security and network connectivity required for IT operations.

**When to use:**
Use `physical.datacenter` for:
- Entire datacenter facilities
- Colocation facilities
- Enterprise data centers
- Edge data centers

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `tier` | string | Uptime Institute tier | `"Tier I"`, `"Tier II"`, `"Tier III"`, `"Tier IV"`, `"Tier IV Gold"` |
| `total_sqft` | integer | Total facility area | `10000` |
| `power_capacity_kw` | integer | Total power capacity | `5000` |
| `cooling_capacity_tons` | integer | Cooling capacity | `800` |
| `rack_count` | integer | Total racks | `120` |
| `certifications` | array | Compliance certifications | `["ISO27001", "SOC2"]` |

**Example:**
```json
{
  "id": "custom::MXP-DC-01",
  "type": "physical.datacenter",
  "name": "Milan Primary Datacenter",
  "provider": {
    "name": "custom",
    "type": "Enterprise Datacenter",
    "native_id": "MXP-DC-01",
    "namespace": "acme-infrastructure"
  },
  "properties": {
    "site_code": "MXP",
    "tier": "Tier III",
    "total_sqft": 15000,
    "power_capacity_kw": 3200,
    "cooling_capacity_tons": 600,
    "rack_count": 120,
    "certifications": ["ISO27001", "SOC2"],
    "address": "Via Example 123, Milan, Italy",
    "timezone": "Europe/Rome"
  },
  "status": "active",
  "location": {
    "city": "Milan",
    "country": "IT",
    "region": "EMEA"
  }
}
```


#### Buildings

**Type:** `physical.building`

**Definition:**
A building is a physical structure within a datacenter campus or facility. Buildings may contain multiple floors with equipment rooms, offices and support facilities.

**When to use:**
Use `physical.building` for:
- Individual buildings within datacenter campuses
- Multi-building facilities
- Separate structures within a site

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `building_number` | string | Building identifier | `"01"`, `"Building A"` |
| `total_floors` | integer | Number of floors | `3` |
| `total_sqft` | integer | Building area | `5000` |
| `power_capacity_kw` | integer | Building power capacity | `1500` |

**Example:**
```json
{
  "id": "custom::MXP-BLDG-01",
  "type": "physical.building",
  "name": "MXP Building 01",
  "provider": {
    "name": "custom",
    "type": "Datacenter Building",
    "native_id": "MXP-BLDG-01",
    "namespace": "acme-infrastructure"
  },
  "properties": {
    "building_number": "01",
    "total_floors": 2,
    "total_sqft": 8000,
    "power_capacity_kw": 2000,
    "primary_use": "datacenter"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP"
  }
}
```


#### Floors

**Type:** `physical.floor`

**Definition:**
A floor represents a level within a building. Floors contain rooms, equipment areas and infrastructure distribution systems.

**When to use:**
Use `physical.floor` for:
- Individual floors within buildings
- Raised floor areas
- Equipment levels

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `floor_number` | string or integer | Floor identifier | `1`, `"Ground"`, `"B1"` |
| `floor_sqft` | integer | Floor area | `4000` |
| `raised_floor` | boolean | Raised floor present | `true` |
| `ceiling_height_ft` | integer | Ceiling height | `12` |

**Example:**
```json
{
  "id": "custom::MXP-F1",
  "type": "physical.floor",
  "name": "MXP Floor 1",
  "provider": {
    "name": "custom",
    "type": "Datacenter Floor",
    "native_id": "MXP-F1",
    "namespace": "acme-infrastructure"
  },
  "properties": {
    "floor_number": 1,
    "floor_sqft": 4000,
    "raised_floor": true,
    "raised_floor_height_inches": 24,
    "ceiling_height_ft": 12,
    "cooling_type": "hot-aisle-containment"
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "building": "01"
  }
}
```


#### Rooms

**Type:** `physical.room`

**Definition:**
A room represents a physical space within a floor containing infrastructure equipment. Rooms may be server rooms, network closets, electrical rooms or mechanical spaces.

**When to use:**
Use `physical.room` for:
- Server rooms
- Network closets
- Equipment rooms
- Mechanical rooms
- Electrical rooms

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `room_number` | string | Room identifier | `"101"`, `"Server Room A"` |
| `room_sqft` | integer | Room area | `500` |
| `room_type` | string | Room purpose | `"server"`, `"network"`, `"mechanical"` |
| `cooling_capacity_tons` | integer | Cooling capacity | `20` |

**Example:**
```json
{
  "id": "custom::MXP-F1-R105",
  "type": "physical.room",
  "name": "Floor 1 Server Room 105",
  "provider": {
    "name": "custom",
    "type": "Server Room",
    "native_id": "MXP-F1-R105",
    "namespace": "acme-infrastructure"
  },
  "properties": {
    "room_number": "105",
    "room_sqft": 600,
    "room_type": "server",
    "cooling_capacity_tons": 25,
    "fire_suppression": "FM-200",
    "access_control": true
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "building": "01",
    "floor": "1"
  }
}
```


#### Racks

**Type:** `physical.rack`

**Definition:**
A rack is a standardized equipment enclosure (typically 19-inch or 23-inch wide) that houses servers, network devices, storage and other infrastructure hardware. Racks are measured in rack units (U).

**When to use:**
Use `physical.rack` for:
- Data center equipment racks
- Network equipment cabinets
- Server enclosures

**Common properties:**
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `height_units` | integer | Rack height in U (1U = 1.75") | `42` |
| `width_inches` | integer | Rack width | `19`, `23` |
| `manufacturer` | string | Rack vendor | `"Schneider Electric"`, `"Chatsworth"` |

**Example:**
```json
{
  "id": "schneider::MXP-F1-R01",
  "type": "physical.rack",
  "name": "Floor 1 Rack 01",
  "provider": {
    "name": "schneider",
    "type": "NetShelter SX 42U",
    "native_id": "MXP-F1-R01"
  },
  "properties": {
    "height_units": 42,
    "width_inches": 19,
    "depth_inches": 42,
    "manufacturer": "APC",
    "model": "NetShelter SX",
    "max_weight_lbs": 3000,
    "power_capacity_kva": 20
  },
  "status": "active",
  "location": {
    "datacenter": "MXP",
    "building": "01",
    "floor": "1",
    "room": "105",
    "row": "A",
    "position": "01"
  }
}
```


---

## 7.7 Type selection guidance
This section provides practical guidance for choosing appropriate resource types and understanding when to use standard types versus custom types.


### 7.7.1 Choosing appropriate types

**Decision framework:**
When assigning a type to a resource, follow this decision tree:

1. **Does a standard type exist that accurately describes this resource?**
   - YES > Use the standard type (sections 7.2–7.6)
   - NO > Continue to step 2

2. **Is there a closely related standard type?**
   - YES > Consider using the standard type with vendor specifics in `properties`
   - NO > Continue to step 3

3. **Is this a vendor-specific resource with no standard equivalent?**
   - YES > Use `osiris.<vendor>.<type>` namespaced type
   - NO > Use `osiris.<organization>.<type>` for organization-specific resources

**Semantic alignment:**
Choose the type that best represents the **resource's role and function**, not its implementation details:
```text
Good: compute.vm (even if it's a bare-metal instance acting like a VM)
Bad: osiris.aws.ec2.metal (overly specific)

Good: network.loadbalancer (for any load balancer)
Bad: network.alb, network.nlb.kemp (too vendor-specific)

Good: building.hvac.ahu (for any air handler)
Bad: milanmxp.keller.21c (building specific equipment model, not function)
```

**Balancing generals and specifics:**

- **Prefer general types** when the resource fits a common category
- **Use specific subtypes** when meaningful distinctions exist (e.g. `compute.vm.template` vs `compute.vm`)
- **Avoid excessive depth** - if the type hierarchy exceeds 4 segments, consider whether details belong in `properties`

**Examples:**
| Resource | Standard Type | Rationale |
|----------|---------------|-----------|
| AWS EC2 t3.medium | `compute.vm` | It's a virtual machine instance |
| Azure SQL Database | `application.database` | It's a managed database service |
| Cisco Nexus 9300 | `network.switch` | It's a network switch |
| AWS Lambda function | `compute.function.serverless` | Serverless compute function |
| Kubernetes Pod | `compute.container.pod` | Container orchestration unit |
| HID access panel | `security.access.panel` | Physical access control |
| BACnet VAV controller | `building.hvac.vav` | HVAC zone control |


### 7.7.2 When to use custom types

**Use custom types when:**
1. **No standard type exists**
   - The resource represents a vendor-specific service with unique semantics
   - Example: `osiris.aws.lambda.edge` for Lambda@Edge (distinct from standard Lambda)

2. **Semantics differ significantly**
   - Standard type exists but doesn't capture critical distinctions
   - Example: `osiris.azure.cosmosdb` for CosmosDB if multi-model API selection is critical

3. **Vendor-specific features are essential**
   - Preserving vendor identity is important for consumers
   - Example: `osiris.vmware.vsan` for vSAN storage (specific to VMware ecosystem)

4. **Organization-specific resources**
   - Internal infrastructure components
   - Example: `osiris.com.acme.widget` for proprietary equipment

**Namespace guidelines:**
| Use Case | Namespace Pattern | Example |
|----------|-------------------|---------|
| AWS-specific | `osiris.aws.*` | `osiris.aws.lambda.edge` |
| Azure-specific | `osiris.azure.*` | `osiris.azure.cosmosdb` |
| GCP-specific | `osiris.gcp.*` | `osiris.gcp.cloudrun` |
| Vendor-specific | `osiris.<vendor>.*` | `osiris.cisco.aci` |
| Organization | `osiris.com.<org>.*` | `osiris.com.acme.widget` |

**Documentation:**
When using custom types, producers **SHOULD**:
- Document the type semantics in generator documentation
- Provide examples showing typical usage
- Explain why standard types were insufficient
- Version the generator tool if type semantics change


### 7.7.3 Type mapping examples from well-known providers
This section provides guidance for mapping common provider resources to OSIRIS standard types.

**Provider naming:**
The examples below use canonical provider names as defined in Chapter 4, section 4.3.3. Producers **SHOULD** use consistent lowercase identifiers (e.g. `aws`, `azure`, `gcp`, `oci`, `ibm`, `ali`, `tc`) for well-known providers.

#### AWS Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| `AWS::EC2::Instance` | `compute.vm` | Virtual machine instances |
| `AWS::Lambda::Function` | `compute.function.serverless` | Serverless functions |
| `AWS::ECS::Service` | `application.service` | Container services |
| `AWS::EC2::VPC` | `network.vpc` | Virtual private cloud |
| `AWS::EC2::Subnet` | `network.subnet` | VPC subnets |
| `AWS::EC2::SecurityGroup` | `network.security.group` | Security groups |
| `AWS::ElasticLoadBalancingV2::LoadBalancer` | `network.loadbalancer` | ALB/NLB/GLB |
| `AWS::EC2::Volume` | `storage.volume` | EBS volumes |
| `AWS::S3::Bucket` | `storage.bucket` | Object storage |
| `AWS::EFS::FileSystem` | `storage.filesystem` | Elastic file system |
| `AWS::RDS::DBInstance` | `application.database` | Managed databases |
| `AWS::DynamoDB::Table` | `application.database` | NoSQL database |
| `AWS::SQS::Queue` | `application.queue` | Message queues |
| `AWS::Kinesis::Stream` | `application.eventstream` | Event streams |
| `AWS::ElastiCache::ReplicationGroup` | `application.cache` | Redis/Memcached |

#### Microsoft Azure Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| `Microsoft.Compute/virtualMachines` | `compute.vm` | Virtual machines |
| `Microsoft.Web/sites` (Function App) | `compute.function.serverless` | Azure Functions |
| `Microsoft.ContainerInstance/containerGroups` | `compute.container` | Container instances |
| `Microsoft.Network/virtualNetworks` | `network.vpc` | Virtual networks |
| `Microsoft.Network/subnets` | `network.subnet` | VNet subnets |
| `Microsoft.Network/networkSecurityGroups` | `network.security.group` | NSGs |
| `Microsoft.Network/loadBalancers` | `network.loadbalancer` | Load balancers |
| `Microsoft.Compute/disks` | `storage.volume` | Managed disks |
| `Microsoft.Storage/storageAccounts` (Blob) | `storage.bucket` | Blob storage |
| `Microsoft.Storage/storageAccounts` (Files) | `storage.filesystem` | Azure Files |
| `Microsoft.Sql/servers/databases` | `application.database` | SQL Database |
| `Microsoft.DocumentDB/databaseAccounts` | `application.database` | Cosmos DB |
| `Microsoft.ServiceBus/namespaces` | `application.queue` | Service Bus |
| `Microsoft.Cache/redis` | `application.cache` | Azure Cache for Redis |

#### Google Cloud Platform (GCP) Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| `compute#instance` | `compute.vm` | Compute Engine instances |
| `cloudfunctions#function` | `compute.function.serverless` | Cloud Functions |
| `run#service` | `compute.container` | Cloud Run services |
| `compute#network` | `network.vpc` | VPC networks |
| `compute#subnetwork` | `network.subnet` | Subnets |
| `compute#firewall` | `network.security.group` | Firewall rules |
| `compute#forwardingRule` | `network.loadbalancer` | Load balancers |
| `compute#disk` | `storage.volume` | Persistent disks |
| `storage#bucket` | `storage.bucket` | Cloud Storage |
| `file#instance` | `storage.filesystem` | Filestore |
| `sqladmin#instance` | `application.database` | Cloud SQL |
| `datastore#database` | `application.database` | Firestore |
| `pubsub#topic` | `application.eventstream` | Pub/Sub topics |
| `redis#instance` | `application.cache` | Memorystore Redis |

#### Oracle Cloud Infrastructure (OCI) Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| `oci_core_instance` | `compute.vm` | Compute instance |
| `oci_core_vcn` | `network.vpc` | Virtual Cloud Network |
| `oci_core_subnet` | `network.subnet` | Subnet within a VCN |
| `oci_core_network_security_group` | `network.security.group` | Network Security Group |
| `oci_load_balancer_load_balancer` | `network.loadbalancer` | Load balancer |
| `oci_core_volume` | `storage.volume` | Block storage volume |
| `oci_objectstorage_bucket` | `storage.bucket` | Object Storage bucket |
| `oci_file_storage_file_system` | `storage.filesystem` | File storage (FSS) |
| `oci_database_autonomous_database` | `application.database` | Autonomous or managed database |
| `oci_queue_queue` | `application.queue` | Queue service |
| `oci_streaming_stream` | `application.eventstream` | Streaming (Kafka-compatible) |

#### IBM Cloud Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| `ibm_is_instance` | `compute.vm` | VPC virtual server instance |
| `ibm_is_vpc` | `network.vpc` | Virtual Private Cloud |
| `ibm_is_subnet` | `network.subnet` | VPC subnet |
| `ibm_is_security_group` | `network.security.group` | Security group |
| `ibm_is_lb` | `network.loadbalancer` | Load balancer |
| `ibm_is_volume` | `storage.volume` | Block storage volume |
| `ibm_cos_bucket` | `storage.bucket` | Cloud Object Storage bucket |
| `ibm_database` | `application.database` | Managed database service |
| `ibm_resource_instance` | `application.eventstream` | IBM Event Streams (Kafka) instance |
| `ibm_event_streams_topic` | `application.eventstream` | Event Streams topic |

#### Alibaba Cloud Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| `ALIYUN::ECS::Instance` | `compute.vm` | ECS virtual machine instance |
| `ALIYUN::ECS::VPC` | `network.vpc` | Virtual Private Cloud |
| `ALIYUN::ECS::VSwitch` | `network.subnet` | Subnet (VSwitch) |
| `ALIYUN::ECS::SecurityGroup` | `network.security.group` | Security group |
| `ALIYUN::SLB::LoadBalancer` | `network.loadbalancer` | Server Load Balancer |
| `ALIYUN::ECS::Disk` | `storage.volume` | Block disk |
| `ALIYUN::OSS::Bucket` | `storage.bucket` | Object Storage Service bucket |
| `ALIYUN::NAS::FileSystem` | `storage.filesystem` | NAS file system |
| `ALIYUN::RDS::DBInstance` | `application.database` | Managed relational database |
| `ALIYUN::REDIS::Instance` | `application.cache` | Managed cache |
| `ALIYUN::MNS::Queue` | `application.queue` | Message Service queue |
| `ALIYUN::KAFKA::Instance` | `application.eventstream` | Kafka event stream |

#### Tencent Cloud Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| `tencentcloud_instance` | `compute.vm` | CVM compute instance |
| `tencentcloud_vpc` | `network.vpc` | Virtual Private Cloud |
| `tencentcloud_subnet` | `network.subnet` | Subnet |
| `tencentcloud_security_group` | `network.security.group` | Security group |
| `tencentcloud_clb_instance` | `network.loadbalancer` | CLB load balancer |
| `tencentcloud_cbs_storage` | `storage.volume` | Cloud Block Storage volume |
| `tencentcloud_cos_bucket` | `storage.bucket` | COS object storage bucket |
| `tencentcloud_mysql_instance` | `application.database` | Managed MySQL database |
| `tencentcloud_redis_instance` | `application.cache` | Managed Redis cache |
| `tencentcloud_ckafka_instance` | `application.eventstream` | Kafka event stream |

#### VMware vSphere Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| ESXi Host | `compute.server` | Hypervisor hosts |
| vSphere Cluster | `compute.cluster` | Compute clusters |
| Virtual Machine | `compute.vm` | ESXi VMs |
| VMDK | `storage.volume` | Virtual disks |
| Datastore | `storage.filesystem` | Shared storage |
| vSAN Datastore | `osiris.vmware.vsan` | VMware-specific storage (custom extension type) |
| Distributed Virtual Switch | `network.switch` | Virtual switches |
| Port Group | `network.vlan` | Virtual network segments |

#### Proxmox VE Resource mappings
| Provider Native Resource Type | OSIRIS Standard Type | Notes |
|-------------------|---------------------|-------|
| Proxmox Node | `compute.server` | Physical hypervisor host |
| LXC Container | `compute.container` | Linux container |
| Proxmox Cluster | `compute.cluster` | Multi-node cluster |
| QEMU VM | `compute.vm` | KVM virtual machine |
| Storage (dir/lvmthin/zfs/cephfs/rbd) | `storage.filesystem` | Storage backends |
| VM Disk (vm-100-disk-0) | `storage.volume` | Virtual machine disk |
| VM Network Interface | `network.interface` | Virtual NIC |


**Ambiguous mappings:**
Some resources may map to multiple types depending on context:

| Resource | Context | OSIRIS Type |
|----------|---------|-------------|
| Kubernetes Cluster | Compute pool | `compute.cluster` |
| Kubernetes Pod | Container group | `compute.container.pod` |
| Kubernetes Service | Network endpoint | `network.loadbalancer` or `application.service` |
| AWS RDS Read Replica | Separate database instance | `application.database` |
| AWS RDS Read Replica | Logical part of primary | Modeled via connections, not separate resource |

Producers **SHOULD** choose the mapping that best represents the resource's primary function and document their decision for consumers.

---

# 8 Extension mechanism
## 8.0 Overview
The OSIRIS extension mechanism enables producers to include vendor-specific, domain-specific or organization-specific data without fragmenting the core standard. Extensions preserve interoperability: consumers that do not recognize extension data can safely ignore it while still processing the core topology.

**Extension philosophy:**
- **Standard first:** Use standard fields and types whenever applicable
- **Extend selectively:** Extensions **SHOULD** be the exception, not the default
- **Preserve interoperability:** Unknown extensions must not break consumers
- **Document extensions:** Producers **SHOULD** document extension semantics for consumers

**Terminology:**
- **Extension:** Any data beyond the core OSIRIS specification
- **Namespace:** A prefix that identifies the source or domain of extension data (e.g. `osiris.aws`, `osiris.oci`, `osiris.com.acme`)
- **Standard fields:** Fields defined in this specification (e.g. `id`, `type`, `provider`, `properties`)
- **Custom fields:** Additional fields placed in the `extensions` object

---

## 8.1 Properties vs extensions
OSIRIS provides two mechanisms for including additional data beyond required fields: the `properties` object and the `extensions` object. Understanding when to use each is critical for maintaining interoperability.

### 8.1.1 Properties object
The `properties` object contains resource-specific attributes that describe generic infrastructure characteristics. Properties are **not namespaced** and represent common concepts that apply across vendors and platforms.

**Use properties for:**
- Generic infrastructure attributes (CPU count, RAM size, disk capacity)
- Network configuration (IP addresses, MAC addresses, VLANs)
- Physical characteristics (rack position, power consumption, dimensions)
- Common operational metadata (hostname, DNS name, serial number)

**Properties example:**
```json
{
  "id": "dell::MXP-SRV-001",
  "type": "compute.server",
  "provider": {
    "name": "dell",
    "type": "PowerEdge R770",
    "native_id": "MXP-SRV-001"
  },
  "properties": {
    "hostname": "mxp-srv-001.internal.osiris.com.acme",
    "rack_unit_start": 10,
    "rack_unit_height": 2,
    "power_consumption_watts": 450,
    "service_tag": "ABC1234",
    "cpu": {
      "model": "Intel Xeon Gold 6430",
      "cores": 32,
      "threads": 64,
      "count": 2
    },
    "ram_gb": 512
  }
}
```

**Properties are generic:** The fields above (hostname, rack position, CPU specs) are universally applicable. Any consumer can understand "rack_unit_start" regardless of whether the server is Dell, HPE, or Supermicro.


### 8.1.2 Extensions object
The `extensions` object contains **namespaced** vendor-specific, platform-specific or organization-specific data that extends beyond core OSIRIS semantics. Each top-level key in extensions **MUST** be a namespace string starting with `osiris.` and the value **MUST** be a JSON object.

**Use extensions for:**
- Vendor-specific features (AWS detailed monitoring, VMware Fault tolerance)
- Platform-specific identifiers (Azure subscription IDs, GCP project IDs)
- Proprietary configuration (vendor-specific management interfaces)
- Organization-specific metadata (cost centers, compliance tags, custom workflows)

**Extensions example:**
```json
{
  "id": "aws::i-0abc123def4567890",
  "type": "compute.vm",
  "provider": {
    "name": "aws",
    "type": "AWS::EC2::Instance",
    "native_id": "i-0abc123def4567890",
    "region": "us-east-1"
  },
  "properties": {
    "vcpus": 2,
    "memory_gb": 4,
    "ip_addresses": {
      "private_ip": ["10.0.1.10"]
    }
  },
  "extensions": {
    "osiris.aws": {
      "detailed_monitoring": true,
      "ebs_optimized": false,
      "placement": {
        "availability_zone": "us-east-1a",
        "tenancy": "default"
      },
      "iam_instance_profile": "arn:aws:iam::123456789012:instance-profile/web-server"
    },
    "osiris.custom.billing": {
      "cost_center": "engineering",
      "project_code": "web-platform-2024",
      "budget_owner": "alice@acme.example"
    }
  }
}
```

**Extensions are namespaced:** The `osiris.aws` prefix clearly identifies AWS-specific data, while `osiris.custom.billing` identifies organization-specific billing metadata.


### 8.1.3 Decision framework
When adding data to a resource, follow this decision tree:

```text
Is this data universally applicable across vendors?
+--YES > Use properties
+--NO  > Is this vendor/platform-specific?
    +--YES > Use extensions.osiris.<vendor>
    +--NO  > Is this organization-specific?
        +--YES > Use extensions.osiris.com.<org>.<domain>
```

**Examples:**
| Data | Location | Rationale |
|------|----------|-----------|
| CPU count | `properties.cpu.count` | Universal concept |
| RAM size | `properties.ram_gb` | Universal concept |
| AWS detailed monitoring | `extensions["osiris.aws"].detailed_monitoring` | AWS-specific feature |
| VMware Fault Tolerance | `extensions.osiris.vmware.fault_tolerance` | VMware-specific feature |
| Cost center | `extensions.osiris.com.acme.billing.cost_center` | Organization-specific |
| IP address | `properties.ip_addresses` | Universal concept |
| Azure subscription ID | `provider.subscription_id` | Provider metadata (not extension) |


### 8.1.4 Extension field structure
Extension objects **MUST** be nested under the `osiris.<namespace>` key. Producers **MUST NOT** place extension data directly under `extensions` without a namespace prefix.

**Correct:**
```json
"extensions": {
  "osiris.aws": {
    "detailed_monitoring": true
  }
}
```

**Incorrect:**
```json
"extensions": {
  "detailed_monitoring": true
}
```

**Rationale:** Namespacing prevents collision between extensions from different sources and enables consumers to selectively process extensions they understand.


### 8.1.4.1 Extension object key style
Extension objects **SHOULD** follow these key naming conventions:

**Recommended practices:**
- **Use snake_case for keys:** `detailed_monitoring`, `cost_center`, `project_code`
- **Prefer nested objects over dotted keys:** Safer for document databases (MongoDB, etc.)
- **Use consistent naming within a namespace:** Maintain style across all extensions in the same namespace

**GOOD example: Recommended style**
```json
"extensions": {
  "osiris.aws": {
    "detailed_monitoring": true,      
    "placement": {                     
      "availability_zone": "us-east-1a",
      "partition_number": null
    },
    "iam_instance_profile": {         
      "arn": "arn:aws:iam::123456789012:instance-profile/web",
      "id": "AIPAI23HZ27SI6FQMGNQ2"
    }
  }
}
```

**BAD example: Discouraged dotted keys**
```json
"extensions": {
  "osiris.aws": {
    "placement.availability_zone": "us-east-1a",
    "placement.partition_number": null
  }
}
```

**Rationale:** Dotted keys can cause issues with document databases that interpret dots as nesting operators. Using actual nested objects is safer and more widely compatible.

**Exception: Vendor-native shapes**
When exporting lossless representations of vendor APIs, producers **MAY** preserve the vendor's native key style (camelCase, PascalCase, etc.) if explicitly documented:

```json
"extensions": {
  "osiris.aws.raw": {
    "DetailedMonitoring": true,
    "EbsOptimized": false
  }
}
```

If using vendor-native shapes, producers **SHOULD**:
- Use a sub-namespace (e.g. `osiris.aws.raw`) to signal non-standard casing
- Document the preserved format
- Consider providing both normalized and raw representations


### 8.1.5 Multiple extension namespaces
A single resource **MAY** contain extensions from multiple namespaces. Each namespace is independent.

**Example:**
```json
{
  "id": "vmware::vm-web-001",
  "type": "compute.vm",
  "extensions": {
    "osiris.vmware": {
      "fault_tolerance": false,
      "tools_version": "12.0.0",
      "vmx_version": "19"
    },
    "osiris.com.acme.monitoring": {
      "alert_policy": "critical",
      "on_call_team": "operations"
    },
    "osiris.com.acme.compliance": {
      "pci_scope": true,
      "data_classification": "restricted"
    }
  }
}
```

Each namespace is processed independently. A consumer that understands `osiris.vmware` can use that data while safely ignoring `osiris.com.acme.monitoring` and `osiris.com.acme.compliance`.

> [!NOTE]
> This example uses organization-specific namespaces (`osiris.com.acme.*`) for production extensions. For short-lived experiments, producers **MAY** use `osiris.custom.*`, but should migrate to organization-specific namespaces when extensions stabilize.


### 8.1.6 Provider-specific metadata vs extensions
The `provider` object contains metadata about the resource's origin (provider name, native ID, region, account). The `extensions` object contains additional vendor-specific **properties or features** of the resource itself.

**Provider object (origin metadata):**
```json
"provider": {
  "name": "aws",
  "type": "AWS::EC2::Instance",
  "native_id": "i-0abc123",
  "region": "us-east-1",
  "account": "123456789012"
}
```

**Extensions object (vendor-specific features):**
```json
"extensions": {
  "osiris.aws": {
    "detailed_monitoring": true,
    "ebs_optimized": false
  }
}
```


**Rule of thumb:** If the data answers "where is this resource?" or "what is the provider's identifier?", it belongs in `provider`. If the data answers "what vendor-specific features does this resource have?", it belongs in `extensions`.


### 8.1.7 Forward compatibility
Consumers **MUST** accept resources with unknown extension namespaces. Consumers **MAY** ignore extension data they do not recognize.

Producers **SHOULD** ensure that core topology (resources, connections, groups) is understandable even if consumers ignore all extension data.

**Example:** A consumer that does not understand `osiris.aws` extensions can still process the resource:

```json
{
  "id": "aws::i-0abc123",
  "type": "compute.vm",
  "provider": { "name": "aws", "native_id": "i-0abc123" },
  "properties": { "vcpus": 2, "memory_gb": 4 },
  "extensions": {
    "osiris.aws": { "detailed_monitoring": true }
  }
}
```

The consumer can identify the resource (`aws::i-0abc123`), its type (`compute.vm`) and its basic properties (`vcpus`, `memory_gb`) without understanding AWS-specific extensions.

---

## 8.2 Vendor-specific extensions
Vendor-specific extensions are additional resource attributes that are meaningful only within a specific vendor's ecosystem. These attributes belong in the `extensions` object under the vendor's namespace.


### 8.2.1 Vendor namespace structure
Vendor namespaces follow the pattern `osiris.<vendor>`, where `<vendor>` is a canonical lowercase identifier for the platform or vendor.

A vendor namespace key in `extensions` **MUST** map to a JSON object.

**Well-known vendor namespaces examples (non-exhaustive):**

| Vendor | Namespace | Example Usage |
|--------|-----------|---------------|
| AWS | `osiris.aws` | `"extensions": { "osiris.aws": { "detailed_monitoring": true } }` |
| Azure | `osiris.azure` | `"extensions": { "osiris.azure": { "managed_disk_type": "Premium_LRS" } }` |
| GCP | `osiris.gcp` | `"extensions": { "osiris.gcp": { "preemptible": true } }` |
| VMware | `osiris.vmware` | `"extensions": { "osiris.vmware": { "fault_tolerance": false } }` |
| Proxmox | `osiris.proxmox` | `"extensions": { "osiris.proxmox": { "qemu_agent": true } }` |
| Arista | `osiris.arista` | `"extensions": { "osiris.arista": { "mlag_enabled": false } }` |
| Cisco | `osiris.cisco` | `"extensions": { "osiris.cisco": { "trustsec_enabled": true } }` |
| Dell | `osiris.dell` | `"extensions": { "osiris.dell": { "idrac_version": "5.10" } }` |

Producers **SHOULD** use consistent canonical identifiers for well-known vendors. Organization-specific namespaces **SHOULD** use domain-scoped namespaces (see section 8.4.3).


### 8.2.2 AWS specific extensions
AWS resources often have features specific to the AWS ecosystem. These belong under `osiris.aws`.

**Example: EC2 instance with AWS-specific properties**
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
    "account": "123456789012"
  },
  "properties": {
    "vcpus": 2,
    "memory_gb": 4,
    "instance_type": "t3.medium"
  },
  "extensions": {
    "osiris.aws": {
      "detailed_monitoring": true,
      "ebs_optimized": false,
      "ena_support": true,
      "hypervisor": "xen",
      "placement": {
        "availability_zone": "us-east-1a",
        "tenancy": "default",
        "partition_number": null
      },
      "iam_instance_profile": {
        "arn": "arn:aws:iam::123456789012:instance-profile/web-server",
        "id": "AIPAI23HZ27SI6FQMGNQ2"
      }
    }
  }
}
```


### 8.2.3 Azure specific extensions
Azure resources may include platform-specific metadata under `osiris.azure`.

**Example: Azure VM with platform-specific properties**
```json
{
  "id": "azure::/subscriptions/sub-123/resourceGroups/rg-prod/providers/Microsoft.Compute/virtualMachines/web-vm-01",
  "type": "compute.vm",
  "name": "web-vm-01",
  "provider": {
    "name": "azure",
    "type": "Microsoft.Compute/virtualMachines",
    "native_id": "/subscriptions/sub-123/resourceGroups/rg-prod/providers/Microsoft.Compute/virtualMachines/web-vm-01",
    "region": "eastus",
    "subscription_id": "sub-123",
    "resource_group": "rg-prod"
  },
  "properties": {
    "vm_size": "Standard_D2s_v3",
    "vcpus": 2,
    "memory_gb": 8
  },
  "extensions": {
    "osiris.azure": {
      "priority": "Regular",
      "eviction_policy": null,
      "license_type": "Windows_Server",
      "zones": ["1"],
      "managed_disk_type": "Premium_LRS"
    }
  }
}
```


### 8.2.4 GCP specific extensions
GCP resources often expose Google-specific features such as preemptibility/spot behavior, Shielded VM settings, service accounts and metadata flags. These belong under `osiris.gcp`.

**Example: GCE instance with GCP-specific properties**
```json
{
  "id": "gcp::projects/acme-prod/zones/us-central1-a/instances/web-01",
  "type": "compute.vm",
  "name": "web-01",
  "provider": {
    "name": "gcp",
    "type": "compute#instance",
    "native_id": "projects/acme-prod/zones/us-central1-a/instances/web-01",
    "region": "us-central1",
    "account": "acme-prod"
  },
  "properties": {
    "vcpus": 2,
    "memory_gb": 8,
    "machine_type": "e2-standard-2"
  },
  "extensions": {
    "osiris.gcp": {
      "zone": "us-central1-a",
      "preemptible": false,
      "spot": true,
      "termination_action": "STOP",
      "shielded_vm": {
        "secure_boot": true,
        "vtpm": true,
        "integrity_monitoring": true
      },
      "service_accounts": [
        {
          "email": "web-01@acme-prod.iam.gserviceaccount.com",
          "scopes": [
            "https://www.googleapis.com/auth/cloud-platform"
          ]
        }
      ],
      "scheduling": {
        "automatic_restart": false,
        "on_host_maintenance": "TERMINATE"
      },
      "labels": {
        "env": "prod",
        "app": "web"
      }
    }
  }
}
```


### 8.2.5 VMware specific extensions
VMware environments have specific features like Fault Tolerance, vMotion and DRS.

**Example: VMware VM with ESXi-specific properties**
```json
{
  "id": "vmware::vm-web-001",
  "type": "compute.vm",
  "name": "web-001",
  "provider": {
    "name": "vmware",
    "native_id": "vm-web-001",
    "region": "mxp-datacenter"
  },
  "properties": {
    "vcpus": 4,
    "memory_gb": 16,
    "guest_os": "ubuntu64Guest"
  },
  "extensions": {
    "osiris.vmware": {
      "tools_status": "toolsOk",
      "tools_version": "12.0.0",
      "fault_tolerance": false,
      "vmx_version": "19",
      "drs_behavior": "fullyAutomated",
      "cpu_hot_add_enabled": false,
      "memory_hot_add_enabled": false
    }
  }
}
```


### 8.2.6 Proxmox specific extensions
Proxmox environments have platform-specific capabilities and metadata such as VMID, node placement, HA state, QEMU machine type and storage backend details.

**Example: Proxmox VM with Proxmox-specific properties**
```json
{
  "id": "proxmox::vm-web-001",
  "type": "compute.vm",
  "name": "web-001",
  "provider": {
    "name": "proxmox",
    "native_id": "101",
    "region": "mxp-datacenter"
  },
  "properties": {
    "vcpus": 4,
    "memory_gb": 16,
    "guest_os": "ubuntu"
  },
  "extensions": {
    "osiris.proxmox": {
      "vmid": 101,
      "node": "pve-01",
      "ha_enabled": true,
      "ha_state": "started",
      "qemu_machine": "pc-q35-8.1",
      "bios": "ovmf",
      "tags": ["prod", "web"],
      "storage": {
        "backend": "ceph",
        "pool": "rbd",
        "volumes": [
          {
            "id": "vm-101-disk-0",
            "size_gb": 60,
            "type": "scsi"
          }
        ]
      }
    }
  }
}
```


### 8.2.7 On-premise vendor extensions
Physical infrastructure from vendors like Dell, Arista, or Cisco may include hardware-specific extensions.

**Example: Dell server with iDRAC management extensions**
```json
{
  "id": "dell::MXP-SRV-R01-001",
  "type": "compute.server",
  "name": "MXP-SRV-R01-001",
  "provider": {
    "name": "dell",
    "type": "PowerEdge R770",
    "native_id": "MXP-SRV-R01-001"
  },
  "properties": {
    "service_tag": "ABC1234",
    "rack_unit_start": 10,
    "cpu": {
      "model": "Intel Xeon Gold 6430",
      "cores": 32,
      "count": 2
    },
    "ram_gb": 512
  },
  "extensions": {
    "osiris.dell": {
      "idrac_version": "5.10.00.00",
      "idrac_ip": "10.0.100.10",
      "lifecycle_controller_version": "4.10.10.10",
      "bios_version": "2.15.0",
      "system_id": 2137
    }
  }
}
```

**Example: Arista switch with EOS-specific extensions**
```json
{
  "id": "arista::MXP-LEAF-01",
  "type": "network.switch",
  "name": "MXP-LEAF-01",
  "provider": {
    "name": "arista",
    "type": "DCS-7050X3-48YC12",
    "native_id": "MXP-LEAF-01"
  },
  "properties": {
    "management_ip": "10.0.10.11",
    "site": "mxp-datacenter",
    "rack": "R05",
    "rack_unit_start": 22,
    "interfaces": [
      { "name": "Ethernet1", "type": "physical", "speed": "100G", "admin_status": "up", "oper_status": "up" },
      { "name": "Ethernet2", "type": "physical", "speed": "100G", "admin_status": "up", "oper_status": "up" },
      { "name": "Ethernet49", "type": "physical", "speed": "25G", "admin_status": "up", "oper_status": "down" }
    ]
  },
  "extensions": {
    "osiris.arista": {
      "eos_version": "4.30.2F",
      "system_mac": "00:1c:73:aa:bb:cc",
      "serial_number": "JPE12345678",
      "mlag": {
        "enabled": true,
        "domain_id": "MLAG01",
        "peer_link": "Port-Channel1000",
        "peer_address": "192.0.2.2",
        "state": "active"
      },
      "vxlan": {
        "enabled": true,
        "vtep_ip": "10.0.255.11"
      },
      "hardware": {
        "supervisor": "fixed",
        "transceivers_present": 28
      }
    }
  }
}
```


### 8.2.8 Extension data types
Extension values **MAY** be any valid JSON type: strings, numbers, booleans, objects, arrays, or null. Producers **SHOULD** use appropriate JSON types that reflect the data semantics.

**Type examples:**
```json
"extensions": {
  "osiris.aws": {
    "detailed_monitoring": true,
    "placement_group": null,
    "cpu_credits": "unlimited",
    "network_performance": "Up to 5 Gigabit",
    "hibernation_configured": false,
    "metadata_options": {
      "http_tokens": "optional",
      "http_put_response_hop_limit": 1
    },
    "block_device_mappings": [
      {
        "device_name": "/dev/sda1",
        "ebs": {
          "volume_id": "vol-0abc123",
          "delete_on_termination": true
        }
      }
    ]
  }
}
```


### 8.2.9 Extension stability
Extension schemas **MAY** evolve independently of the core OSIRIS specification. Producers **SHOULD** maintain backward compatibility within an extension namespace when possible.

If breaking changes to an extension schema are unavoidable, producers **MAY** version the extension namespace:

```json
"extensions": {
  "osiris.aws.v1": { ... },
  "osiris.aws.v2": { ... }
}
```

Consumers **SHOULD** prefer the highest version they understand and gracefully handle unknown versions.

---

## 8.3 Custom resource types
Custom resource types enable producers to represent vendor-specific or organization-specific infrastructure that does not map cleanly to standard types defined in Chapter 7. Custom types use the `osiris.<namespace>` prefix to distinguish them from standard types.


### 8.3.1 When to create custom types
Producers **SHOULD** use standard resource types (Chapter 7) whenever possible. Custom types are appropriate when:

1. **No standard type exists** and the resource semantics are vendor-specific
2. **Standard type exists but semantics differ significantly**
3. **Vendor-specific features are essential** to understanding the resource
4. **Organization-specific infrastructure** requires custom representation

**Decision tree:**

```text
Does a standard type capture the resource's primary function?
+-- YES > Use the standard type
+-- NO  > Does a vendor-specific type exist for this resource?
    +-- YES > Use the vendor type (osiris.<vendor>.*)
    +-- NO  > Does this resource represent organization-specific infrastructure?
        +-- YES > Create custom type (osiris.com.<org>.*)
```


### 8.3.2 Custom type naming
Custom resource types **MUST** use the `osiris.<namespace>` prefix followed by dot-separated segments describing the resource type. Type identifiers **MUST** follow the same format rules as standard types (see Chapter 4, section 4.2.3):

- **MUST** be in lowercase letters and digits only
- **MUST** have dot (`.`) as the only separator
- **MUST** have no underscores, hyphens, or spaces
- **MUST** have no leading, trailing, or consecutive dots

**GOOD valid custom types:**
```text
osiris.aws.lambda.edge
osiris.azure.cosmosdb
osiris.vmware.vsan
osiris.cisco.aci.fabric
osiris.com.acme.widget.v2
```

**BAD invalid custom types:**
```text
osiris.aws.vpc_peering
osiris.azure.cosmos-db
osiris.Acme.Widget
.osiris.aws.lambda
osiris.aws..lambda
```


### 8.3.3 Vendor-specific types
Vendor-specific types use the pattern `osiris.<vendor>.*` where `<vendor>` is a well-known identifier.

**Example: AWS Lambda@Edge (distinct from standard serverless)**
```json
{
  "id": "aws::edge-func-auth",
  "type": "osiris.aws.lambda.edge",
  "name": "cloudfront-auth",
  "provider": {
    "name": "aws",
    "type": "AWS::Lambda::Function",
    "native_id": "edge-func-auth",
    "region": "us-east-1"
  },
  "properties": {
    "runtime": "python3.11",
    "memory_mb": 128
  },
  "extensions": {
    "osiris.aws": {
      "edge_region": "us-east-1",
      "cloudfront_distribution": "E1234567890ABC"
    }
  }
}
```

**Rationale:** Lambda@Edge executes at CloudFront edge locations with different semantics than standard Lambda functions. The custom type `osiris.aws.lambda.edge` explicitly signals these distinct semantics.

**Alternative approach:** If edge execution is a deployment detail rather than a fundamental semantic difference, producers **MAY** use the standard type `compute.function.serverless` and place edge-specific metadata in extensions:

```json
{
  "id": "aws::edge-func-auth",
  "type": "compute.function.serverless",
  "extensions": {
    "osiris.aws": {
      "execution_environment": "edge",
      "edge_region": "us-east-1"
    }
  }
}
```

Producers **SHOULD** document their type selection rationale.


### 8.3.4 Organization-specific types
Organizations **MAY** define custom types for proprietary or organization-specific infrastructure using reverse domain notation: `osiris.com.<organization>.*`.

**Example: Organization-specific monitoring appliance**
```json
{
  "id": "custom::monitoring-appliance-01",
  "type": "osiris.com.acme.monitoring.appliance",
  "name": "monitoring-appliance-01",
  "provider": {
    "name": "acme",
    "type": "Acme Monitoring Appliance v3",
    "native_id": "monitoring-appliance-01"
  },
  "properties": {
    "rack_unit_start": 5,
    "power_consumption_watts": 200
  },
  "extensions": {
    "osiris.com.acme": {
      "firmware_version": "3.2.1",
      "collector_count": 4,
      "retention_days": 90
    }
  }
}
```

**Reverse domain notation:**
| Organization | Domain | Custom Type Namespace |
|-------------|--------|----------------------|
| Acme Corp | acme.com | `osiris.com.acme.*` |
| Example Org | example.org | `osiris.org.example.*` |
| Research Institute | research.edu | `osiris.edu.research.*` |


### 8.3.5 Custom type documentation
Producers that use custom types **SHOULD** document:
- Type semantics and purpose
- When to use the custom type vs standard types
- Properties and extensions specific to the type
- Example usage

**Documentation example:**

```text
Type: osiris.com.acme.widget

Purpose: Represents Acme Corp's proprietary network widget appliance

Use this type when:
- The resource is an Acme Widget hardware appliance
- The appliance provides both routing and deep packet inspection

Standard type alternative:
- Use network.router if only routing functionality is relevant
- Use security.firewall if only DPI functionality is relevant

Required properties:
- firmware_version (string): Widget firmware version
- widget_mode (string): "router" | "firewall" | "hybrid"

Extensions (osiris.com.acme):
- license_key (string): Widget license key
- management_vlan (integer): Management VLAN ID
```


### 8.3.6 Consumer handling of custom types
Consumers **MUST** accept resources with unrecognized custom types. Consumers **MAY** treat unrecognized types as generic resources based on the namespace:

- `osiris.aws.*` > Assume AWS-specific resource
- `osiris.azure.*` > Assume Azure-specific resource
- `osiris.com.<org>.*` > Assume organization-specific resource

Consumers **SHOULD** preserve custom type information when re-exporting or transforming topology data.


### 8.3.7 Type stability and evolution
Custom types **MAY** evolve over time. Producers **SHOULD** maintain semantic stability within a custom type when possible.

If a custom type's semantics change significantly, producers **MAY**:
1. Create a new type with a version suffix (e.g. `osiris.aws.lambda.edge.v2`)
2. Deprecate the old type with a documented migration path
3. Support both types during a transition period

---

## 8.4 Namespacing
Namespace management prevents collision between extensions from different sources and enables safe evolution of the OSIRIS ecosystem.

#### Governance boundary (OSIRIS governed vs vendor defined)
OSIRIS governs:
- Namespace format rules (e.g. `osiris.<vendor>` and reverse-domain organization namespaces)
- Registry and collision-avoidance rules for well-known namespaces

OSIRIS does not govern:
- The internal structure and semantics of `extensions["osiris.<vendor>"]` payloads
- Provider-specific `state` values and other opaque vendor enums

Extension payloads may evolve independently of the OSIRIS specification. Consumers **MUST** treat unknown extension fields as opaque and **MUST** ignore what they do not understand.


### 8.4.1 Namespace format
Extension namespaces **MUST** follow the pattern `osiris.<identifier>` where `<identifier>` is a dot-separated lowercase string.

**Namespace format rules:**
- **MUST** start with `osiris.` prefix
- **MUST**  use lowercase letters, digits and dots only
- **MUST** have no underscores, hyphens, or spaces
- **MUST** have no consecutive dots
- **MUST** have minimum two segments (e.g. `osiris.aws`, not just `osiris`)

**GOOD examples of valid namespaces:**
```
osiris.aws
osiris.azure.devtest
osiris.com.acme
osiris.custom.billing
```

**BAD examples of invalid namespaces:**
```
aws
osiris.AWS
osiris.acme_corp
osiris.
osiris_
```


### 8.4.2 Registered well-known namespaces
The `osiris.` prefix is reserved by this specification for namespaces that follow OSIRIS naming rules and collision-avoidance conventions.

OSIRIS governs the **namespace rules** and (when applicable) the **registry** of well-known namespaces. OSIRIS does **not** govern the **internal semantics** of vendor or organization-specific payloads carried under these namespaces.

The following vendor namespaces are **registered** as well-known prefixes to encourage consistent producer behavior for infrastructure management systems (hyperscalers, cloud providers, virtualization, networking, compute, storage, OT/industrial platforms).

#### Hyperscalers and Cloud providers
| Namespace | Intended for | Status |
|-----------|--------------|--------|
| `osiris.aws` | Amazon Web Services | Registered |
| `osiris.azure` | Microsoft Azure | Registered |
| `osiris.gcp` | Google Cloud Platform | Registered |
| `osiris.oci` | Oracle Cloud Infrastructure | Registered |
| `osiris.ibm` | IBM Cloud | Registered |
| `osiris.ali` | Alibaba Cloud | Registered |
| `osiris.tc` | Tencent Cloud | Registered |
| `osiris.openstack` | OpenStack | Registered |
| `osiris.cloudflare` | Cloudflare | Registered |
| `osiris.digitalocean` | DigitalOcean | Registered |
| `osiris.linode` | Linode | Registered |
| `osiris.leaseweb` | Leaseweb | Registered |
| `osiris.ovh` | OVH | Registered |
| `osiris.hetzner` | Hetzner | Registered |
| `osiris.scaleway` | Scaleway | Registered |

#### Virtualization & HCI
| Namespace | Intended for | Status |
|-----------|--------------|--------|
| `osiris.vmware` | VMware vSphere / ESXi | Registered |
| `osiris.proxmox` | Proxmox VE | Registered |
| `osiris.nutanix` | Nutanix | Registered |

#### Networking & security
| Namespace | Intended for | Status |
|-----------|--------------|--------|
| `osiris.arista` | Arista Networks | Registered |
| `osiris.ciena` | Ciena | Registered |
| `osiris.cisco` | Cisco | Registered |
| `osiris.edgecore` | Edgecore Networks | Registered |
| `osiris.nokia` | Nokia (networking) | Registered |
| `osiris.juniper` | Juniper Networks | Registered |
| `osiris.hpearuba` | HPE Aruba | Registered |
| `osiris.paloalto` | Palo Alto Networks | Registered |
| `osiris.fortinet` | Fortinet | Registered |
| `osiris.checkpoint` | Check Point | Registered |
| `osiris.f5` | F5 Networks | Registered |

#### Compute & storage vendors
| Namespace | Intended for | Status |
|-----------|--------------|--------|
| `osiris.dell` | Dell Technologies | Registered |
| `osiris.hpe` | Hewlett Packard Enterprise | Registered |
| `osiris.lenovo` | Lenovo | Registered |
| `osiris.supermicro` | Supermicro | Registered |
| `osiris.netapp` | NetApp | Registered |
| `osiris.purestorage` | Pure Storage | Registered |
| `osiris.hitachi` | Hitachi Vantara | Registered |

#### Enterprise platforms & ITSM/CMDB
| Namespace | Intended for | Status |
|-----------|--------------|--------|
| `osiris.sap` | SAP (landscape/platform management) | Registered |
| `osiris.servicenow` | ServiceNow CMDB / ITSM | Registered |
| `osiris.bmc` | BMC (CMDB/ITSM) | Registered |

#### OT & Industrial ecosystems
| Namespace | Intended for | Status |
|-----------|--------------|--------|
| `osiris.siemens` | Siemens OT / Industrial systems | Registered |
| `osiris.emerson` | Emerson OT / Industrial systems | Registered |
| `osiris.eh` | Endress+Hauser OT / Industrial systems | Registered |
| `osiris.vaisala` | Vaisala OT / Industrial systems | Registered |
| `osiris.ifm` | IFM Electronic OT / Industrial systems | Registered |
| `osiris.schneider` | Schneider Electric OT / Industrial systems | Registered |
| `osiris.rockwell` | Rockwell Automation OT / Industrial systems | Registered |
| `osiris.abb` | ABB OT / Industrial systems | Registered |
| `osiris.honeywell` | Honeywell OT / Industrial systems | Registered |
| `osiris.hid` | HID OT / Industrial systems | Registered |
| `osiris.fanuc` | Fanuc OT / Industrial systems | Registered |
| `osiris.omron` | Omron OT / Industrial systems | Registered |
| `osiris.yaskawa` | Yaskawa OT / Industrial systems | Registered |
| `osiris.mitsubishi` | Mitsubishi Electric OT / Industrial systems | Registered |
| `osiris.yokogawa` | Yokogawa OT / Industrial systems | Registered |
| `osiris.ge` | GE Digital / industrial ecosystems | Registered |


#### Governance scope (namespaces vs semantics)
OSIRIS governs the **namespace rules** and (when applicable) the **registry policy** for well-known namespace prefixes under `extensions` (e.g. collision avoidance, canonical vendor identifiers).

OSIRIS does **not** govern the **internal semantics** of vendor/organization extensions (i.e. the fields inside `extensions["osiris.<vendor>"]`) and does **not** govern provider-defined enumerations such as `state`. Consumers **MUST** treat such values as opaque.


> [!NOTE]
> - These namespaces are intended to standardize vendor-specific extension placement for commonly used infrastructure management systems.
> - Third-party producers/parsers **MAY** emit these namespaces when exporting resources sourced from the corresponding vendor/platform (e.g. a discovery tool exporting AWS resources may emit `extensions.osiris.aws`).
> - Not all canonical `provider.name` values require a registered vendor namespace. Technologies and software components (e.g. databases, middleware, application frameworks) can be valid `provider.name` values without requiring a well-known `osiris.<vendor>` extension namespace.

**Rationale:** This list focuses on systems that **manage** infrastructure resources (hyperscalers and cloud platforms, virtualization stacks, networking/security, compute/storage vendors and OT/industrial ecosystems). Keeping the registry constrained avoids turning the specification into a general catalog of all technologies, while still enabling consistent, interoperable vendor extension usage.


### 8.4.3 Organization namespaces
Organizations creating proprietary extensions **SHOULD** use a domain-scoped namespace to avoid collisions:

- `osiris.com.<organization>`
- `osiris.org.<organization>`
- `osiris.net.<organization>`
- `osiris.edu.<institution>`

**Examples:**

| Organization | Domain | Recommended namespace |
|-------------|--------|----------------------|
| Acme Corporation | acme.com | `osiris.com.acme` |
| Example University | example.edu | `osiris.edu.example` |
| Research Institute | research.org | `osiris.org.research` |

Domain-scoped namespaces are collision-resistant because they are derived from an internet domain controlled by the organization.


### 8.4.4 Custom namespaces
For short-lived experiments or early community drafts, producers **MAY** use the `osiris.custom.*` namespace.

**Important:** The `osiris.custom.*` namespace is **not collision-resistant**. Multiple organizations using `osiris.custom.billing` will conflict. For any extension intended to persist or be used in production, producers **SHOULD** use reverse domain notation (e.g. `osiris.com.<org>.*`).

**Recommended usage:**
- **Experiments/prototypes:** `osiris.custom.*` (temporary)
- **Production/persistent:** `osiris.com.<org>.*` (collision-resistant)

**Example: Experimental extension (temporary)**
```json
"extensions": {
  "osiris.custom.draft.monitoring": {
    "experimental_metric": true,
    "note": "RFC draft for community review"
  }
}
```

**Example: Production extension (recommended)**
```json
"extensions": {
  "osiris.com.acme.monitoring": {
    "alert_policy": "critical",
    "on_call_team": "platform-ops"
  }
}
```

The `osiris.custom` namespace is appropriate for:
- Proof-of-concept extensions
- Community RFC drafts before standardization
- Short-lived experimental features

Producers **SHOULD** migrate from `osiris.custom.*` to organization-specific namespaces when extensions are production-ready.


### 8.4.5 Namespace registration
For OSIRIS v1.0, namespace registration is **informal** and community-driven. Organizations are encouraged to:
1. Document their namespace usage publicly (e.g. in a GitHub repository)
2. Publish extension schemas for consumer reference
3. Coordinate with the OSIRIS community to avoid collisions

Future versions of OSIRIS **MAY** introduce a formal namespace registry.


### 8.4.6 Namespace collision avoidance
Namespace collisions occur when multiple parties use the same namespace with different semantics. To avoid collisions:

**Producers SHOULD:**
- Use vendor namespaces (`osiris.aws`) only for the corresponding vendor
- Use reverse domain notation for organization namespaces
- Document namespace usage publicly
- Avoid reusing existing namespace patterns with different semantics

**Consumers SHOULD:**
- Accept multiple extension namespaces in the same resource
- Process known namespaces and ignore unknown ones
- Log warnings for unexpected namespace combinations


### 8.4.7 Namespace versioning
If extension semantics change significantly, producers **MAY** version namespaces:

```json
"extensions": {
  "osiris.aws.v1": {
    "detailed_monitoring": true
  },
  "osiris.aws.v2": {
    "monitoring": {
      "level": "detailed",
      "interval_seconds": 60
    }
  }
}
```

Versioned namespaces enable gradual migration. Producers **SHOULD**:
- Support old and new versions during a transition period
- Document migration paths from old to new versions
- Provide examples showing both versions


### 8.4.8 Forward compatibility
Consumers **MUST** accept resources with unknown extension namespaces.

Consumers encountering unknown namespaces **SHOULD**:
- Log the unknown namespace for debugging
- Continue processing known fields and namespaces
- Preserve unknown namespace data when re-exporting

This forward compatibility ensures that new extensions do not break existing consumers.

---

## 8.5 Extension best practices
### 8.5.1 For producers
**Use standard fields and types first:**
- Prefer standard types (Chapter 7) over custom types
- Prefer `properties` over `extensions` for generic data
- Only create custom types when standard types are insufficient

**Document extensions:**
- Publish extension schemas for consumer reference
- Provide examples showing typical usage
- Explain when to use extensions vs standard fields
- Version extension schemas if semantics change

**Validate extension data:**
- Ensure extension data is well-formed JSON
- Use appropriate JSON types (boolean, number, string, object, array)
- Validate data against extension schemas before emitting

**Maintain stability:**
- Avoid breaking changes to extension schemas when possible
- Use versioned namespaces for incompatible changes
- Provide migration paths for deprecated extensions


### 8.5.2 For consumers
**Accept unknown extensions:**
- Do not reject resources with unrecognized extension namespaces
- Process known fields and ignore unknown extensions
- Log unknown extensions for debugging

**Preserve extension data:**
- When re-exporting topology, preserve extension fields
- Do not strip extensions consumers do not understand

**Handle missing extensions gracefully:**
- Do not assume extension fields are present
- Provide defaults for missing extension values
- Degrade gracefully when extension data is unavailable

**Provide configuration:**
- Enable users to control extension processing (enable/disable specific namespaces)
- Allow selective extension filtering for privacy or security
- Support extension validation against schemas


### 8.5.3 Common patterns
**Pattern 1: Vendor-specific feature flags**
```json
"extensions": {
  "osiris.aws": {
    "detailed_monitoring": true,
    "ebs_optimized": false
  }
}
```

**Pattern 2: Organization metadata**
```json
"extensions": {
  "osiris.com.acme.billing": {
    "cost_center": "engineering",
    "project": "web-platform"
  },
  "osiris.com.acme.compliance": {
    "pci_scope": true,
    "sox_audited": false
  }
}
```

**Pattern 3: Provider console/management references (non-primary identity)**
```json
"extensions": {
  "osiris.azure": {
    "portal_url": "https://portal.azure.com/#@/resource/subscriptions/.../resourceGroups/.../providers/...",
    "zones": ["1", "2"]
  }
}
```

> [!NOTE]
> Provider identifiers used to locate the resource (subscription/project/account, region, native resource ID) **SHOULD** be placed in `provider`. Use extensions for vendor UX links, secondary references, or tool-specific foreign keys.

**Pattern 4: Hardware management interfaces**
```json
"extensions": {
  "osiris.dell": {
    "idrac_ip": "10.0.100.10",
    "idrac_version": "5.10.00.00",
    "idrac_console_url": "https://10.0.100.10"
  }
}
```

---

## 8.6 Summary
The OSIRIS extension mechanism balances flexibility and interoperability:

- **Properties** contain generic infrastructure attributes applicable across vendors
- **Extensions** contain namespaced vendor-specific, platform-specific, or organization-specific data
- **Custom types** represent resources that do not map to standard types
- **Namespaces** prevent collisions and enable ecosystem growth

**Key principles:**
- Standard first, extend selectively
- Namespace all extensions (`osiris.<namespace>`)
- Consumers must accept unknown extensions
- Producers should document extension semantics

The extension mechanism enables the OSIRIS ecosystem to evolve without fragmenting the core specification. Producers can represent vendor-specific infrastructure while maintaining interoperability with consumers that understand only core OSIRIS fields.

---

# 9 Validation
## 9.0 Overview
Validation ensures that OSIRIS documents conform to the specification and can be reliably consumed by implementations. This chapter defines validation requirements at three levels: **structural** (JSON schema compliance), **semantic** (referential integrity and constraint checking) and **domain** (type and property conventions).

**Purpose of validation:**
- **Quality assurance:** Ensures producers emit well-formed documents
- **Interoperability:** Guarantees consumers can parse documents safely
- **Debuggability:** Provides clear error messages when issues occur
- **Forward compatibility:** Enables graceful handling of unknown extensions

**Who should validate:**
- **Producers** **SHOULD** validate documents before emission to catch errors early
- **Consumers** **MUST** validate documents at an appropriate level before processing
- **Tools** **MAY** provide validation services for OSIRIS documents

---

## 9.1 JSON Schema
### 9.1.1 Schema purpose
The OSIRIS JSON Schema formally defines the document structure, data types and constraints that **MUST** be satisfied by all OSIRIS documents. The schema provides **structural validation**: it verifies that:

- Documents are syntactically valid JSON format
- Required fields are present
- Field types are correct (string, object, array, etc.)
- Field values match defined patterns (e.g. version format, type naming)
- Array elements conform to expected structures

The complete JSON Schema is provided in **Appendix A (JSON Schema Definition)**.


### 9.1.2 Schema location
The canonical JSON Schema for OSIRIS v1.0 is versioned in the OSIRIS repository under `schema/v1.0/` and is published for consumption at:

```text
https://osirisjson.org/schema/v1.0/osiris.schema.json
```

Examples in Chapter 10 are intended to conform to this canonical schema.
OSIRIS documents **SHOULD** reference the schema using the `$schema` field at the top level:

```json
{
  "$schema": "https://osirisjson.org/schema/v1.0/osiris.schema.json",
  "version": "1.0.0",
  "metadata": { ... },
  "topology": { ... }
}
```

Consumers **SHOULD** validate documents against the referenced schema. Producers **MAY** include the `$schema` field to enable automatic validation by schema-aware tools.


### 9.1.3 Schema validation tools (non-normative)
OSIRIS is intended to be validated **locally** by default to preserve privacy. Implementations **SHOULD** avoid uploading infrastructure inventories or topology snapshots to remote services.

The following validation tools are **RECOMMENDED** reference implementations. They are **non-normative** and do not affect the OSIRIS specification itself.

#### VS Code validator (recommended)
A VS Code extension provides the best developer experience and privacy model:

- Runs entirely on the user’s machine (offline, no upload)
- Highlights issues inline (squiggles, diagnostics, quick fixes)
- Supports validation levels (e.g. **basic schema**, **semantic rules**, **strict mode**)
- Can explain errors by rule ID and link to the relevant spec section

#### Command-line validators (CI / automation)
Command-line validators are useful for CI pipelines and automated exports:

- Validate files and directories (`osiris validate <file|dir>`)
- Output machine-readable results (`--format json`) for CI parsing
- Support validation levels and strictness options

#### Web validators (optional, client-side)
A web-based validator can be provided for convenience and demos, but **SHOULD** run **client-side** in the browser to avoid transmitting sensitive data.

If a remote validation API is offered, it **MUST** be opt-in and clearly documented as server-side processing.

> [!NOTE]
> Implementations **SHOULD** keep validator logic consistent across tools by reusing a shared validation engine where possible (e.g. a common library that performs JSON Schema checks and OSIRIS semantic rule checks).


### 9.1.4 Schema conformance requirements
All OSIRIS v1.0 documents **MUST** conform to the JSON Schema defined in Appendix A. Producers **MUST** emit documents that pass schema validation. Consumers **MUST** reject documents that fail structural validation or provide clear diagnostic messages identifying validation failures.

**Structural validation is non-negotiable:** Documents that fail JSON Schema validation are malformed and **MUST NOT** be processed by consumers except for diagnostic or debugging purposes.


### 9.1.5 Schema extensibility
The JSON Schema permits unknown fields in objects where extensibility is expected (top-level, metadata, resources, connections, groups, properties, extensions). This ensures forward compatibility when:

- New OSIRIS versions introduce fields
- Producers include custom metadata
- Extensions add vendor-specific data

Consumers **MUST** accept documents with unknown fields that pass schema validation. The schema uses `"additionalProperties": true` in extensible objects to permit forward-compatible evolution.

---

## 9.2 Minimum required fields (baseline interoperability)
This section enumerates required fields at each level of an OSIRIS document. Required fields **MUST** be present and **MUST** contain values of the correct type. Missing or null required fields constitute validation errors.


### 9.2.1 Top-level required fields
Every OSIRIS document **MUST** be a JSON object containing:

| Field | Type | Format | Description |
|-------|------|--------|-------------|
| `version` | string | `MAJOR.MINOR.PATCH` | OSIRIS specification version (e.g. `"1.0.0"`) |
| `metadata` | object | see 9.2.2 | Document metadata |
| `topology` | object | see 9.2.3 | Infrastructure topology data |

**Validation rules:**
- **V-DOC-001:** Document **MUST** be a JSON object (not array, string, number, etc.)
- **V-DOC-002:** Document **MUST** contain `version`, `metadata` and `topology` fields
- **V-DOC-003:** `version` **MUST** be a string matching `^\d+\.\d+\.\d+$` (SemVer format)
- **V-DOC-004:** For OSIRIS v1.0 documents, `version` **MUST** be `"1.0.0"` or later v1.x.y versions

**Valid example:**
```json
{
  "version": "1.0.0",
  "metadata": {
    "timestamp": "2026-01-01T10:30:00Z"
  },
  "topology": {
    "resources": []
  }
}
```

**Invalid examples:**
```text
// Missing version field
{
  "metadata": { "timestamp": "2026-01-01T10:30:00Z" },
  "topology": { "resources": [] }
}

// Invalid version format missing patch version
{
  "version": "1.0",
  "metadata": { "timestamp": "2026-01-01T10:30:00Z" },
  "topology": { "resources": [] }
}

// Not an object (array)
[
  { "version": "1.0.0", ... }
]
```


### 9.2.2 Metadata required fields
The `metadata` object **MUST** contain:

| Field | Type | Format | Description |
|-------|------|--------|-------------|
| `timestamp` | string | ISO 8601 with timezone | Document generation timestamp |

**Validation rules:**
- **V-META-001:** Metadata object **MUST** contain `timestamp` field
- **V-META-002:** `timestamp` **MUST** be a valid ISO 8601 timestamp with timezone designator
- **V-META-003:** `timestamp` **MUST** include date, time and timezone (Z or offset like +00:00)

**Valid timestamps:**
```text
"timestamp": "2026-01-01T10:30:00Z"              // UTC
"timestamp": "2026-01-01T10:30:00+00:00"         // UTC with offset
"timestamp": "2026-01-01T05:30:00-05:00"         // Eastern Time
"timestamp": "2026-01-01T11:30:00+01:00"         // Central European Time
```

**Invalid timestamps:**
```text
"timestamp": "2026-01-01"                        // Missing time
"timestamp": "2026-01-01T10:30:00"               // Missing timezone
"timestamp": "2026-01-01 10:30:00Z"              // Invalid separator
"timestamp": "Jan 1, 2026"                       // Wrong format
```

**Recommended fields (OPTIONAL but encouraged):**
- `generator.name` (string): Name of generating tool
- `generator.version` (string): Version of generating tool
- `scope` (object): Description of what infrastructure is represented


### 9.2.3 Topology required fields
The `topology` object **MUST** contain:

| Field | Type | Description |
|-------|------|-------------|
| `resources` | array | Array of resource objects (may be empty) |

**Validation rules:**
- **V-TPGY-001:** Topology object **MUST** contain `resources` field
- **V-TPGY-002:** `resources` **MUST** be an array (may be empty: `[]`)
- **V-TPGY-003:** If present, `connections` **MUST** be an array
- **V-TPGY-004:** If present, `groups` **MUST** be an array

**Minimal valid topology:**
```json
{
  "resources": []
}
```

**Topology with all fields:**
```json
{
  "resources": [ ... ],
  "connections": [ ... ],
  "groups": [ ... ]
}
```

> [!NOTE]
> `connections` and `groups` are **OPTIONAL**. If omitted, they are treated as empty arrays. Producers **MAY** omit these fields entirely or provide them as empty arrays.


### 9.2.4 Resource required fields
Every resource object **MUST** contain:

| Field | Type | Format | Description |
|-------|------|--------|-------------|
| `id` | string | document-unique | Unique identifier for resource |
| `type` | string | dot notation | Resource type classification |
| `provider` | object | see 9.2.4.1 | Provider attribution |

**Validation rules:**
- **V-RES-001:** Resource **MUST** contain `id`, `type` and `provider` fields
- **V-RES-002:** Resource `id` **MUST** be a non-empty string
- **V-RES-003:** Resource `id` **MUST** be unique within the document
- **V-RES-004:** Resource `type` **MUST** be a non-empty string
- **V-RES-005:** Resource `type` **MUST** match format: `^[a-z][a-z0-9]*(\.[a-z][a-z0-9]*)*$`
- **V-RES-006:** Resource `type` **MUST** contain at least two segments (e.g. `compute.vm`, not just `compute`)
- **V-RES-007:** Resource `provider` **MUST** be an object


#### 9.2.4.1 Provider required fields
The `provider` object within a resource **MUST** contain:

| Field | Type | Format | Description |
|-------|------|--------|-------------|
| `name` | string | lowercase | Provider/vendor name |

**Validation rules:**
- **V-PROV-001:** Provider object **MUST** contain `name` field
- **V-PROV-002:** Provider `name` **MUST** be a non-empty string
- **V-PROV-003:** Provider `name` **MUST** match format: `^[a-z0-9]+(\.[a-z0-9]+)*$` (lowercase letters, digits, dots only)
- **V-PROV-004:** Provider `name` **SHOULD** be a well-known canonical name (aws, azure, gcp, vmware, dell, cisco, arista, etc.)

**Valid provider objects:**
```text
{ "name": "aws" }
{ "name": "azure" }
{ "name": "dell" }
{ "name": "cisco.aci" }  // Multi-segment allowed
```

**Invalid provider objects:**
```text
{ "name": "AWS" }           // Not lowercase
{ "name": "amazon-aws" }    // Contains hyphen
{ "name": "" }              // Empty string
{ }                         // Missing name field
```

**Recommended fields (OPTIONAL):**
- `type` (string): Vendor-specific resource type (e.g. `"AWS::EC2::Instance"`, `"PowerEdge R770"`)
- `native_id` (string): Vendor's native identifier (e.g. `"i-0abc123"`)
- `region` (string): Geographic region or zone
- `account` (string): Account, subscription or project ID


### 9.2.5 Connection required fields
Every connection object **MUST** contain:

| Field | Type | Format | Description |
|-------|------|--------|-------------|
| `id` | string | document-unique | Unique identifier for connection |
| `source` | string | resource ID reference | Source resource ID |
| `target` | string | resource ID reference | Target resource ID |
| `type` | string | dot notation | Connection type |

**Validation rules:**
- **V-CONN-001:** Connection **MUST** contain `id`, `source`, `target` and `type` fields
- **V-CONN-002:** Connection `id` **MUST** be a non-empty string
- **V-CONN-003:** Connection `id` **MUST** be unique within the document
- **V-CONN-004:** Connection `source` **MUST** reference a valid resource `id`
- **V-CONN-005:** Connection `target` **MUST** reference a valid resource `id`
- **V-CONN-006:** Connection `type` **MUST** be a non-empty string
- **V-CONN-007:** Connection `type` **MUST** match format: `^[a-z][a-z0-9]*(\.[a-z][a-z0-9]*)*$`

**Valid connection:**
```json
{
  "id": "conn-web-to-db",
  "source": "aws::i-0abc123",
  "target": "aws::db-prod-01",
  "type": "dependency"
}
```

**Invalid connections:**
```text
// Missing required type field
{
  "id": "conn-001",
  "source": "aws::i-0abc123",
  "target": "aws::db-prod-01"
}

// Invalid type format
{
  "id": "conn-001",
  "source": "aws::i-0abc123",
  "target": "aws::db-prod-01",
  "type": "TCP_CONNECTION"  // Uppercase, underscore
}

// Dangling reference
{
  "id": "conn-001",
  "source": "aws::i-0abc123",
  "target": "nonexistent-resource",  // Not in resources array
  "type": "dependency"
}
```


### 9.2.6 Group required fields
Every group object **MUST** contain:

| Field | Type | Format | Description |
|-------|------|--------|-------------|
| `id` | string | document-unique | Unique identifier for group |
| `type` | string | dot notation | Group type |

**Validation rules:**
- **V-GRP-001:** Group **MUST** contain `id` and `type` fields
- **V-GRP-002:** Group `id` **MUST** be a non-empty string
- **V-GRP-003:** Group `id` **MUST** be unique within the document
- **V-GRP-004:** Group `type` **MUST** be a non-empty string
- **V-GRP-005:** Group `type` **MUST** match format: `^[a-z][a-z0-9]*(\.[a-z][a-z0-9]*)*$`
- **V-GRP-006:** If present, group `members` **MUST** be an array of strings
- **V-GRP-007:** If present, each group `members` entry **MUST** reference a valid resource `id`
- **V-GRP-008:** If present, group `children` **MUST** be an array of strings
- **V-GRP-009:** If present, each group `children` entry **MUST** reference a valid group `id`

**Valid group:**
```json
{
  "id": "grp-production-vpc",
  "type": "network.vpc",
  "members": [
    "aws::i-0abc123",
    "aws::i-0def456"
  ]
}
```

**Invalid groups:**
```text
// Missing type
{
  "id": "grp-001",
  "members": [ "aws::i-0abc123" ]
}

// Invalid type format
{
  "id": "grp-001",
  "type": "Production_VPC",  // Uppercase, underscore
  "members": [ "aws::i-0abc123" ]
}

// Dangling member reference
{
  "id": "grp-001",
  "type": "network.vpc",
  "members": [
    "aws::i-0abc123",
    "nonexistent-resource"  // Not in resources array
  ]
}
```

---

## 9.3 Validation rules
Beyond structural requirements enforced by JSON Schema, OSIRIS defines **semantic** and **domain** validation rules. These rules address referential integrity, naming conventions, type validity and extension usage.


### 9.3.1 Validation levels
OSIRIS defines three validation levels that consumers **MAY** implement based on their requirements:

#### Level 1: Structural validation (REQUIRED)

**What it validates:**
- JSON syntax is correct
- Required fields are present
- Field types are correct (string, object, array, etc.)
- Field values match format patterns

**Implementation:** JSON Schema validation (Appendix A)
**Outcome:** Document is **syntactically valid**
**Consumers MUST implement:** Level 1 validation is mandatory


#### Level 2: Semantic validation (RECOMMENDED)

**What it validates:**
- ID uniqueness within document
- Referential integrity (connections > resources, groups > resources)
- Type format compliance (lowercase, dots only)
- Provider naming conventions
- Extension namespace format

**Implementation:** Custom validator logic after JSON Schema validation
**Outcome:** Document is **semantically valid** and internally consistent
**Consumers SHOULD implement:** Level 2 validation catches common producer errors


#### Level 3: Domain validation (OPTIONAL)

**What it validates:**
- Resource types are recognized (standard types from Chapter 7)
- Connection types are appropriate for resource types
- Properties follow conventions for resource types
- Extension namespaces are known/registered

**Implementation:** Domain-aware validator with knowledge of OSIRIS taxonomy
**Outcome:** Document uses **well-known types** and follows conventions
**Consumers MAY implement:** Level 3 validation provides additional quality checks but is not required for conformance


### 9.3.2 Identity validation rules

**Rule category:** Semantic (Level 2)

These rules ensure that identifiers are unique and well-formed:

**V-ID-001: Resource ID uniqueness**
- **Rule:** Every resource `id` **MUST** be unique within the document
- **Level:** Error
- **Example violation:**
  ```text
  "resources": [
    { "id": "aws::i-0abc123", "type": "compute.vm", ... },
    { "id": "aws::i-0abc123", "type": "storage.volume", ... }  // Duplicate
  ]
  ```

**V-ID-002: Connection ID uniqueness**
- **Rule:** Every connection `id` **MUST** be unique within the document
- **Level:** Error
- **Example violation:**
  ```text
  "connections": [
    { "id": "conn-001", ... },
    { "id": "conn-001", ... }  // Duplicate
  ]
  ```

**V-ID-003: Group ID uniqueness**
- **Rule:** Every group `id` **MUST** be unique within the document
- **Level:** Error
- **Example violation:**
  ```text
  "groups": [
    { "id": "grp-prod", "type": "administrative", ... },
    { "id": "grp-prod", "type": "network.vpc", ... }  // Duplicate
  ]
  ```

**V-ID-004: Non-empty IDs**
- **Rule:** Resource, connection and group IDs **MUST** be non-empty strings
- **Level:** Error
- **Example violation:**
  ```text
  { "id": "", "type": "compute.vm", ... }  // Empty ID
  ```

**V-ID-005: Resource ID format recommendation**
- **Rule:** Resource IDs **SHOULD** use `provider::native-id` format when applicable
- **Level:** Warning
- **Example recommendation:**
  ```text
  // Recommended
  { "id": "aws::i-0abc123", ... }
  
  // Not recommended but valid
  { "id": "my-web-server", ... }
  ```

**V-ID-006: Cross-scope uniqueness**
- **Rule:** Resource, connection and group IDs occupy separate namespaces
- **Guidance:** Same ID string **MAY** be used for a resource, connection and group without conflict
- **Example (valid):**
  ```text
  "resources": [ { "id": "vpc-001", ... } ],
  "groups": [ { "id": "vpc-001", ... } ]  // Valid (different namespaces)
  ```


### 9.3.3 Referential integrity rules

**Rule category:** Semantic (Level 2)

These rules ensure that ID references resolve correctly:

**V-REF-001: Connection source validity**
- **Rule:** Connection `source` **MUST** reference an existing resource `id`
- **Level:** Error
- **Example violation:**
  ```text
  "resources": [
    { "id": "aws::i-0abc123", ... }
  ],
  "connections": [
    { "source": "aws::i-0def456", ... }  // Resource doesn't exist
  ]
  ```

**V-REF-002: Connection target validity**
- **Rule:** Connection `target` **MUST** reference an existing resource `id`
- **Level:** Error
- **Example violation:**
  ```text
  "resources": [
    { "id": "aws::i-0abc123", ... }
  ],
  "connections": [
    { "target": "aws::db-nonexistent", ... }  // Resource doesn't exist
  ]
  ```

**V-REF-003: Group member validity**
- **Rule:** Each group `members` entry **MUST** reference an existing resource `id`
- **Level:** Error
- **Example violation:**
  ```text
  "resources": [
    { "id": "aws::i-0abc123", ... }
  ],
  "groups": [
    {
      "members": [
        "aws::i-0abc123",
        "aws::i-0nonexistent"  // Resource doesn't exist
      ]
    }
  ]
  ```

**V-REF-004: Group children validity**
- **Rule:** Each group `children` entry **MUST** reference an existing group `id`
- **Level:** Error
- **Example violation:**
  ```text
  "groups": [
    { "id": "grp-parent", "children": ["grp-child"] },
    // grp-child doesn't exist
  ]
  ```

**V-REF-005: Circular group nesting**
- **Rule:** Group hierarchies **MUST NOT** contain cycles
- **Level:** Error
- **Example violation:**
  ```text
  "groups": [
    { "id": "grp-a", "children": ["grp-b"] },
    { "id": "grp-b", "children": ["grp-a"] }  // Circular reference
  ]
  ```

**V-REF-006: Dangling references handling**
- **Rule:** Consumers **SHOULD** report dangling references as warnings
- **Guidance:** Consumers **MAY** skip invalid connections/groups while processing valid resources
- **Level:** Warning (downgradeable from error for resilience)


### 9.3.4 Type format rules

**Rule category:** Structural (Level 1) and Semantic (Level 2)

These rules ensure type identifiers are well-formed:

**V-TYPE-001: Type must be lowercase**
- **Rule:** Type strings **MUST** contain only lowercase letters and digits
- **Level:** Error
- **Example violations:**
  ```text
  "type": "Compute.VM"    // Uppercase
  "type": "compute.VM"    // Uppercase segment
  "type": "Compute"       // Uppercase
  ```

**V-TYPE-002: Type must use dot separator**
- **Rule:** Type segments **MUST** be separated by dots (`.`)
- **Prohibition:** Underscores (`_`), hyphens (`-`) and spaces are **NOT ALLOWED** in type strings
- **Level:** Error
- **Example violations:**
  ```text
  "type": "compute_vm"    // Underscore
  "type": "compute-vm"    // Hyphen
  "type": "compute vm"    // Space
  ```

**V-TYPE-003: Type must not start or end with dot**
- **Rule:** Type strings **MUST NOT** start or end with a dot
- **Level:** Error
- **Example violations:**
  ```text
  "type": ".compute.vm"   // Leading dot
  "type": "compute.vm."   // Trailing dot
  ```

**V-TYPE-004: Type must not have consecutive dots**
- **Rule:** Type strings **MUST NOT** contain consecutive dots (`..`)
- **Level:** Error
- **Example violations:**
  ```text
  "type": "compute..vm"   // Consecutive dots
  "type": "compute...vm"  // Multiple consecutive dots
  ```

**V-TYPE-005: Type must have minimum segments**
- **Rule:** Type strings **MUST** contain at least two segments (category and subcategory)
- **Level:** Error
- **Example violations:**
  ```text
  "type": "compute"       // Only one segment
  "type": "vm"            // Only one segment
  ```

**V-TYPE-006: Type format pattern**
- **Rule:** Type strings **MUST** match regex: `^[a-z][a-z0-9]*(\.[a-z][a-z0-9]*)+$`
- **Guidance:** Start with lowercase letter, followed by lowercase letters/digits, then one or more dot-separated segments with same format
- **Level:** Error

**V-TYPE-007: Custom type prefix**
- **Rule:** Custom types **MUST** use `osiris.` prefix
- **Guidance:** Standard types **MUST NOT** use `osiris.` prefix
- **Level:** Error
- **Example:**
  ```text
  // Standard type
  "type": "compute.vm"
  
  // Custom type
  "type": "osiris.aws.lambda.edge"
  
  // Standard type with osiris prefix
  "type": "osiris.compute.vm"
  ```

**V-TYPE-008: Type depth recommendation**
- **Rule:** Type strings **SHOULD NOT** exceed 4-5 segments
- **Guidance:** Highly specific details belong in `properties`, not type strings
- **Level:** Warning (informational, not error)


### 9.3.5 Provider validation rules

**Rule category:** Semantic (Level 2)

These rules ensure provider information is well-formed:

**V-PROV-001: Provider name lowercase**
- **Rule:** Provider `name` **MUST** be lowercase
- **Level:** Error
- **Example violations:**
  ```text
  "provider": { "name": "AWS" }      // Uppercase
  "provider": { "name": "Azure" }    // Mixed case
  ```

**V-PROV-002: Provider name format**
- **Rule:** Provider `name` **MUST** match pattern: `^[a-z0-9]+(\.[a-z0-9]+)*$`
- **Allowed:** Lowercase letters, digits, dots only
- **Not allowed:** Hyphens, underscores, spaces, uppercase
- **Level:** Error
- **Example violations:**
  ```text
  "provider": { "name": "amazon-aws" }   // Hyphen
  "provider": { "name": "cisco_aci" }    // Underscore
  ```

**V-PROV-003: Well-known provider names**
- **Rule:** Producers **SHOULD** use canonical provider names for well-known vendors
- **Canonical names:** aws, azure, gcp, oci, vmware, proxmox, dell, hpe, cisco, arista, juniper, paloalto, fortinet, etc.
- **Level:** Warning
- **Example recommendations:**
  ```text
  // Recommended
  "provider": { "name": "aws" }
  
  // Not recommended but valid
  "provider": { "name": "amazon" }
  "provider": { "name": "amazonwebservices" }
  ```

**V-PROV-004: Provider field recommendations**
- **Rule:** Provider objects **SHOULD** include `type`, `native_id` and `region` when applicable
- **Level:** Informational


### 9.3.6 Extension validation rules

**Rule category:** Semantic (Level 2)
These rules ensure extensions follow the namespace conventions defined in Chapter 8:

**V-EXT-001: Extension namespace prefix**
- **Rule:** Extension fields **MUST** use `osiris.<namespace>` prefix
- **Level:** Error
- **Example:**
  ```text
  // Valid
  "extensions": {
    "osiris.aws": { ... },
    "osiris.com.acme": { ... }
  }
  
  // Invalid (missing osiris prefix)
  "extensions": {
    "aws": { ... },
    "custom": { ... }
  }
  ```

**V-EXT-002: Extension namespace format**
- **Rule:** Extension namespaces **MUST** match pattern: `^osiris\.[a-z][a-z0-9]*(\.[a-z][a-z0-9]*)*$`
- **Level:** Error
- **Example violations:**
  ```text
  "extensions": {
    "osiris.AWS": { ... },          // Uppercase
    "osiris.my-company": { ... },   // Hyphen
    "osiris.": { ... }               // Incomplete
  }
  ```

**V-EXT-003: Unknown extensions**
- **Rule:** Consumers **MUST** accept documents with unknown extension namespaces
- **Guidance:** Consumers **MAY** log warnings for unrecognized extensions but **MUST NOT** reject documents
- **Level:** Informational

**V-EXT-004: Extension namespace registration**
- **Rule:** Producers **SHOULD** document custom extension namespaces
- **Guidance:** Extension namespaces **SHOULD** use reverse domain notation for organization-specific extensions (e.g. `osiris.com.acme`)
- **Level:** Informational


### 9.3.7 Domain validation rules

**Rule category:** Domain (Level 3, OPTIONAL)

These rules validate against known types and conventions:

**V-DOM-001: Known resource types**
- **Rule:** Resource types **SHOULD** be standard types from Chapter 7
- **Guidance:** Consumers **MAY** warn about unrecognized types but **MUST NOT** reject documents
- **Level:** Warning (informational)

**V-DOM-002: Known connection types**
- **Rule:** Connection types **SHOULD** be standard types from Chapter 5
- **Standard types:** network, dependency, contains, dataflow, physical, plus protocol-specific subtypes
- **Level:** Warning (informational)

**V-DOM-003: Known group types**
- **Rule:** Group types **SHOULD** be standard types from Chapter 6
- **Standard types:** administrative, structural, plus category-specific subtypes
- **Level:** Warning (informational)

**V-DOM-004: Property conventions**
- **Rule:** Properties **SHOULD** follow naming conventions for resource types
- **Example:** `compute.vm` resources typically have `vcpus`, `memory_gb`, `instance_type`
- **Level:** Informational

**V-DOM-005: Connection semantics**
- **Rule:** Connections **SHOULD** make semantic sense for resource type pairs
- **Example:** `dependency` connections typically link application components
- **Level:** Informational


### 9.3.8 Validation error levels
OSIRIS defines three error severity levels:

#### ERROR (MUST Fix)
**Severity:**
Critical - document is malformed

**Action:**
Producers **MUST** fix errors before emission. Consumers **MUST** reject documents or emit clear error messages.

**Examples:**
- Missing required fields
- Invalid JSON syntax
- Duplicate IDs
- Dangling references
- Invalid type formats

**Consumer behavior:**
Stop processing or provide diagnostic information only


#### WARNING (SHOULD Fix)
**Severity:**
Important - document may have issues

**Action:**
Producers **SHOULD** address warnings. Consumers **SHOULD** log warnings but **MAY** continue processing.

**Examples:**
- Non-standard provider names
- Missing recommended fields
- Unrecognized but valid types
- Non-recommended ID formats

**Consumer behavior:**
Log warning, continue processing


#### INFO (MAY Fix)

**Severity:**
Informational - potential improvements

**Action:**
Producers **MAY** address informational issues. Consumers **MAY** log info messages.

**Examples:**
- Optional fields not present
- Property naming suggestions
- Type depth recommendations
- Extension namespace suggestions

**Consumer behavior:**
Optional logging, continue processing


### 9.3.9 Validation implementation guidance

#### For producers

**Validation workflow:**
1. Generate OSIRIS document
2. Validate against JSON Schema (Level 1)
   - If fails: Fix errors and retry
3. Run semantic validation (Level 2)
   - If errors: Fix and retry
   - If warnings: Log and optionally fix
4. Optionally run domain validation (Level 3)
   - Log informational messages
5. Emit document


**Best practices:**
- Validate before emission (catch errors early)
- Log all warnings and errors during generation
- Provide clear error messages with context
- Test validator with invalid documents
- Include validation in CI/CD pipelines


#### For consumers

**Validation workflow:**
1. Receive OSIRIS document
2. Validate against JSON Schema (Level 1)
   - If fails: Reject document with error message
3. Run semantic validation (Level 2)
   - If errors: Reject or warn user
   - If warnings: Log warnings
4. Optionally run domain validation (Level 3)
   - Log informational messages
5. Process document


**Best practices:**
- Always perform Level 1 (structural) validation
- Implement Level 2 (semantic) validation for production systems
- Provide clear, actionable error messages
- Allow users to configure validation strictness
- Log validation events for debugging
- Handle partial failures gracefully (e.g. skip invalid connections, process valid resources)


#### Validation libraries

**Reference implementations:**
Producers and consumers **SHOULD** use or provide validation libraries that implement:
- JSON Schema validation (Level 1)
- Semantic rule checking (Level 2)
- Optional domain validation (Level 3)

---

## 9.4 Validation examples
### 9.4.1 Valid minimal document

```json
{
  "$schema": "https://osirisjson.org/schema/v1.0/osiris.schema.json",
  "version": "1.0.0",
  "metadata": {
    "timestamp": "2026-01-01T18:30:00Z",
    "generator": {
      "name": "",
      "version": ""
    }
  },
  "topology": {
    "resources": []
  }
}
```

**Validation result:**
- Level 1: PASS (structure valid)
- Level 2: PASS (no semantic issues)
- Level 3: PASS (no domain issues)


### 9.4.2 Valid document with resources and connections

```json
{
  "$schema": "https://osirisjson.org/schema/v1.0/osiris.schema.json",
  "version": "1.0.0",
  "metadata": {
    "timestamp": "2026-01-01T18:30:00Z",
    "generator": {
      "name": "osiris-aws-parser",
      "version": "1.0.0"
    }
  },
  "topology": {
    "resources": [
      {
        "id": "aws::i-0abc123",
        "type": "compute.vm",
        "provider": {
          "name": "aws",
          "type": "AWS::EC2::Instance",
          "native_id": "i-0abc123",
          "region": "us-east-1",
          "account": "123456789012"
        }
      },
      {
        "id": "aws::db-prod-01",
        "type": "application.database",
        "provider": {
          "name": "aws",
          "type": "AWS::RDS::DBInstance",
          "native_id": "db-prod-01",
          "region": "us-east-1",
          "account": "123456789012"
        }
      }
    ],
    "connections": [
      {
        "id": "conn-web-to-db",
        "source": "aws::i-0abc123",
        "target": "aws::db-prod-01",
        "type": "dependency",
        "direction": "forward"
      }
    ]
  }
}
```

**Validation result:**
- Level 1: PASS (structure valid)
- Level 2: PASS (IDs unique, references valid)
- Level 3: PASS (standard types used)


### 9.4.3 Invalid document (missing required field)

```text
{
  "$schema": "https://osirisjson.org/schema/v1.0/osiris.schema.json",
  "version": "1.0.0",
  "metadata": {
    "timestamp": "2026-01-02T10:35:00Z",
    "generator": {
      "name": "osiris-aws-parser",
      "version": "1.0.0"
    }
  },
  "topology": {
    "resources": [
      {
        "id": "aws::i-0abc123",
        "type": "compute.vm"
        // Missing required "provider" field
      }
    ]
  }
}
```

**Validation result:**
- Level 1: FAIL
  - Error V-RES-001: Resource missing required field 'provider'
  - Location: topology.resources[0]


### 9.4.4 Invalid document (dangling reference)

```text
{
  "version": "1.0.0",
  "metadata": {
    "timestamp": "2026-01-02T10:35:00Z",
    "generator": {
      "name": "osiris-aws-parser",
      "version": "1.0.0"
    }
  },
  "topology": {
    "resources": [
      {
        "id": "aws::i-0abc123",
        "type": "compute.vm",
        "provider": { "name": "aws" }
      }
    ],
    "connections": [
      {
        "id": "conn-001",
        "source": "aws::i-0abc123",
        "target": "aws::db-nonexistent",  // Resource doesn't exist, ID doesn't match
        "type": "dependency"
      }
    ]
  }
}
```

**Validation result:**
- Level 1: PASS (structure valid)
- Level 2: FAIL
  - Error V-REF-002: Connection target references non-existent resource 'aws::db-nonexistent'
  - Location: topology.connections[0].target


### 9.4.5 Invalid document (invalid type format)

```text
{
  "version": "1.0.0",
  "metadata": {
    "timestamp": "2026-01-02T10:55:00Z",
    "generator": {
      "name": "osiris-aws-parser",
      "version": "1.0.0"
    }
  },
  "topology": {
    "resources": [
      {
        "id": "aws::i-0abc123",
        "type": "Compute_VM",  // Uppercase and underscore
        "provider": { "name": "aws" }
      }
    ]
  }
}
```

**Validation result:**
- Level 1: FAIL
  - Error V-TYPE-001: Type must be lowercase (found: 'Compute_VM')
  - Error V-TYPE-002: Type must use dot separator, not underscore
  - Location: topology.resources[0].type

---

## 9.5 Summary
OSIRIS validation operates at three levels:

1. **Structural (Level 1) REQUIRED:** JSON Schema validation
2. **Semantic (Level 2) RECOMMENDED:** Referential integrity and naming rules
3. **Domain (Level 3) OPTIONAL:** Type recognition and conventions

**Key validation rules:**
- Required fields must be present (document, metadata, topology, resources, connections, groups)
- IDs must be unique within their scope (resources, connections, groups)
- References must resolve (connections > resources, groups > resources)
- Types must use lowercase dot notation (e.g. `compute.vm`, not `Compute_VM`)
- Provider names must be lowercase (e.g. `aws`, not `AWS`)
- Extensions must use `osiris.*` namespace prefix

**Validation workflow:**
1. Producers **MUST** validate before emission and fix errors before publishing
3. Consumers **MUST** validate on receipt and reject or flag invalid documents (per their policy)
5. Unknown extensions **MUST** be accepted (forward compatibility)

**Implementation guidance:**
- Use JSON Schema for Level 1 validation
- Implement custom validation logic for Level 2
- Optionally implement domain awareness for Level 3
- Provide clear error messages with location information
- Log warnings without failing validation
- Allow configuration of validation strictness

The validation rules defined in this chapter ensure that OSIRIS documents are well-formed, internally consistent and interoperable across implementations while maintaining forward compatibility for ecosystem evolution.

---

# 10 Examples
This chapter references complete, validated examples in the OSIRIS repository. 
All examples are available at:

```text
https://github.com/osirisjson/osiris/tree/main/examples/v1.0
```

**Each example is:**
- Validated against the OSIRIS v1.0 schema
- Illustrative and intended to be schema-valid for testing and reference
- Informative where examples conflict with the specification, the specification is authoritative.


## 10.1 IT Minimal Cloud Provider infrastructure
### 10.1.0 Overview
This example demonstrates the absolute minimum valid OSIRIS document for a cloud provider. Showcasing alternative example of cloud platforms beyond the major hyperscalers.

It showcases:
- Minimal valid document for cloud provider
- Alternative cloud provider representation
- Simple droplet with required fields only
- Different provider naming and ID format


### 10.1.1 Scenario
A simplest cloud infrastructure on DigitalOcean:
- Single droplet (virtual machine)
- Provider specific ID format
- NYC3 region
- Used for: demonstrating non-hyperscaler providers, validation testing


### 10.1.2 Example
[View](../../examples/v1.0/IT/cloud/osiris_minimal_cloud_provider_infrastructure.json)


### 10.1.3 Key features demonstrated
**Cloud provider:**
- DigitalOcean instead alternative of major hyperscalers
- Shows OSIRIS supports any cloud platform
- Provider specific ID format

**Use cases:**
- Starting point for DigitalOcean infrastructure
- Template for other cloud providers
- Validation testing

---

## 10.2 IT Minimal Hyperscaler infrastructure
### 10.2.0 Overview
This example demonstrates the absolute minimum valid OSIRIS document for hyperscaler infrastructure. It represents a single virtual machine with minimal required fields, serving as the simplest possible starting point.

It showcases:
- Minimal valid OSIRIS document structure
- Single resource with only required fields
- Essential provider attribution
- Bare minimum metadata


### 10.2.1 Scenario
The simplest possible cloud infrastructure representation:
- Single EC2 instance (or equivalent VM)
- Required fields only (id, type, provider)
- Minimal metadata (timestamp only)
- No connections or groups
- Used for: testing, validation, learning the basic structure


### 10.2.2 Example
[View](../../examples/v1.0/IT/hyperscalers/osiris_minimal_hyperscalers_infrastructure.json)


### 10.2.3 Key features demonstrated
**Absolute minimum fields:**
- Document structure: version, metadata, topology
- Resource minimum: id, type, provider
- Provider minimum: name only
- Metadata minimum: timestamp only

**Use cases:**
- Learning OSIRIS basics
- Testing schema validators
- Template for new documents
- Demonstrating required vs. optional fields

---

## 10.3 IT Simple Hyperscaler infrastructure
### 10.3.0 Overview
This example demonstrates a basic three-tier web application running entirely in AWS. 

It showcases:
- Simple resource topology (compute, database, load balancer)
- Basic connection types (dependency, route)
- Standard cloud provider attribution
- Minimal but complete metadata


### 10.3.1 Scenario
A production web application with:
- Application Load Balancer (public-facing)
- EC2 instance running the web application
- RDS PostgreSQL database
- Direct dependencies: ALB > EC2 > RDS


### 10.3.2 Example
[View](../../examples/v1.0/IT/hyperscalers/osiris_simple_hyperscaler_infrastructure.json)


#### 10.3.3 Key features demonstrated
**Resource diversity:**
- Network resource (load balancer)
- Compute resource (EC2 instance)
- Application resource (database)

**Provider attribution:**
- Consistent AWS provider naming
- Native type mapping (AWS::EC2::Instance)
- Regional and account context

**Connections:**
- `route` type for traffic flow (ALB > EC2)
- `dependency` type for application relationship (EC2 > RDS)
- Directional semantics with `forward`

**Properties:**
- Network configuration (IPs, VPCs, subnets)
- Instance specifications (type, image, platform)
- Database configuration (engine, storage, endpoint)

---

## 10.4 IT Hyperscaler infrastructure belonging with Resource Group and VNet membership
### 10.4.0 Overview
Hyperscaler environments often have hierarchical containers (e.g. Azure Resource Groups, GCP Projects, AWS VPCs). OSIRIS can represent this belonging using groups (recommended for inventory/reporting) without requiring traversal semantics.


### 10.4.1 Scenario
This example represents a **multi-hyperscaler environment** where infrastructure resources originate from different providers (e.g. AWS, Azure, GCP) but are described in a **single OSIRIS document**.

It demonstrates:
- How a single topology can include heterogeneous resources across multiple providers.
- How cross-provider dependencies can be expressed using explicit connections.
- How cloud “ownership / belonging” containers (e.g. Resource Groups, Projects, VPC/VNet) can be represented for documentation purposes.


### 10.4.2 Example
[View](../../examples/v1.0/IT/hyperscalers/osiris_hyperscaler_infrastructure_belonging.json)

#### 10.4.3 Key features demonstrated
- **Multi-provider attribution:** resources include `provider.name` and native identifiers to preserve provenance and correlation.
- **Cross-provider topology:** connections can link resources even when they come from different vendors or platforms.
- **Belonging or ownership modeling:** hierarchical cloud containers (e.g. Azure Resource Group, GCP Project, AWS VPC or Azure VNet) can be represented using:
  - **Groups** (recommended for inventory/reporting and documentation)
  - **`contains` connections** (recommended when containment must be traversed as topology).
- **Forward compatibility:** consumers can ignore unknown resource fields and extension namespaces while still loading the document.

---

## 10.5 IT Multi-Hyperscalers environment
### 10.5.0 Overview
This example demonstrates infrastructure spanning AWS and Azure, showcasing:
- Multiple cloud providers in a single topology
- Cross-cloud resource relationships
- Provider-specific native types
- Unified representation of diverse infrastructure


### 10.5.1 Scenario
A distributed application with:
- Azure front-end (App Service)
- AWS compute tier (EC2)
- Azure database (SQL Database)
- Cross-cloud dependencies and data flows


### 10.5.2 Example
[View](../../examples/v1.0/IT/hyperscalers/osiris_multi_hyperscaler_environment.json)


### 10.5.3 Key features demonstrated
**Multi-provider resources:**
- Azure resources (App Service, SQL Database, Redis)
- AWS resources (EC2)
- Mixed provider types in single topology

**Cross-cloud connections:**
- Azure > AWS dependency (frontend to API)
- AWS > Azure dependency (API to database)
- Demonstrates cloud interconnection patterns

**Provider diversity:**
- Azure native IDs use ARM resource paths
- AWS native IDs use standard format
- Provider-specific properties maintained

**Complex relationships:**
- Multiple connection paths
- Shared resources (Redis accessed by both frontend and API)
- Bidirectional connections for cache access

---

## 10.6 IT Hybrid infrastructure on Hyperscaler and On-Premise
### 10.6.0 Overview
This example demonstrates a hybrid environment combining cloud and on-premise infrastructure:
- AWS cloud resources
- Physical on-premise servers (MXP datacenter)
- Custom provider for on-premise equipment
- Hybrid connectivity and dependencies


### 10.6.1 Scenario
An enterprise hybrid deployment:
- AWS public cloud for web tier
- On-premise physical servers for legacy applications
- On-premise physical storage
- Hybrid connections (VPN, direct connect)
- Mixed virtualized and physical infrastructure

### 10.6.2 Example
[View](../../examples/v1.0/IT/hybrid/osiris_hybrid_hyperscaler_on_premise.json)


### 10.6.3 Key features demonstrated
**Hybrid topology:**
- Cloud resources (AWS EC2, VPN)
- Physical on-premise servers (Dell R770)
- Physical storage infrastructure

**Custom provider usage:**
- `provider.name: "custom"` for on-premise equipment
- Required `namespace` field with reverse-domain notation
- Custom provider for equipment not from standard cloud vendors

**Physical infrastructure details:**
- Complete hardware specifications (CPU, memory, storage)
- Physical location (datacenter, rack, rack unit)
- Serial numbers and asset tags

**Hybrid connectivity:**
- VPN connection bridging cloud and on-premise
- Connections spanning environments

**Grouping:**
- Logical groups (cloud resources)
- Physical groups (datacenter site, rack)
- Hierarchical organization

---

## 10.7 IT Minimal On-Premise infrastructure
### 10.7.0 Overview
This example demonstrates the absolute minimum valid OSIRIS document for on-premise physical infrastructure. It represents a single physical server with custom provider attribution.

It showcases:
- Minimal valid document for on-premise equipment
- Custom provider with required namespace
- Physical server resource type (not virtual machine)
- Site-based identification pattern


### 10.7.1 Scenario
The simplest on-premise infrastructure representation:
- Single physical server in MXP datacenter
- Custom provider (non-cloud vendor)
- Required namespace field (osiris.it.mxp)
- Datacenter site attribution
- Minimal metadata (timestamp only)
- Used for: learning on-premise modeling, validation testing, template for datacenter exports


### 10.7.2 Example
[View](../../examples/v1.0/IT/on-premise/osiris_minimal_on_premise_infrastructure.json)


### 10.7.3 Key features demonstrated
**Custom provider requirements:**
- `provider.name: "custom"` for non-cloud equipment
- **Required** `namespace` field with reverse-domain notation
- Format: `osiris.{country}.{organization}` or `osiris.{com}.{company}`
- Example: `osiris.it.mxp` ( This example use ICAO code, MXP is referred to a Datacenter located in Milan.)

**Physical infrastructure:**
- Resource type: `compute.server` (not `compute.vm`)
- Distinguishes physical hardware from virtual machines
- Appropriate for bare-metal servers

**ID construction:**
- On-premise format: `site::identifier`
- Site code: `mxp` (Milan datacenter)
- Identifier: `server-01` (simple, human-readable)
- Pattern enables easy organization by datacenter

**Site attribution:**
- `provider.site`: Datacenter or facility identifier
- `provider.native_id`: Internal asset tracking number
- Enables correlation with DCIM systems

**Use cases:**
- Starting point for on-premise OSIRIS modeling
- Custom provider namespace validation
- Template for datacenter asset exports
- Demonstrating physical vs. virtual resources
- Asset management system integration

---

## 10.8 IT On-Premise Network topology
### 10.8.0 Overview
This example demonstrates detailed network infrastructure with:
- Physical network equipment (switches, routers)
- Detailed interface configurations
- Physical connectivity (fiber optics, copper)
- Network hierarchy and segmentation
- Complex connection properties


### 10.8.1 Scenario
A datacenter network spine-leaf architecture:
- Spine switches (high-capacity core)
- Leaf switches (server connectivity)
- Physical fiber connections between tiers
- Server network interfaces
- Detailed transceiver and cable specifications


### 10.8.2 Example
[View](../../examples/v1.0/IT/on-premise/osiris_on_premise_network_topology.json)


### 10.8.3 Key features demonstrated
**Detailed network equipment:**
- Switch specifications (Arista DCS-7050SX3-48YC12)
- Port configurations and capabilities
- Software versions (EOS 4.29.2F)
- Management information

**Physical connectivity types:**
- `physical.fiber` connections (100G QSFP28)
- `physical.copper` connections (10G SFP+)
- Different media types in same topology

**Interface details:**
- Source and target interface names
- Port numbers and types
- Transceiver specifications (vendor, part number, serial)
- Cable specifications (type, length, connector)

**Network-specific properties:**
- Layer 1 specifications (speed, duplex)
- VLANs and trunking
- Network bonding/teaming
- MTU configurations

**Hierarchical grouping:**
- Logical tiers (spine, leaf)
- Physical pods
- Nested groups (fabric containing layers)
- Network segmentation

---

## 10.9 OT Minimal infrastructure
### 10.9.0 Overview
This example demonstrates the absolute minimum valid OSIRIS document for Operational Technology (OT) infrastructure. It represents a single environmental sensor, showcasing the simplest OT device representation.

It showcases:
- Minimal valid OT document structure
- OT-specific resource type (`ot.sensor.environmental`)
- Custom provider for OT equipment
- Physical facility monitoring device
- Bridge between IT documentation and OT systems


### 10.9.1 Scenario
The simplest OT infrastructure representation:
- Single environmental sensor in MXP datacenter
- Temperature and humidity monitoring device
- Custom provider with namespace (non-IT vendor)
- Datacenter facility monitoring system
- No network connections shown (monitoring only)
- Used for: learning OT modeling, demonstrating IT/OT distinction, validation testing

**Real-world context:**
Environmental sensors are ubiquitous in:
- Datacenters (temperature, humidity monitoring)
- Server rooms (thermal monitoring)
- Industrial facilities (environmental control)
- Building management systems (HVAC monitoring)
- Cold storage facilities (temperature tracking)


### 10.9.2 Example
[View](../../examples/v1.0/OT/osiris_minimal_ot_infrastructure.json)

### 10.9.3 Key features demonstrated
**OT resource types:**
- `ot.sensor.environmental` - Distinguishes OT from IT resources
- Operational Technology namespace (`ot.*`)
- Physical monitoring equipment
- Facility management systems integration

**IT vs OT distinction:**
- IT resources: `compute.*`, `network.*`, `storage.*`
- OT resources: `ot.sensor.*`, `ot.camera.*`, `ot.access.*`
- Clear domain separation
- Enables IT/OT convergence modeling

**Minimal OT requirements:**
- Smallest valid OT document
- Required fields: id, type, provider (with name and namespace)
- Custom provider for non-cloud OT vendors
- Site attribution for physical location

**ID construction:**
- OT format: `site::device-identifier`
- Site code: `mxp` (example of a datacenter location)
- Device identifier: `sensor-temp-01`
- Human-readable, location-aware naming

**Use cases:**
- Starting point for OT infrastructure modeling
- Template for building management systems
- Facility monitoring system exports
- Environmental monitoring integration
- IT/OT convergence documentation

---

## 10.10 OT Cross connection with IT Network
### 10.10.0 Overview
**IT/OT network convergence** with security segmentation:
- Separate IT and OT network segments
- Industrial firewall
- SCADA equipment
- Secure cross-segment connectivity


### 10.10.1 Scenario
- IT Zone: Enterprise servers, users
- OT Zone: SCADA, PLCs, HMI
- Firewall: Paloalto at boundary


### 10.10.2 Example
[View](../../examples/v1.0/OT/osiris_it_ot_cross_network_topology.json)


### 10.10.3 Key features demonstrated
- Zone-based segmentation
- Industrial firewall configuration
- SCADA/ICS equipment
- Security policies in connection properties
- Logical grouping by network segment

---

## 10.11 OT Industrial printer
### 10.11.0 Overview
**Zebra ZT620** industrial printer connected to network infrastructure.


### 10.11.1 Scenario
Warehouse label printing:
- Thermal transfer printer
- Network connectivity
- Integration with WMS


### 10.11.2 Example
[View](../../examples/v1.0/OT/osiris_industrial_printer.json)


### 10.11.3 Key features demonstrated
- Industrial equipment specifications
- Network integration
- PoE considerations
- OT device on IT network

---

## 10.12 OT Security camera
### 10.12.0 Overview
IP **security camera system** with:
- Axis IP cameras
- Network Video Recorder (NVR)
- PoE network switches


### 10.12.1 Scenario
- Multiple cameras (entrance, server room)
- Central NVR for storage
- RTSP video streams


### 10.12.2 Example
[View](../../examples/v1.0/OT/osiris_security_camera.json)


### 10.12.3 Key features demonstrated
- PoE power requirements (class 4)
- Video stream specifications
- `flow` connection for RTSP streams
- NVR storage capacity

---

## 10.13 OT Door access control
### 10.13.0 Overview
Physical security **access control system** with:
- Access control panel/controller
- Door card readers
- Network-connected locks


### 10.13.1 Scenario
- `ot.access.controller` - Central panel
- `ot.access.reader` - Card/biometric readers
- `ot.access.lock` - Electronic door locks


### 10.13.2 Example
[View](../../examples/v1.0/OT/osiris_door_access_control.json)


### 10.13.3 Key features demonstrated
- OT-specific resource types
- `control` connection type
- Physical location tracking
- Controller to reader relationships

---

## 10.14 Summary
### About example generators
The examples in this repository include `generator` metadata showing hypothetical parser names. These parsers are planned for future development and do not yet exist in the v1.0 release.

**Current Status:**
Examples are hand-crafted for documentation and validation.

**Future Parsers:**
The OSIRIS project will begin developing reference parsers for well known hyperscalers and brands:
- AWS (osiris-aws-parser)
- Azure (osiris-azure-parser)
- GCP (osiris-gcp-parser)
- OCI (osiris-oci-parser)
- IBM (osiris-ibm-parser)
- Alibaba Cloud (osiris-ali-parser)
- Tencent Cloud (osiris-tc-parser)
- Proxmox (osiris-proxmox-parser)
- VMware (osiris-vmware-parser)
- Arista (osiris-arista-parser)
- Cisco (osiris-cisco-parser)
- Ciena (osiris-ciena-parser)
- Nokia (osiris-nokia-parser)

To create your own parsers follow the implementation guidelines on chapter 11.


### 10.14.1 Common patterns
**ID construction:**
- Hyperscaler and Cloud Providers: `provider::native-id` (e.g. `aws::i-0abc123`)
- On-premise: `site::identifier` (e.g. `mxp::srv-r770-001`)
- Consistent formatting across all examples

**Provider attribution:**
- Standard providers use lowercase names (`aws`, `azure`)
- Custom providers require `namespace` field
- Native types preserve vendor format (`AWS::EC2::Instance`)

**Network Connection directionality:**
- `forward`: Unidirectional flow (A > B)
- `bidirectional`: Symmetric relationship (A <-> B)
- `reverse`: Reverse flow (B > A)

**Properties structure:**
- Type-specific properties in `properties` object
- Vendor extensions in `extensions` object
- Labels and metadata in `tags` object

---

# 11 Implementation guidelines
## 11.0 Overview
This chapter provides practical guidance for implementing OSIRIS **producers** (parsers) and **consumers** (tools that read and process OSIRIS documents). These guidelines are intended to improve interoperability, stability and long-term maintainability across implementations.

OSIRIS is designed for real-world infrastructure exports that may be incomplete or partially discoverable. Implementations **SHOULD** prioritize correctness, determinism and forward compatibility.

---

## 11.1 Parser development
### 11.1.1 Core responsibilities
A parser (also called a **producer**) is any component that generates OSIRIS documents from a source system (API, inventory database, CLI outputs, telemetry snapshots etc.).

A producer **MUST**:
- Emit documents that pass **JSON Schema validation** (Chapter 9).
- Populate required fields (`version`, `metadata.timestamp` and required resource fields).
- Produce stable, well-formed `id` values and valid references.

A producer **SHOULD**:
- Provide `metadata.generator` with a stable tool name and version.
- Describe export boundaries in `metadata.scope` (accounts/projects/subscriptions, regions, environments, sites).
- Produce deterministic outputs for the same input (see 11.1.3).


### 11.1.2 Mapping strategy
**Type mapping**
- Producers **SHOULD** map to standard resource types from Chapter 7 whenever possible.
- When a native object has no suitable standard mapping, producers **MAY**:
  - Use a custom type following the rules in Chapter 7.
  - Represent additional semantics developing dedicated `extensions` (Chapter 8).

**Provider attribution**
- `provider.name` **MUST** identify the originating platform/vendor in lowercase.
- `provider.native_id` **SHOULD** capture the primary native identifier used by the provider to easily locate the resource.
- Additional provider context (e.g. `region`, `account`, `subscription`, `project`, `site`) **SHOULD** be included when applicable and stable.

**Properties vs extensions**
- Generic, cross-vendor attributes **SHOULD** go in `properties`.
- Vendor-specific or organization-specific details **SHOULD** go in `extensions` using a namespaced key (Chapter 8).
- Producers **SHOULD NOT** duplicate the same data in both `properties` and `extensions` unless required for interoperability.


### 11.1.3 Identity and ID stability
**Stable identity** is critical for topology merging, diffing and downstream automation.

Producers **MUST** ensure:
- Resource `id` values are **unique** within the document.
- Connection/group references resolve to valid resource IDs (Chapter 9 semantic rules).
- IDs remain stable across exports when the underlying entity is the same.

**Recommended ID patterns (examples)**
- Cloud and hyperscaler: `provider::native-id`
  Examples: `aws::i-0abc123`, `azure::/subscriptions/.../virtualMachines/vm01`
- On-prem: `site::identifier`
  Examples: `mxp::sw-core-01`, `mxp::srv-r770-001`
- OT: `site::identifier`
  Examples: `mxp-plant-01::sensor-temp-01`

**Determinism**
- If the source provides a stable unique identifier, producers **SHOULD** build `id` from it.
- Producers **SHOULD NOT** generate random IDs for real resources.
- If a stable native identifier is not available, producers **MAY** derive a deterministic ID from a stable tuple (e.g. `{site, name, serial}`) and **SHOULD** document the strategy.


### 11.1.4 Relationship extraction for connections and groups
Producers **SHOULD** emit explicit relationships whenever they are known:
- Network connectivity, dependency, flow, containment, attachment, routing and similar (Chapter 5).
- Logical boundaries (VPC, subnet, security zone, cluster, rack, availability zone, etc.) as groups (Chapter 6).

Guidance:
- Use **connections** when the relationship must be traversed as a graph edge (e.g. network path, dependency chain, flow).
- Use **groups** for classification, organization and boundaries (e.g. ownership, cost center, environment, zone).
- Producers **SHOULD** avoid encoding relationships implicitly inside `properties` when they can be expressed as `connections` or `groups`.


### 11.1.5 Partial data and unknowns
OSIRIS supports incomplete inventories.

Producers **SHOULD**:
- Omit optional fields when unknown rather than emitting incorrect values.
- Prefer conservative modeling: it is better to omit a connection than to invent one.
- Use `tags` or namespaced metadata to document limitations (e.g. “no routing table available”, “LLDP disabled”).

Producers **MUST NOT**:
- Emit placeholders that look real in production exports (e.g. fake serials, fake hostnames, fake IPs) unless explicitly flagged as redacted/anonymized.


### 11.1.6 Validation workflow for producers
A producer **MUST** validate the output before publishing:

1. **Level 1 (structural):** JSON Schema validation  
2. **Level 2 (semantic):** ID uniqueness, reference integrity, type format rules  
3. **Level 3 (domain):** optional type recognition and best-practice checks

Producers **SHOULD** fail the export pipeline on Level 1 errors.  
Producers **SHOULD** treat Level 2 errors as export failures unless explicitly configured otherwise.


### 11.1.7 Document splitting and export scope
Large infrastructures may require multiple split documents.

Producers **MAY** split by:
- Provider hierarchy (account/subscription/project, or subscription/resource group/resources)
- Region and availability zone
- Environment (prod/stage/dev)
- Physical site (data center, plant)
- Domain boundary (IT vs OT)

When splitting, producers **SHOULD**:
- Ensure scope is clearly described in `metadata.scope`.
- Keep IDs consistent across documents (so consumers can merge reliably).


### 11.1.8 Logging and telemetry
Producers are often executed in automated pipelines (CI/CD, scheduled exports, inventory collectors). Consistent logging and basic telemetry greatly improve troubleshooting, reliability and performance tuning.


#### 11.1.8.1 What to log during parsing
Producers **SHOULD** emit structured logs (JSON logs recommended) with a stable set of fields.

Recommended log events:
- **Run start/end**
  - Export scope summary (provider, account/subscription/project, region/site, environment)
  - Input source (API/CLI/file) and collector version
- **Discovery summary**
  - Counts: discovered resources, emitted resources, emitted connections, emitted groups
  - Skipped items and reasons (unsupported type, missing permissions, filtered by scope)
- **Normalization decisions**
  - Type mapping used (native type > OSIRIS type) when non-obvious
  - ID strategy (native ID used vs derived tuple)
- **Validation results**
  - Schema validation pass/fail
  - Semantic validation pass/fail
  - Rule identifiers and JSONPath for failures (when applicable)

Logs **SHOULD** include:
- `run_id` (unique per export execution)
- `generator.name` and `generator.version`
- `scope` identifiers from `metadata.scope`
- `severity` (`debug`, `info`, `warn`, `error`)
- `event` (stable event name)
- Optional `resource_id` (OSIRIS `id`) and/or `provider.native_id` when a log line is resource-specific

Producers **MUST NOT** log secrets or sensitive values (see Chapter 13).

**Example of a structured log output:**
```json
{
  "timestamp": "2026-01-16T22:23:45Z",
  "severity": "info",
  "event": "discovery_complete",
  "run_id": "20260115-102340-aws-prod",
  "generator": {
    "name": "osiris-aws-parser",
    "version": "1.0.0"
  },
  "scope": {
    "provider": "aws",
    "account": "123456789012",
    "regions": ["eu-west-1"]
  },
  "counts": {
    "resources_discovered": 847,
    "resources_emitted": 842,
    "connections_emitted": 1203,
    "groups_emitted": 45,
    "skipped": 5
  },
  "skipped_reasons": {
    "unsupported_type": 3,
    "missing_permissions": 2
  }
}
```


#### 11.1.8.2 Performance metrics to track
Producers **SHOULD** track basic metrics to detect regressions and capacity issues:

- **Timing**
  - Total runtime
  - Time spent per phase: discovery, normalization, relationship inference, validation, serialization
- **Volume**
  - Resources emitted, connections emitted, groups emitted
  - Input objects fetched/parsed (if different from emitted)
- **API/IO**
  - API request count (by endpoint if possible)
  - API error count and retry count
  - Rate-limit/backoff occurrences
- **Quality**
  - Validation errors and warnings count (by rule ID if available)
  - Skipped/filtered count (by reason)
- **Resource usage (optional)**
  - Peak memory usage
  - Output document size (bytes)

Metrics **SHOULD** be emitted in a machine-readable form suitable for pipeline dashboards.


#### 11.1.8.3 Error reporting best practices
Producers **SHOULD** classify failures into clear categories:

- **Source access errors**
  - Authentication/authorization failures, missing permissions, rate limits
- **Parsing / normalization errors**
  - Unexpected native formats, unsupported types, missing required source fields
- **Validation errors**
  - Level 1 (schema) and Level 2 (semantic) failures
- **Operational errors**
  - IO failures, serialization issues, timeouts

When reporting an error, producers **SHOULD** include:
- A stable error code or rule identifier (when applicable)
- Human-readable message
- JSONPath to the failing OSIRIS element (if the error is in emitted data)
- The smallest useful context (e.g. provider name, native id, OSIRIS id)
- A remediation hint (e.g. required permission, missing API scope, mapping fix)

Producers **SHOULD**:
- Exit with non-zero status on Level 1 errors by default.
- Provide a configurable mode to downgrade selected semantic issues to warnings only when explicitly requested.
- Avoid cascading failures: continue collecting other resources when safe, but fail the run if the resulting output would be invalid.

Producers **MUST**:
- Never include credentials, tokens or secrets in logs, error messages or stack traces (see Chapter 13).
- Avoid logging raw payloads unless explicitly enabled in debug mode and appropriately redacted.


#### 11.1.8.4 Observability platforms integration

> [!NOTE]
> OSIRIS remains a **static snapshot format**.  
> This subsection describes **optional integration patterns** for shipping OSIRIS snapshots into observability platforms.  
> The transport, storage, indexing and retention model are implementation concerns and **outside the OSIRIS core scope**.

Producers deployed in production environments **MAY** integrate with observability platforms in two ways:

**Parser operational telemetry** (monitoring the parser itself):
- **Metrics systems**: Zabbix, Prometheus, Datadog, CloudWatch, Azure Monitor
  - Track parser performance, API usage, validation results
- **Log aggregation**: ELK, Splunk, Loki, CloudWatch Logs
  - Centralize parser logs for troubleshooting
- **Tracing**: OpenTelemetry, Jaeger, Zipkin
  - For parsers with complex multi-step flows

**Infrastructure topology snapshots** (OSIRIS documents as observability artifacts):
- Observability platforms **MAY** ingest OSIRIS documents as a **snapshot series** to enable:
  - Topology change tracking and visualization
  - Configuration drift detection
  - Incident correlation with infrastructure changes
  - Compliance monitoring across snapshots
- Platforms with topology/service map capabilities may support this natively or via plugins
- Producers emitting documents for this purpose **SHOULD** run at consistent intervals and maintain stable IDs across snapshots

When integrating with observability systems, producers **SHOULD**:
- Use consistent metric names with appropriate prefixes (e.g. `osiris.parser.aws.`)
- Include standard labels/tags:
  - `parser_name`, `parser_version`
  - `scope_provider`, `scope_region`
  - `snapshot_timestamp`, `document_size_bytes`
  - `resource_count`, `connection_count`, `group_count`
- Support sampling/filtering to manage high-volume telemetry

Producers **MUST** ensure that telemetry integration does not expose sensitive data (Chapter 13).

---

## 11.2 Consumer implementation
### 11.2.1 Version handling and negotiation
Consumers **MUST** read `version` and apply compatibility rules (Chapter 12).  
Consumers **MUST** ignore unknown fields as required for forward compatibility.

Consumers **SHOULD**:
- Support all `1.x.y` documents within the same major version when feasible.
- Provide clear diagnostics when a document uses an unsupported major version.


### 11.2.2 Consumer validation policy
Consumers **MUST** perform **Level 1** validation (or equivalent structural checks) before processing.
Consumers **SHOULD** perform **Level 2** validation before building graph structures.

Consumers **MAY** implement configurable strictness:
- `basic`: Level 1 only
- `default`: Level 1 + Level 2
- `strict`: Level 1 + Level 2 + selected Level 3 rules


### 11.2.3 Graph construction and traversal
Consumers **SHOULD** treat:
- `resources` as nodes
- `connections` as directed/bidirectional/undirected edges based on `direction`
- `groups` as membership relations (one-to-many references)

Consumers **SHOULD** build efficient indexes:
- `resourceById`
- `connectionsBySource`, `connectionsByTarget`
- `groupsById`, `membershipsByResource`

Consumers **MUST NOT** assume ordering in arrays is meaningful.


### 11.2.4 Unknown types and extensions
Consumers **MUST** accept unknown:
- Resource types
- Group types
- Connection types (if structurally valid)
- Extension namespaces

Consumers encountering unknown types **SHOULD**:
- Preserve them when re-exporting/translating
- Display type strings verbatim for debugging
- Continue processing known core fields


### 11.2.5 Merging diff and snapshot correlation
Consumers often process multiple OSIRIS documents over time.

Recommended approach:
- Treat `metadata.timestamp` as the snapshot point in time.
- Use stable `resource.id` as the primary key for correlation.
- If merging multiple documents, namespace collisions **MUST** be prevented (IDs must remain unique in the merged graph).

Consumers **SHOULD** support “soft merge” strategies when duplicates occur (e.g. prefer newest timestamp, or prefer a configured source).

---

## 11.3 Best practices
This section provides recommended practices for **producers** (parsers/exporters) and **consumers** (readers/ingestion tools) to maximize interoperability, stability and long-term maintainability.


### 11.3.1 Best practices for producers

- **Prefer standard types first**  
  Map native objects to the standard resource types from Chapter 7 whenever possible. Use custom types only when no suitable standard type exists.

- **Use `properties` for generic data and `extensions` for vendor/org specifics**  
  Put broadly applicable attributes in `properties`. Put vendor-specific or organization-specific details in `extensions` using a namespaced key (Chapter 8). Avoid duplicating the same data in both locations unless required for interoperability.

- **Generate stable, deterministic `id` values**  
  IDs should remain stable across exports for the same underlying entity. Prefer building `id` from stable provider/native identifiers. Avoid random IDs for real resources.

- **Always include provider traceability**  
  Populate `provider.name` and strongly prefer `provider.native_id`. Include stable scope context (account/subscription/project, region/site) when available.

- **Model relationships explicitly**  
  Use `connections` for graph edges that must be traversed (paths, dependencies, flows). Use `groups` to represent boundaries and classification (zones, clusters, environments, ownership). Avoid encoding relationships only inside `properties`.

- **Be conservative when data is incomplete**  
  Omit unknown optional fields rather than guessing. Prefer missing relationships over invented ones. Document known collection limitations via `tags` or namespaced metadata/extension fields.

- **Validate before publishing**  
  Integrate validation in the export pipeline: Level 1 (schema) and Level 2 (semantic) should be treated as failures by default. Use Level 3 checks as optional strict mode.

- **Keep exports reproducible**  
  For the same input snapshot, outputs should be deterministic. Avoid non-deterministic ordering or unstable derived values.

- **Split documents by stable boundaries**  
  For large infrastructures, split by account/subscription/project, region, environment, site or IT/OT domain boundary. Ensure `metadata.scope` clearly describes what the document contains.

**Recommended minimum `metadata`**
```json
"metadata": {
  "timestamp": "2026-01-01T10:30:00Z",
  "generator": { 
    "name": "osiris-aws-parser", 
    "version": "1.0.0" },
  "scope": {
    "provider": "aws",
    "regions": ["eu-west-1"],
    "account": "123456789012",
    "environment": "prod"
  }
}
```


### 11.3.2 Best practices for consumers

- **Validate early, fail safely**
  - Consumers **MUST** perform **Level 1** validation (or equivalent structural checks) before processing.
  - Consumers **SHOULD** perform **Level 2** validation before building graph structures (IDs, references, type format rules).
  - Consumers **MAY** offer configurable strictness (e.g. `basic`, `default`, `strict`) to match different use cases.

- **Implement forward compatibility by default**
  - Consumers **MUST** ignore unknown fields to support newer documents and extensions.
  - Consumers **MUST** accept unknown resource, connection and group types if the objects are structurally valid.
  - Consumers **MUST** accept unknown `extensions` namespaces and treat them as opaque data.

- **Do not rely on array ordering**
  - Consumers **MUST NOT** assume ordering of `resources`, `connections` or `groups` is meaningful.
  - Consumers **SHOULD** operate using IDs and indexes rather than positions.

- **Build indexes before complex processing**
  Consumers **SHOULD** build efficient lookup structures such as:
  - `resourceById`
  - `connectionsBySource`, `connectionsByTarget`
  - `groupsById`
  - `membershipsByResource` (reverse membership index)

- **Preserve fidelity during transformation**
  - When filtering, translating or re-exporting documents, consumers **SHOULD** preserve:
    - unknown fields
    - unknown types
    - unknown extension namespaces
  - Consumers **SHOULD NOT** drop or rename data unless explicitly configured.

- **Treat the topology as a graph**
  - Consumers **SHOULD** interpret:
    - `resources` as nodes
    - `connections` as edges (directed/bidirectional/undirected based on `direction`)
    - `groups` as membership relations (classification/boundaries)
  - Consumers **SHOULD** keep a clear separation between graph edges (`connections`) and classification (`groups`).

- **Support snapshot correlation and diffing**
  - Consumers **SHOULD** treat `metadata.timestamp` as the snapshot time.
  - Consumers **SHOULD** use stable `resource.id` values as primary keys for correlation across snapshots.
  - When merging documents, consumers **MUST** prevent ID collisions (IDs must be unique in the merged graph).

- **Provide actionable diagnostics**
  Consumers **SHOULD** emit diagnostics that include:
  - severity (`error` / `warning` / `info`)
  - rule identifier when available (from Chapter 9)
  - JSONPath (or equivalent) to the failing element
  - a human-readable message
  - a suggested fix or remediation hint

- **Handle partial data gracefully**
  - Consumers **SHOULD** continue processing when optional fields are missing.
  - Consumers **SHOULD** avoid inferring relationships unless explicitly required and clearly marked as derived.
  - Consumers **MAY** expose confidence or provenance for derived insights.

- **Security-aware ingestion**
  - Consumers **SHOULD** treat all string fields (including `extensions`) as untrusted input.
  - Consumers **SHOULD** sanitize or escape data before rendering in UIs or exporting to other formats.
  - Consumers **SHOULD** support redaction and filtering policies before storing or sharing documents (see Chapter 13).


### 11.3.3 Common pitfalls
This section highlights recurring implementation mistakes and provides practical tips to improve interoperability across exporters, validators, pipelines and visualization tools.

#### Common pitfalls to avoid

- **Unstable resource IDs**
  - Using random UUIDs for real resources breaks correlation, diffing and merging across snapshots.
  - **Suggestion:** derive `id` deterministically from stable provider identifiers (`provider.name` + `provider.native_id`) or a stable tuple when native IDs are not available.

- **Duplicate or conflicting identity fields**
  - Storing the same identifier in multiple places (e.g. `id`, `provider.native_id` and a custom extension) without a clear rule creates ambiguity.
  - **Suggestion:** keep `id` as the document identifier and `provider.native_id` as the authoritative provider locator; use extensions only for additional native identifiers.

- **Encoding relationships implicitly**
  - Hiding dependencies or topology relationships in `properties` (strings like `connected_to`, `peer`, `uplink`) prevents graph traversal and consistent tooling.
  - **Suggestion:** use `connections` for traversable edges and `groups` for classification/boundaries.

- **Misusing groups as topology edges**
  - Treating group membership as a network link or dependency leads to incorrect graph semantics.
  - **Suggestion:** groups organize and classify; connections express relationships that can be traversed.

- **Rejecting unknown types or namespaces**
  - Consumers that fail on unknown resource types or unknown extension namespaces are not forward-compatible.
  - **Suggestion:** accept unknown types/namespaces if structurally valid; preserve them as opaque data.

- **Assuming array ordering is meaningful**
  - Depending on the order of `resources`, `connections` or `groups` causes non deterministic behavior across exporters and serializers.
  - **Suggestion:** always index by `id` and traverse via references.

- **Dropping unknown fields during transformation**
  - Normalizers and converters that remove fields they do not recognize can silently destroy information.
  - **Suggestion:** preserve unknown fields by default; only drop data when explicitly configured.

- **Inventing values for unknowns**
  - Filling missing fields with “fake but plausible” values (serials, IPs, hostnames) contaminates datasets and downstream automation.
  - **Suggestion:** omit unknown optional fields; if anonymization is required, mark it clearly and use consistent redaction patterns.

- **Merging documents without collision control**
  - Combining OSIRIS documents can introduce duplicate IDs and broken references.
  - **Suggestion:** define a merge strategy and enforce uniqueness (e.g. stable global IDs, or deterministic prefixing rules at merge time).

- **Overloading `extensions` with core semantics**
  - Putting essential cross-tool data only in extensions reduces interoperability.
  - **Suggestion:** place broadly relevant attributes in core fields (`name`, `provider`, `properties`), use extensions for vendor/org-specific details.

---

### 11.3.4 Interoperability tips and checklist

#### Tips
- **Normalize identity early**
  - In ingestion pipelines, build a consistent internal key using `{resource.id}` first and `{provider.name + provider.native_id}` as a secondary correlation hint.

- **Emit and consume explicit scope**
  - Producers should populate `metadata.scope`; consumers should surface it for users and use it to avoid accidental cross-scope merges.

- **Prefer conservative inference**
  - If you infer connections or groups (e.g. from naming, subnets, LLDP), mark them as derived (e.g. via `tags` or namespaced metadata) and avoid overwriting explicit data.

- **Use validation levels consistently**
  - Producers should fail exports on Level 1 and usually Level 2 errors.
  - Consumers should offer strictness modes and clearly distinguish errors vs warnings.

- **Preserve unknown namespaces end-to-end**
  - A good ecosystem behavior is: parse > validate > enrich > re-export without losing unknown namespaces or future fields.

#### Practical checklist
- [ ] Resource IDs are deterministic and stable across exports for the same entity.
- [ ] All `connections` and `groups` reference valid `resource.id` values.
- [ ] Provider traceability is present (`provider.name` and preferably `provider.native_id`, plus stable scope context).
- [ ] Consumers ignore unknown fields, unknown types and unknown extension namespaces without failing.
- [ ] No implementation assumes array ordering for correctness.
- [ ] Transformation tools preserve unknown fields unless explicitly configured otherwise.
- [ ] Merge operations prevent ID collisions and do not break references.
- [ ] No invented “real-looking” values are emitted for unknown data unless explicitly flagged as redacted/anonymized.