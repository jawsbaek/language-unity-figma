# Figma Language Consistency Plugin – Specification and Ideas

## Goal
Build a Figma plugin that scans text layers, flags non-standard or disallowed terms, and recommends or auto-applies approved alternatives from a company language resource pack to ensure consistent tone, terminology, and style (e.g., ko-KR and en-US).

## Primary Users and Use Cases
- Designers: Validate copy in designs before handoff.
- UX Writers: Batch review consistency across pages/files.
- Localization/Brand team: Enforce terminology policy and produce compliance reports.

## MVP Feature Set
- Text scan: Traverse current page or selection for TEXT nodes.
- Language pack import: Load company glossary/rules (JSON/YAML) from local file or URL.
- Suggestions panel: List issues with location, severity, rule, and recommended replacement.
- Quick fixes: Apply single fix or “apply all”; support undo/redo.
- Inline highlights: Overlays to show flagged spans in canvas.
- Ignore list: Per-file/session ignores; add custom allowlist entries.
- Batch report: Export CSV/JSON of issues (term, rule, node id, page, status).
- Settings: Scope (selection/page/file), severity filters, locale, live-on-typing toggle.
- Offline-first: Matching runs locally; remote sync optional and disabled by default.

## Post-MVP (Nice-to-have)
- Tone-of-voice linting (formal/casual) with rule weights.
- Unit/number/date style normalization (e.g., MB vs MiB, punctuation rules).
- Brand/feature name casing enforcement (e.g., iPhone, macOS).
- Korean-specific checks: spacing norms, particles, punctuation normalization.
- Auto-fix preview diffs with accept/reject per change.
- Team policy sync via URL with version pinning and signature.

## Language Pack Structure (Proposed)
- Terms: preferred, aliases, forbidden, examples/notes.
- Rules: regex/style rules with metadata (id, description, severity, fix suggestion, examples).
- Scopes: global vs domain-specific; locales (ko-KR, en-US, etc.).
- Versioning and checksum for integrity.

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

## Core Flows
1. Scan: Open plugin → load/sync language pack → choose scope → run scan → results in panel with counts, filters, and canvas highlights.
2. Fix: Select an issue → view context + recommended fix → apply single fix or apply-all by rule/group → plugin updates the specific text range in node.characters.
3. Report: Export issues to CSV/JSON; status can be Open/Fixed/Ignored.

## Architecture (Figma Plugin)
- UI: iframe UI (React/Preact or vanilla) for panel; communicates via postMessage with main thread.
- Controller: Main thread handles node traversal, selections, and text updates.
- Matching Engine: Pure TypeScript; rules compiled to efficient regex; token-based matching where needed.
- Data: Load pack via FilePicker or HTTPS fetch; cache in figma.clientStorage with version key.
- Performance: Streaming traversal (yield to main loop), debounce live checks, cap issues per node with “load more”.

## Technical Choices
- Language: TypeScript.
- Build: Vite or esbuild for main + UI bundles.
- Packaging: manifest.json v2; permissions: read/write text nodes; optional network for pack fetch.
- Text processing: regex-first with optional lightweight morphological heuristics for Korean; no external AI in MVP.

## Non-Functional Requirements
- Privacy: Rules and results stored locally; remote fetch is opt-in; no design text leaves machine unless enabled.
- Reliability: All changes undoable; batch transactions for apply-all.
- Performance: Scan current page < 2s for typical files (≤5k text nodes); memory-safe.
- Accessibility: Keyboard navigation; readable contrasts; focus states.

## Minimal Data Model
- Rule: id, severity, matcher (regex/string/alias), replacement, note, examples.
- Issue: id, ruleId, pageId, nodeId, range (start,end), original, suggested, status.
- Settings: packSource, scope, filters, locale, liveCheck.

## UI Outline
- Sidebar tabs: Issues | Rules | Settings | Report.
- Issues list: Group by page/rule; filter by severity; search.
- Inspector: Context snippet with highlighted span; actions: Replace, Ignore, Copy fix.
- Settings: Load/sync pack, severity filters, locale selector, live checks.

## Rule DSL (Light)
- String rules (exact/alias), Regex rules (flags), Transform rules (normalize numbers/units/case), Composite rules (AND/OR chains).
- Evaluation order: forbidden → preferred → style rules; stop at first auto-fix unless multi-pass is enabled.

## Security
- Validate pack checksum/signature (optional); CORS-safe fetch; strict JSON schema validation; sandboxed UI.

## Telemetry (Optional, Off by Default)
- Local counters only; if enabled, anonymous counts per rule for tuning; no text payloads.

## Testing Strategy
- Unit tests: matcher engine, transforms, rule parsing.
- Integration tests: mocked Figma nodes for traversal and fix application.
- Golden tests: sample ko-KR sentences (spacing, particles, brand names).

## Rollout Plan
- Phase 1 (MVP): Local pack, page scan, suggestions, single/bulk fix, report export.
- Phase 2: Live-on-typing, advanced style rules, Korean-specific packs.
- Phase 3: Team sync, policy versions, analytics.
- Phase 4: Optional cloud AI rewrite suggestions (privacy-preserving).

## Risks and Mitigations
- False positives: Severity, ignore/allowlist, examples, preview diffs.
- Performance on large files: Paginated results, scan scopes, throttled background chunking.
- Korean NLP complexity: Start with regex heuristics; consider opt-in WASM tokenizer later.

## Ideas Backlog
- Term hover tooltip in canvas with approved term.
- “Suggest alternatives” palette action on selected text.
- Batch punctuation/quotes normalization across nodes.
- Unit/number style normalization (MB vs MiB, thousands separators).
- Automatic case/style for product names and UI labels.
- Tone score per frame with presets.
- Policy gates: block export/handoff if high-severity issues remain (team option).
- Rule playground: test sentence vs rule with live preview.
- Per-team rule overrides and inheritance (global → product → file).
- Figma comments: add high-severity issue comments linking to rule.
- Variable/Token integration for text tokens.
- Localization hooks: export flagged strings to glossary/TMS.
- Smart diff: inline delta preview before apply-all.
- Cross-file scanning and summaries (recent files).
- “Learn exceptions”: add false-positive patterns to ignore regex quickly.
- Rule packs by locale (ko-KR/en-US) and domain (B2B/consumer).
- Keyboard triage: J/K navigate, A apply, I ignore.
- CLI companion (future): lint .fig via API.
- Cloud pack registry with versioning and changelogs.
- Link to brand writing guidelines from plugin.

## Next Steps
1. Confirm language pack format (share a sample or adopt the schema above).
2. Approve MVP scope and priorities.
3. Create a technical task breakdown (matching engine, traversal, UI, storage) and timeline.
