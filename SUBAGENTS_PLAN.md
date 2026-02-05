# Plan: Supercharging Sofle ZMK with Specialized Subagents

This plan outlines the creation of specialized "Skills" (subagents) for the `opencode` environment to automate and optimize the iteration of your Sofle keymap.

## 🤖 Proposed Subagents

### 1. ZMK Logic Specialist (`zmk-logic`)
*   **Purpose**: Expert in Devicetree logic, `zmk-helpers` macros, and behavior fine-tuning.
*   **Capabilities**: 
    *   Tuning HRM timings (`tapping-term`, `require-prior-idle`).
    *   Implementing complex `mod-morphs`, `tap-dances`, and `combos`.
    *   Refactoring raw Devicetree into clean, macro-based logic.

### 2. Ergonomic & Layout Critic (`zmk-ergo`)
*   **Purpose**: Optimizes key placement based on ergonomics and the German language.
*   **Capabilities**:
    *   Analyzing Same Finger Use (SFU) and bigram frequency.
    *   Optimizing symbol placement for coding (C++, Python, Nix).
    *   Ensuring logical consistency across layers (Nav, Sym, Num).

### 3. Build & Manifest Sentinel (`zmk-build`)
*   **Purpose**: Manages dependencies and troubleshoots the build system.
*   **Capabilities**:
    *   Debugging `west build` errors and Devicetree compilation failures.
    *   Managing `west.yml` (pinning versions, adding modules).
    *   Managing Kconfig options in `sofle.conf`.

### 4. Keymap Visualization Master (`zmk-viz`)
*   **Purpose**: Keeps the visual documentation in sync with the code.
*   **Capabilities**:
    *   Automating `just draw` on successful builds.
    *   Fine-tuning `keymap-drawer` configurations (`draw/config.yaml`).
    *   Ensuring the SVG output is clear and well-labeled.

---

## 🛠 Step-by-Step Execution Plan

### Phase 1: Infrastructure (Today)
1.  **Create Agent Directory**: Setup `.opencode/agents/` in the project root.
2.  **Initialize Agent Instruction files**: Write the foundational instructions for each agent:
    *   `.opencode/agents/zmk-logic.md`
    *   `.opencode/agents/zmk-ergo.md`
    *   `.opencode/agents/zmk-build.md`
    *   `.opencode/agents/zmk-viz.md`

### Phase 2: Instruction Engineering
3.  **Define the Logic Expert**: Feed it the current `AGENTS.md` and `zmk-helpers` documentation.
4.  **Define the Ergo Critic**: Program it with German QWERTZ typing stats and the `keys_de.h` mapping logic.
5.  **Define the Build Sentinel**: Give it access to Zephyr/ZMK troubleshooting guides.

### Phase 3: Integration
6.  **Register Skills**: Ensure the `Task` tool can invoke these specialized agents.
7.  **Create an Iteration Loop**:
    *   `zmk-ergo` suggests a change.
    *   `zmk-logic` implements it in `base.keymap`.
    *   `zmk-build` verifies the compilation with `just build sofle`.
    *   `zmk-viz` updates the drawing with `just draw`.

---

## 🚀 Next Steps
I have created the directory structure and drafted the initial instruction sets in `.opencode/agents/`. 

To start iterating:
1.  **Run an Ergonomic Audit**: Ask the `zmk-ergo` agent to review your current `base.keymap`.
2.  **Optimize Logic**: Let `zmk-logic` implement a new behavior or tune your HRMs.
3.  **Verify & Visualize**: Use `zmk-build` and `zmk-viz` to confirm changes and update the documentation.
