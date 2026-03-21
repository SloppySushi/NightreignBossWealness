# Nightreign Boss Weakness Explorer

A single-page reference tool for **Elden Ring: Nightreign** that shows every boss's damage negations and status ailment resistances.

---

## Project Structure

```
index.html       — the entire app (HTML + CSS + JS, no build step)
boss.json        — all boss data: stats, expeditions, jail bosses, field bosses, castle bosses
icons/           — stat type icons (physical, slash, fire, etc.) and expedition icons
banners/         — expedition banner images shown on Night 3 section headers
```

The app loads `boss.json` via `fetch()` at startup from:
```
https://raw.githubusercontent.com/SloppySushi/NightreignBossWealness/refs/heads/main/boss.json
```

---

## Features

### Six views (tabs at the top)

| Tab | What it shows |
|---|---|
| **Expedition View** | All three nights of a selected expedition, with collapsible sections per night |
| **Night 3 Filter** | Filter Night 3 boss(es) by choosing a Night 1 and/or Night 2 boss you encountered |
| **All Night 3 Bosses** | Every nightlord across all expeditions, filterable by expedition |
| **Jail Bosses** | Bosses that appear in jail encounters, split by Weak Reward and Strong Reward |
| **Field Bosses** | Open-world field encounter bosses, split by Normal and Major |
| **Castle Bosses** | Castle bosses split by Area, Basement, and Top |

On **mobile (< 700 px wide)** the tab buttons are replaced by a compact dropdown select so the header never overflows.

### Simplified / Detailed toggle

- **Simplified** — colour-only heat map; stat icons in column headers; boss icon shown in cell. Best for a quick visual scan.
- **Detailed** — shows exact numbers, phase names, and full stat labels. Best for precise lookups.

### Sortable columns

Click any stat column header to sort the table by that stat. Click again to reverse; click a third time to reset.  
Useful for questions like *"which Night 1 boss is weakest to Holy?"*

### Collapsible sections

Click any section header row (Night 1 / Night 2 / Night 3, or sub-sections like Weak Reward / Strong Reward) to collapse or expand it. Nested collapse is fully supported — collapsing a parent hides all of its children regardless of their own state.

### Night 3 banner art

Night 3 section headers display the boss's artwork as a background when a banner image is available in `banners/`.

### Expedition button icons

The expedition selector buttons show each nightlord's icon from `icons/` alongside the name.

### Tooltips

Hover over a stat icon header to see its name. Hover over a boss cell in Simplified view to see all phase/form names.

---

## boss.json structure

```jsonc
{
  "bosses": {
    "<boss-key>": [
      // One object per phase/form (has a "name" field):
      { "name": "phase name", "physical": "0%", "slash": "-10%", /* … */ "poison": 154, /* … */ },
      // Last entry (no "name") holds visual assets:
      { "icon": "icons/…", "banner": "banners/…" }
    ]
  },

  "expedition": {
    "<expedition-key>": {
      "first night":  ["boss-key", …],
      "second night": ["boss-key", …],
      "third night":  ["boss-key"]
    }
  },

  "icon": {
    "physical": "icons/…",
    "slash":    "icons/…"
    // … one entry per stat column
  },

  "jail bosses": {
    "weak reward":   ["boss-key", …],
    "strong reward": ["boss-key", …]
  },

  "field bosses": {
    "normal": ["boss-key", …],
    "major":  ["boss-key", …]
  },

  "catle": {
    "area":     ["boss-key", …],
    "basement": ["boss-key", …],
    "top":      ["boss-key", …]
  }
}
```

**Stat value meanings:**
- **Resistance columns** (`physical` → `holy`) — percentage strings. Negative = weakness (boss takes *more* damage); positive = resistance (boss takes *less*). `"0%"` is neutral.
- **Ailment columns** (`poison` → `madness`) — integers. Lower = procs faster. `null` = immune.

> **Note:** The JSON key for castle data is currently `"catle"` (typo). The app handles both `"catle"` and `"castle bosses"` transparently.

---

## Credits

Thanks to **katya** and **boost** on Discord for support and ideas.  
Data sourced from community research and the [Elden Ring Nightreign wiki](https://eldenringnightreign.wiki.fextralife.com).