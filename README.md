# LifeTracker

A personal life-tracking web app that runs entirely as a static site on GitHub Pages. No backend, no build step, no external APIs. All data lives in `localStorage`. The app is a collection of standalone HTML files — a home dashboard linking out to dedicated trackers for different areas of life.

---

## File Structure

```
index.html      Home dashboard with calendar and tracker cards
fitness.html    PPL workout tracker (Push / Pull / Legs / Abs)
nommies.html    Restaurant tracker with map and Elo ranking
chores.html     Daily/weekly chore tracker
media.html      Movies, TV shows, and books tracker
README.md       This file
```

---

## Pages

### Home Dashboard (`index.html`)

- Time-aware greeting with optional name from settings
- **Monthly calendar** — dots appear automatically from workout logs and restaurant entry dates. Tap any day to open a bottom sheet and manually log Workout, Chores, Nommie, or Rest with an optional note. Manual entries stored in `lifetracker_v1_calendar`
- **Tracker cards** — live stat summary for each tracker, tappable to navigate. Cards are drag-to-reorder via a grip handle, works on desktop (HTML5 drag) and mobile (touch). Order persists in `lifetracker_v1_card_order`
- Settings panel (gear icon) to set your name

### Fitness (`fitness.html`)

A Push / Pull / Legs / Abs workout tracker with a dark high-contrast aesthetic (Barlow Condensed + Barlow fonts).

- **Push, Pull, Legs, Abs panels** — exercises with sets/reps, rest times, target muscles, expandable technique notes. Full warmup sequence on Legs panel
- **Log tab** — streak cards, navigable monthly calendar with tap-to-edit day popout, 7-day muscle volume bar chart, session history (last 60 entries with delete)
- **Multi-type logging** — a single day can log multiple types (e.g. Push + Abs)
- **Desktop sidebar** on screens >= 768px; bottom nav on mobile
- Daily rotating motivational quote in the top bar
- Data stored in `ppl_v3`

### Nommies (`nommies.html`)

A restaurant tracker with four tabs: List, Map, Ranking, Stats.

- **List** — cards with name, cuisine, neighborhood, status, rating, date, Maps link, notes preview. Filter, sort, and named lists (Visited, Want to Go, Favorites, custom)
- **Map** — Leaflet.js (OpenStreetMap, no API key). Custom SVG pins by status. Geocoding via Nominatim fires automatically on new Visited entries
- **Ranking** — Elo-based leaderboard with head-to-head comparison game (binary search, Better/Same/Worse). Tier badges S/A/B/C/D by percentile
- **Stats** — total, visited, avg rating, ranked count, on-map count, top cuisines and cities
- Import: Google Maps CSV, generic CSV, JSON. Export: JSON
- Data stored in `lifetracker_v1_restaurants`

### Chores (`chores.html`)

- Define chores with name, emoji, and frequency (daily or specific days)
- **Today tab** — tappable rows with live streak counts
- **This week tab** — 7-day completion grid
- **Manage tab** — add, edit, delete chores
- Stats strip: best streak, completions this month, week rate
- Data stored in `lifetracker_v1_chores`

### Media (`media.html`)

- Three tabs: Movies, TV, Books. Each entry: title, creator, genre, rating, status, date, notes. TV tracks seasons
- Filter by status, search by title or creator
- Stats strip: total, completed, added this year, average rating
- Data stored in `lifetracker_v1_media`

---

## Design System

| Token | Value |
|---|---|
| Background | `#F9F7F4` (warm off-white) |
| Card | `#FFFFFF` |
| Text | `#1A1A1A` |
| Muted text | `#888880` |
| Accent | `#C4714A` (dusty terracotta) |
| Accent light | `#F5E8E0` |
| Border | `#EDEBE7` |
| Heading font | DM Serif Display |
| Body font | DM Sans |
| Border radius | 16px (cards), 12px (inputs/buttons) |

`fitness.html` uses its own dark theme: Barlow Condensed + Barlow, `#0d0d0d` background, colored accents per day type.

---

## localStorage Keys

| Key | Used by | Contents |
|---|---|---|
| `ppl_v3` | `fitness.html`, `index.html` | `{ 'YYYY-MM-DD': { types:[], rest:bool } }` |
| `lifetracker_v1_restaurants` | `nommies.html`, `index.html` | `{ restaurants: [...] }` |
| `lifetracker_v1_chores` | `chores.html`, `index.html` | `{ habits: [...], completions: {...} }` |
| `lifetracker_v1_media` | `media.html`, `index.html` | `{ movies: [...], tv: [...], books: [...] }` |
| `lifetracker_v1_calendar` | `index.html` | `{ 'YYYY-MM-DD': { types:[], note:'' } }` |
| `lifetracker_v1_card_order` | `index.html` | `['nommies','fitness','chores','media']` |
| `lifetracker_v1_settings` | `index.html` | `{ name: '' }` |

---

## Technical Notes

- Every `.html` file is fully self-contained with inline CSS and JS
- Lucide icons: `cdn.jsdelivr.net/npm/lucide@0.383.0/dist/umd/lucide.min.js`
- Leaflet.js: `cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js`
- Google Fonts: DM Serif Display + DM Sans (light pages), Barlow Condensed + Barlow (fitness)
- No frameworks, no npm, no build step

---

## Deployment

1. Put all files in the root of a GitHub repository
2. Go to **Settings → Pages**, set source to the `main` branch, root folder
3. Site serves at `https://yourusername.github.io/your-repo-name/`

---

## Changelog

### v1.3.2 — Fitness log tab cleanup + calendar Sun-first
Removed the "Muscles (last 7 days)" bar chart and session history list from the Log tab in `fitness.html` — both are redundant given the home dashboard calendar. `renderMuscleSummary` and `renderHistory` functions removed entirely; `refreshLog` simplified to just `renderCal` + `calcStreaks`. Calendar header and start-of-week logic changed from Monday-first to Sunday-first.

### v1.3.1 — Hunter Stats removed
`hunter-stats.html` deleted. All references removed from `fitness.html`: Hunter button, sidebar link, bottom nav button, associated CSS. `switchDay` nav logic simplified.

### v1.3.0 — Home calendar, chores, file renames, travel removed
`index.html` rebuilt with monthly calendar (auto-dots + manual logging) and drag-to-reorder cards. `habits.html` → `chores.html` (text and localStorage key updated). `restaurants.html` → `nommies.html`. `travel.html` removed. Leaflet CDN switched to cdnjs.

### v1.2.1 — Nommies UI rename + fitness overhaul
Restaurant tracker user-facing text updated to "Nommies". Fitness page replaced: desktop sidebar, multi-type day logging, calendar day popout, motivational quotes, iOS safe area insets.

### v1.2.0 — Nommies: Leaflet map + Elo ranking
Leaflet map tab with Nominatim geocoding and custom SVG pins. Elo ranking minigame using binary search comparisons. Ranking leaderboard tab. Tier badges by percentile. 4-tab nav.

### v1.1.1 — Nommies: Google Maps CSV fix + inline delete
Full RFC 4180 CSV parser. Auto-detection of Google Maps export columns. Inline delete button on cards. URL/Maps link field. Notes search. XSS protection.

### v1.1.0 — PPL fitness tracker + Hunter Stats
Replaced generic fitness page with Push / Pull / Legs tracker (dark theme). Added Hunter Stats gamified companion (Solo Leveling theme, rank system E→S, muscle SVG diagram, quests, achievements). Both read `ppl_v3`.

### v1.0.1 — Lucide CDN fix
Pinned Lucide to `@0.383.0` via jsDelivr. Added `if(window.lucide)` guard before every `lucide.createIcons()` call.

### v1.0.0 — Initial build
Home dashboard, habits tracker, fitness tracker, restaurant tracker, media tracker, travel tracker. Shared design system established.
