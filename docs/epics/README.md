# Figma Language Plugin - Epics and Stories Overview

## MVP Epics (Ready for Development)

### Epic 1: Language Pack & Rule Engine
**Goal**: Foundation for loading and processing language packs with rule evaluation
- **S1.1**: Load language pack from local file ✅ (Story created)
- **S1.2**: Load language pack from URL ✅ (Story created)  
- **S1.3**: Rule evaluation engine supporting string/alias/forbidden and regex rules
- **S1.4**: Settings UI for scope, locale, severity filters

### Epic 2: Scan & Issue Surfacing  
**Goal**: Core scanning functionality with issues panel and visual highlights
- **S2.1**: Traverse TEXT nodes by scope and extract content with references
- **S2.2**: Issues panel with grouping, search, and filters
- **S2.3**: Inline highlights for flagged text spans in canvas

### Epic 3: Fix Application & Undo
**Goal**: Reliable fix application with comprehensive undo support
- **S3.1**: Apply single fix with full undo compatibility
- **S3.2**: Apply-all by rule/group with atomic transaction batching
- **S3.3**: Ignore/allowlist management per session/file

### Epic 4: Reporting
**Goal**: Comprehensive reporting and export capabilities
- **S4.1**: Export issues to CSV with configurable fields
- **S4.2**: Export to JSON with full metadata for automation
- **S4.3**: Issue status lifecycle management (Open → Fixed/Ignored)

### Epic 5: Performance & Reliability
**Goal**: Production-ready performance and error handling
- **S5.1**: Streamed traversal with yielding; <2s scan target for ≤5k nodes
- **S5.2**: Issue display optimization with pagination for large datasets
- **S5.3**: Robust error handling and non-blocking UI during operations

### Epic 6: Privacy & Security
**Goal**: Privacy-first operation with secure remote pack fetching
- **S6.1**: Default privacy - no design text leaves device
- **S6.2**: Secure remote fetch opt-in with CORS/failure handling
- **S6.3**: JSON schema and checksum/signature validation for packs

## Post-MVP Epics (Future Phases)

### Epic 7: Live Linting
- **S7.1**: Debounced linting while typing
- **S7.2**: Inline suggestion affordances

### Epic 8: Advanced Style & ko-KR Heuristics  
- **S8.1**: Numbers/units/date normalization and brand name casing
- **S8.2**: Korean-specific spacing/particle/punctuation rules

### Epic 9: Diff Previews
- **S9.1**: Preview before apply-all with accept/reject per item

### Epic 10: Team Policy Sync
- **S10.1**: Sync rulesets from URL with version pinning
- **S10.2**: Local overrides and inheritance (global → product → file)

### Epic 11: Cross-File Scanning & Analytics
- **S11.1**: Multi-file scan summary and aggregated metrics
- **S11.2**: Anonymous opt-in telemetry dashboard

## Development Status

### Completed
- ✅ PRD sharded into individual sections
- ✅ All 6 MVP epics created with detailed descriptions
- ✅ Stories 1.1 and 1.2 created with full acceptance criteria

### Next Steps
1. Complete remaining user stories for all MVP epics (S1.3 through S6.3)
2. Create detailed story files with tasks and acceptance criteria
3. Prioritize epic development order based on dependencies
4. Begin development with Epic 1 (Language Pack & Rule Engine)

## Epic Dependencies
```
Epic 1 (Foundation) 
    ↓
Epic 2 (Scanning) → Epic 5 (Performance)
    ↓                    ↓
Epic 3 (Fixes) → Epic 4 (Reporting) → Epic 6 (Security)
```

## File Structure
```
docs/
├── epics/
│   ├── epic-1-language-pack-rule-engine.md
│   ├── epic-2-scan-issue-surfacing.md
│   ├── epic-3-fix-application-undo.md
│   ├── epic-4-reporting.md
│   ├── epic-5-performance-reliability.md
│   ├── epic-6-privacy-security.md
│   └── README.md (this file)
├── stories/
│   ├── 1.1-load-language-pack-local-file.md
│   ├── 1.2-load-language-pack-url.md
│   └── [remaining stories to be created]
└── prd/ (sharded PRD sections)
```
