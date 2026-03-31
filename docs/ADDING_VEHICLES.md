# Adding a New Vehicle

This guide walks through adding a new car to VAG CODER, from gathering tweaks to submitting a PR.

---

## Step 1: Gather Your Tweaks

Before writing any code, collect all the coding changes for your vehicle. Sources:

- **AudiWorld forums** — search for your model + "OBDeleven coding thread"
- **Audizine forums** — search for your model + "VCDS mod list"
- **Ross-Tech VCDS wiki** — https://wiki.ross-tech.com
- **OBDeleven One-Click Apps** — the app descriptions list the module/coding path
- **Your own testing** — if you've coded something yourself, document it

For each tweak, record:

| Field | Example |
|-------|---------|
| Module address | `09` |
| Module name | `Central Electronics` |
| Coding type | Long Coding or Adaptation |
| Field/byte/bit | `sl_at_drl` or `Byte 6, Bit 4` |
| Old value | `not_active` |
| New value | `active` |
| What it does | Tail lights stay on with DRLs |
| Risk | Low / Medium / High |
| Security PIN needed? | No / Yes (which PIN) |
| Tested on | 2018 Audi A4 2.0T US Premium Plus |

---

## Step 2: Create Vehicle Documentation

Copy `vehicles/TEMPLATE.md` to a new file:

```
vehicles/AUDI_B9_A4_2018_US.md
```

Fill in all sections: vehicle info, confirmed modules, tweak list, known issues, and contributor info.

---

## Step 3: Add to the Codebase

Open `index.html` and locate the `RS5_MODULES` constant (search for `const RS5_MODULES`).

**Option A: Add to existing database**

If your car shares a platform (e.g. another B9 Audi), you can add tweaks to the existing modules and use a new `compat` tag:

```javascript
// In COMPAT_LABELS, add:
"b9_a4": { label: "A4 B9 ✓", color: "#4ade80", bg: "rgba(74,222,128,0.12)" },

// In a module's tweaks array, add:
{
  id: "your_tweak_id",
  name: "Your Tweak Name",
  desc: "What it does",
  module: "09",
  coding: "Long Coding: field_name → new_value",
  type: "long",
  risk: "low",
  compat: "b9_a4"
},
```

**Option B: Create a separate vehicle database**

For a completely different platform (e.g. MQB Golf), create a new modules constant:

```javascript
const GOLF_MK7_MODULES = {
  "09": {
    name: "Central Electronics", address: "09",
    tweaks: [
      // your tweaks here
    ]
  },
  // more modules...
};
```

Then update the app to support vehicle selection (or submit it as a separate HTML file for now).

---

## Step 4: Compatibility Tags

Every tweak must have a `compat` field. Create tags that are specific enough to be useful:

```javascript
// Good — tells you exactly what cars
"b9": "2019 Audi B9 RS5"
"b9_a4": "2017-2019 Audi B9 A4"
"b9_all": "All B9 platform cars"
"mqb_golf7": "VW Golf MK7/7.5"

// Bad — too vague
"audi": "Some Audi"
"vag": "Some VAG car"
```

Add your tags to `COMPAT_LABELS`:

```javascript
const COMPAT_LABELS = {
  // existing...
  "b9_a4": { label: "A4 B9 ✓", color: "#4ade80", bg: "rgba(74,222,128,0.12)" },
  "b9_a4_pp": { label: "A4 PREM PLUS", color: "#fbbf24", bg: "rgba(251,191,36,0.12)" },
};
```

---

## Step 5: Test Locally

1. Open `index.html` in Chrome
2. Verify your vehicle's modules appear in the Modules tab
3. Search for a few of your tweaks — make sure they render correctly
4. Check that compatibility badges display properly
5. If possible, connect to your car and verify the BLE flow works

---

## Step 6: Submit a PR

1. Fork this repo
2. Create a branch: `git checkout -b add-2018-audi-a4-b9`
3. Commit your changes with a clear message
4. Push and open a PR with:
   - Vehicle: Year, Make, Model, Market, Trim
   - Number of tweaks added
   - Which tweaks you personally verified vs sourced from forums
   - Any known issues or caveats

---

## Tips

- **Start small.** Even 5-10 verified tweaks for a new vehicle is valuable. Others will add more.
- **Be honest about testing.** Mark tweaks as forum-sourced if you haven't tested them yourself.
- **Include the OBDeleven field names AND the byte/bit numbers** when possible — different tools show different interfaces.
- **Note your software version.** MIB2 vs MIB3, firmware version if known.
- **Don't include ECU tuning.** This project is for configuration coding (convenience, lighting, comfort features), not engine/transmission performance tunes.
