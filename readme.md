<div align="center">

# Swarm

**Autonomous Multiplayer DOM Reconstruction Engine for the Web**

[![Vanilla JS](https://img.shields.io/badge/Vanilla_JS-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](https://bmshifat.pro.bd/assets/js/swarm-plugin.js)
[![Dependencies](https://img.shields.io/badge/Dependencies-0-10b981?style=flat-square)](https://bmshifat.pro.bd/assets/js/swarm-plugin.js)
[![Bundle Size](https://img.shields.io/badge/Size-168_KB-00f3ff?style=flat-square)](https://bmshifat.pro.bd/assets/js/swarm-plugin.js)
[![Isolation](https://img.shields.io/badge/DOM-Shadow_DOM_Isolated-8b5cf6?style=flat-square)](https://bmshifat.pro.bd/assets/js/swarm-plugin.js)
[![Author](https://img.shields.io/badge/Author-BM_Shifat-0b7fe0?style=flat-square)](https://www.facebook.com/bmshifat0/)

[Quickstart](#quickstart) • [Configuration](#configuration) • [JavaScript API](#javascript-api) • [Architecture](#architecture) • [Shortcuts](#hud--keyboard-controls)

</div>

---

## Overview

**Swarm** is a zero-dependency, drop-in JavaScript plugin that snapshots any live webpage into a structural blueprint and replays it being built from scratch by a swarm of autonomous multiplayer cursors. 

Without requiring a build step or external assets, Swarm analyzes computed styles, typography, inline SVGs, media elements, and continuous CSS animations in real time—rendering the reconstruction inside an isolated Shadow DOM canvas while preserving the host application's state and event listeners.

---

## Quickstart

Add the single-line script tag right before the closing `</body>` tag of any HTML document:

```html
<script src="[https://bmshifat.pro.bd/assets/js/swarm-plugin.js](https://bmshifat.pro.bd/assets/js/swarm-plugin.js)"></script>
```

Once loaded, Swarm mounts a subtle floating trigger button on the viewport edge. Visitors can click it at any time to watch the page rebuild, or press `Esc` to return to the live site immediately.

---

## Core Capabilities

| Capability | Technical Implementation |
| :--- | :--- |
| **Live DOM Blueprinting** | Traverses the active DOM tree, capturing computed styles, layout geometry, pseudo-elements (`::before` / `::after`), form states, and inline SVG symbol references. |
| **Multiplayer Cursor Simulation** | Deploys up to 90 concurrent agents utilizing Boids-inspired flocking, crosshair box-drawing, I-beam live typing, pen-tool vector paths, and ruler-snapping guides. |
| **Component Deduplication** | Automatically detects repeated DOM structures (cards, grids, list items) and simulates `Alt`-drag component duplication followed by inline content overrides. |
| **Continuous Motion Preservation** | Extracts active `@keyframes` rules and Web Animations API phase states so infinite animations (orbits, marquees, floating elements) continue running seamlessly. |
| **Procedural Web Audio Engine** | Synthesizes a 16th-note quantized E-major pentatonic glass piano, stereo-panned mechanical keystrokes, and convolution reverb using the native Web Audio API. |
| **Strict Shadow DOM Isolation** | Mounts inside a `<swarm-rebuild>` custom element with `contain: strict`, ensuring zero CSS leakage or mutation of the underlying page. |

---

## Configuration

### 1. Script `data-*` Attributes

Configure behavior declaratively on the `<script>` tag:

```html
<script
  src="[https://bmshifat.pro.bd/assets/js/swarm-plugin.js](https://bmshifat.pro.bd/assets/js/swarm-plugin.js)"
  data-position="middle-right"
  data-opacity="0.35"
  data-invisible="true"
  data-accent="#00f3ff"
  data-autostart="false">
</script>
```

| Attribute | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `data-position` | `string` | `"middle-right"` | Button placement: `middle-right`, `middle-left`, `top-left`, `top-center`, `top-right`, `bottom-left`, `bottom-center`, `bottom-right`. |
| `data-autostart` | `boolean` | `false` | Automatically initiates the rebuild sequence as soon as the document loads. |
| `data-button` | `boolean` | `true` | Mounts the default floating trigger button. Set to `false` when using custom triggers. |
| `data-invisible` | `boolean` | `true` | Renders the button in a low-opacity ghost state until hovered or focused. |
| `data-opacity` | `number` | `0.35` | Resting opacity (`0.0` – `1.0`) when `data-invisible="true"`. |
| `data-label` | `string` | `"Swarm"` | Accessible `aria-label` applied to the floating trigger button. |
| `data-accent` | `string` | `"#00f3ff"` | Hex color used for the button's hover focus ring and glow effect. |

### 2. Global Configuration Object

Alternatively, define `window.SwarmConfig` prior to loading the script:

```html
<script>
  window.SwarmConfig = {
    button: true,
    invisible: true,
    opacity: 0.4,
    position: "bottom-right",
    accent: "#00f3ff",
    autostart: false
  };
</script>
<script src="[https://bmshifat.pro.bd/assets/js/swarm-plugin.js](https://bmshifat.pro.bd/assets/js/swarm-plugin.js)"></script>
```

---

## JavaScript API

Swarm exposes a global `window.Swarm` controller for programmatic integration.

### Declarative HTML Trigger

Attach the `data-swarm` attribute to any HTML element to bind a click toggle automatically:

```html
<button type="button" data-swarm>Rebuild Page</button>
```

### Programmatic Methods

```javascript
// Start the DOM capture and rebuild sequence
Swarm.start();

// Terminate the active rebuild and remove the overlay
Swarm.stop();

// Toggle between active and inactive states
Swarm.toggle();

// Check whether the engine is currently running (returns boolean)
const active = Swarm.isRunning();
```

### Lifecycle Events

```javascript
window.addEventListener("swarm:starting", () => {
  console.log("Swarm engine initialized.");
});
```

---

## HUD & Keyboard Controls

While the rebuild overlay is active, an interactive Heads-Up Display (HUD) is available at the bottom of the viewport:

| Control / Shortcut | Action |
| :--- | :--- |
| `Esc` | Immediately exits the overlay and restores the live page. |
| `M` | Toggles procedural Web Audio synthesis on or off. |
| **Tour** | Locks manual scrolling and glides down section-by-section as layers finish building. |
| **1× / 2× / 3×** | Cycles the execution speed multiplier for all active cursor agents. |
| **Go live** | Smoothly dissolves the canvas back into the live interactive webpage upon completion. |
| **Replay** | Resets the blueprint and restarts the rebuild from the top of the document. |

---

## Architecture

1. **Viewport Scan & Inert Cloning:** The engine pre-scrolls tall documents to trigger lazy-loaded assets, then imports nodes into an inert `HTMLDocument` instance so custom elements and embedded scripts do not re-execute.
2. **Dependency DAG Construction:** Captured rects, typography runs, vector borders, and media nodes are compiled into a Directed Acyclic Graph (DAG). Child elements wait for their parent containers to be drawn before unlocking.
3. **Agent Task Scheduling:** Idle cursors pull unlocked tasks within the active viewport frontier, selecting specialized routines (`drawBox`, `drawText`, `drawLine`, `dragGuide`, `duplicate`, `editText`).
4. **Real-Time Audio Quantization:** Structural completions are mapped to pitch octaves based on element surface area and horizontal screen coordinates, quantized to a 104 BPM 16th-note grid.

---

## Browser Support & Notes

- **Supported Browsers:** Chrome 88+, Edge 88+, Firefox 90+, Safari 15.4+ (requires Shadow DOM, `Element.getAnimations()`, and Web Audio API).
- **Cross-Origin Media:** Cross-origin `<iframe>` elements are rendered as labeled placeholders; `<canvas>` and `<video>` elements capture still frames or muted playback where CORS policies permit.
- **Reduced Motion:** Respects `prefers-reduced-motion: reduce` when configured with page-load triggers.

---

<div align="center">

**Swarm © 2026 All rights reserved | Made with ❤️ by [BM Shifat](https://www.facebook.com/bmshifat0/)**

</div>