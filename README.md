# LifeTracker

A personal life-tracking web app that runs entirely as a static site on GitHub Pages. No backend, no build step, no external APIs. All data lives in `localStorage`. The app is a collection of standalone HTML files — a home dashboard linking out to dedicated trackers for different areas of life.

---

## File Structure

```
index.html      Home dashboard with calendar and tracker cards
fitness.html    PPL workout tracker (Push / Pull / Legs / Abs)
nommies.html    Restaurant tracker with map and Elo ranking
chores.html     Chore and task tracker with rich scheduling
README.md       This file
```

---

## Pages

### Home Dashboard (`index.html`)

- Time-aware greeting with optional name from settings
- **Monthly calendar** — dots appear automatically from workout logs and restaurant entry dates. Tap any day to open a bottom sheet and manually log Workout, Chores, Nommie, or Rest with an optional note. Manual entries stored in `lifetracker_v1_calendar`
- **Tracker cards** — live stat summary for each tracker, tappable to navigate. Cards are drag-to-reorder via a grip handle, works on desktop (HTML5 drag) and mobile (touch). Order persists in `lifetracker_v1_card_order`
- Settings panel (gear icon) to set your name
- Dark mode toggle (moon/sun icon) in the top-right header

### Fitness (`fitness.html`)

A Push / Pull / Legs / Abs workout tracker with a dark high-contrast aesthetic (Barlow Condensed + Barlow fonts).

- **Push, Pull, Legs, Abs panels** — exercises with sets/reps, rest times, target muscles, expandable technique notes. Full warmup sequence on Legs panel
- **Log tab** — streak cards, navigable monthly calendar with tap-to-edit day popout, session history (last 60 entries with delete)
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
- Dark mode toggle in the top-right nav
- Data stored in `lifetracker_v1_restaurants`

### Chores (`chores.html`)

A full-featured chore and task tracker with rich scheduling, four tabs, and a completion flow with optional notes.

**Data model.** Each chore stores a name, emoji, category, type (recurring or one-time), priority (low, medium, high), optional notes, created date, due date, last completed date, and active status. Recurring chores support daily, weekly (specific days), every N days, and monthly (specific day of month) frequencies. One-time tasks have a single due date and are automatically deactivated once completed.

**Today tab.** Shows everything due today plus any overdue items, sorted by priority then due date. An overdue banner at the top counts items that need attention; overdue rows are visually distinct with a red border. Tapping a chore expands an inline note field before confirming completion, then shows a toast. Completed items can be tapped again to undo. A separate Upcoming section below shows items due in the next 7 days.

**Calendar tab.** Monthly calendar with colored dots indicating completed days, days with due chores, and days where chores were missed. Tap any day to open a detail sheet listing what was completed (with any note), what was due, and the status of each item. Navigate between months with chevron buttons.

**Log tab.** Chronological history of all completions, newest first. Each entry shows the chore name, date, category, and any note recorded at completion time. Filter by name or category. Delete individual entries with confirmation.

**Manage tab.** Full list of all chores with active, archived, and all views. Add or edit chores via a bottom-sheet modal with all fields. Archive chores to hide them without losing history; unarchive them later. Delete chores and all associated history. Drag-to-reorder via grip handles (desktop HTML5 drag).

**Stats strip.** Shown at the top of every tab: best current daily streak, total completions this month, and on-time completion rate for the past 7 days.

**Migration.** On load, any existing v1 data (habits array with daily/weekly frequencies, completions as arrays of IDs) is automatically converted to the v2 schema with no data loss.

- Dark mode toggle in the top-right nav
- Data stored in `lifetracker_v1_chores`

---

## Design System

### Light mode

| Token | Value |
|---|---|
| Background | `#F2EFE9` (warm parchment) |
| Card | `#FDFCFA` |
| Text | `#1A1A1A` |
| Muted text | `#888880` |
| Accent | `#C4714A` (dusty terracotta) |
| Accent light | `#F5E8E0` |
| Border | `#EDEBE7` |

### Dark mode

| Token | Value |
|---|---|
| Background | `#1C1917` (warm dark brown) |
| Card | `#28251F` |
| Text | `#F0EDE8` |
| Muted text | `#8A8480` |
| Accent | `#D4845A` (slightly brighter terracotta) |
| Accent light | `#3A2419` |
| Border | `#3A3630` |

Dark mode is toggled via a moon/sun button in the header of each light-theme page. Preference is saved to `lifetracker_v1_settings` and applied immediately on page load. On first visit, the OS `prefers-color-scheme` setting is used as the default. `fitness.html` is always dark and has no toggle.

### Fonts and shape

| Token | Value |
|---|---|
| Heading font | DM Serif Display |
| Body font | DM Sans |
| Border radius | 16px (cards), 12px (inputs/buttons) |

### Calendar dot colors

All light-theme pages share the same dot conventions. Colors were chosen to be distinguishable across red-green (deuteranopia/protanopia) and blue-purple (tritanopia) colorblindness:

| Dot | Color | Meaning |
|---|---|---|
| Workout | `#E8A020` amber | Logged workout |
| Chores / Due | `#2BA896` teal | Chore logged or item due |
| Nommie / Completed | `#C4714A` terracotta (accent) | Restaurant visit or chore completed |
| Overdue | `#E07B27` orange | Missed or overdue item |
| Rest | `#AAAAAA` gray | Rest day |

Amber, teal, terracotta, orange, and gray are all distinguishable under deuteranopia, protanopia, and tritanopia. `fitness.html` uses its own per-type dot colors (push/pull/legs/abs) and is exempt.

---

## localStorage Keys

| Key | Used by | Contents |
|---|---|---|
| `ppl_v3` | `fitness.html`, `index.html` | `{ 'YYYY-MM-DD': { types:[], rest:bool } }` |
| `lifetracker_v1_restaurants` | `nommies.html`, `index.html` | `{ restaurants: [...] }` |
| `lifetracker_v1_chores` | `chores.html`, `index.html` | `{ version: 2, chores: [...], completions: { 'YYYY-MM-DD': [{id, note}] } }` |
| `lifetracker_v1_calendar` | `index.html` | `{ 'YYYY-MM-DD': { types:[], note:'' } }` |
| `lifetracker_v1_card_order` | `index.html` | `['nommies','fitness','chores']` |
| `lifetracker_v1_settings` | `index.html`, `chores.html`, `nommies.html` | `{ name: '', dark: bool }` |

---

## Technical Notes

- Every `.html` file is fully self-contained with inline CSS and JS
- Lucide icons: `cdn.jsdelivr.net/npm/lucide@0.383.0/dist/umd/lucide.min.js`
- Leaflet.js: `cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js`
- Google Fonts: DM Serif Display + DM Sans (light pages), Barlow Condensed + Barlow (fitness)
- No frameworks, no npm, no build step
- All viewports lock to `user-scalable=no, maximum-scale=1.0` to prevent accidental pinch-zoom
- Dark mode uses `html.dark` class with CSS custom property overrides; no JS style manipulation at runtime

---

## Deployment

1. Put all files in the root of a GitHub repository
2. Go to **Settings → Pages**, set source to the `main` branch, root folder
3. Site serves at `https://yourusername.github.io/your-repo-name/`

---

## Changelog

### v1.4.5 — Bug fixes + tritanopia-safe colors + home page cleanup
Three fixes in one pass. (1) Media tracker card removed from `index.html` home dashboard — the card definition and its `lifetracker_v1_media` read were still present after `media.html` was deleted in v1.4.1. (2) Dark mode toggle on the home page was broken: the swipe-to-dismiss utility had been accidentally injected as inline content inside the `<script src>` tag for Lucide (which browsers ignore), so `initSwipeDismiss` was never defined and the toggle wiring ran outside any script tag entirely. Fixed by moving all JS into the main script block with a self-closing external script tag. (3) Calendar dot and chip colors updated for tritanopia (blue-purple colorblindness) safety: workout dot changed from indigo (`#6C5CE7`) to amber (`#E8A020`), chore/due dot changed from blue (`#2D9CDB`) to teal (`#2BA896`). The full set of amber, teal, terracotta, orange, and gray is now distinguishable under deuteranopia, protanopia, and tritanopia. Updated across `index.html`, `chores.html`, and `nommies.html`.

### v1.4.4 — Dark mode
Dark mode added to `index.html`, `chores.html`, and `nommies.html` (fitness is always dark and excluded). A moon/sun toggle button sits in each page's top-right header. Clicking it toggles the `html.dark` class and saves the preference to `lifetracker_v1_settings` as `{ dark: bool }`. On first visit, the OS `prefers-color-scheme` setting is used as the default. Dark palette uses warm brown-blacks (`#1C1917` bg, `#28251F` card, `#3A3630` border) to match the app's existing warm character. The accent is nudged slightly brighter to `#D4845A` for better contrast on dark surfaces. All semantic colors (overdue red, priority states, status badges, chip active states) have dark-mode variants. The `lifetracker_v1_settings` key is updated to `{ name, dark }`, and `saveSettings` merges keys rather than replacing the object, so name and dark preference never clobber each other.

### v1.4.3 — Autocomplete disabled
`autocomplete="off"` added to every text input and textarea across `index.html`, `chores.html`, and `nommies.html`. Covers all form fields: chore name, emoji, category, notes, completion notes, log search, all nommies fields, search/filter bars, the settings name field, and the calendar day note. Hidden and file inputs left unchanged.

### v1.4.2 — Swipe-to-dismiss + pinch-zoom lock
All bottom sheets and modals can now be dismissed by dragging down. Dragging past 30% of the sheet's height, or a fast flick, closes it with a matching animation; releasing before the threshold snaps it back. A pill-shaped drag handle is shown at the top of every sheet as a visual cue. Sheets covered: `index.html` (calendar day sheet, settings panel), `chores.html` (add/edit modal), `nommies.html` (add/edit, import, manage lists, and Elo modals), `fitness.html` (calendar day popout, confirm/delete modal). All pages also now set `maximum-scale=1.0, user-scalable=no` in the viewport meta tag to prevent accidental pinch-zoom.

### v1.4.1 — Colorblind-safe palette + remove media tracker
`media.html` removed. Color scheme audited for red-green colorblindness (deuteranopia/protanopia). `index.html` calendar: workout dot changed from orange-red to indigo, chore dot updated to blue, keeping nommie as terracotta and rest as gray. `chores.html`: priority dots changed from red/yellow/green to orange/blue/gray (high/medium/low); calendar overdue dot changed from red to orange (`#E07B27`); "due" day-detail status changed from yellow to blue. Light background softened from `#F9F7F4` to `#F2EFE9` and card from `#FFFFFF` to `#FDFCFA`.

### v1.4.0 — Chores overhaul
`chores.html` fully rebuilt. New data model (v2 schema) supports recurring chores (daily, weekly by day, every N days, monthly) and one-time tasks, each with emoji, category, priority (low/medium/high), notes, and active/archived status. Four tabs: Today (overdue banner, priority sorting, inline completion notes, upcoming section), Calendar (dots for completions and missed chores, tap-to-view day detail sheet), Log (full history with filter and per-entry delete), Manage (active/archived filter, drag-to-reorder, full edit/archive/delete). Stats strip shows best streak, monthly completions, and 7-day on-time rate. Migration function converts all existing v1 data automatically. `lifetracker_v1_chores` schema updated to `{ version: 2, chores: [...], completions: { 'YYYY-MM-DD': [{id, note}] } }`.

### v1.3.3 — Nommies: singular rename + map fix
Singular form of "Nommie" renamed to "Nommy" throughout `nommies.html`: modal titles (New Nommy, Edit Nommy), placeholder text, save toast, delete confirm dialog, and achievement toast. "Nommies" (plural) is unchanged. Map blank-screen bug fixed: `mapView` now gets `display:block` explicitly on tab switch (previously set to `''` which could inherit `none` from the stylesheet), and `invalidateSize` now fires via `requestAnimationFrame` plus a 300ms fallback so Leaflet always measures the container after it's fully visible.

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
Home dashboard, habits tracker, fitness tracker, restaurant tracker, travel tracker. Shared design system established.
