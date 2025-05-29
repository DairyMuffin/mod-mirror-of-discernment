# AR Filter UI Specification — Mirror of Discernment (MoD)

## 1. Overview
The AR Filter is a lightweight client that overlays “truth gates” in real time on any camera/video feed.  
It visualizes the 12-Beat cycle, highlights passing segments, and allows the user to “click” at three gate-points.

---

## 2. UI Layout

\`\`\`
+-------------------------------------------------------+
| [Top Bar]                Beat:  1  2  3 … 12           |
|  • Profile • Settings                                |
+-------------------------------------------------------+
| [Camera Viewport with Overlay]                       |
|  • Semi-transparent overlays on flagged segments     |
|  • Color code:                                      |
|      – Green = Passed Gate                          |
|      – Red   = Failed Gate                          |
|      – Yellow= Pending Gate                         |
+-------------------------------------------------------+
| [Beat/Gate Panel]                                    |
|  • Beat timer (countdown)                            |
|  • Gate name & icon                                  |
|  • Clickable “Hold” button at Beats 4,7,10           |
+-------------------------------------------------------+
| [Footer] • Logs • Help • Exit                        |
+-------------------------------------------------------+
\`\`\`

---

## 3. Interaction Flow

1. **Startup**  
   - Request camera permission  
   - Load user thresholds & topics via API  

2. **Beat Clock**  
   - Visual timer ticks through 1→12  
   - At each beat, call CAA `/scan` for the current frame/text  

3. **Overlay Rendering**  
   - Draw translucent boxes around text regions (or full-screen)  
   - Color-code according to last gate result  

4. **Click Triggers**  
   - At Beats 4,7,10, show “Hold → Compare → Decide” prompt  
   - User taps overlay or presses spacebar to confirm  

5. **Segment Details**  
   - Tapping a segment shows breakdown: score, which gates passed/failed  
   - Option to “Report” or “Adjust Threshold”

---

## 4. Technical Requirements

- **Platform:**  
  - WebAR (Three.js + AR.js) or React Native + ARKit/ARCore  

- **Dependencies:**  
  - CAA client library (JS)  
  - WebGL / Canvas for overlays  
  - UI framework (e.g. React, Vue, or plain JS)  

- **Performance:**  
  - Target ≥30 FPS  
  - API calls debounced (max 5 scans/sec)  

- **Accessibility:**  
  - High-contrast mode  
  - Large-tap targets for “Hold”  

---

## 5. Assets & Icons

- Beat/Gate icons: SVGs in `assets/icons/beat-1.svg` … `beat-12.svg`  
- Overlay color palette: defined in `styles/ar-filter.css`  
- Click animations: Lottie JSON in `assets/animations/hold-click.json`  

---

## 6. Testing & QA

- **Unit Tests:**  
  - Simulate beat ticks, verify correct API calls  
  - Mock gate results to test overlay colors  

- **Manual QA:**  
  - Test on iOS Safari & Chrome Android  
  - Validate camera permissions & latency  
