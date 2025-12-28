# Implementation Plan - Direct Canvas Editing with Auto-Save

## Overview

This implementation plan breaks down the feature into discrete, manageable coding tasks. Each task builds incrementally on previous tasks, starting with core infrastructure and progressing to UI components and integration.

**Note:** This feature reuses existing drag/move functionality from CanvasView. See `CODEBASE_ANALYSIS.md` for details on what already exists.

---

## Tasks

- [ ] 1. Extend DesignFrame component with resize handles



  - Add resize handle elements (corners and edges) to DesignFrame
  - Style resize handles with appropriate cursor feedback
  - Implement onMouseDown handlers for resize handles
  - Pass resize callbacks to parent component
  - _Requirements: 1.1, 1.2_

- [ ]* 1.1 Write property test for cursor feedback on hover
  - **Feature: direct-canvas-editing, Property 15: Cursor Feedback on Hover**
  - **Validates: Requirements 1.1**

- [ ] 2. Implement resize drag handlers in CanvasView
  - Create resize state management (similar to existing dragState)
  - Implement handleResizeStart, handleResizeMove, handleResizeEnd
  - Add real-time dimension updates during resize drag
  - Implement bounds validation (minimum 100x100px, canvas boundaries)
  - _Requirements: 1.2, 1.5_

- [ ]* 2.1 Write property test for real-time dimension update
  - **Feature: direct-canvas-editing, Property 2: Real-Time Dimension Update During Drag**
  - **Validates: Requirements 1.2**

- [ ]* 2.2 Write property test for resize bounds enforcement
  - **Feature: direct-canvas-editing, Property 1: Resize Bounds Enforcement**
  - **Validates: Requirements 1.5**

- [ ] 3. Create SaveManager service with debounce logic
  - Create SaveManager service class with debounce (500ms)
  - Implement save queue for batching operations
  - Create methods to update HTML files with new CSS values
  - Implement metadata file creation/updates
  - Implement retry logic with exponential backoff (max 3 retries)
  - _Requirements: 4.1, 4.2, 4.4, 4.5_

- [ ]* 3.1 Write property test for debounce efficiency
  - **Feature: direct-canvas-editing, Property 8: Auto-Save Debounce Efficiency**
  - **Validates: Requirements 4.2**

- [ ]* 3.2 Write property test for auto-save timing
  - **Feature: direct-canvas-editing, Property 9: Auto-Save Timing**
  - **Validates: Requirements 4.1**

- [ ]* 3.3 Write property test for save failure retry
  - **Feature: direct-canvas-editing, Property 14: Save Failure Retry**
  - **Validates: Requirements 4.4**

- [ ] 4. Create UndoRedoManager service
  - Create UndoRedoManager service class
  - Implement undo/redo stack data structures (max 50 states)
  - Implement pushState, undo, redo methods
  - Add canUndo() and canRedo() helper methods
  - _Requirements: 6.1, 6.2, 6.3_

- [ ]* 4.1 Write property test for undo-redo round trip
  - **Feature: direct-canvas-editing, Property 10: Undo-Redo Round Trip**
  - **Validates: Requirements 6.1, 6.3**

- [ ]* 4.2 Write property test for redo restoration
  - **Feature: direct-canvas-editing, Property 11: Redo Restoration**
  - **Validates: Requirements 6.2, 6.3**

- [ ] 5. Create PropertiesPanel component
  - Create PropertiesPanel component with input fields
  - Add fields for width, height, x, y, opacity
  - Implement property change handlers
  - Add numeric input validation
  - Implement custom attributes editor
  - _Requirements: 3.1, 3.2, 3.5_

- [ ]* 5.1 Write property test for property edit persistence
  - **Feature: direct-canvas-editing, Property 7: Property Edit Persistence Round Trip**
  - **Validates: Requirements 3.3**

- [ ]* 5.2 Write property test for properties panel visibility
  - **Feature: direct-canvas-editing, Property 16: Properties Panel Visibility**
  - **Validates: Requirements 3.1, 3.5**

- [ ] 6. Create SaveIndicator component
  - Create SaveIndicator component for status display
  - Implement "Saving..." state display
  - Implement success indicator (brief display, auto-hide after 2s)
  - Implement error indicator with retry button
  - _Requirements: 7.1, 7.2, 7.3, 7.4_

- [ ] 7. Integrate resize with SaveManager in CanvasView
  - Connect resize handlers to SaveManager
  - Queue resize operations for auto-save
  - Update frame state after successful save
  - Handle save errors and display notifications
  - _Requirements: 1.3, 1.4, 4.1_

- [ ]* 7.1 Write property test for resize persistence
  - **Feature: direct-canvas-editing, Property 3: Resize Persistence Round Trip**
  - **Validates: Requirements 1.3**

- [ ] 8. Integrate move with SaveManager in CanvasView
  - Connect existing drag handlers to SaveManager
  - Queue move operations for auto-save
  - Update frame state after successful save
  - Handle save errors and display notifications
  - _Requirements: 2.2, 4.1_

- [ ]* 8.1 Write property test for move persistence
  - **Feature: direct-canvas-editing, Property 6: Move Persistence Round Trip**
  - **Validates: Requirements 2.2**

- [ ]* 8.2 Write property test for position bounds enforcement
  - **Feature: direct-canvas-editing, Property 4: Position Bounds Enforcement**
  - **Validates: Requirements 2.3**

- [ ]* 8.3 Write property test for real-time position update
  - **Feature: direct-canvas-editing, Property 5: Real-Time Position Update During Drag**
  - **Validates: Requirements 2.1**

- [ ] 9. Integrate PropertiesPanel with CanvasView
  - Display PropertiesPanel when frame is selected
  - Hide PropertiesPanel when frame is deselected
  - Connect property changes to SaveManager
  - Update frame state on property change
  - _Requirements: 3.1, 3.2, 3.3, 3.4, 3.5_

- [ ]* 9.1 Write property test for selection state consistency
  - **Feature: direct-canvas-editing, Property 12: Selection State Consistency**
  - **Validates: Requirements 5.1, 5.2, 5.4**

- [ ]* 9.2 Write property test for deselection clears UI
  - **Feature: direct-canvas-editing, Property 13: Deselection Clears UI**
  - **Validates: Requirements 5.3**

- [ ] 10. Integrate UndoRedoManager with CanvasView
  - Add keyboard event listeners for Ctrl+Z and Ctrl+Y
  - Connect undo/redo buttons to UndoRedoManager
  - Push frame state to undo stack on changes
  - Update frame state on undo/redo
  - Trigger save on undo/redo completion
  - Disable undo/redo buttons when stacks are empty
  - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5_

- [ ] 11. Integrate SaveIndicator with SaveManager
  - Connect SaveManager events to SaveIndicator
  - Display indicator on save start
  - Display success indicator on save complete
  - Display error indicator on save failure
  - Auto-hide success indicator after 2 seconds
  - _Requirements: 7.1, 7.2, 7.3, 7.4_

- [ ] 12. Wire up all components together
  - Test full resize workflow (select → drag resize → save → verify file)
  - Test full move workflow (select → drag move → save → verify file)
  - Test property edit workflow (select → edit property → save → verify file)
  - Test undo/redo with file persistence
  - Test multiple frames interaction
  - Test concurrent edits and saves
  - _Requirements: 1.1, 2.1, 3.1, 4.1, 5.1, 6.1, 7.1_

- [ ] 13. Checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ]* 14. Write integration tests
  - Test full resize workflow (select → drag → save → verify file)
  - Test full move workflow (select → drag → save → verify file)
  - Test property edit workflow (select → edit → save → verify file)
  - Test undo/redo with file persistence
  - Test multiple frames interaction
  - Test concurrent edits and saves
  - _Requirements: 1.1, 2.1, 3.1, 4.1, 5.1, 6.1, 7.1_

- [ ] 15. Final Checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.



- [ ] 14. Implement CodeGenerationService
  - Create service to scan codebase for context
  - Implement HTML/CSS structure extraction
  - Implement component pattern detection
  - Implement design token collection
  - Create AI prompt for code generation
  - Implement design file update logic
  - Add error handling and retry logic
  - _Requirements: 9.1, 9.2, 9.3, 9.4, 9.6_

- [ ]* 14.1 Write property test for codebase context collection
  - **Feature: direct-canvas-editing, Property 17: Codebase Context Collection**
  - **Validates: Requirements 9.1, 9.2**

- [ ]* 14.2 Write property test for code generation round trip
  - **Feature: direct-canvas-editing, Property 18: Code Generation Round Trip**
  - **Validates: Requirements 9.3, 9.4**

- [ ]* 14.3 Write property test for metadata cleanup after accept
  - **Feature: direct-canvas-editing, Property 19: Metadata Cleanup After Accept**
  - **Validates: Requirements 9.7**

- [ ]* 14.4 Write property test for code generation error handling
  - **Feature: direct-canvas-editing, Property 20: Code Generation Error Handling**
  - **Validates: Requirements 9.6**

- [ ] 15. Add "Accept Changes" button to CanvasView
  - Create button UI in CanvasView toolbar
  - Connect button to CodeGenerationService
  - Display loading state during generation
  - Show success/error notifications
  - Refresh canvas preview after successful generation
  - _Requirements: 9.1, 9.5_

- [ ] 16. Integrate CodeGenerationService with CanvasView
  - Wire up "Accept Changes" button to code generation
  - Handle generation start/complete/error events
  - Update UI state during generation
  - Refresh design frames after code update
  - Clear metadata after successful generation
  - _Requirements: 9.1, 9.4, 9.5, 9.7_

- [ ] 17. Final Integration Checkpoint
  - Ensure all tests pass, ask the user if questions arise.

- [ ]* 18. Write end-to-end integration tests
  - Test full workflow: resize → accept → code generation → verify file
  - Test codebase scanning and context collection
  - Test AI code generation with various changes
  - Test error handling and retry logic
  - Test metadata cleanup after accept
  - _Requirements: 9.1, 9.2, 9.3, 9.4, 9.5, 9.6, 9.7_

- [ ] 19. Final Checkpoint - All tests pass
  - Ensure all tests pass, ask the user if questions arise.

