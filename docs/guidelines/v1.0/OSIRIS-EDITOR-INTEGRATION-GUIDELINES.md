# OSIRIS JSON Editor Integration Guidelines<!-- omit in toc -->
| Field     | Value |
| --------- | ----- |
| Authors   | Tia Zanella [skhell](https://github.com/skhell) |
| Revision  | 1.0.0-DRAFT |
| Creation date      | 08 February 2026 |
| Last revision date | 20 February 2026 |
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
<!-- work in progress -->


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