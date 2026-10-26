# Smart Extension Organizer (Manifest V3)

An advanced, ultra-compact, and high-performance Chrome Extension built with native ES6+ JavaScript, HTML5, and modern CSS to organize, categorize, and batch-manage all installed browser extensions.

---

## 🌟 Key Features & Architecture

### 1. Permanent Categories & Simultaneous View
* **Simultaneous Category Rendering:** Unlike old-school single-category dropdowns, all categories are displayed at once in the popup (e.g., *AI & Automation*, *Web Dev & Tools*, *Video & Audio*, *PDF & Documents*, *Security & Privacy*, *Productivity*, *Uncategorized*).
* **Non-Destructive Toggling:** Switching an extension in one category directly calls `chrome.management.setEnabled()` on only that extension without affecting other categories.
* **Split Active / Inactive Dynamic Accordions:**
  * **Active Zone (Always Visible):** Immediately displays currently enabled extensions.
  * **Inactive Drawer (Collapsible):** Displays disabled extensions with dimmed icons (`grayscale(0.85) opacity(0.55)`).
  * **Dynamic Movement:** Toggling an extension ON moves it into the Active Zone; toggling OFF moves it into the Inactive Drawer.
* **Category Master Micro-Switch:** Compact switch in each category header to pause (disable) or resume (enable) all extensions in that category at once (with immunity for Pinned extensions).
* **Smart Auto-Categorization on First Run:** Automatically scans names and descriptions for keywords upon initial install.

### 2. Cross-Category "Extension Sets" & Baseline Snapshot/Revert Engine
* **Extension Sets:** Cross-category batch profiles (e.g. *Coding Set*, *Deep Research Set*, *Media Focus*) that can include extensions cherry-picked across multiple categories.
* **The Baseline State Machine:**
  1. **Default Baseline:** Normal state when no Set is active.
  2. **Set Activation:** Captures a `baselineSnapshot` (`Record<extensionId, boolean>`) of all installed extensions, enables all extensions in the target Set, and disables non-pinned extensions.
  3. **Switching Between Sets:** Preserves original `baselineSnapshot` intact while applying the new Set.
  4. **Deactivating / Reverting:** Restores every extension back to its exact baseline state and clears the active Set.

### 3. Core Pinned / Shielded Extensions (Always-On Immunity)
* Mark essential extensions (e.g., Adblockers, Password Managers) as **Pinned (Shielded)**.
* **Immunity Rule:** Pinned extensions are marked with an amber shield badge (`🛡️`) and are completely immune when switching Extension Sets, toggling Category Master Switches OFF, or clicking the Global Freeze button.

### 4. Global "Zero-Bloat / Panic Freeze" Mode
* 1-Click Header button (`❄️ Freeze All`) snapshots all currently active non-pinned extensions, disables them to immediately free browser RAM, and restores them when clicked again.

### 5. Smart Website Auto-Rules (Background Service Worker)
* Automatically evaluates URL patterns via `chrome.tabs.onUpdated` and `chrome.tabs.onActivated`.
* Supports wildcard (`*://*.youtube.com/*`) and regex patterns.
* Actions: `enableWhen`, `disableWhen`, `enableOnly`, and `disableOnly`.
* **1-Click Domain Binding:** Popup quick-action binds the active tab's domain to categories or extensions with 1 click.

### 6. Sorting Engine & Drag-and-Drop Board (Management Studio)
* **Custom Drag & Drop Board:** Reorder categories top-to-bottom and drag extensions within or across categories with HTML5 drag-and-drop.
* **Visual Drop Indicators:** Subtle `.dragging` opacity/indigo glow and crisp emerald insertion lines (`.drop-target-before` / `.drop-target-after`).
* **Precision Quick-Move Controls:** One-click `⇈` (Move to #1 Top), `↑` (Move Up 1 spot), and `↓` (Move Down 1 spot) for rapid manual ranking.
* **Non-Destructive Sorting Modes:**
  * `Custom (Drag & Drop)` — User manual ranking.
  * `Name (A → Z)` / `Name (Z → A)` — Dynamic alphabetical sorting by custom alias or name.
  * `Most Used` — Ranked dynamically by enable/toggle count, with a 1-click `↺ Reset Rank` button.
  * `Dev & New First` — Prioritizes unpacked development extensions (`DEV` badge).
  * *Non-Destructive Memory:* Switching modes preserves the custom layout intact in `customExtensionOrder`.
* **Live Storage Sync:** Instant real-time reflection between Management Studio and the Popup view via `chrome.storage.onChanged`.

### 7. Obsidian Dark Theme & Ultra-Compact UI/UX
* **Strict Obsidian Dark Mode:** Deep carbon `#0B0D12` background, category cards `#131720`, elevated surfaces `#1A202C`, emerald `#10B981` active switches, and indigo `#6366F1` Set glow.
* **Ultra-Compact Micro-Switches:** Custom CSS switch (26px × 14px, 10px thumb).
* **Dark-Icon Visibility Fix:** Offscreen HTML5 `<canvas>` relative luminance analyzer automatically adds a subtle light pill backdrop to near-black icons so they never disappear on dark surfaces.
* **Layout Density Toggle:** 1-Click switch between **2-Column Mini-Cards** and **1-Column Dense Rows**.
* **Full-Page Options Studio:** Dedicated dashboard for Category & Set Builder, Website Auto-Rules Manager (with Rule URL Match Tester), Aliases & Pinned bulk editor, and 1-Click JSON Backup & Restore.
* **Chrome Side Panel:** Native Manifest V3 side panel support.

---

## 📁 File Structure

```
Smart Extension Organizer/
├── manifest.json
├── icons/
│   ├── icon-16.png
│   ├── icon-32.png
│   ├── icon-48.png
│   └── icon-128.png
└── src/
    ├── common/
    │   ├── storage.js          # Storage schema, defaults, auto-categorizer & CRUD
    │   ├── ext-controller.js   # chrome.management wrapper, mutex, Set state machine & canvas luminance
    │   └── rules-engine.js     # URL wildcard/regex matcher & rule evaluator
    ├── background/
    │   └── service-worker.js   # Lifecycle, rules evaluation on tab events, badge counter
    ├── popup/
    │   ├── popup.html
    │   ├── popup.css
    │   └── popup.js
    ├── sidepanel/
    │   ├── sidepanel.html
    │   └── sidepanel.css
    └── options/
        ├── options.html
        ├── options.css
        └── options.js
```

---

## 🚀 How to Install & Load in Chrome

1. Open Google Chrome.
2. Navigate to `chrome://extensions`.
3. Enable **Developer mode** toggle in the top-right corner.
4. Click **Load unpacked**.
5. Select the `C:\My Created Extensions\Smart Extension Organizer` folder.
6. The extension is now loaded and ready!
