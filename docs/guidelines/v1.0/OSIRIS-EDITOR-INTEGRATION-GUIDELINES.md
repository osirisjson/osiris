# OSIRIS JSON Editor Integration Guidelines<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 08 February 2026 |
| Last revision date | 21 February 2026 |
| Status    | Draft |
| Document ID | OSIRIS-ADG-EI-1.0 |
| Document URI | [OSIRIS-ADG-EI-1.0](https://github.com/osirisjson/osiris-editor-integrations/tree/main/docs/guidelines/v1.0/OSIRIS-EDITOR-INTEGRATION-GUIDELINES.md) |
| Document Name | OSIRIS JSON Editor Integration Guidelines |
| Specification ID | OSIRIS-1.0 |
| Specification URI | [OSIRIS-1.0](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) |
| Schema URI | [OSIRIS-1.0](https://osirisjson.org/schema/v1.0/osiris.schema.json) |
| License   | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Repository | [github.com/osirisjson/osiris-editor-integrations](https://github.com/osirisjson/osiris-editor-integrations) |

# Table of Content
- [Table of Content](#table-of-content)
- [1 Extension architecture](#1-extension-architecture)
  - [1.1 LSP vs. direct integration](#11-lsp-vs-direct-integration)
  - [1.2 Consuming @osirisjson/core diagnostics](#12-consuming-osirisjsoncore-diagnostics)
    - [1.2.1 Pre-validation JSON parse failures (editor behavior)](#121-pre-validation-json-parse-failures-editor-behavior)
  - [1.3 Schema resolution alignment](#13-schema-resolution-alignment)
- [2 User experience standards](#2-user-experience-standards)
  - [2.1 Visualizing diagnostics](#21-visualizing-diagnostics)
  - [2.2 Autocomplete snippets and schema awareness](#22-autocomplete-snippets-and-schema-awareness)
  - [2.3 Resource navigation](#23-resource-navigation)
- [3 Performance constraints](#3-performance-constraints)
  - [3.1 Debounce logic for large documents](#31-debounce-logic-for-large-documents)
  - [3.2 Background worker strategy](#32-background-worker-strategy)
- [4 Telemetry and privacy](#4-telemetry-and-privacy)
  - [4.1 Opt-in telemetry principles](#41-opt-in-telemetry-principles)
  - [4.2 Privacy wall](#42-privacy-wall)
  - [4.3 Offline-first validation](#43-offline-first-validation)


# 1 Extension architecture
Editor integrations bring OSIRIS validation and navigation directly into the authoring environment. The reference integration targets VS Code/VSCodium, but the patterns defined here apply to any editor that supports the Language Server Protocol (LSP) or direct library embedding.

Editor integrations **MUST** delegate all validation behavior to `@osirisjson/core`. They **MUST NOT** ship their own validation logic, re-interpret diagnostic codes or alter severity assignments. The extension is a presentation and interaction layer, not a validation engine.

> [!NOTE]
> Back-reference: The editor pipeline contract is defined in [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) section 3.3.
> Validation engine internals (pipeline stages, short-circuit behavior, range resolution) are defined in [OSIRIS-ADG-TLB-CORE-1.0](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-TOOLBOX-CORE.md).
> The diagnostic code registry and severity model are defined in [OSIRIS-ADG-VL-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-VALIDATION-LEVELS.md).
> Repository boundaries for IDE UX changes are defined in [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) section 2.1.2.

---

## 1.1 LSP vs. direct integration
OSIRIS editor integrations **MAY** adopt either an LSP-based or direct-integration architecture. Both approaches are valid; the choice depends on target editor and performance requirements.

**LSP-based (recommended for multi-editor reach):**
The extension runs an LSP server (Node.js process) that imports `@osirisjson/core` and responds to `textDocument/didOpen`, `textDocument/didChange` and `textDocument/didSave` notifications. The LSP server converts `Diagnostic[]` from the engine into LSP `Diagnostic` objects and publishes them via `textDocument/publishDiagnostics`. This approach enables a single server implementation to support VS Code, VSCodium, Neovim and any LSP-capable editor.

**Direct integration (recommended for VS Code-first development):**
The extension imports `@osirisjson/core` directly in the extension host process and pushes diagnostics to the VS Code `DiagnosticCollection` API. This avoids the LSP protocol overhead and simplifies the initial development path. The extension **SHOULD** be structured so that the validation-and-mapping logic can be extracted into an LSP server later without rewriting.

**Regardless of approach, the non-negotiable rule is:** validation behavior comes from `@osirisjson/core`. The extension maps engine output to editor APIs; it does not implement, filter or reorder validation rules.

---

## 1.2 Consuming @osirisjson/core diagnostics
The engine returns a `Diagnostic[]` array. The extension maps each entry to the editor's native diagnostic model.

**Mapping rules:**
- `severity` maps directly: `"error"` -> editor Error, `"warning"` -> editor Warning, `"info"` -> editor Information/Hint.
- `code` **SHOULD** be surfaced in the diagnostic UI (e.g. as a clickable code in the Problems panel) and **SHOULD** link to the corresponding documentation page for that code.
- `range` (0-based, LSP-style) maps directly to VS Code/LSP ranges. When `range` is absent (source text not provided to the engine), the extension **SHOULD** fall back to highlighting line 0, character 0 and include the `path` (JSON Pointer) in the message for manual location.
- `related` locations **SHOULD** be mapped to LSP `DiagnosticRelatedInformation` to enable cross-reference navigation (e.g. "connection target not found" -> jump to the connection definition).
- `fix` metadata **SHOULD** be mapped to VS Code Code Actions (quick fixes). Fixes from the engine are deterministic and non-destructive; the extension **SHOULD** expose them as user-invoked Code Actions (quick fixes) and apply the workspace edit when the user selects the action.

**Source text requirement:**
To populate `range` fields, the extension **MUST** pass the raw document text to `@osirisjson/core` via the `sourceText` option. Without source text, the engine can only produce JSON Pointer paths and the editor cannot render inline squiggles accurately.

**Profile selection:**
The extension **SHOULD** expose a configuration setting (e.g. `osiris.validationProfile`) that maps to the engine's `profile` option (`basic`, `default`, `strict`). The default **SHOULD** be `default`. Changes to the profile setting **SHOULD** trigger re-validation of all open OSIRIS documents.

### 1.2.1 Pre-validation JSON parse failures (editor behavior)
`@osirisjson/core.validate()` requires a parsed JSON object. If the document cannot be parsed as JSON, the extension MUST NOT call the engine.
Instead, it SHOULD surface the editor’s native JSON syntax diagnostics and retry OSIRIS validation only after parsing succeeds.

---

## 1.3 Schema resolution alignment
Editor schema resolution **MUST** follow the same offline-first policy as the CLI (defined in [OSIRIS-ADG-TLB-CLI-1.0](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-TOOLBOX-CLI.md) section 2.3).

**Resolution order:**
1. **Extension setting override:** a workspace or user setting (e.g. `osiris.schemaPath`) pointing to a local schema file.
2. **Document `$schema` field:** used as a version hint to select the appropriate bundled schema. The extension **MUST NOT** fetch the URL from the network.
3. **Bundled default:** the engine's bundled schema matched by the document's `version` field using `MAJOR.MINOR` routing.

**Rationale:** a document that validates in the editor **MUST** produce the same result in CI via `@osirisjson/cli`. Schema resolution divergence between editor and CLI is a critical defect.

---

# 2 User experience standards
This chapter defines UX patterns for editor integrations. It specifies **what** the experience should be, not implementation details (CSS, DOM, theme tokens). Implementations follow the target editor's native APIs and design language.

---

## 2.1 Visualizing diagnostics
**Squiggles and inline markers:**
Diagnostics with `range` **MUST** render as inline squiggles (underlines) at the reported location. The squiggle style follows editor convention: wavy underline for errors, wavy underline for warnings (typically different color), and dots or subtle underline for info-level hints.

**Severity-to-presentation mapping:**
| Engine severity | Editor presentation | Problems panel grouping |
|---|---|---|
| `error` | Error squiggle (red) | Errors section |
| `warning` | Warning squiggle (yellow) | Warnings section |
| `info` | Information hint (blue) | Info section |

The extension **MUST NOT** invent custom severity levels or recolor diagnostics beyond the editor's native severity palette.

**Hover content:**
When the user hovers over a squiggle, the extension **SHOULD** display: the diagnostic `message`, the `code` as a clickable link to its documentation page and the `path` (JSON Pointer) for orientation in large documents.

**Related locations:**
For cross-reference diagnostics (e.g. `V-REF-002`: connection target not found), the hover **SHOULD** include links to related locations. In VS Code, this maps to `DiagnosticRelatedInformation` which renders as clickable file:line references in the Problems panel.

---

## 2.2 Autocomplete snippets and schema awareness
Schema-aware editing features improve authoring speed and reduce structural errors before validation runs.

**JSON Schema-driven completions:**
The extension **SHOULD** provide context-aware completions derived from the OSIRIS JSON Schema. The schema is the same bundled artifact used for validation (section 1.3). Completions include field names at the current cursor context, enum values for constrained fields (e.g. `direction`: `"bidirectional"`, `"forward"`, `"reverse"`) and required field scaffolding when creating a new object.

**Snippets for common structures:**
The extension **SHOULD** provide snippets for frequently authored structures: a minimal OSIRIS document skeleton (`version`, `metadata`, `topology` with an empty resources array), a resource template with required fields (`id`, `type`, `provider`) and connection/group templates. Snippets **SHOULD** use the editor's native snippet engine (e.g. VS Code `SnippetString` with tab stops) and **MUST** produce output that passes Stage 1 validation when placeholder values are filled in.

**ID reference completions:**
When the cursor is inside a field that references an ID (e.g. `connections[].source`, `connections[].target`, `groups[].members[]`, `groups[].children[]`), the extension **SHOULD** offer completions populated from the current document's resource and group IDs. This requires the extension to maintain a lightweight in-memory index of IDs from the current document, rebuilt on each validation pass or on document change.

---

## 2.3 Resource navigation
OSIRIS documents describe graphs. Navigating relationships (connections, group membership) is a core editing task.

**Go-to-definition for ID references:**
When the user invokes "Go to Definition" (or equivalent) on an ID string that appears in `connections[].source`, `connections[].target`, `groups[].members[]` or `groups[].children[]`, the extension **SHOULD** navigate to the resource or group definition with that `id` in the same document. This requires the same in-memory ID index described in section 2.2.

**Find all references:**
The extension **SHOULD** support "Find All References" on a resource or group `id` field, returning all locations where that ID is referenced (connections, group membership, group children). This enables users to understand the impact of renaming or removing a resource.

**Document outline:**
The extension **SHOULD** contribute to the editor's document outline (VS Code Outline view / breadcrumbs) with a structured tree: top-level sections (`metadata`, `topology`), then `resources`, `connections` and `groups` as expandable nodes, with individual items labeled by `id` or `name`.

---

# 3 Performance constraints
OSIRIS documents range from minimal topologies (<1 KB) to large-scale inventories with thousands of resources. Editor integrations **MUST** remain responsive regardless of document size.

**Diagnostic bounding**
Editor integrations **MUST** keep diagnostic rendering bounded. They **SHOULD** keep engine caps at defaults or lower for UI responsiveness and **MUST NOT** publish unbounded diagnostic arrays.
(See engine diagnostic capping and ecosystem performance constraints.)

> [!NOTE]
> Back-reference: Ecosystem performance constraints are defined in [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) section 5.1.
> The engine's concurrency model (synchronous, single-threaded, consumer-side parallelism) is defined in [OSIRIS-ADG-TLB-CORE-1.0](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-TOOLBOX-CORE.md) section 3.1.

---

## 3.1 Debounce logic for large documents
The extension **MUST** debounce validation to avoid triggering a full pipeline run on every keystroke.

**Debounce strategy:**
- On `textDocument/didChange`: reset a debounce timer. Do not validate immediately.
- After the debounce interval elapses with no further changes: trigger validation.
- On `textDocument/didSave`: trigger validation immediately (bypass debounce). Users expect save to produce fresh results.

**Recommended debounce intervals:**

| Document size | Interval | Rationale |
|---|---|---|
| < 100 KB | 300 ms | Small documents validate fast; near-instant feedback. |
| 100 KB – 1 MB | 750 ms | Avoid repeated validation of mid-size documents during rapid editing. |
| > 1 MB | 1500 ms | Large documents take longer to validate; longer debounce prevents stacking. |

These intervals are **recommendations**, not normative requirements. Implementations **MAY** adjust based on measured validation latency and **SHOULD** expose a user-configurable setting (e.g. `osiris.validationDelay`).

**Cancellation:**
If a new edit arrives while validation is in progress, the extension **SHOULD** cancel the in-flight validation (or discard its results) and schedule a new run. Stale diagnostics from a cancelled run **MUST NOT** be published to the editor.

---

## 3.2 Background worker strategy
Validation **MUST NOT** block the editor's UI thread.

**VS Code / direct integration:**
`@osirisjson/core` is synchronous. To avoid blocking the extension host, the extension **SHOULD** run validation in a `Worker` thread (Node.js `worker_threads`). The main thread sends the document text to the worker; the worker runs `validate()` and returns `Diagnostic[]`. The main thread maps results to the editor's diagnostic API.

**LSP-based integration:**
The LSP server process is inherently separate from the editor UI thread. Validation runs in the server process. The server **SHOULD** handle cancellation via LSP `$/cancelRequest` or by discarding stale results when a newer document version arrives.

**Progress indication:**
For documents where validation takes more than 500 ms, the extension **SHOULD** display a progress indicator (e.g. VS Code status bar item or progress notification) so the user knows validation is running. The indicator **MUST** be dismissed when validation completes or is cancelled.

---

# 4 Telemetry and privacy
OSIRIS documents describe infrastructure topology, which is inherently sensitive. The extension's telemetry and network behavior **MUST** respect this reality.

> [!NOTE]
> Back-reference: The OSIRIS specification's security considerations are defined in [OSIRIS-JSON-v1.0](https://github.com/osirisjson/osiris/tree/main/specification/v1.0/OSIRIS-JSON-v1.0.md) Chapter 13.
> Cross-cutting security constraints are defined in [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) section 5.2.

---

## 4.1 Opt-in telemetry principles
If the extension collects any telemetry, it **MUST** be strictly opt-in. Telemetry **MUST** be disabled by default and enabled only through an explicit user action (e.g. a settings toggle with clear disclosure).

**Permitted telemetry (when opted in):**
- Extension activation/deactivation events
- Validation profile usage (which profile is selected, not what documents are validated)
- Aggregate diagnostic counts by code family (e.g. "3 V-REF errors, 1 V-DOM warning") without document content
- Performance metrics (validation duration, document size bucket) for optimization

**Prohibited telemetry (regardless of opt-in):**
- Document content (partial or complete), resource IDs, names or property values
- File paths or workspace structure
- Network topology, IP addresses or any field values from OSIRIS documents
- Diagnostic messages (which may contain document-derived values like IDs)

---

## 4.2 Privacy wall
The extension **MUST** enforce a hard boundary between document content and any outbound data.

**The rule:**
No document content or document-derived data (IDs, values, paths, topology) may be transmitted. Network features (e.g. update checks) are allowed only if they are strictly independent of document content.

**Crash reports:**
If the extension reports crashes, stack traces **MUST** be scrubbed of any document-derived values (IDs, paths, field values) before transmission. Only extension code paths and engine version information are permitted.

---

## 4.3 Offline-first validation
Validation **MUST** work fully offline. This is a non-negotiable architectural constraint inherited from the ecosystem (see [OSIRIS-ADG-1.0](https://github.com/osirisjson/osiris/tree/main/docs/guidelines/v1.0/OSIRIS-ARCHITECTURE.md) section 5.1 and [OSIRIS-ADG-TLB-CORE-1.0](https://github.com/osirisjson/osiris-toolbox/tree/main/docs/guidelines/v1.0/OSIRIS-TOOLBOX-CORE.md) section 1.1.3).

**Mandatory constraints:**
- The extension **MUST** bundle all required schemas. It **MUST NOT** require network access to validate a document.
- Schema resolution **MUST NOT** trigger HTTP requests to `osirisjson.org` or any other endpoint during validation.
- The extension **MUST** function identically on air-gapped machines, behind corporate proxies and in offline environments.

**Optional network features:**
The extension **MAY** offer opt-in network features (e.g. checking for extension updates, fetching newer schema versions via an explicit "update schemas" command). These features **MUST** be clearly separated from validation behavior, disabled by default in restricted environments and documented in the extension's privacy policy.