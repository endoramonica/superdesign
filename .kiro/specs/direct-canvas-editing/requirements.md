# Requirements Document - Direct Canvas Editing with Auto-Save

## Introduction

This feature enables users to directly edit and resize design components on the canvas, similar to Figma's interface. Users can drag to resize components, modify properties in real-time, and all changes are automatically saved to the design files. This eliminates the need to use AI chat for simple design adjustments and provides a more intuitive, direct editing experience.

## Glossary

- **Canvas**: The grid-based design workspace where design frames are displayed
- **Design Frame**: A single HTML/SVG design component rendered in an iframe
- **Resize Handle**: Visual indicator (corner/edge) that allows dragging to resize a frame
- **Direct Editing**: Modifying component properties without using AI chat
- **Auto-Save**: Automatically persisting changes to the design file without user action
- **Design File**: HTML/SVG file stored in `.superdesign/design_iterations/` (original code remains unchanged)
- **Frame Metadata**: JSON data containing position, size, and other frame properties (stored separately in `.meta.json`)
- **Property Panel**: UI panel showing editable properties of selected frame
- **Metadata File**: `.meta.json` file that stores frame dimensions, position, and properties (NOT the HTML/CSS code itself)

## Requirements

### Requirement 1

**User Story:** As a designer, I want to resize design components by dragging their edges/corners, so that I can quickly adjust component dimensions without using AI chat.

#### Acceptance Criteria

1. WHEN a user hovers over a frame edge or corner THEN the system SHALL display a resize cursor (↔ or ↗)
2. WHEN a user drags a frame edge or corner THEN the system SHALL update the frame dimensions in real-time
3. WHEN a user releases the mouse after resizing THEN the system SHALL update the width and height CSS properties in the HTML file
4. WHEN a user releases the mouse after resizing THEN the system SHALL persist the updated HTML file with new dimensions
5. WHEN resizing a frame THEN the system SHALL maintain the iframe content aspect ratio or allow free resizing based on user preference
6. IF a user attempts to resize below minimum dimensions (100x100px) THEN the system SHALL prevent the resize and maintain current size

### Requirement 2

**User Story:** As a designer, I want to move design components by dragging them on the canvas, so that I can reposition components without using AI chat.

#### Acceptance Criteria

1. WHEN a user clicks and drags a frame THEN the system SHALL move the frame to the new position in real-time
2. WHEN a user releases the mouse after moving THEN the system SHALL update the position CSS properties (transform or top/left) in the HTML file
3. WHEN a user releases the mouse after moving THEN the system SHALL persist the updated HTML file with new position
4. WHEN moving a frame THEN the system SHALL prevent frames from moving outside the canvas boundaries
5. WHEN a frame is selected THEN the system SHALL display selection handles (corners and edges) for resizing

### Requirement 3

**User Story:** As a designer, I want to edit component properties directly in a properties panel, so that I can modify component attributes without using AI chat.

#### Acceptance Criteria

1. WHEN a user selects a frame THEN the system SHALL display a properties panel showing editable fields
2. WHEN a user modifies a property value in the panel THEN the system SHALL update the frame in real-time
3. WHEN a user finishes editing a property THEN the system SHALL persist the change to the design file
4. WHEN a frame is deselected THEN the system SHALL close or hide the properties panel
5. WHERE properties include width, height, position (x, y), opacity, and custom data attributes THEN the system SHALL allow editing all of these

### Requirement 4

**User Story:** As a designer, I want changes to be automatically saved, so that I don't lose my work and can focus on designing without manual save actions.

#### Acceptance Criteria

1. WHEN a user modifies a frame property (size, position, or attribute) THEN the system SHALL automatically save the change within 500ms
2. WHEN multiple rapid changes occur THEN the system SHALL debounce saves to prevent excessive file writes
3. WHEN a save operation completes THEN the system SHALL provide visual feedback (e.g., brief save indicator)
4. IF a save operation fails THEN the system SHALL display an error notification and retry automatically
5. WHEN the design file is saved THEN the system SHALL update the file metadata with timestamp and change information

### Requirement 5

**User Story:** As a designer, I want to see which frame is currently selected, so that I know which component I'm editing.

#### Acceptance Criteria

1. WHEN a user clicks on a frame THEN the system SHALL highlight the frame with a selection border
2. WHEN a frame is selected THEN the system SHALL display resize handles at corners and edges
3. WHEN a user clicks outside all frames THEN the system SHALL deselect the current frame and hide selection handles
4. WHEN a frame is selected THEN the system SHALL display its properties in the properties panel

### Requirement 6

**User Story:** As a designer, I want to undo and redo my changes, so that I can correct mistakes without losing work.

#### Acceptance Criteria

1. WHEN a user presses Ctrl+Z (or Cmd+Z) THEN the system SHALL undo the last change
2. WHEN a user presses Ctrl+Y (or Cmd+Y) THEN the system SHALL redo the last undone change
3. WHEN undo/redo is performed THEN the system SHALL update the frame and persist to file
4. WHEN the undo stack is empty THEN the system SHALL disable the undo button/action
5. WHEN the redo stack is empty THEN the system SHALL disable the redo button/action

### Requirement 7

**User Story:** As a designer, I want to see a visual indicator when changes are being saved, so that I know the system is persisting my work.

#### Acceptance Criteria

1. WHEN a save operation begins THEN the system SHALL display a save indicator (e.g., "Saving..." text or icon)
2. WHEN a save operation completes successfully THEN the system SHALL display a brief success indicator
3. WHEN a save operation fails THEN the system SHALL display an error indicator with retry option
4. WHEN multiple saves are queued THEN the system SHALL show a single indicator for the batch operation

### Requirement 8

**User Story:** As a designer, I want to understand that resizing updates the HTML code directly, so that I know the changes are persisted in the design file.

#### Acceptance Criteria

1. WHEN a user resizes a frame THEN the system SHALL update the width and height CSS properties in the HTML file (e.g., style="width: 1000px; height: 700px;")
2. WHEN a user moves a frame THEN the system SHALL update the position CSS properties in the HTML file (e.g., style="transform: translate(100px, 100px);" or top/left)
3. WHEN a user modifies a property THEN the system SHALL update the corresponding CSS property in the HTML file
4. WHEN changes are saved THEN the system SHALL write the updated HTML file back to disk
5. WHERE the HTML file structure remains the same THEN the system SHALL only modify CSS property values, not the HTML structure

### Requirement 9

**User Story:** As a designer, I want to accept my canvas changes and have the system automatically generate updated code, so that my visual changes are reflected in the actual HTML/CSS.

#### Acceptance Criteria

1. WHEN a user clicks "Accept Changes" button THEN the system SHALL scan the codebase to gather context about the design
2. WHEN the system scans the codebase THEN it SHALL collect information about current HTML/CSS structure, styling patterns, and component architecture
3. WHEN the system has gathered context THEN it SHALL send the canvas changes and codebase context to AI for code generation
4. WHEN AI generates updated code THEN the system SHALL update the original design file with the new HTML/CSS
5. WHEN code generation completes THEN the system SHALL display a success notification and refresh the canvas preview
6. IF code generation fails THEN the system SHALL display an error notification with retry option
7. WHEN changes are accepted THEN the system SHALL clear the metadata file and reset to default frame properties

