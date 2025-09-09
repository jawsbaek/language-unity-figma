# PRD: Figma Language Consistency Plugin

Version: 1.0  
Owner: Product Manager  
Date: {YYYY-MM-DD}

## 1. Summary
A Figma plugin that scans text layers to enforce consistent company terminology, tone, and style using a centrally managed language resource pack. It flags non-standard or disallowed terms, recommends approved alternatives, and can auto-fix issues, enabling design and content consistency prior to handoff.

## 2. Goals
- Ensure copy in Figma adheres to company language standards (terminology, tone, style).
- Reduce manual review time for UX writers and brand teams.
- Provide actionable, fast suggestions and safe auto-fixes with full undo support.

## 3. Non-Goals
- Full grammar correctness or AI rewriting beyond defined rules (MVP).
- Replacing localization workflows or providing translation memory (initially).
- Persisting or processing design text off-device by default.

## 4. Target Users and Personas
- Designers: Validate copy quickly during design iterations.
- UX Writers: Batch-review and resolve consistency issues across files/pages.
- Localization/Brand Team: Enforce policies and audit compliance.

## 5. Problem Statement
Inconsistent terminology and style in design mockups cause rework during handoff, slow localization, and brand inconsistencies. Current reviews are manual and error-prone. A plugin that applies policy-backed rules in Figma reduces friction and improves quality.

## 6. Scope
- Figma plugin (desktop/web) supporting scanning, suggestions, and auto-fixes on TEXT nodes.
- Language pack input via JSON/YAML from local file or URL, cached locally.
- Reporting (CSV/JSON) of issues and statuses.

Out of scope (MVP): cross-file scanning beyond the open file, AI-based rewrites, cloud analytics.

## 7. User Stories (Representative)
1. As a Designer, I can scan the current page for disallowed terms and see approved replacements.
2. As a UX Writer, I can apply auto-fixes in bulk and undo changes if needed.
3. As a Brand Manager, I can export a report of issues by rule and page for audits.
4. As a Designer, I can ignore false positives and add temporary allowlist entries.
5. As a UX Writer, I can filter issues by severity and locale.

## 8. Key Flows
- Scan: Open plugin → load/sync language pack → choose scope (selection/page/file) → run scan → view grouped results and canvas highlights.
- Fix: Select issue → view context + recommendation → apply single fix or apply-all per rule/group → undo supported.
- Report: Export results (CSV/JSON) including rule id, page, node id, original, suggestion, status.

## 9. Functional Requirements (MVP)
- FR1: Traverse TEXT nodes in selection/page/file.
- FR2: Load language pack from file picker or URL; cache via figma.clientStorage.
- FR3: Rules evaluation: preferred/aliases/forbidden + regex style rules, with severity levels.
- FR4: Display issues list with filters (severity, page, rule), search, and counts.
- FR5: Inline highlights (lightweight overlays) for flagged spans.
- FR6: Apply Fix (single) and Apply All (by rule/group) with atomic undo batches.
- FR7: Ignore/allowlist per session/file; unignore management.
- FR8: Export report (CSV/JSON) with selected fields.
- FR9: Settings: scope, locale, severity filters, live-on-typing toggle (default off).
- FR10: Offline-first; remote fetch is opt-in.

## 10. Post-MVP / Future
- Tone-of-voice linting with weighted rules.
- Numbers/units/date normalization; brand name casing enforcement.
- Korean-specific heuristics (spacing, particles, punctuation) and optional tokenizer (WASM).
- Live linting while typing; auto-fix diff previews.
- Team policy sync with version pinning and signature.
- Cross-file scanning and analytics.

## 11. Language Pack Structure (Baseline)
See `docs/figma-language-plugin-spec.md` for full schema. Summary:
- terms[]: { preferred, aliases[], forbidden[], note }
- rules[]: { id, desc, severity, pattern: {type, source}, fix, examples }
- locale, version, checksum/signature (optional), scopes (global/domain-specific)

Example (ko-KR):
```json
{
  "version": "1.0.0",
  "locale": "ko-KR",
  "terms": [
    { "preferred": "로그인", "aliases": ["로긴", "Login"], "forbidden": ["Sign in*"], "note": "UI copy uses 로그인" }
  ],
  "rules": [
    {
      "id": "ko-spacing-001",
      "desc": "띄어쓰기: ‘되다/돼’ 구분",
      "severity": "warning",
      "pattern": { "type": "regex", "source": "(?<=)됬|됏" },
      "fix": "됐",
      "examples": { "bad": "됬다", "good": "됐다" }
    }
  ]
}
```

## 12. UX/UI Requirements
- Sidebar UI with tabs: Issues | Rules | Settings | Report.
- Issues: Group by page/rule; list with status (Open/Fixed/Ignored), context preview.
- Inspector: Selected issue shows snippet, rule details, actions (Replace, Ignore, Copy fix).
- Keyboard navigation (basic): Up/Down select, Enter to open, shortcuts for apply/ignore.
- Accessibility: Color contrast, focus states, screen-reader labels.

## 13. Architecture & Technical Choices
- Figma plugin main thread: traversal, edits, selection handling.
- UI (iframe): React/Preact or vanilla; communicate via postMessage.
- Matching Engine: TypeScript; regex-first; token heuristics optional.
- Data: figma.clientStorage cache; optional remote fetch via HTTPS.
- Build: Vite/esbuild; manifest v2; permissions: read/write text nodes; optional network.

## 14. Privacy, Security, Compliance
- Default: No design text leaves device.
- Remote pack fetch is opt-in; validate checksum/signature when provided.
- Strict JSON schema validation for packs; handle CORS/network failures gracefully.

## 15. Performance & Reliability
- Scan current page (<≈5k TEXT nodes) under 2 seconds on typical hardware.
- Streamed traversal with yielding; debounce live checks.
- Cap per-node issues shown; pagination for large result sets.
- All mutations are undoable; apply-all uses batched transactions.

## 16. Telemetry (Optional, Off by Default)
- Local counters only; if enabled, anonymous counts by rule (no text payloads).
- Future: Team-controlled opt-in telemetry with clear privacy notice.

## 17. Metrics & KPIs
- Adoption: Weekly active users of plugin.
- Efficiency: Avg time to scan and resolve issues per page.
- Quality: % reduction in post-handoff copy revisions.
- Coverage: # of issues found/fixed per file; false-positive rate (via ignores).

## 18. Release Plan
- Phase 1 (MVP): Local pack, page scan, suggestions, single/bulk fix, report export.
- Phase 2: Live-on-typing, advanced style rules, ko-KR specific packs.
- Phase 3: Team sync, policy versioning, analytics.
- Phase 4: Optional AI rewrite suggestions (privacy-preserving) and cross-file scans.

## 19. Dependencies & Risks
Dependencies:
- Figma plugin API (stable text node APIs, overlays).
- Pack authoring/maintenance (brand team/UX writing).

Top Risks & Mitigations:
- False positives → Provide severity levels, ignore/allowlist, clear examples, preview diff.
- Performance on large files → Yielding traversal, scope limits, pagination, throttling.
- Korean NLP complexity → Start with regex heuristics; optional WASM tokenizer later.
- Policy drift → Versioned pack with checksum; update notifications.

## 20. Acceptance Criteria (MVP)
- AC1: Users can load a language pack (file/URL) and cache it locally.
- AC2: Scanning the current page lists issues with rule, location, and suggestion.
- AC3: Users can apply single fix and apply-all by rule with undo support.
- AC4: Inline highlights appear for flagged text spans.
- AC5: Users can ignore issues and export a CSV/JSON report.
- AC6: No design text leaves the device by default; remote fetch is opt-in.
- AC7: Performance target: page scan < 2s (typical file size).

## 21. Open Questions
- Do we need pack signing/verification in MVP or post-MVP?
- Which locales are prioritized (ko-KR first; en-US next)?
- Should we include live-on-typing in MVP or Phase 2?
- Do we need team-level policy inheritance in MVP?

## 22. Appendix
- Detailed Specification: `docs/figma-language-plugin-spec.md`

## 23. Epics and Story List

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
