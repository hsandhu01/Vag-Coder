# Web Bluetooth Troubleshooting

Common issues connecting to your OBD dongle and how to fix them.

---

## "Web Bluetooth not supported"

**Cause:** Your browser doesn't support the Web Bluetooth API.

**Fix:**
- Use **Chrome** or **Edge** on desktop (Windows, macOS, Linux) or Android
- iOS Safari, Firefox, and Brave do NOT support Web Bluetooth
- If running inside an iframe (like Claude.ai's artifact viewer), download the HTML file and open it directly — iframes block Bluetooth permissions

---

## "Access to the feature bluetooth is disallowed by permissions policy"

**Cause:** The page is running inside a sandboxed iframe.

**Fix:** Download `index.html` and open it directly from your filesystem in Chrome. Don't open it inside another app's webview.

---

## Scan finds no devices

**Possible causes:**
1. **Dongle not powered.** The OBDeleven needs the car's ignition to be ON (accessory mode or run mode) to get power from the OBD-II port.
2. **Already paired to another device.** If your phone's OBDeleven app is connected, it's holding the BLE connection. Close the app or disconnect from your phone's Bluetooth settings first.
3. **Dongle not in range.** BLE range is typically 5-10 meters. Be near the car.
4. **Wrong dongle type.** Some older ELM327 adapters are Bluetooth Classic (not BLE). Web Bluetooth only works with BLE (Bluetooth Low Energy). The OBDeleven Gen 1 and Gen 2 are both BLE.

---

## Connects but "Could not find write characteristic"

**Cause:** The app couldn't discover the GATT service/characteristic UUIDs for your specific dongle.

**Fix:**
1. Disconnect and try again — sometimes the GATT discovery fails on first attempt
2. If it persists, your dongle may use different UUIDs than the ones we scan for. Open a GitHub issue with your dongle model and we can add support

**Currently supported UUID sets:**
- `000018f0` series (common OBDeleven)
- `0000fff0` series (generic ELM327 BLE)
- `e7810a71` series (alternative ELM327)
- `0000ffe0` series (Vgate and others)

---

## Commands return empty or "NO DATA"

**Possible causes:**
1. **Wrong protocol.** The app initializes with `ATSP6` (ISO 15765-4 CAN). If your car uses a different protocol, try `ATSP0` (auto-detect) in the terminal.
2. **Module not responding.** Not all modules respond to all commands. Some require the correct diagnostic session to be open first.
3. **Timeout too short.** Some modules take longer to respond. Try the command in the Terminal with a manual wait.

---

## Connection drops randomly

**Possible causes:**
1. **BLE interference.** Move closer to the car / dongle.
2. **Ignition state changed.** If the car goes to sleep or the ignition turns off, the dongle loses power.
3. **Browser tab backgrounded.** Chrome may throttle or disconnect BLE for background tabs. Keep the tab active.

---

## Terminal commands work but "Apply" doesn't change anything

The "Apply" button sends raw UDS commands which work for some types of long coding. However, many tweaks (especially Adaptations) require navigating the OBDeleven app's adaptation menu rather than raw UDS writes.

**Recommended workflow:**
1. Use VAG CODER to **find and understand** what you want to change
2. Use the **coding instructions** shown for each tweak as a step-by-step guide
3. For Adaptations, use the OBDeleven app's adaptation interface (free — no credits needed for adaptations)
4. For Long Coding byte/bit changes, you can either use VAG CODER's terminal or OBDeleven's coding screen

---

## Android-specific issues

- **Location permission required.** Android requires location permission for BLE scanning. Chrome will prompt you — accept it.
- **Nearby devices permission (Android 12+).** You may also need to grant "Nearby devices" permission in Android settings.

---

## Still stuck?

Open a GitHub issue with:
- Your browser and version
- Your OS
- Your OBD dongle model
- The exact error message
- Screenshots if possible
