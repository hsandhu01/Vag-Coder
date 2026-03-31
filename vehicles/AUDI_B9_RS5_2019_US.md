# 2019 Audi RS5 (B9) — US Spec

## Vehicle Info

| Field | Value |
|-------|-------|
| Platform | B9 (MLB Evo) |
| Engine | EA839 2.9L V6 Twin-Turbo |
| Transmission | ZF8 (AL552) |
| Infotainment | MIB2 |
| Model Years | 2018-2019 (pre-facelift) |
| Market | United States |

## Trims Available (US)

- **Premium Plus** — no HUD, may lack Driver Assistance Package
- **Prestige** — HUD, B&O audio, Driver Assistance Package, 360° cameras

## Module Count

20 modules with 76 tweaks in the database.

## Security Access PINs

| Module | Address | PIN |
|--------|---------|-----|
| Central Electronics | 09 | 20113 |
| Central Convenience | 46 | 20103 |
| ABS Brakes | 03 | 40168 |
| Adaptive Cruise Control | 13 | 20103 |
| Front Sensors / ADAS | A5 | 20103 |
| Trunk Electric | 6D | 12345 |
| TV Tuner | 57 | 20103 |
| Sunroof | CA | 20103 |

## Important Notes

- **Hood must be unlatched** for coding changes to take effect (2019+ requirement)
- **MIB2 infotainment** — wireless CarPlay is NOT possible (requires MIB3 hardware in 2020+ facelift)
- **B9 vs B9.5 coding differences:** Some field names changed between B9 and B9.5. For example, "Lock with Ignition On" uses `automatic_unlock_nar` on B9 but `central_locking_system_lock_unlock_at_engine_running` on B9.5
- **DRL dimming with turn signals** does not work with LED Matrix headlights
- **Headlamp wash disable** has been reported as NOT found on some 2019 RS5 units — may depend on build date
- **Auto unlock on approach** confirmed working on '19 RS5 but works best from front/side. Approaching from rear rarely triggers it. Low battery key fobs may trigger false alarm (error B131D29)
- **RS5 heartbeat sound** — the welcome sound is the Audi heartbeat on RS models, vs a harp on A/S models

## Option-Dependent Tweaks

These tweaks only work if your specific car has the option:

| Tweak | Requires |
|-------|----------|
| Sport Display in HUD | Head-Up Display (Prestige) |
| B&O Speaker Light Bar Off | Bang & Olufsen audio |
| Seat Ventilation Stage 3 | Ventilated seats |
| Massage Seat Duration | Massage seats |
| ACC Default Distance | Adaptive Cruise Control |
| 360° OPS View | 360° camera system |
| Sunroof Slide Open | Sunroof/moonroof |
| Auto Hold Button | Requires hardware retrofit |

## Community Sources

- [AudiWorld B9 OBDeleven Thread](https://www.audiworld.com/forums/audi-a5-s5-rs5-b9-220/***official***-b9-a5-s5-rs5-obdeleven-coding-thread-updated-8-6-2018-a-2954987/)
- [Audizine B9.5 OBDeleven Thread](https://www.audizine.com/forum/showthread.php/959007-OBDEleven-B9-5)
- [Audizine VCDS B9 Mod List](https://www.audizine.com/forum/showthread.php/958772-**-VCDS-Mod-List-for-B9-**)

## Contributors

- Initial database compiled from community sources and verified on a 2019 RS5 Sportback US-spec
