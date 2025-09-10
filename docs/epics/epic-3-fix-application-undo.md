# Epic 3: Fix Application & Undo (MVP)

## Epic Goal
Provide users with reliable and efficient fix application capabilities, including single fixes, bulk operations, and comprehensive undo support, while enabling flexible ignore/allowlist management for false positives.

## Epic Description
This epic implements the core remediation functionality that allows users to act on identified language issues. The system provides both granular control through single fix applications and efficiency through bulk operations, all backed by robust undo capabilities. Additionally, it handles the reality of false positives through sophisticated ignore and allowlist management.

## Business Value
- Enables rapid resolution of language consistency issues
- Provides safety through comprehensive undo capabilities
- Reduces friction through bulk fix operations
- Handles edge cases and false positives gracefully

## User Stories

### S3.1: Apply Single Fix with Undo Support
**As a** Designer or UX Writer  
**I want to** apply individual fixes to flagged text with full undo support  
**So that** I can carefully review and implement changes with confidence

**Acceptance Criteria:**
- User can apply a single fix to a specific flagged text range
- Fix updates the actual node.characters in Figma
- All fixes are fully compatible with Figma's native undo system
- Visual feedback confirms successful fix application
- Fixed issues are marked as resolved in the issues panel

**Definition of Done:**
- Single fix application implemented
- Figma undo integration working correctly
- UI feedback for successful operations
- Issue status updates functional
- Comprehensive testing of edge cases

---

### S3.2: Apply-All with Atomic Transaction Batching
**As a** UX Writer  
**I want to** apply multiple fixes in bulk with progress indication  
**So that** I can efficiently resolve many issues while maintaining system stability

**Acceptance Criteria:**
- Apply-all functionality works by rule type or issue group
- Operations use atomic transaction batching for reliability
- Progress UI shows completion status during bulk operations
- Entire batch can be undone as a single operation
- System remains responsive during large bulk operations

**Definition of Done:**
- Bulk fix application implemented
- Atomic transaction system working
- Progress indication UI complete
- Batch undo functionality verified
- Performance testing for large operations

---

### S3.3: Ignore/Allowlist Management
**As a** User  
**I want to** ignore false positives and manage allowlist entries  
**So that** I can customize the plugin to match my specific content needs

**Acceptance Criteria:**
- User can ignore individual issues per session or file
- Ignored issues are hidden from the issues panel
- Allowlist entries can be managed and removed (unignore)
- Ignore settings persist appropriately (session vs file scope)
- Clear indication of ignored vs active issues

**Definition of Done:**
- Ignore functionality implemented for both scopes
- Allowlist management UI complete
- Persistence mechanism working correctly
- Clear visual distinction for ignored issues
- Unignore functionality verified

## Technical Requirements

### Fix Application System
- Direct integration with Figma's node.characters API
- Precise text range replacement capabilities
- Atomic operation handling for reliability
- Integration with Figma's native undo system

### Batch Processing
- Transaction batching for bulk operations
- Progress tracking and user feedback
- Cancellation support for long-running operations
- Memory-efficient processing of large fix sets

### State Management
- Issue status tracking (open/fixed/ignored)
- Allowlist persistence (session/file scoped)
- Undo/redo state coordination
- Real-time UI updates reflecting state changes

### Performance Optimization
- Efficient text replacement algorithms
- Batched DOM updates for UI responsiveness
- Background processing for large operations
- Memory management for undo history

## Dependencies
- Epic 2: Scan & Issue Surfacing (for issue identification)
- Figma Plugin API for text manipulation and undo integration
- Established UI framework from previous epics

## Risks and Mitigations
- **Risk**: Text replacement breaking formatting or styles
  - **Mitigation**: Careful handling of text ranges and style preservation
- **Risk**: Undo system conflicts with Figma native undo
  - **Mitigation**: Proper integration testing with Figma's undo API
- **Risk**: Performance issues with large bulk operations
  - **Mitigation**: Implement progressive processing with user feedback

## Success Metrics
- Fix application success rate > 99%
- User satisfaction with undo reliability
- Bulk operation completion times
- False positive management effectiveness

## Definition of Epic Done
- All 3 user stories completed with acceptance criteria met
- Undo system integration verified and reliable
- Bulk operations perform within acceptable time limits
- Ignore/allowlist system handles edge cases correctly
- Integration testing with various text formatting scenarios complete
