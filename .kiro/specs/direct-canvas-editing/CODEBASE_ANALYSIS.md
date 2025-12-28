# Codebase Analysis - Direct Canvas Editing Feature

## Existing Functionality Analysis

### 1. Drag & Move Functionality (ALREADY EXISTS ✅)

**Location:** `src/webview/components/CanvasView.tsx`

**Current Implementation:**
```typescript
// Drag state management (lines 80-87)
const [dragState, setDragState] = useState<DragState>({
    isDragging: false,
    draggedFrame: null,
    startPosition: { x: 0, y: 0 },
    currentPosition: { x: 0, y: 0 },
    offset: { x: 0, y: 0 }
});

// Drag handlers (lines 437-500)
const handleDragStart = (fileName: string, startPos: GridPosition, mouseEvent: React.MouseEvent) => { ... }
const handleDragMove = (mousePos: GridPosition) => { ... }
const handleDragEnd = () => { ... }
```

**What's Already Implemented:**
- ✅ Frame selection by clicking
- ✅ Drag-to-move functionality
- ✅ Real-time position updates during drag
- ✅ Grid snapping on drag end
- ✅ Custom position storage in `customPositions` state
- ✅ Mouse event handlers (onMouseDown, onMouseMove, onMouseUp)

**What's Missing:**
- ❌ Resize handles (corners/edges)
- ❌ Resize drag functionality
- ❌ CSS property updates in HTML files
- ❌ Auto-save to file system
- ❌ Properties panel for editing
- ❌ Undo/redo functionality

---

### 2. Frame Selection (ALREADY EXISTS ✅)

**Location:** `src/webview/components/CanvasView.tsx`

**Current Implementation:**
```typescript
// Frame selection state (line 73)
const [selectedFrames, setSelectedFrames] = useState<string[]>([]);

// Selection handler (line 502+)
const handleFrameSelect = (fileName: string) => {
    setSelectedFrames(prev => 
        prev.includes(fileName) ? prev : [fileName]
    );
    // ... context update logic
};
```

**What's Already Implemented:**
- ✅ Single frame selection
- ✅ Visual selection feedback (isSelected prop)
- ✅ Context update on selection

**What's Missing:**
- ❌ Properties panel display
- ❌ Selection handles/resize handles
- ❌ Deselection on click outside

---

### 3. Frame Rendering (ALREADY EXISTS ✅)

**Location:** `src/webview/components/DesignFrame.tsx`

**Current Implementation:**
- ✅ Renders HTML/SVG in iframe
- ✅ Viewport switching (mobile/tablet/desktop)
- ✅ Frame metadata display
- ✅ Copy prompt functionality

**What's Missing:**
- ❌ Resize handles rendering
- ❌ Direct CSS editing
- ❌ Property panel integration

---

## What Needs to Be Built

### 1. Resize Functionality (NEW)
- Add resize handles to DesignFrame component
- Implement resize drag handlers
- Update frame dimensions in real-time
- Persist CSS changes to HTML file

### 2. Properties Panel (NEW)
- Create PropertiesPanel component
- Display editable properties (width, height, x, y, opacity)
- Handle property change events
- Validate numeric inputs

### 3. Auto-Save Service (NEW)
- Create SaveManager service
- Implement debounce logic (500ms)
- Update HTML files with new CSS values
- Create/update metadata files
- Implement retry logic for failed saves

### 4. Undo/Redo (NEW)
- Create UndoRedoManager service
- Maintain undo/redo stacks
- Integrate with CanvasView
- Add keyboard shortcuts (Ctrl+Z, Ctrl+Y)

### 5. Save Status Indicator (NEW)
- Create SaveIndicator component
- Display save status (saving, success, error)
- Auto-hide after completion

---

## Integration Points

### Existing Code to Extend

1. **CanvasView.tsx**
   - Add resize handlers alongside drag handlers
   - Integrate PropertiesPanel component
   - Integrate SaveManager service
   - Integrate UndoRedoManager service
   - Add keyboard event listeners for undo/redo

2. **DesignFrame.tsx**
   - Add resize handles rendering
   - Add resize event handlers
   - Pass resize callbacks to parent

3. **Canvas Types** (`src/webview/types/canvas.types.ts`)
   - Extend DragState to include resize state
   - Add FrameData interface
   - Add SaveState interface

---

## File Structure to Create

```
src/webview/
├── components/
│   ├── PropertiesPanel.tsx          (NEW)
│   ├── SaveIndicator.tsx            (NEW)
│   ├── CanvasView.tsx               (EXTEND)
│   └── DesignFrame.tsx              (EXTEND)
├── services/
│   ├── SaveManager.ts               (NEW)
│   └── UndoRedoManager.ts           (NEW)
├── types/
│   └── canvas.types.ts              (EXTEND)
└── utils/
    └── (existing utilities)
```

---

## Key Differences from Current Implementation

| Feature | Current | New |
|---------|---------|-----|
| **Move** | ✅ Moves frame position on canvas | ✅ Same + updates CSS in HTML file |
| **Resize** | ❌ Not implemented | ✅ Drag handles to resize + update CSS |
| **Persistence** | ❌ Only stores in state | ✅ Saves to HTML file + metadata |
| **Auto-save** | ❌ Manual save only | ✅ Debounced auto-save (500ms) |
| **Properties** | ❌ No panel | ✅ Edit width, height, x, y, opacity |
| **Undo/Redo** | ❌ Not implemented | ✅ Full undo/redo support |
| **Save Feedback** | ❌ No indicator | ✅ Visual save status |

---

## Implementation Strategy

### Phase 1: Resize Functionality
1. Add resize handles to DesignFrame
2. Implement resize drag handlers in CanvasView
3. Update frame dimensions in real-time
4. Test resize bounds validation

### Phase 2: Properties Panel
1. Create PropertiesPanel component
2. Integrate with CanvasView
3. Implement property change handlers
4. Add input validation

### Phase 3: Auto-Save
1. Create SaveManager service
2. Implement debounce logic
3. Update HTML files with CSS changes
4. Create metadata files
5. Implement retry logic

### Phase 4: Undo/Redo
1. Create UndoRedoManager service
2. Integrate with CanvasView
3. Add keyboard shortcuts
4. Test undo/redo with file persistence

### Phase 5: Save Indicator
1. Create SaveIndicator component
2. Integrate with SaveManager
3. Display save status
4. Auto-hide on completion

---

## Reusable Code from Existing Implementation

### Drag State Management Pattern
```typescript
// Can reuse this pattern for resize state
const [dragState, setDragState] = useState<DragState>({
    isDragging: false,
    draggedFrame: null,
    startPosition: { x: 0, y: 0 },
    currentPosition: { x: 0, y: 0 },
    offset: { x: 0, y: 0 }
});
```

### Mouse Event Handling Pattern
```typescript
// Can reuse this pattern for resize handlers
onMouseMove={(e) => {
    if (dragState.isDragging) {
        const rect = e.currentTarget.getBoundingClientRect();
        const mousePos = transformMouseToCanvasSpace(e.clientX, e.clientY, rect);
        handleDragMove(mousePos);
    }
}}
onMouseUp={handleDragEnd}
onMouseLeave={handleDragEnd}
```

### Coordinate Transformation
```typescript
// Can reuse this function for resize calculations
const transformMouseToCanvasSpace = (clientX: number, clientY: number, canvasRect: DOMRect): GridPosition => {
    const transformState = transformRef.current?.instance?.transformState;
    const currentScale = transformState?.scale || 1;
    const currentTranslateX = transformState?.positionX || 0;
    const currentTranslateY = transformState?.positionY || 0;
    
    const rawMouseX = clientX - canvasRect.left;
    const rawMouseY = clientY - canvasRect.top;
    
    return {
        x: (rawMouseX - currentTranslateX) / currentScale,
        y: (rawMouseY - currentTranslateY) / currentScale
    };
};
```

---

## Summary

**Good News:** 
- 60% of the drag/move functionality already exists
- Frame selection is already implemented
- Mouse event handling infrastructure is in place

**What to Build:**
- Resize handles and resize logic (20% of work)
- Properties panel (10% of work)
- Auto-save service (5% of work)
- Undo/redo service (3% of work)
- Save indicator (2% of work)

**Estimated Effort:** 
- Reuse existing drag infrastructure
- Extend with resize functionality
- Add new services for save and undo/redo
- Total: ~40% new code, ~60% reuse/extension

