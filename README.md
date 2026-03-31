# VAG CODER

**Free, open-source OBD-II coding tool for VAG vehicles via Web Bluetooth.**

No credits. No subscriptions. Just plug in your OBDeleven (or any ELM327 BLE dongle), open the app in Chrome, and start coding.

![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Web%20Bluetooth-blue)
![Vehicles](https://img.shields.io/badge/vehicles-Audi%20B9-red)

---

## What Is This?

VAG CODER is a standalone HTML app that connects to your car's OBD-II port via Bluetooth and provides a full database of coding tweaks you can apply — without paying per-change credits. It runs entirely in your browser with zero server dependencies.

### Currently Supported

| Vehicle | Year | Tweaks | Status |
|---------|------|--------|--------|
| Audi RS5 (B9) US-Spec | 2019 | 76 | ✅ Full |

> **Want your car added?** See [CONTRIBUTING.md](CONTRIBUTING.md) — we need your help to expand the database.

### Features

- **Web Bluetooth connection** to OBDeleven / ELM327 BLE dongles
- **76+ community-verified tweaks** across 20 ECU modules
- **Compatibility badges** — instantly see what works on your exact car
- **Full ECU backup & restore** — reads long coding from all modules and saves to JSON
- **Raw UDS terminal** — send any ELM327 AT command or hex directly
- **Queue system** — batch multiple changes and apply them in sequence
- **Security access PINs** — pre-loaded for all known B9 modules
- **Searchable database** — find any tweak instantly
- **Zero dependencies** — single HTML file, no build step, no server

---

## Quick Start

### Requirements

- **Browser:** Chrome or Edge (desktop or Android). Web Bluetooth is NOT supported on iOS/Safari.
- **OBD Dongle:** OBDeleven, Vgate iCar Pro, or any ELM327-based BLE adapter
- **Vehicle:** Ignition ON, engine OFF, hood unlatched (required for 2019+ Audi)

### Usage

1. **Download** `index.html` from this repo (or clone it)
2. **Open** the file in Chrome/Edge
3. **Plug** your OBDeleven into the car's OBD-II port
4. **Click** "Scan & Connect" and select your dongle
5. **Go to Backup tab** and run a full ECU backup first
6. **Download** the backup JSON and save it somewhere safe
7. **Browse** the Modules tab, search for tweaks, add to queue
8. **Apply** your changes

### Important Safety Notes

- **Always back up before making any changes** — use the Backup tab
- **Hood must be unlatched** on 2019+ Audi for coding to take effect
- **Ignition ON, engine OFF** while coding
- **Some modules require security access** — PINs are shown on each module card
- **You are responsible** for all changes made to your vehicle

---

## Project Structure

```
vag-coder/
├── index.html              # The app (single-file, self-contained)
├── README.md               # This file
├── CONTRIBUTING.md          # How to add vehicles, tweaks, and contribute
├── LICENSE                  # MIT License
├── CHANGELOG.md            # Version history
├── docs/
│   ├── ARCHITECTURE.md     # How the app works internally
│   ├── ADDING_VEHICLES.md  # Step-by-step guide to add a new car
│   ├── CODING_GUIDE.md     # How VAG long coding and adaptations work
│   └── BLUETOOTH.md        # Web Bluetooth troubleshooting
├── vehicles/
│   ├── AUDI_B9_RS5_2019_US.md  # Vehicle-specific notes
│   └── TEMPLATE.md         # Template for new vehicle entries
└── .github/
    └── ISSUE_TEMPLATE/
        ├── new_vehicle.md   # Issue template: request new vehicle
        ├── new_tweak.md     # Issue template: submit a tweak
        └── bug_report.md    # Issue template: report a bug
```

---

## How It Works

The app uses the [Web Bluetooth API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Bluetooth_API) to connect to BLE OBD dongles. It sends ELM327 AT commands for initialization, then uses UDS (Unified Diagnostic Services) protocol to read/write ECU data.

The coding database is a JavaScript object embedded in the HTML file. Each tweak contains:
- Module address and name
- Human-readable coding instructions (Long Coding byte/bit or Adaptation channel)
- Risk level (low/medium/high)
- Compatibility tag for the target vehicle

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for the full technical breakdown.

---

## Contributing

We want this to cover every VAG vehicle on the road. See [CONTRIBUTING.md](CONTRIBUTING.md) for full details, but the short version:

- **Add a tweak you've verified** → Open a PR or issue with the module, coding path, and what car you tested on
- **Add a new vehicle** → Fork the repo, copy the vehicle template, fill in the module database
- **Fix a bug** → PRs welcome
- **Improve the UI** → PRs welcome

---

## Credits

- **-=Hot|Ice=-** and the [AudiWorld OBDeleven Coding Thread](https://www.audiworld.com/forums/audi-a5-s5-rs5-b9-220/***official***-b9-a5-s5-rs5-obdeleven-coding-thread-updated-8-6-2018-a-2954987/) community for the original B9 coding database
- **heymoe** for converting the thread to Google Docs
- **Audizine VCDS B9 Thread** for additional coding paths
- **gizliozellikacma.com** for the comprehensive tested coding list

---

## License

MIT — see [LICENSE](LICENSE) for details. Use at your own risk. This software comes with no warranty. You are solely responsible for any modifications made to your vehicle.

---

## Disclaimer

This tool sends real diagnostic commands to your vehicle's ECU modules. Incorrect coding can cause warning lights, disable safety features, or require dealer intervention to fix. Always back up your coding before making changes. The authors and contributors of this project accept no responsibility for any damage caused by using this tool.
