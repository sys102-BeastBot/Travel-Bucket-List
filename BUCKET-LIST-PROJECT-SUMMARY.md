# Travel Bucket List — Project Summary
**Last Updated:** April 26, 2026  
**Project Owner:** Sherman (sys102)  
**Live URL:** https://sys102-beastbot.github.io/Travel-Bucket-List

---

## What We Built

A fully interactive, single-file HTML travel bucket list web app with 547 destinations spanning every country on Earth. No backend, no database — everything runs in the browser with localStorage persistence. Each visitor gets their own independent copy to customize.

---

## File Locations

| File | Path |
|------|------|
| Canonical workspace file | `\\wsl.localhost\Ubuntu\home\sys102\.openclaw\workspace\bucket-list-project\index.html` |
| GitHub repo | `https://github.com/sys102-BeastBot/Travel-Bucket-List` |
| Live site | `https://sys102-beastbot.github.io/Travel-Bucket-List` |

**Important:** The workspace `index.html` is the source of truth. Always edit there, then push to GitHub to update the live site.

---

## Destination Database

- **547 total destinations** (after systematic global audit)
- **Inclusion rule:** hist ≥ 7 OR nature ≥ 7 OR cult ≥ 7
- **ID range:** 1–547 (id:355 Bagan duplicate removed, Kunya-Urgench retained at 355)
- **Coverage:** Every continent, 130+ countries audited via Claude Sonnet API calls across 31 geographic regions

### How Destinations Were Built
1. **Manual phase (ids 1–205):** Built iteratively in chat, country by country, filling gaps as they were identified
2. **Automated audit (ids 206–547):** Python script (`pending/from-desktop/bucket-list-audit.py`) ran overnight via Claude Code, calling Claude Sonnet API once per geographic region, cross-referencing against current list to avoid duplicates. Found 343 new qualifying destinations.
3. **Deduplication:** Claude Code ran a duplicate check — found 1 duplicate (Bagan Archaeological Zone id:355 vs existing id:28/48), removed it.

### Scoring System (1–10 each)
| Metric | Key | Notes |
|--------|-----|-------|
| Historical significance | `hist` | Included in composite score |
| Natural beauty | `nature` | Included in composite score |
| Logistical difficulty | `logist` | Included in composite score |
| Physical difficulty | `phys` | Included in composite score |
| Cultural immersion | `cult` | Included in composite score |
| Anti-overtourism | `antiOT` | Included in composite score |
| Safety | `safety` | **Display only — excluded from composite score** |

**Composite score** = weighted average of 6 metrics (not safety), scaled 0–100. Weights adjustable via UI.

### Wonders Tracking
- New 7 Wonders of the World (2007)
- Ancient 7 Wonders
- Natural 7 Wonders
- Filterable via toolbar dropdown

---

## Features

### Toolbar
- **Search** — searches name, country, notes fields
- **Sort** — by composite score, any individual metric, or A–Z
- **Wonders filter** — Any Wonder / New 7 / Ancient / Natural
- **Country filter** — dynamically populated from all 547 destinations
- **Continent filter** — 7 continents
- **⚙ Weights** — opens weights panel to rebalance scoring
- **✓ Saved indicator** — flashes on every auto-save
- **↓ Export** — downloads JSON backup of personal data
- **↑ Import** — restores from JSON backup (smart merge)
- **↺ Reset** — clears localStorage, reloads fresh
- **+ Add** — opens modal to add custom destinations

### Weights Panel
- 6 number inputs (1–10) for each scoring metric
- Replaces sliders (which were visually broken)
- Re-ranks list in real time on input

### Filter Bar
- Status chips: All / Wishlist / Planned / Visited / ⭐ Starred
- Top 10 trip group chips (by group size)

### Table View (default)
- Frozen header row
- Scrollable body
- Columns: ★ # | Destination | Hist | Nature | Logist | Phys | Cult | Anti | Score | Safety | Status
- ★ click toggles star (saves to localStorage)
- Row click opens edit modal
- Trip group colored dot — click to filter by group
- Continent color tag
- Wonder badges
- Safety badge (color-coded: Very Safe / Safe / Caution / High Risk / Danger)
- Composite score ring (color: green/gold/red by threshold)

### Card View
- Grid layout
- Bar charts for each metric
- Safety badge
- Trip group label

### Edit Modal
- All fields editable: name, country, continent, status, link, notes, trip groups, all scores, safety
- Wiki ↗ button auto-fills Wikipedia URL from destination name
- Delete button
- Wonders preserved on save (not overwritten)

### localStorage Auto-Save
- Key: `bucketlist_v2`
- Saves on every edit (saveDest, deleteDest, toggleStar)
- Loads on startup, merges saved data over base list
- New destinations added to base list appear automatically for existing users
- Personal edits (status, stars, notes) survive app updates as long as LS_KEY doesn't change

### Header Stats
- Total / Visited / Planned / Wishlist / Starred / Wonders counts
- Updates live

---

## Trip Groups

- **101 groups** with 2+ destinations (133 total defined, singletons excluded from display)
- **Top 10** shown as chips in filter bar
- Color-coded dots in table rows
- Full legend at bottom of page
- Click dot or chip to filter list to that group
- **Grouping philosophy:** 2-week-or-less natural traveler routes, cross-border where geographically natural (e.g. Mekong covers Thailand/Laos/Vietnam/Cambodia; Levant covers Israel/Jordan/Lebanon; Southern Cone covers Chile/Argentina/Uruguay)

### Top 10 Groups by Size
| Group | Label | Destinations |
|-------|-------|-------------|
| balkans | Adriatic Balkans | 15 |
| central-europe | Central Europe | 13 |
| egypt | Egypt Highlights | 8 |
| namibia | Namibia Road Trip | 7 |
| levant | The Levant | 7 |
| south-africa | South Africa | 7 |
| caucasus | Caucasus | 7 |
| greenland | Greenland | 6 |
| baltics | Baltics | 6 |
| west-africa-heritage | West Africa Heritage | 6 |

---

## Reference Links

- **238 destinations** point to UNESCO WHC pages (`whc.unesco.org`)
  - 51 already had UNESCO links before the update
  - 187 upgraded from Wikipedia to UNESCO
- **268 destinations** point to Wikipedia (no UNESCO listing exists)
- **41** no change needed
- UNESCO data sourced from Wikidata SPARQL endpoint (UNESCO API was down; 1,270 sites fetched and cached at `pending/from-desktop/unesco-cache.json`)

---

## Scripts & Automation

| Script | Location | Purpose |
|--------|----------|---------|
| `bucket-list-audit.py` | `pending/from-desktop/` | Global destination gap audit — calls Claude Sonnet API per region |
| `bucket-list-audit-task.md` | `pending/from-desktop/` | BeastBot task brief for the audit |
| `build-trip-groups.py` | `pending/from-desktop/` | Rebuilds TRIP_GROUPS block from audit results, merges into HTML |
| `update-links.py` | `pending/from-desktop/` | Updates destination links: UNESCO first, Wikipedia fallback |
| `bucket-list-audit-results.json` | `pending/from-beastbot/` | Full audit results (343 new entries with scores, notes, group suggestions) |
| `unesco-cache.json` | `pending/from-desktop/` | Cached UNESCO WHC site list from Wikidata (1,270 sites) |

---

## Publishing Workflow

1. Edit `index.html` in workspace (via Claude Desktop / Claude Code)
2. Push to GitHub:
   ```bash
   git -C /home/sys102/.openclaw/workspace/bucket-list-project add index.html && git commit -m "description" && git push
   ```
3. GitHub Pages rebuilds automatically (~60 seconds)
4. Live site updates at the same URL — no link change needed

**Visitor data safety:** Pushing updates preserves visitor localStorage data as long as:
- The `LS_KEY` value (`bucketlist_v2`) is never changed
- Existing destination IDs are never reused for different destinations

---

## Design

- **Fonts:** Cormorant Garamond (headers, scores) + DM Sans (UI)
- **Palette:** Warm paper tones (`#faf7f2` background, `#1a1814` ink, `#c49a3c` gold)
- **Continent colors:** Each continent has a distinct color used for tags, card borders, trip group dots
- **Safety colors:** Green (9-10) → Sage (7-8) → Yellow (5-6) → Orange (3-4) → Red (1-2)

---

## Key Decisions Made

| Decision | Rationale |
|----------|-----------|
| Safety excluded from composite score | Safety is a travel planning factor, not a measure of how worthwhile a destination is |
| localStorage key = `bucketlist_v2` | Never change this — would orphan all visitor data |
| Inclusion rule: hist≥7 OR nature≥7 OR cult≥7 | Mechanical threshold, no editorial overrides |
| No cap on destination count | Include everything that qualifies |
| Single HTML file | No dependencies, works offline, trivial to share and host |
| index.html naming | Required for GitHub Pages to serve at root URL |
| Cross-border trip groups | Follow natural traveler routes, not political borders |

---

## Known Limitations / Future Ideas

- Trip groups for 343 audit-generated destinations were auto-assigned and may need refinement (only 12 back-filled by the build script due to regex matching constraints)
- Balkans group (15 destinations) is likely too large for a single 2-week trip — could split into Adriatic Coast vs Inland Balkans
- Egypt group (8 destinations) spans Cairo/Giza and Luxor/Aswan which are really two separate trips
- No server-side storage — visitors who clear browser cache lose their data (Export button is the mitigation)
- No user accounts or sharing of personal lists between people
- Could add: trip planning export (PDF itinerary), map view, "similar to" recommendations
