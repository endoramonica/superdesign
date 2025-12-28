# Design Document - Direct Canvas Editing with Auto-Save

## Overview

This design document outlines the architecture and implementation strategy for adding direct canvas editing capabilities to Superdesign. The feature allows users to resize, move, and edit component properties directly on the canvas with automatic persistence, similar to Figma's interface.

## Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Canvas View (React)                      │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ CanvasView Component                                 │  │
│  │ - Manages frame selection state                      │  │
│  │ - Handles mouse events (drag, resize)                │  │
│  │ - Coordinates with DesignFrame components            │  │
│  └──────────────────────────────────────────────────────┘  │
│                              │                               │
│         ┌────────────────────┼────────────────────┐         │
│         │                    │                    │         │
│         ▼                    ▼                    ▼         │
│  ┌─────────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ DesignFrame     │  │ Properties   │  │ Save Manager │  │
│  │ Component       │  │ Panel        │  │              │  │
│  │                 │  │              │  │ - Debounce   │  │
│  │ - Render iframe │  │ - Edit width │  │ - Persist    │  │
│  │ - Handle resize │  │ - Edit height│  │ - Retry      │  │
│  │ - Handle move   │  │ - Edit x, y  │  │ - Feedback   │  │
│  │ - Show handles  │  │ - Edit attrs │  │              │  │
│  └─────────────────┘  └──────────────┘  └──────────────┘  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
                    ┌──────────────────────┐
                    │ File System (VS Code)│
                    │ - Read design files  │
                    │ - Write changes      │
                    │ - Update metadata    │
                    └──────────────────────┘
```

## Components and Interfaces

### 1. CanvasView Component (Enhanced)

**Responsibilities:**
- Manage canvas-level state (selected frame, zoom, pan)
- Handle mouse events for frame selection
- Coordinate between DesignFrame and PropertiesPanel
- Manage undo/redo stack
- Provide "Accept Changes" button to trigger code generation

**Key Methods:**
```typescript
interface CanvasViewState {
  selectedFrameId: string | null;
  frames: FrameData[];
  undoStack: FrameData[][];
  redoStack: FrameData[][];
  zoom: number;
  pan: { x: number; y: number };
  isGeneratingCode: boolean;
}

interface CanvasViewMethods {
  selectFrame(frameId: string): void;
  deselectFrame(): void;
  updateFrameProperty(frameId: string, property: string, value: any): void;
  undo(): void;
  redo(): void;
  saveFrames(): Promise<void>;
  acceptChanges(): Promise<void>; // NEW: Trigger code generation
}
```

### 2. DesignFrame Component (Enhanced)

**Responsibilities:**
- Render design content in iframe
- Display resize handles when selected
- Handle drag-to-resize interactions
- Handle drag-to-move interactions
- Emit events to parent (CanvasView)

**Key Props:**
```typescript
interface DesignFrameProps {
  id: string;
  filePath: string;
  width: number;
  height: number;
  x: number;
  y: number;
  isSelected: boolean;
  onSelect: (id: string) => void;
  onResize: (id: string, width: number, height: number) => void;
  onMove: (id: string, x: number, y: number) => void;
  onPropertyChange: (id: string, property: string, value: any) => void;
}
```

### 3. PropertiesPanel Component (New)

**Responsibilities:**
- Display editable properties of selected frame
- Provide input fields for width, height, x, y, opacity
- Allow editing of custom attributes
- Emit property change events

**Key Props:**
```typescript
interface PropertiesPanelProps {
  frame: FrameData | null;
  onPropertyChange: (property: string, value: any) => void;
}

interface FrameData {
  id: string;
  filePath: string;
  width: number;
  height: number;
  x: number;
  y: number;
  opacity: number;
  customAttributes: Record<string, any>;
  metadata: {
    lastModified: number;
    version: number;
  };
}
```

### 4. SaveManager Service (New)

**Responsibilities:**
- Debounce save operations
- Persist frame data to design files
- Handle save failures and retries
- Provide save status feedback

**Key Methods:**
```typescript
interface SaveManager {
  queueSave(frameId: string, data: FrameData): void;
  flushSaves(): Promise<void>;
  onSaveStart: (callback: () => void) => void;
  onSaveComplete: (callback: (success: boolean) => void) => void;
  onSaveError: (callback: (error: Error) => void) => void;
}
```

### 5. UndoRedoManager Service (New)

**Responsibilities:**
- Maintain undo/redo stacks
- Track frame state changes
- Provide undo/redo operations
- Limit stack size to prevent memory issues

**Key Methods:**
```typescript
interface UndoRedoManager {
  pushState(state: FrameData[]): void;
  undo(): FrameData[] | null;
  redo(): FrameData[] | null;
  canUndo(): boolean;
  canRedo(): boolean;
  clear(): void;
}
```

### 6. CodeGenerationService (New)

**Responsibilities:**
- Scan codebase to gather context
- Collect HTML/CSS structure and patterns
- Send changes to AI for code generation
- Update design files with generated code
- Handle generation failures and retries

**Key Methods:**
```typescript
interface CodeGenerationService {
  scanCodebase(designFilePath: string): Promise<CodebaseContext>;
  generateCode(changes: FrameChanges, context: CodebaseContext): Promise<string>;
  updateDesignFile(filePath: string, newCode: string): Promise<void>;
  onGenerationStart: (callback: () => void) => void;
  onGenerationComplete: (callback: (success: boolean) => void) => void;
  onGenerationError: (callback: (error: Error) => void) => void;
}

interface CodebaseContext {
  htmlStructure: string;
  cssStyles: string;
  componentPatterns: string[];
  designTokens: Record<string, any>;
  dependencies: string[];
}

interface FrameChanges {
  frameId: string;
  originalDimensions: { width: number; height: number };
  newDimensions: { width: number; height: number };
  originalPosition: { x: number; y: number };
  newPosition: { x: number; y: number };
  propertyChanges: Record<string, any>;
}
```

## Data Models

### Frame Metadata File

Each design file will have an associated `.meta.json` file storing frame properties. **The HTML file is updated with new CSS values.**

**Example structure:**
```
.superdesign/design_iterations/
├── table_1.html              ← Original HTML code (UPDATED with new CSS values)
├── table_1.meta.json         ← Frame metadata (stores frame references and properties)
```

**How it works:**

1. **Before resize:**
```html
<div id="frame_1" style="width: 800px; height: 600px; position: absolute; left: 100px; top: 100px;">
  <!-- content -->
</div>
```

2. **After resize to 1000x700:**
```html
<div id="frame_1" style="width: 1000px; height: 700px; position: absolute; left: 100px; top: 100px;">
  <!-- content -->
</div>
```

3. **Metadata file tracks frame references:**
```json
{
  "filePath": ".superdesign/design_iterations/table_1.html",
  "frames": [
    {
      "id": "frame_1",
      "selector": "#frame_1",
      "width": 1000,
      "height": 700,
      "x": 100,
      "y": 100,
      "metadata": {
        "lastModified": 1703779200000,
        "version": 2
      }
    }
  ]
}
```

**Key Point:** When a user resizes or moves a frame on the canvas:
- ✅ The CSS properties in `table_1.html` are updated (width, height, position)
- ✅ The `.meta.json` file is updated with new dimensions/position
- ✅ The HTML file is persisted with the new CSS values
- ❌ The HTML structure remains unchanged (only CSS values are modified)

### Frame State Structure

```typescript
interface FrameState {
  id: string;
  filePath: string;
  position: { x: number; y: number };
  size: { width: number; height: number };
  properties: {
    opacity: number;
    zIndex: number;
    customAttributes: Record<string, any>;
  };
  metadata: {
    createdAt: number;
    lastModified: number;
    version: number;
  };
}
```

## Correctness Properties

A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.

### Property 1: Resize Bounds Enforcement
*For any* frame resize operation with target dimensions, the resulting frame dimensions SHALL be clamped to minimum bounds (100x100px) and SHALL NOT exceed canvas boundaries.
**Validates: Requirements 1.5**

### Property 2: Real-Time Dimension Update During Drag
*For any* drag-to-resize operation, the frame dimensions SHALL update in real-time as the mouse moves, reflecting the current drag delta.
**Validates: Requirements 1.2**

### Property 3: Resize Persistence Round Trip
*For any* frame resize operation followed by release, the new dimensions SHALL be persisted to the design file and reading the file SHALL return the same dimensions.
**Validates: Requirements 1.3**

### Property 4: Position Bounds Enforcement
*For any* frame move operation, the resulting position SHALL keep the frame within canvas boundaries (x ≥ 0, y ≥ 0, x + width ≤ canvasWidth, y + height ≤ canvasHeight).
**Validates: Requirements 2.3**

### Property 5: Real-Time Position Update During Drag
*For any* drag-to-move operation, the frame position SHALL update in real-time as the mouse moves, reflecting the current drag delta.
**Validates: Requirements 2.1**

### Property 6: Move Persistence Round Trip
*For any* frame move operation followed by release, the new position SHALL be persisted to the design file and reading the file SHALL return the same position.
**Validates: Requirements 2.2**

### Property 7: Property Edit Persistence Round Trip
*For any* property modification in the properties panel, after editing completes, the change SHALL be persisted to the design file and reading the file SHALL return the new value.
**Validates: Requirements 3.3**

### Property 8: Auto-Save Debounce Efficiency
*For any* rapid sequence of property changes within the debounce window (e.g., 500ms), the system SHALL perform only one save operation instead of multiple saves.
**Validates: Requirements 4.2**

### Property 9: Auto-Save Timing
*For any* frame property modification, a save operation SHALL be initiated within 500ms of the last change.
**Validates: Requirements 4.1**

### Property 10: Undo-Redo Round Trip
*For any* sequence of frame modifications followed by undo operations equal to the number of modifications, the frame state SHALL return to its original state.
**Validates: Requirements 6.1, 6.3**

### Property 11: Redo Restoration
*For any* sequence of undo operations followed by redo operations equal to the number of undos, the frame state SHALL return to the state before the undo sequence.
**Validates: Requirements 6.2, 6.3**

### Property 12: Selection State Consistency
*For any* frame selection action, the selected frame SHALL display selection handles and the properties panel SHALL display the frame's current properties.
**Validates: Requirements 5.1, 5.2, 5.4**

### Property 13: Deselection Clears UI
*For any* deselection action (clicking outside frames), the previously selected frame SHALL no longer display selection handles and the properties panel SHALL be hidden.
**Validates: Requirements 5.3**

### Property 14: Save Failure Retry
*For any* failed save operation, the system SHALL automatically retry the save operation and eventually succeed or display an error notification.
**Validates: Requirements 4.4**

### Property 15: Cursor Feedback on Hover
*For any* hover over a frame edge or corner, the cursor style SHALL change to indicate resize capability (↔ or ↗).
**Validates: Requirements 1.1**

### Property 16: Properties Panel Visibility
*For any* frame selection, the properties panel SHALL be visible and display editable fields for width, height, x, y, opacity, and custom attributes.
**Validates: Requirements 3.1, 3.5**

### Property 17: Codebase Context Collection
*For any* code generation request, the system SHALL scan the codebase and collect HTML structure, CSS styles, component patterns, and design tokens.
**Validates: Requirements 9.1, 9.2**

### Property 18: Code Generation Round Trip
*For any* accepted canvas changes, the system SHALL generate updated code and update the design file such that reading the file returns the new code.
**Validates: Requirements 9.3, 9.4**

### Property 19: Metadata Cleanup After Accept
*For any* accepted changes, after code generation completes successfully, the metadata file SHALL be cleared and frame properties SHALL be reset to defaults.
**Validates: Requirements 9.7**

### Property 20: Code Generation Error Handling
*For any* failed code generation, the system SHALL display an error notification and provide a retry option.
**Validates: Requirements 9.6**

## Error Handling

### Resize Errors
- **Minimum size violation**: Prevent resize, show tooltip "Minimum size is 100x100px"
- **Canvas boundary violation**: Clamp dimensions to canvas bounds
- **Invalid input**: Validate numeric inputs, show error message

### Move Errors
- **Canvas boundary violation**: Clamp position to canvas bounds
- **Invalid coordinates**: Validate numeric inputs

### Save Errors
- **File write failure**: Retry up to 3 times with exponential backoff
- **File locked**: Show notification "File is locked, retrying..."
- **Permission denied**: Show error "Cannot save, check file permissions"

### Property Edit Errors
- **Invalid value type**: Show type error, revert to previous value
- **Out of range**: Clamp to valid range or show error
- **Parsing error**: Show error message, allow user to correct

## Testing Strategy

### Unit Tests

- Test resize bounds validation
- Test position bounds validation
- Test debounce logic
- Test undo/redo stack operations
- Test property validation
- Test save retry logic

### Property-Based Tests

- **Property 1**: Generate random resize operations and verify bounds are enforced
- **Property 2**: Generate random move operations and verify position stays within canvas
- **Property 3**: Generate random property changes, verify persistence after debounce
- **Property 4**: Generate random modification sequences, verify undo returns to original state
- **Property 5**: Generate random selections, verify handles and panel display correctly
- **Property 6**: Generate rapid changes, verify only one save occurs
- **Property 7**: Simulate save failures, verify retry and eventual success

### Integration Tests

- Test full resize workflow (select → drag → save → verify file)
- Test full move workflow (select → drag → save → verify file)
- Test property edit workflow (select → edit → save → verify file)
- Test undo/redo with file persistence
- Test multiple frames interaction
- Test concurrent edits and saves

