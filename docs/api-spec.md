```markdown
# AR Filter UI Specification — Mirror of Discernment (MoD)

## 1. Overview

The AR Filter is a lightweight, cross-platform client that overlays “truth gates” in real time on any camera or video feed. It visualizes the 12-Beat cycle, highlights passing segments, and enables user interaction at three key gate-points.

## 2. UI Layout

```

+-------------------------------------------------------------+
\| \[Top Bar] Beat: 1 2 3 ... 12   |   • Profile • Settings     |
+-------------------------------------------------------------+
\| \[Camera Viewport with Overlay]                             |
\|   • Semi-transparent overlays on flagged segments           |
\|   • Color coding:                                           |
\|       - Green   = Passed Gate                               |
\|       - Red     = Failed Gate                               |
\|       - Yellow  = Pending Gate                              |
+-------------------------------------------------------------+
\| \[Beat/Gate Panel]                                           |
\|   • Beat timer (countdown)                                  |
\|   • Gate name & icon                                        |
\|   • Clickable “Hold” button at Beats 4, 7, 10               |
+-------------------------------------------------------------+
\| \[Footer]   • Logs   • Help   • Exit                         |
+-------------------------------------------------------------+

```

## 3. Interaction Flow

1. **Startup**
   - Request camera permissions.
   - Load user thresholds and topics via API.

2. **Beat Clock**
   - Visual timer cycles through Beats 1–12.
   - At each beat, call the CAA `/scan` endpoint for the current frame or extracted text.

3. **Overlay Rendering**
   - Draw translucent overlays around text regions or across the viewport.
   - Apply color coding based on the latest gate result.

4. **Click Triggers**
   - At Beats 4, 7, and 10, display a “Hold → Compare → Decide” prompt.
   - User confirms by tapping the overlay or pressing the spacebar.

5. **Segment Details**
   - Tapping a segment reveals a breakdown: score, passed/failed gates.
   - Options to “Report” an issue or “Adjust Threshold.”

## 4. Technical Requirements

- **Platform Support:**
  - WebAR (Three.js + AR.js) or React Native with ARKit/ARCore.
- **Dependencies:**
  - CAA client library (JavaScript).
  - WebGL/Canvas for rendering overlays.
  - UI framework (React, Vue, or vanilla JS).
- **Performance:**
  - Target ≥30 FPS.
  - Debounce API calls (maximum 5 scans per second).
- **Accessibility:**
  - High-contrast mode.
  - Large tap targets for “Hold” actions.

## 5. Assets & Icons

- **Beat/Gate Icons:** SVGs located in `assets/icons/beat-1.svg` through `beat-12.svg`.
- **Overlay Color Palette:** Defined in `styles/ar-filter.css`.
- **Click Animations:** Lottie JSON files in `assets/animations/hold-click.json`.

## 6. Testing & Quality Assurance

- **Unit Testing:**
  - Simulate beat ticks and verify correct API call frequency.
  - Mock gate results to validate overlay color logic.
- **Manual QA:**
  - Test on iOS Safari and Android Chrome.
  - Validate camera permissions, UI responsiveness, and latency.
```
