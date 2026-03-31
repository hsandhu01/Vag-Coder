# VAG Coding Guide

A quick primer on how coding works in VW/Audi/Skoda/SEAT vehicles, for people who are new to OBDeleven or VCDS.

---

## What Is "Coding"?

Every modern VAG vehicle has dozens of electronic control units (ECUs) — small computers that manage everything from headlights to the transmission. Each ECU stores configuration data that controls its behavior. "Coding" means reading and changing that configuration data.

There are two types of changes you can make:

### Long Coding

Long Coding is a block of binary data stored in an ECU. It's organized as a series of **bytes** (numbered 0, 1, 2, ...), and each byte has 8 **bits** (numbered 0-7). Each bit is a switch: checked (1) or unchecked (0).

Example: To enable needle sweep on startup:
- Module: `17 - Instrument Cluster`
- Byte: `1`
- Bit: `0`
- Change: `0 → 1` (uncheck → check)

In OBDeleven, you navigate to the module → Coding → tap the byte/bit toggle → save.

Some Long Coding fields have named options instead of raw byte/bit values:
- Field: `sl_at_drl`
- Change: `not_active → active`

### Adaptation

Adaptations are named configuration channels within an ECU. They typically have human-readable names and value options.

Example: To disable start/stop via A/C:
- Module: `08 - Air Conditioning`
- Adaptation channel: `A/C comfort param for start/stop function`
- Old value: `5`
- New value: `0`

In OBDeleven, you navigate to the module → Adaptation → search for the channel → change the value → save.

---

## Security Access

Some modules require a security PIN before you can make changes. The ECU will reject your coding attempt without it.

Known B9 Audi PINs:
| Module | PIN |
|--------|-----|
| 09 - Central Electronics | 20113 |
| 13 - Adaptive Cruise Control | 20103 |
| A5 - Front Sensors / ADAS | 20103 |
| 46 - Central Convenience | 20103 |
| 03 - ABS Brakes | 40168 |
| 6D - Trunk Electric | 12345 |
| 57 - TV Tuner | 20103 |
| CA - Sunroof | 20103 |

In OBDeleven, you enter the security code before accessing Long Coding or Adaptation for that module.

---

## Important Rules

1. **Always back up first.** Before changing anything, read and save the current coding for every module you plan to modify.

2. **Hood must be open on 2019+ Audis.** The car's firewall blocks coding changes when the hood is closed. Just pull the interior release — you don't need to prop it fully open.

3. **Ignition ON, engine OFF.** Turn the key to position 2 (or press start without brake) but don't start the engine.

4. **One change at a time.** Make a change, verify it works, then move to the next. If something breaks and you changed 10 things at once, you won't know which one caused it.

5. **Cycle ignition after coding.** Many changes don't take effect until you turn the car off and back on. Some (like MMI changes) require an MMI reboot: press and hold Nav/Map + Radio + center knob simultaneously.

6. **Some changes are irreversible without a dealer.** This is rare for the tweaks in our database (which are all configuration-level), but be aware that flashing firmware or modifying calibration data can require ODIS to fix.

---

## UDS Protocol Basics

Under the hood, all coding is done via UDS (Unified Diagnostic Services), a standardized protocol that works over CAN bus. The ELM327 dongle translates between BLE and CAN.

Common UDS services used in coding:

| Service ID | Name | Purpose |
|-----------|------|---------|
| `10` | Diagnostic Session Control | Open/close diagnostic sessions |
| `22` | Read Data By Identifier | Read a coding value |
| `2E` | Write Data By Identifier | Write a coding value |
| `27` | Security Access | Authenticate with a PIN |
| `31` | Routine Control | Run ECU routines |

In the VAG CODER terminal, you can send these as raw hex:
- `10 03` — open extended diagnostic session
- `22 F1 87` — read VW coding identifier
- `10 01` — return to default session

---

## Module Addresses

Every ECU has a hex address. Common B9 Audi modules:

| Address | Module |
|---------|--------|
| 01 | Engine ECU |
| 02 | Transmission |
| 03 | ABS / ESC |
| 08 | HVAC |
| 09 | Central Electronics |
| 13 | Adaptive Cruise Control |
| 16 | Steering Column |
| 17 | Instrument Cluster |
| 19 | CAN Gateway |
| 36 | Driver Seat |
| 44 | Steering Assist |
| 46 | Central Convenience |
| 5F | Infotainment / MMI |
| 82 | Head-Up Display |
| A5 | Front Sensors / ADAS |
| A9 | Soundaktor |
| CA | Sunroof |

---

## Risk Levels

Our database tags every tweak with a risk level:

- **Low** — cosmetic or convenience changes. Worst case: a setting you don't like that you can revert. Examples: needle sweep, DRL behavior, comfort blinker count.

- **Medium** — affects driving behavior or multi-module coding that's harder to revert. Examples: exhaust flap, steering response, lane assist activation (requires changes across 5 modules).

- **High** — touches safety systems or could require dealer intervention if done wrong. Examples: ABS configuration, airbag module changes. Our database currently has no high-risk tweaks.
