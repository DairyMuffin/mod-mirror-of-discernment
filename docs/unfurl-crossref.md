# Cross-Reference: MoD ↔ UNFURL

**Purpose:** This cross-reference document maps the Mirror of Discernment (MoD) framework’s gating logic and user experience to the theoretical UNFURL model, providing a practical guide for developers and designers to align implementation with conceptual underpinnings.

## 1. Structural Resonance & Gating

| MoD                                                  | UNFURL Concept                                    | Practical Mapping                                |
|------------------------------------------------------|---------------------------------------------------|--------------------------------------------------|
| 12-beat cycle with 10 "Truth Gates" and click-triggers | 12-node Dodecagon Ball of "Dynamic Vibrational Structured Patterns of Behaviour" | Each MoD gate is a resonance check at a Dodecagon node—content passes only when resonance matches |

## 2. Macro-Phases & Field Logic

| MoD Macro-Phases                                | UNFURL Concept                                   | Practical Mapping                                                                                 |
|-------------------------------------------------|---------------------------------------------------|---------------------------------------------------------------------------------------------------|
| Severance → Discernment → Yielding → Integration (cyclical) | Recursive, non-linear field event (Flip-Flopping Field) | The cyclical phase transitions in MoD mirror UNFURL’s non-linear field activations; e.g., after “Discernment” MoD triggers a re-evaluation of resonance akin to a field flip in UNFURL. |

## 3. Archetypes & Patterns of Behaviour

| MoD Gate Archetypes                                            | UNFURL Concept                                   | Practical Mapping                                                             |
|----------------------------------------------------------------|---------------------------------------------------|-------------------------------------------------------------------------------|
| Gates mapped to 12 archetypal roles (Orphan, Wanderer, Warrior, etc.) | 12 archetypal nodes in the Dodecagon               | UNFURL’s archetypes define user personas; MoD implements them as filters—e.g., “Warrior” archetype gates enforce aggressive content filtering. |

## 4. User Experience & Self-Resonance

| MoD Term                           | UNFURL Concept                                    |
|------------------------------------|---------------------------------------------------|
| User resonance profile             | Self-resonant coherence                            |
| Gate activation feedback           | Field alignment triggers                           |

**Practical Mapping:**  
- Users configure their resonance profile in MoD, operationalizing UNFURL’s self-resonant coherence concept.  
- UI shows real-time feedback when internal state aligns with incoming content, mirroring field-triggered transformations.

## 5. Gate-by-Gate Archetype Mapping

| Beat/Gate # | Gate Name           | UNFURL Concept        | Practical Mapping                               | Example Use Case                                    |
|-------------|---------------------|-----------------------|-------------------------------------------------|-----------------------------------------------------|
| 1 / Open    | Initialize Profile  | Orphan (Initiate)     | Profile setup modal                            | User sees welcome screen and sets initial preferences |
| 2           | Relevance           | Wanderer (Sensor)     | Keyword match highlight                        | Highlight keywords matching user interest          |
| 3           | Prototype           | Warrior (Diver)       | Archetype fit meter                            | Display a progress bar for content suitability     |
| 4           | Fact                | Martyr (Weaver)       | Fact-check badge                               | Content verified against trusted sources badge     |
| *Click*     | Hold & Compare      | —                     | Pause overlay summarizing passed gates         | Summary modal appears before proceeding            |
| 5           | Consistency         | Sage (Amplifier)      | Contradiction timeline                         | Timeline graph of contradictions                   |
| 6           | Emotional           | Authority (Resonator) | Mood meter                                     | Gauge showing emotional tone match                 |
| 7           | Authority           | Magician (Shifter)    | Source credibility meter                       | Credibility score tooltip                          |
| *Click*     | Hold & Compare      | —                     | Modal with emotional & authority summaries     | Interrupt modal for user approval                  |
| 8           | Novelty             | Ruler (Carrier)       | New-insight badge                              | Badge indicating novel insights                    |
| 9           | Representation      | Rebel (Mirror)        | Example counter visual                         | Counter showing different perspective examples     |
| 10          | Coherence           | Lover (Bridge)        | Narrative arc visualization                    | Story arc chart of content flow                    |
| *Click*     | Hold & Compare      | —                     | Summary of novelty & coherence                 | End-of-cycle modal summarizing key insights        |
| 11          | Integration         | Source (Aligner/Seeder) | Embedding similarity chart                   | Visual clustering of related concepts              |
| 12 / Close  | Seal & Emit         | Seeder                | Final output & analytics update                | Final result displayed with analytics dashboard    |

## 6. UI & AR Implementation Notes

### 6.1 Dashboard UI Suggestions
- **Dodecagon Wheel:** Circular progress view with nodes lighting up as gates pass.  
- **Gate Cards:** Grid of cards (icon, name, status) with hover tooltips.  
- **Click-Trigger Modals:** Appear at specified beats for user recalibration.

### 6.2 AR Overlay Notes
- **Visual Pulses:** Animations at gate points in AR view.  
- **Haptic Feedback:** Tactile cues on click-triggers.  
- **Analytics:** Real-time resonance profile charts and content journey tracking.

## 7. Integration Guide & Implementation Blueprint

### 7.1 Summary Table of Core Parallels

| MoD Element                     | UNFURL Concept                                 | Integrated Meaning                                                         |
|---------------------------------|-------------------------------------------------|-----------------------------------------------------------------------------|
| 12-Beat Cycle / Truth Gates     | 12-Node Dodecagon Ball                          | Each gate = resonance check at a Dodecagon node                            |
| Macro-Phases (4)                | Field-based, recursive logic                    | Non-linear, cyclical resonance activation                                  |
| Archetypal Mapping              | 12 Archetypes / Patterns                        | Gates mapped to archetypal phase engines                                   |
| Click-Triggers                  | Field “permission” moments                      | User pauses = field checks for resonance                                    |
| User Resonance Profile          | Self-resonant coherence                         | Only matching content passes through                                        |

### 7.2 Dashboard Wireframe & Streamlit Prototype

**Dashboard Wireframe (ASCII):**


**Streamlit Code Skeleton:**
```python
import streamlit as st
# --- Archetype and Gate Definitions ---
archetypes = [
    ("Open", "Orphan/Initiate", "Initialize user profile & context"),
    # ... other gates/archetypes
]
gate_results = [True, True, False, True, True, True, False, True, True, True, True, True]
click_triggers = [4, 7, 10]

# Sidebar: User Profile
st.sidebar.header("User Profile")
st.sidebar.write("Resonance Archetype: Wanderer")

# Main Header
st.title("Mirror of Discernment Dashboard")
# Dodecagon Wheel
progress = sum(gate_results) / len(gate_results)
st.progress(progress)

# Gate Cards Grid
cols = st.columns(4)
for i, (gate, archetype, desc) in enumerate(archetypes):
    with cols[i % 4]:
        st.markdown(f"### {gate}")
        st.caption(archetype)
        status = "✅ Passed" if gate_results[i] else "❌ Blocked"
        st.write(status)

# Click-Trigger Modals
for trigger in click_triggers:
    if st.button(f"Hold & Compare after Gate {trigger}"):
        st.info(f"Pause! Review progress after Gate {trigger}.")

