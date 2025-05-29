# Mirror of Discernment (MoD) Architecture

## 1. Overview
**Mirror of Discernment** is an AR-ready “truth-filter” overlay that sits in front of any video or AR feed.  
It calls into the Content Alignment API (CAA), runs a 12-Beat / 3-Click / 10-Gate truth pipeline, and pushes analytics into the QBSM dashboard.

---

## 2. Key Concepts

- **12-Beat Cycle**  
  1. **Open**  
  2–11. Gate 1…Gate 10  
  12. **Close**

- **10 Truth Gates**  
  1. Relevance  
  2. Prototype  
  3. Fact  
  4. Consistency  
  5. Emotional  
  6. Authority  
  7. Novelty  
  8. Representation  
  9. Coherence  
  10. Integration

- **3 Click-Triggers**  
  After Beat 4, 7, 10 → “Hold → Compare → Decide”

- **4 Macro-Phases**  
  1. Severance (2–4)  
  2. Discernment (5–6)  
  3. Yielding (7–9)  
  4. Integration (10–11)

---

## 3. System Components

\`\`\`text
┌────────────────────────────────────────────────────────┐
│                   Mirror of Discernment              │
│  (Mobile/Web AR client & UI)                         │
│   └─ AR Beat Clock, Click UI, Gate Overlays          │
│       └── Calls → POST /scan on CAA                  │
├────────────────┬──────────────────────────────────────┤
│    deps/caa    │  Content Alignment API               │
│ (Flask service)│  • /scan endpoint                     │
│                │  • segments + scores JSON             │
├────────────────┼──────────────────────────────────────┤
│ deps/qsbm-dash │  Streamlit QBSM Dashboard             │
│                │  • historical analytics               │
│                │  • user profile & model integration   │
└────────────────┴──────────────────────────────────────┘
\`\`\`

---

## 4. Dataflow Sequence

1. **Open**  
   - Load user profile (topics, thresholds)  
   - Start beat clock

2. **Beat 2 – Gate 1 (Relevance)**  
   - Filter segments by topic tags  

3. **Beat 3 – Gate 2 (Prototype)**  
   - Map each segment to an archetype  

4. **Beat 4 – Gate 3 (Fact)**  
   - Quick API fact-check  

   **CLICK 1:** pause → compare → decide  

5. **Beat 5 – Gate 4 (Consistency)**  
6. **Beat 6 – Gate 5 (Emotional)**  
7. **Beat 7 – Gate 6 (Authority)**  

   **CLICK 2:** pause → compare → decide  

8. **Beat 8 – Gate 7 (Novelty)**  
9. **Beat 9 – Gate 8 (Representation)**  
10. **Beat 10 – Gate 9 (Coherence)**  

    **CLICK 3:** pause → compare → decide  

11. **Beat 11 – Gate 10 (Integration)**  
12. **Close**  
    - Collate passed segments, render overlay, log results  

---

## 5. Beat ↔ Archetype Mapping

| Beat | Gate Name      | Archetype      | QBSM Phase     |
|:----:|:---------------|:---------------|:---------------|
|  1   | Open           | Orphan (Twist) | Twist          |
|  2   | Relevance      | Wanderer       | Ping           |
|  3   | Prototype      | Warrior        | Hold           |
|  4   | Fact           | Martyr         | Pong           |
|  5   | Consistency    | Sage           | Shift          |
|  6   | Emotional      | Magician       | Ping (2)       |
|  7   | Authority      | Ruler          | Lift           |
|  8   | Novelty        | Rebel          | Pong (2)       |
|  9   | Representation | Lover          | Gap            |
| 10   | Coherence      | Seeker         | Final Ping     |
| 11   | Integration    | Shadow         | —              |
| 12   | Close          | Source         | —              |

---

## 6. Technology Stack

- **AR Client**:  
  - WebAR (Three.js + AR.js) or React Native + ARKit/ARCore  
  - Beat clock timer (JavaScript/Python)  
  - Gate overlays & click UI

- **CAA**:  
  - Flask + Python  
  - Dockerized on port 8080  
  - `/scan` JSON API

- **QBSM Dashboard**:  
  - Streamlit + Python  
  - Dockerized on port 8501

- **CI/CD**:  
  - GitHub Actions (CI build, tests, Docker push)  
  - Automated release tagging

- **Deployment**:  
  - Docker Compose or Kubernetes (Helm chart)  
  - Secrets via GitHub Vault or similar

---

## 7. Next Steps

1. **API Spec** → `api-spec.md`  
2. **Gate Logic** → `gate-logic.md`  
3. **AR Filter UI** → `ar-filter-spec.md`  
4. **Deployment** → `deployment-guide.md`  
5. **FPGA Sensor** → `fpga-sensor-spec.md`  
6. **CI Pipeline** → `ci-pipeline.md`  
7. **Security Review** → `security-review.md`  

> As soon as this file lands in `docs/architecture.md`, we have our north-star.  
> Let me know which spec you’d like next, or if you need any tweaks here!  
