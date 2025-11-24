# Ad Builder / Ad Layer – Custom Scripts

This repo contains small, dependency-free **Custom Scripts** for Ad Builder / Ad Layer.

All scripts are:

- Written in **plain ES5** (no `let`, `const`, arrow functions, or template literals).
- Designed for the **Ad Layer runtime**, using `_this` and vanilla DOM.
- Safe to drop into either:
  - a specific object (text box, image, etc.), or  
  - the **stage** (`adl--stage`) for global coordination.
- Structured with a clear **config block at the top** of each file and proper **cleanup** via `_this.at('destroy', …)`.

> ℹ️ Stage DOM id is always `adl--stage`.  
> To get the inner text node of a text object, each script uses:  
> `var textEl = _this[0].querySelector('.adl-text');`

---

## Usage

1. Open a script file in this repo.
2. Copy the entire contents.
3. In Ad Builder:
   - Select either the **stage** or a **specific object**, depending on the script.
   - Open the **Custom script** panel.
   - Paste the script and save.
4. Follow the **“Setup” comment block at the top of the script** (e.g. set target ids/classes on objects).

---

## Scripts

### `targetText.js`

**Purpose:**  
Treat the current text object as the **source**, and mirror its text into one or more **target text objects** on the stage, using their DOM ids.

**Key ideas:**

- Uses `_this[0].querySelector('.adl-text')` to read the current object’s text.
- Copies that text into each target object’s `.adl-text`.
- Optional live sync using `MutationObserver` when available.
- Registers cleanup handlers with `_this.at('destroy', …)` to disconnect observers.

**Where to attach:**  
Attach `targetText.js` to the **source text object** whose text should drive the others.

**How to configure:**

Edit the `CFG` block at the top of the script:

- `CFG.targetIds` – array of object ids to mirror text into.
- `CFG.syncOnInit` – copy text once on load.
- `CFG.syncOnMutation` – keep targets in sync when the source changes.

---

## Conventions

All scripts follow these conventions:

- **ES5 only:** use `var` and `function () {}`.
- **Minimal dependencies:** vanilla DOM + Ad Layer’s `_this` + `$` when available.
- **Safe selectors:** target elements via `_this[0]` or `#adl--stage`, not global query selectors tied to internal ids.
- **Lifecycle-safe:** if a script adds an interval, animation, or observer, it registers a cleanup handler:
  ```js
  _this.at('destroy', function () {
    // clear timers, observers, animations here
  });
