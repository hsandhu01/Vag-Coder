# Contributing to VAG CODER

Thanks for wanting to help. The goal is to build a free, community-maintained coding database for every VAG vehicle. Here's how to contribute.

---

## Ways to Contribute

### 1. Submit a Verified Tweak

If you've successfully coded something on your car, we want it in the database.

**What we need:**

- **Your vehicle:** Year, model, market (US/EU/UK), trim level
- **Module address and name** (e.g. `09 - Central Electronics`)
- **Coding type:** Long Coding or Adaptation
- **Exact coding path:** The field name, byte/bit, old value, new value
- **What it does:** Clear description of the change
- **Risk level:** Low (cosmetic/convenience), Medium (affects driving behavior), High (safety-related)
- **Confirmation:** Did it work? Any side effects?

**How to submit:**

Option A: Open an issue using the "New Tweak" template
Option B: Fork → edit → PR (see below)

---

### 2. Add a New Vehicle

This is the highest-impact contribution. If you own a VAG vehicle that isn't in the database yet, you can add it.

**Step-by-step:**

1. **Fork** this repo
2. **Copy** `vehicles/TEMPLATE.md` to `vehicles/YOUR_CAR.md` (e.g. `AUDI_B9_A4_2018_US.md`)
3. **Fill in** every tweak you've verified on your car
4. **Edit** `index.html` — add your vehicle's module database to the `RS5_MODULES` object (we'll rename this to a generic name as more vehicles are added)
5. **Add** a compatibility tag for each tweak (`compat: "b9"` etc.)
6. **Submit** a PR with the title: `[Vehicle] Add YEAR MAKE MODEL`

See [docs/ADDING_VEHICLES.md](docs/ADDING_VEHICLES.md) for the full guide with code examples.

---

### 3. Verify Existing Tweaks on Your Car

Even if your exact model is already in the database, compatibility can vary by:
- Model year (B9 vs B9.5)
- Market (US vs EU vs UK vs ROW)
- Trim level (Premium, Premium Plus, Prestige)
- Software version (MIB2 vs MIB3)
- Factory options (ACC, HUD, B&O, Matrix LED, etc.)

If you try a tweak and it works (or doesn't), open an issue or PR to update the compatibility tags.

---

### 4. Fix Bugs / Improve the App

The app is a single `index.html` file. PRs welcome for:
- BLE connection improvements
- UI/UX enhancements
- New features (e.g. batch import/export, coding diff view)
- Accessibility improvements
- Mobile layout fixes

---

## Tweak Database Format

Each tweak is a JavaScript object inside the `RS5_MODULES` constant in `index.html`:

```javascript
{
  id: "unique_snake_case_id",        // Unique across all tweaks
  name: "Human Readable Name",       // Short, clear title
  desc: "What this does and any caveats.",  // 1-2 sentences
  module: "09",                      // Module address
  coding: "Long Coding: field_name → new_value (from old_value)",  // Exact instructions
  type: "long",                      // "long" = Long Coding, "adapt" = Adaptation
  risk: "low",                       // "low", "medium", or "high"
  compat: "b9",                      // Compatibility tag (see below)
}
```

### Compatibility Tags

| Tag | Meaning | Badge Color |
|-----|---------|-------------|
| `b9` | Confirmed on 2019 Audi B9 RS5 US | Green |
| `b9.5` | B9.5 facelift only (2020+) | Red (greyed out) |
| `mib3` | MIB3 infotainment only (2020+) | Red (greyed out) |
| `req:hud` | Requires Head-Up Display option | Yellow |
| `req:bo` | Requires Bang & Olufsen audio | Yellow |
| `req:acc` | Requires Adaptive Cruise Control | Yellow |
| `req:massage` | Requires massage seats | Yellow |
| `req:vent` | Requires ventilated seats | Yellow |
| `req:360cam` | Requires 360° camera system | Yellow |
| `req:sunroof` | Requires sunroof | Yellow |
| `req:hardware` | Requires physical hardware retrofit | Orange |

When adding new vehicles, create new compat tags as needed (e.g. `b8`, `c7`, `mqb`, etc.) and add them to the `COMPAT_LABELS` constant.

---

## Coding Instructions Format

Be as specific as possible. Include:

**For Long Coding:**
```
Long Coding: field_name → new_value (from old_value)
```
Or if using byte/bit:
```
Long Coding: Byte X, Bit Y → 1 (check) or 0 (uncheck)
```

**For Adaptations:**
```
Adaptation: [IDE_number] Channel name → new_value (from old_value)
```

**If security access is needed:**
```
Security: PIN_CODE → then Long Coding/Adaptation: ...
```

**If multiple modules are involved:**
```
Module 09: field → value. Also Module 17: Byte X, Bit Y → 1
```

---

## Pull Request Guidelines

1. **One vehicle or feature per PR.** Don't mix a new vehicle with bug fixes.
2. **Test your changes.** Open `index.html` in Chrome and make sure the app loads, search works, and your new tweaks appear.
3. **Use clear commit messages:**
   - `feat: add 2018 Audi A4 B9 US vehicle database`
   - `tweak: add exhaust flap coding for B9 S4`
   - `fix: BLE reconnect after disconnect`
   - `docs: update Bluetooth troubleshooting guide`
4. **Include your vehicle info** in the PR description so reviewers know what was tested.
5. **Don't break existing vehicles.** If you're restructuring the code, make sure all existing tweaks still render correctly.

---

## Issue Guidelines

- **Search first** — someone may have already reported your issue or submitted your tweak
- **Use the templates** — they exist to make sure we get the info we need
- **Be specific** — "Mirror fold doesn't work" is less helpful than "Mirror fold via key fob (Module 46, mirror_retraction_at_comfort_close) has no effect on 2020 S5 Sportback US Premium Plus"

---

## Code of Conduct

- Be helpful and respectful
- Don't submit unverified tweaks as verified
- Don't submit tweaks that could damage vehicles without clear risk warnings
- Credit the original source when adding tweaks from forums or other communities

---

## Questions?

Open a Discussion thread or an issue. We're here to help you get your car's database added.
