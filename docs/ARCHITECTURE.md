# Architecture

VAG CODER is a single-file HTML application with no external dependencies (other than Google Fonts). Everything — HTML structure, CSS styles, JavaScript logic, and the coding database — lives in `index.html`.

---

## Why a Single File?

- **Zero setup.** Download one file, open it in Chrome. No npm, no build tools, no server.
- **Works offline.** Once loaded, the app doesn't need internet (except for font loading, which falls back to system fonts).
- **Easy to fork.** Copy the file, edit the database, done.
- **Portable.** Save it on a USB stick, email it, put it on your phone's file manager.

---

## App Structure (inside index.html)

### 1. HTML (top)

The DOM structure is static — a warning overlay, header, nav bar, and empty content divs for each view:

```
#warning-screen  → Safety disclaimer (shown first)
#main-app
  .header         → Logo, car name, connection status
  .nav            → Dashboard | Modules | Backup | Terminal | Queue
  #view-home      → Connect card or dashboard with popular tweaks
  #view-modules   → Module list with search, or single-module detail
  #view-backup    → Full ECU backup/restore
  #view-terminal  → Raw ELM327/UDS command terminal
  #view-apply     → Queue review and apply log
```

### 2. CSS (in `<style>`)

All styles use CSS custom properties (variables) defined in `:root`. The design uses a dark charcoal/brass color scheme. No CSS frameworks.

### 3. JavaScript (in `<script>`)

The JS is organized into sections:

**BLE Configuration**
- `BLE_SERVICES`, `WRITE_CHARS`, `NOTIFY_CHARS` — UUID arrays for common OBD BLE dongles
- `ELM_INIT` — ELM327 initialization command sequence

**Coding Database**
- `COMPAT_LABELS` — compatibility badge definitions
- `RS5_MODULES` — the main database object, keyed by module address
- `POPULAR_IDS` — IDs of tweaks shown on the dashboard quick-add section

**State Management**
- Single `state` object holds all app state
- `render()` is called after every state change — it re-renders the active view

**BLE Connection**
- `connectBLE()` — scans for devices, connects GATT, discovers services/characteristics, runs ELM init
- `sendCommand(cmd, timeout)` — sends a command and returns a promise that resolves when `>` prompt is received
- `disconnect()` — cleanly disconnects GATT

**Backup/Restore**
- `BACKUP_MODULES` — list of all module addresses to scan
- `runBackup()` — iterates through modules, reads coding identifiers via UDS, saves to JSON
- `downloadBackup()` — creates a blob and triggers download
- `restoreFromBackup()` — reads backup JSON and writes coding back

**Rendering**
- `renderHome()`, `renderModules()`, `renderBackup()`, `renderTerminal()`, `renderApply()` — each rebuilds its view's innerHTML
- `renderTweakRow()` — compact tweak card for dashboard
- `renderTweakRowFull()` — expanded tweak card with coding instructions

---

## Data Flow

```
User clicks "Scan & Connect"
  → navigator.bluetooth.requestDevice()
  → device.gatt.connect()
  → discover services → find write/notify characteristics
  → start notifications (response listener)
  → send ELM_INIT commands sequentially
  → state.connected = true → render()

User adds tweak to queue
  → toggleQueue(tweakId) → state.queue.push(tweak) → render()

User clicks "Apply"
  → for each tweak in queue:
    → sendCommand("10 03")    // open diagnostic session
    → sendCommand("22 F1 87") // read current coding
    → sendCommand("2E F1 87 ...") // write new coding
    → sendCommand("10 01")    // close session
  → clear queue → render()
```

---

## Adding a New Vehicle

The database is a plain JS object. To add a vehicle:

1. Create a new constant (e.g. `A4_B9_MODULES`) or add entries to the existing structure
2. Each module needs: `name`, `address`, optional `security` PIN, and a `tweaks` array
3. Each tweak needs: `id`, `name`, `desc`, `module`, `coding`, `type`, `risk`, `compat`
4. Add new `compat` tags to `COMPAT_LABELS` if needed
5. Eventually the app will support a vehicle selector dropdown — for now, the database is per-file

See [ADDING_VEHICLES.md](ADDING_VEHICLES.md) for the step-by-step guide.

---

## Limitations

- **Web Bluetooth only** — no USB OBD support (would need a native app or WebSerial for wired adapters)
- **iOS not supported** — Apple blocks Web Bluetooth entirely
- **ELM327 protocol** — the app speaks ELM327 AT commands over BLE. Some advanced diagnostic operations (like flashing firmware) require KWP2000 or direct UDS that ELM327 can't do
- **No automatic coding** — the app sends raw UDS commands for "apply," but many tweaks require navigating OBDeleven's adaptation menu. The coding instructions are shown as a reference guide.
- **Single vehicle per file** — multi-vehicle support with a selector UI is a planned enhancement
