# 9. Functional Requirements (MVP)
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
