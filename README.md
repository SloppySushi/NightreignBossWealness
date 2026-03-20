# Nightreign Boss Weakness Explorer

A single-page reference tool for Elden Ring: Nightreign that shows every boss's damage negations and status ailment resistances. Designed to be downloaded and used locally — no server required.

---

## Project Structure

```
index.html       — the entire app (HTML + CSS + JS, no build step)
boss.json        — all boss data: stats, expeditions, jail bosses, field bosses
icons/           — stat type icons (physical, slash, fire, etc.) and expedition icons
banners/         — expedition banner images shown on Night 3 section headers
```

The app loads `boss.json` via `fetch()` at startup. Both files must be served from the same directory (opening `index.html` directly as a `file://` URL may block the fetch in some browsers — use a local server or browser extension that allows local fetches).

---

## Features

### Five views (tabs at the top)

| Tab | What it shows |
|---|---|
| **Expedition View** | All three nights of a selected expedition, with collapsible sections per night |
| **Night 3 Filter** | Filter Night 3 boss(es) by choosing a Night 1 and/or Night 2 boss you encountered |
| **All Night 3 Bosses** | Every nightlord across all expeditions, filterable by expedition |
| **Jail Bosses** | All bosses that can appear in jail encounters |
| **Field Bosses** | All bosses that can appear as open-world field encounters |

### Simplified / Detailed toggle

- **Simplified** — colour-only heat map, stat icons in column headers, boss name shown in cell. Best for a quick visual scan from across the room.
- **Detailed** — shows exact numbers, phase names, and full stat labels. Best for precise lookups.

### Sortable columns

Click any stat column header to sort the table by that stat. Click again to reverse, click a third time to reset. Useful for answering questions like *"which Night 1 boss is weakest to Holy?"*

### Collapsible sections

Click the night banner row (Night 1 / Night 2 / Night 3 header) to collapse or expand that section. The banner stays visible so you can reopen it. Night 3 headers show a boss art image as a background when one is available.

### Tooltips

Hover over a stat icon to see its name. Hover over a boss cell in Simplified view to see the phase/form names.

---

## Data format (`boss.json`)

```jsonc
{
  "bosses": {
    // Each key is a unique boss identifier.
    // The value is an array of phase objects — one per phase/form.
    "tricephalos": [
      { "name": "Gladius", "physical": "0%", ..., "poison": 542, ... }
    ],
    // Multi-phase bosses have multiple entries in the array:
    "night aspect ph1": [
      { "name": "Heolstor Phase 1", "physical": "0%", ... }
    ]
  },

  "expedition": {
    // Each key is an expedition name.
    // Night arrays may contain strings (single boss) or arrays (packs that spawn together).
    "tricephalos": {
      "first night":  [["demi-human queen", "demi-human swordmaster"], "bell bearing"],
      "second night": ["fell omen", ["tree sentinel", "royal cavalry"]],
      "third night":  ["tricephalos"],
      "icon":   "icons/tricephalos-expedition-...-min.png",
      "banner": "banners/gladius-everdark-4-nightreign-full.jpg"
    }
  },

  "icon": {
    // Stat-type icons used in Simplified view column headers
    "physical": "icons/standard-damage-...",
    "slash":    "icons/slash-damage-...",
    ...
  },

  "jail bosses":  ["ancient dragon", "banished knights", ...],
  "field bosses": ["ancestor spirit", "bell bearing", ...]
}
```

**Stat values:**
- Resistance/weakness columns (`physical` through `holy`) are percentage strings. Negative = weakness (boss takes more damage), positive = resistance (boss takes less). `0%` is neutral.
- Ailment threshold columns (`poison` through `madness`) are integers. Lower = procs faster. `null` = immune.

---

## Running locally

The simplest approach is Python's built-in HTTP server:

```bash
# Python 3
python3 -m http.server 8000
# then open http://localhost:8000
```

Or use any static file server (VS Code Live Server, `npx serve`, Caddy, nginx, etc.).

---

## Offline use

Once loaded in the browser, the page works without a network connection (fonts may fall back to system fonts). Images (icons and banners) will show their text fallback if unreachable — this is intentional and does not affect any data.

---

## Credits

Thanks to **katya** and **boost** on Discord for support and ideas.  
Data sourced from community research and the [Elden Ring Nightreign wiki](https://eldenringnightreign.wiki.fextralife.com).
