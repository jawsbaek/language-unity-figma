# Epic 2: Scan & Issue Surfacing (MVP)

## Epic Goal
Implement comprehensive text scanning capabilities that traverse Figma TEXT nodes, identify language and style issues, and present them through an intuitive issues panel with visual highlights on the canvas.

## Epic Description
This epic focuses on the core scanning functionality that makes language issues visible to users. The system will traverse TEXT nodes based on user-selected scope, extract and analyze text content, and present findings through both a detailed issues panel and visual canvas highlights. The implementation emphasizes user experience through effective grouping, filtering, and search capabilities.

## Business Value
- Enables rapid identification of language consistency issues
- Provides clear visibility into rule violations across designs
- Reduces manual review time through automated scanning
- Offers flexible scoping to match different workflow needs

## User Stories

### S2.1: TEXT Node Traversal and Content Extraction
**As a** Plugin System  
**I want to** traverse TEXT nodes by scope and extract content with proper references  
**So that** I can analyze text content while maintaining traceability to source nodes

**Acceptance Criteria:**
- System can traverse TEXT nodes in selection, current page, or entire file
- Text content is extracted with node ID and page references preserved
- Nested text formatting and styles are handled correctly
- Empty or whitespace-only nodes are appropriately filtered
- Performance remains acceptable for files with many TEXT nodes (≤5k nodes < 2s)

**Definition of Done:**
- Node traversal algorithm implemented for all scopes
- Text extraction preserves node/page references
- Handling of complex text formatting complete
- Performance benchmarks met
- Unit tests for traversal logic

---

### S2.2: Issues Panel with Grouping and Filtering
**As a** Designer or UX Writer  
**I want to** view identified issues in an organized panel with grouping and filtering  
**So that** I can efficiently review and prioritize language consistency problems

**Acceptance Criteria:**
- Issues are grouped by page and rule type with clear counts
- Search functionality works across issue descriptions and text content
- Filters available for severity level, page, and rule type
- Issue list shows context preview and rule information
- Pagination or virtual scrolling for large result sets

**Definition of Done:**
- Issues panel UI implemented with all grouping options
- Search and filter functionality working
- Performance optimized for large issue sets
- Clear visual hierarchy and information architecture
- Accessibility features implemented

---

### S2.3: Inline Canvas Highlights
**As a** User  
**I want to** see visual highlights on flagged text spans in the canvas  
**So that** I can quickly locate and understand issues in context

**Acceptance Criteria:**
- Lightweight overlays appear on flagged text spans
- Highlights are visually distinct but not obtrusive
- Toggle functionality to show/hide highlights
- Highlights update when issues are resolved or ignored
- Performance impact is minimal on canvas rendering

**Definition of Done:**
- Canvas overlay system implemented
- Visual design follows Figma plugin guidelines
- Toggle controls functional
- Highlight updates work correctly
- Performance impact tested and optimized

## Technical Requirements

### Scanning Architecture
- Efficient node traversal algorithms
- Scope-based filtering (selection/page/file)
- Text extraction with metadata preservation
- Incremental scanning for large files

### Issues Management
- Issue data structure with full traceability
- Grouping and sorting algorithms
- Search indexing for fast filtering
- Status tracking (open/fixed/ignored)

### UI Components
- Responsive issues panel layout
- Virtualized lists for performance
- Filter and search controls
- Canvas overlay rendering system

### Performance Optimization
- Streaming traversal with yielding to main thread
- Debounced search and filter operations
- Efficient canvas highlight rendering
- Memory management for large issue sets

## Dependencies
- Epic 1: Language Pack & Rule Engine (for rule evaluation)
- Figma Plugin API for node access and canvas overlays
- UI framework for issues panel implementation

## Risks and Mitigations
- **Risk**: Performance degradation with large files
  - **Mitigation**: Implement streaming traversal and result pagination
- **Risk**: Canvas highlight visual conflicts
  - **Mitigation**: Design subtle, toggleable highlight system
- **Risk**: Complex text formatting edge cases
  - **Mitigation**: Comprehensive testing with varied text content

## Success Metrics
- Scan completion time meets performance targets
- User engagement with issues panel features
- Accuracy of text extraction and issue identification
- Visual highlight effectiveness (user feedback)

## Definition of Epic Done
- All 3 user stories completed with acceptance criteria met
- Performance targets achieved for typical file sizes
- UI/UX meets Figma plugin standards
- Canvas highlighting system working reliably
- Comprehensive testing across various content types
