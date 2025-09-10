# 19. Dependencies & Risks
Dependencies:
- Figma plugin API (stable text node APIs, overlays).
- Pack authoring/maintenance (brand team/UX writing).

Top Risks & Mitigations:
- False positives → Provide severity levels, ignore/allowlist, clear examples, preview diff.
- Performance on large files → Yielding traversal, scope limits, pagination, throttling.
- Korean NLP complexity → Start with regex heuristics; optional WASM tokenizer later.
- Policy drift → Versioned pack with checksum; update notifications.
