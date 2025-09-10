# 13. Architecture & Technical Choices
- Figma plugin main thread: traversal, edits, selection handling.
- UI (iframe): React/Preact or vanilla; communicate via postMessage.
- Matching Engine: TypeScript; regex-first; token heuristics optional.
- Data: figma.clientStorage cache; optional remote fetch via HTTPS.
- Build: Vite/esbuild; manifest v2; permissions: read/write text nodes; optional network.
