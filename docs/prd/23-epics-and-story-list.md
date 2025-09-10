# 23. Epics and Story List

### E1 — Language Pack & Rule Engine (MVP)
- S1.1 Load language pack from local file (JSON/YAML); validate against schema; cache via clientStorage.
- S1.2 Load language pack from URL (opt-in network); checksum/signature validation when provided.
- S1.3 Rule evaluation engine supporting string/alias/forbidden and regex style rules with severity.
- S1.4 Settings UI for scope, locale, severity filters; persistence per file.

### E2 — Scan & Issue Surfacing (MVP)
- S2.1 Traverse TEXT nodes by scope (selection/page/file) and extract text content with node/page refs.
- S2.2 Issues panel: grouped by page/rule with counts, search, filters (severity/page/rule).
- S2.3 Inline highlights: lightweight overlays for flagged spans in canvas; toggle on/off.

### E3 — Fix Application & Undo (MVP)
- S3.1 Apply single fix action on a flagged range; ensure full undo compatibility.
- S3.2 Apply-all by rule/group with atomic transaction batching; progress UI.
- S3.3 Ignore/allowlist per session/file; manage and unignore entries.

### E4 — Reporting (MVP)
- S4.1 Export issues to CSV with fields: page, node id, rule id, severity, original, suggestion, status.
- S4.2 Export to JSON with full metadata for automation.
- S4.3 Status lifecycle: Open → Fixed/ Ignored; reflected in report.

### E5 — Performance & Reliability (MVP)
- S5.1 Streamed traversal with yielding; page scan target < 2s on typical files (≤5k TEXT nodes).
- S5.2 Cap issues per node and paginate results list for large datasets.
- S5.3 Robust error handling and non-blocking UI during long scans.

### E6 — Privacy & Security (MVP)
- S6.1 Default privacy: no design text leaves device; document behavior in UI.
- S6.2 Remote fetch opt-in flow with clear notice; handle CORS/failures gracefully.
- S6.3 JSON schema and optional checksum/signature validation for packs.

---

### E7 — Live Linting (Post-MVP)
- S7.1 Debounced linting while typing with minimal overhead.
- S7.2 Inline suggestion affordances within panel and canvas.

### E8 — Advanced Style & ko-KR Heuristics (Post-MVP)
- S8.1 Numbers/units/date normalization rules and brand name casing enforcement.
- S8.2 Korean-specific spacing/particle/punctuation rules; optional WASM tokenizer.

### E9 — Diff Previews (Post-MVP)
- S9.1 Preview before apply-all: side-by-side or inline diff of changes; accept/reject per item.

### E10 — Team Policy Sync (Post-MVP)
- S10.1 Sync rulesets from URL with version pinning; display changelog and effective version.
- S10.2 Local overrides and inheritance (global → product → file).

### E11 — Cross-File Scanning & Analytics (Post-MVP)
- S11.1 Multi-file scan summary (recent files or selected set); aggregated metrics.
- S11.2 Anonymous, opt-in local telemetry dashboard (counts per rule), no text payloads.
