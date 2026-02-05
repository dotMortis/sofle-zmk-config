# Agent Guide - urob's Sofle ZMK Config

This repository contains a highly optimized and customized ZMK firmware configuration, primarily targeting the **Sofle** keyboard. It is designed for a **German (QWERTZ)** system environment.

## 🛠 Build & Development Commands

The project uses `just` as a command runner and `nix` for environment management. `direnv` is used to automatically load the Nix shell.

- **Initialize Workspace**: `just init`
  - Runs `west init`, `west update`, and `west zephyr-export`. Essential for first-time setup.
- **Build Sofle Firmware**: `just build sofle`
  - Targets the Sofle left and right halves.
- **Build All Targets**: `just build all`
  - Parses `build.yaml` and builds all defined board/shield combinations.
- **List Valid Targets**: `just list`
  - Displays matching targets from `build.yaml`.
- **Update Dependencies**: `just update`
  - Pulls the latest version of ZMK and modules specified in `config/west.yml`.
- **Draw Keymap**: `just draw`
  - Uses `keymap-drawer` to generate `draw/base.svg` from `base.keymap`.
- **Clean Artifacts**: `just clean`
  - Removes the `.build` and `firmware` directories.

## 🎨 Code Style & Conventions

### Devicetree Structure
- **Indentation**: 2 spaces for all levels.
- **Includes**: 
  - Standard ZMK/Zephyr headers: `<dt-bindings/zmk/keys.h>`
  - Local configuration files: `"combos.dtsi"`
- **Macros**: This project relies heavily on [zmk-helpers](https://github.com/urob/zmk-helpers). Always prefer these macros over raw Devicetree syntax:
  - `ZMK_HOLD_TAP(name, ...)`
  - `ZMK_COMBO(name, ...)`
  - `ZMK_MOD_MORPH(name, ...)`
  - `ZMK_MACRO(name, ...)`
  - `ZMK_LAYER(name, ...)`

### Naming Conventions
- **Homerow Mods (HRMs)**:
  - `hml`: Left-hand HRMs.
  - `hmr`: Right-hand HRMs.
- **Aliases**:
  - `XXX`: Used for `&none`.
  - `___`: Used for `&trans`.
  - These are used to maintain visual alignment in keymap grids.
- **Constants**: Uppercase for hardware-related or timing constants (e.g., `QUICK_TAP_MS`, `COMBO_TERM_FAST`).

### Formatting
- **Keymap Grids**: Maintain vertical alignment in `ZMK_LAYER` and `ZMK_BASE_LAYER` definitions. Use comment bars (e.g., `// ╭──────┬──────╮`) to visually represent the rows of the keyboard.

## 🌍 German Layout (QWERTZ)

The configuration uses a custom mapping layer to work with German OS settings while maintaining a logical layout for coding.

- **Macro**: `LANG_DE` is defined in `sofle.keymap` to include `keys_de.h`.
- **Mappings**: 
  - `KEY_Z` maps to the physical 'Y' key (QWERTZ).
  - `KEY_Y` maps to the physical 'Z' key.
  - Umlauts are mapped to US positions: `KEY_OE` (Ö), `KEY_UE` (Ü), `KEY_AE` (Ä), `KEY_SZ` (ß).
- **Symbols**: Use descriptive aliases from `keys_de.h` like `KEY_HASH`, `KEY_PLUS`, `KEY_LT` (<), and `KEY_BACKSLASH`.

## 🧠 Keymap Logic & Behaviors

### Timeless Homerow Mods
HRMs are configured to be "timer-less" and highly resistant to misfires:
- **Flavor**: `balanced`.
- **Tapping Term**: 280ms.
- **Quick Tap**: 175ms.
- **Idle Timeout**: `require-prior-idle-ms = <150>` ensures taps are resolved immediately when typing fast, while holds only trigger from a "cold" start.
- **Positional Constraints**: `hold-trigger-key-positions` is used to prevent modifiers from triggering when the next key is on the same hand.

### Smart Layers & Gimmicks
- **Numword**: Auto-layer for numbers that activates via `SMART_NUM`. It deactivates automatically when any non-number key is pressed.
- **Magic Shift**: A thumb key that intelligently switches between `Repeat`, `Sticky Shift`, and `Caps Word`.
- **Leader Key**: Activated via combo `S+T`. Used for Unicode input (Greek letters, etc.) and system commands (Reset, Bootloader, BT/USB toggle).
- **Smart Mouse**: Layer-based mouse emulation with acceleration and precision modes tuned for 4K displays.

## 📂 Repository Structure

- `/config/sofle.keymap`: Primary entry point for the Sofle build.
- `/config/base.keymap`: The core keymap logic and behavior definitions.
- `/config/keys_de.h`: German layout mapping definitions.
- `/config/combos.dtsi`: All chording/combo definitions.
- `/config/leader.dtsi`: Leader key sequences and Unicode bindings.
- `/config/mouse.dtsi`: Mouse pointing and acceleration settings.
- `/config/west.yml`: Pinning of ZMK version (`v0.3`) and external modules.
- `/build.yaml`: GitHub Actions build matrix configuration.

## 🤖 Agent Orchestration & Workflow

This project uses a hierarchical agent model to ensure safety and precision.

1.  **Planner (Director)**: The main agent acts as the project director. It is responsible for analyzing user requests, performing research, and drafting a comprehensive plan. It delegates specific execution tasks to subagents.
2.  **Workflow**:
    *   **Research & Planning**: The Planner explores the codebase and drafts a step-by-step plan.
    *   **User Approval**: The plan is presented to the user for explicit approval.
    *   **Execution (Builder)**: Once approved, the Builder (main agent) coordinates the execution. The Builder may use subagents for specialized tasks or execute them directly if more efficient.
3.  **Subagent Restrictions**: 
    *   Subagents are **not** allowed to execute plans or make significant structural decisions on their own.
    *   They must be directed by the Planner/Builder for specific, scoped tasks.
    *   They focus on *how* to implement a specific behavior or fix, rather than *what* should be done at a project level.

## ⚠️ Guidelines for Changes

1. **Maintain Alignment**: When editing layers in `base.keymap`, ensure the columns remain aligned for readability.
2. **Prefer Modules**: If adding a new complex behavior, check if there is an existing ZMK module in `west.yml` that handles it.
3. **Verify Build**: Always run `just build sofle` after making changes to ensure the Devicetree compiles correctly.
4. **No Direct Keycodes**: Avoid using raw ZMK keycodes (e.g., `&kp LBKT`) directly in the keymap; use the `KEY_*` aliases from `keys_de.h` to ensure German layout compatibility.
5. **No Commits/Pushes**: Do **not** use git to commit or push changes. The user will handle all version control operations. Focus exclusively on local file modifications and verification.
6. **Kconfig**: System-level settings like Bluetooth power, sleep timeouts, and mouse support should be modified in `config/sofle.conf`.
