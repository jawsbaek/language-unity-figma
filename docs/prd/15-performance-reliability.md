# 15. Performance & Reliability
- Scan current page (<≈5k TEXT nodes) under 2 seconds on typical hardware.
- Streamed traversal with yielding; debounce live checks.
- Cap per-node issues shown; pagination for large result sets.
- All mutations are undoable; apply-all uses batched transactions.
