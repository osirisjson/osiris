# Changelog

All notable changes to the OSIRIS JSON Specification will be documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Package versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

# [1.0.0] - 2026-02-01
### Changed
- Specification to Stable release
- README
- CODE_OF_CONDUCT
- CONTRIBUTING


# [1.0.0-draft.17] - 2026-01-30
### Added
- Appendices (A-D)

### Fixed
- Minor grammar fixes across multiple sections
- Minor refinements to OSIRIS schema semantics
- Minor refinements to topology directionality rules


# [1.0.0-draft.16] - 2026-01-29
### Added
- Chapter 14: (14.1-14.2)
 
### Changed
- Section 8.4.2 added reference and registered namespace for SONiC Network Operating System (open NOS)
 
### Fixed
- Minor grammar fixes in some sections
- Minor refinements to markdown code fences


# [1.0.0-draft.15] - 2026-01-28
### Added
- Chapter 13: (13.1-13.3)

### Changed
- Section 3.3.3 added "intended_audience" and "purpose" this aligns with examples of section 13.1.3


# [1.0.0-draft.14] - 2026-01-26
### Added
- Chapter 12: (12.1-12.3)

### Fixed
- Minor fixes across some sections for standardisation mismatch


# [1.0.0-draft.13] - 2026-01-22
### Added
- Chapter 11: (11.1-11.3)

### Fixed
- Minor fixes across some sections for standardisation mismatch


# [1.0.0-draft.12] - 2026-01-15
### Added
- Chapter 9: (9.1-9.5)
- Chapter 10: (10.1-10.5)
- Drafted first version of core schema osiris.schema.json
- Drafted contributing.md and code_of_conduct.md

### Changed
- Examples for IT and OT environments

### Fixed
- Minor fixes across some sections for standardisation mismatch
- README.md
- Heading and TOC fixes


# [1.0.0-draft.11] - 2026-01-07
### Added
- Chapter 8: (8.1-8.6)

### Fixed
- Minor fixes across some sections for standardisation mismatch


# [1.0.0-draft.10] - 2026-01-04
### Added
- Chapter 7: (7.3-7.7)
- Begin drafting OSIRIS JSON examples in examples/ directory

### Fixed
- Minor fixes across some sections for standardisation mismatch


# [1.0.0-draft.9] - 2025-12-31
### Changed
- Standardized ID format across all object types
- Unified naming conventions improvement
- Enhanced examples throughout chapters 2-6 with realistic data

### Improved
- Resource identity (2.1.2): Added ID construction strategies and namespacing rules
- Chapter 4 (Resources): Improved standards and examples with type, native_id, and region fields
- Chapter 5 (Connections): Added ID guidance and nested interface structures
- Chapter 6 (Groups): Added ID construction and hierarchical examples

### Fixed
- Inconsistent ID formats (now `provider::native-id` throughout)
- Missing provider object fields (type, native_id)
- IP address nesting (now `ip_addresses.private_ip`)
- MXP datacenter naming across on-premise examples
- JSON syntax errors (trailing commas, missing fields)

### Added
- Chapter 7: (7.1-7.2)


# [1.0.0-draft.8] - 2025-12-30
### Changed
- Table of contents and linking with Markdown All in One module


# [1.0.0-draft.7] - 2025-12-30
### Changed
- Dividers between sections on chapter 5 for improved readability.

### Added
- Chapter 6: (6.1-6.6)


# [1.0.0-draft.6] - 2025-12-28
### Changed
- Chapter 4 DELL poweredge model to 770 model for making real examples with Arista DCS-7050SX3-48YC12 equipment and transceivers.

### Added
- Chapter 5: (5.1-5.4)


# [1.0.0-draft.5] - 2025-12-26
### Changed
- TOC
- Sub-section numbering

### Added
- Chapter 4: (4.1-4.5)


# [1.0.0-draft.4] - 2025-12-24
### Added
- Chapter 3: (3.1-3.4)


# [1.0.0-draft.3] - 2025-12-23
### Added
- Chapter 2: (2.1-2.4)


# [1.0.0-draft.2] - 2025-12-18
### Changed
- Titles formatting for better readability and added section dividers.

### Added
- Initial OSIRIS JSON Format Specification draft structure.
- Preface.
- Section 1: Introduction (1.1-1.5).


## [1.0.0-draft.1] - 2025-12-13
### Added
- Initial OSIRIS JSON Format Specification draft structure.
- Written preface

### Changed
- N/A
