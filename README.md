# LifeTracker

A personal life-tracking web app that runs entirely as a static site on GitHub Pages. No backend, no build step, no external APIs. All data lives in `localStorage`. The app is a collection of standalone HTML files — a home dashboard linking out to dedicated trackers for different areas of life.

---

## File Structure

```
index.html          Home dashboard
fitness.html        PPL workout tracker (Push / Pull / Legs)
hunter-stats.html   Gamified fitness stats companion (Solo Leveling theme)
habits.html         Daily habit tracker with streaks
media.html          Movies, TV shows, and books tracker
restaurants.html    Restaurant tracker (Beli-style)
travel.html         Places visited and want-to-visit tracker
README.md           This file
```

---

## Pages

### Home Dashboard (`index.html`)
- Time-aware greeting ("Good morning", "Good afternoon", "Good evening")
- Pulls live stats from each tracker's localStorage and displays them as summary cards
- Each card is a tappable link to its tracker page
- Today strip showing the current date, a habit streak badge (if active), and a nudge if habits are incomplete
- Settings panel (top-right gear icon) to set your name, stored in localStorage

### Fitness — PPL Tracker (`fitness.html`)
A full Push / Pull / Legs workout tracker with a dark, high-contrast aesthetic (Barlow Condensed / Barlow fonts, dark surface palette).

- **Push, Pull, Legs, Abs panels** — each lists exercises with sets/reps, rest times, target muscles, and expandable technique notes
- **Log tab** — streak cards (current streak, total sessions, this week), a backlog entry form to log any past date, and a navigable monthly calendar with color-coded workout dots
- **Muscle summary** — bar chart of muscle groups trained in the last 7 days
- **Session history** — last 60 sessions with delete support
- Links to the Hunter Stats companion page
- Back button (← Home) returns to `index.html`

### Hunter Stats (`hunter-stats.html`)
A gamified stats page companion to the PPL tracker, themed after Solo Leveling.

- **Stats tab** — animated hunter profile card with rank badge (E through S), total XP, overall rank progress bar, and per-muscle attribute levels visualized on an anime-style SVG body diagram with glowing connectors
- **Quests tab** — daily, weekly, and milestone quests with progress bars and XP rewards
- **Feats tab** — achievement grid that unlocks as milestones are hit, with toast-style unlock notifications
- Reads from the same `ppl_v3` localStorage key as `fitness.html`
- Back button (← PPL) returns to `fitness.html`

### Habits (`habits.html`)
- Define up to 10 habits with a name, emoji, and frequency (daily or specific days of the week)
- **Today tab** — large tappable habit rows with live streak counts; tap to mark complete, tap again to unmark
- **This week tab** — 7-day completion grid (rows = habits, columns = days), today highlighted
- **Manage tab** — add, edit, and delete habits
- Stats strip: best streak, total completions this month, week completion rate

### Media (`media.html`)
- Three tabs: Movies, TV, Books
- Each entry: title, creator (director/author), genre, rating (1–10), status, date added, review/notes
- TV shows track seasons watched as a separate field
- Filter by status (Watched, Want to Watch, DNF, etc.)
- Stats strip: total, completed count, added this year, average rating
- Floating + button quick-add, tap any card to edit, delete from edit modal

### Restaurants (`restaurants.html`)
- Restaurant cards: name, cuisine, neighborhood, rating (1–10), status (Visited / Want to Go / Favorite), date added, notes, and optional Google Maps URL
- Named lists: Visited, Want to Go, Favorites (defaults), plus unlimited custom lists
- Search bar and filter panel (cuisine, city, min rating, sort order); search also matches inside notes
- Stats view: total tracked, average rating, top cuisines, cities breakdown
- **Import:** Google Maps saved list CSV (Title/Note/URL columns fully parsed), generic CSV, or JSON backup — with duplicate detection and preview before confirming
- **Export:** JSON backup download
- Cards show inline 🗺 Maps link when a URL is stored; tap the card body to edit
- **Delete:** ✕ button directly on each card for quick deletion, or via trash icon inside the edit modal

### Travel (`travel.html`)
- Each entry: city, country, country code (for flag emoji), date visited (month/year), trip type (solo, partner, family, friends, work), rating, notes, status (Visited / Want to Go)
- Grid card layout with auto-generated flag emojis from country code
- World regions breakdown: auto-calculates continent from country name
- Stats strip: cities visited, countries, continents, most recent trip
- Filter by status, trip type; search by city or country
- Export and import JSON backup

---

## Design System

| Token | Value |
|---|---|
| Background | `#F9F7F4` (warm off-white) — light pages |
| Card | `#FFFFFF` |
| Text | `#1A1A1A` |
| Muted text | `#888880` |
| Accent | `#C4714A` (dusty terracotta) |
| Accent light | `#F5E8E0` |
| Border | `#EDEBE7` |
| Heading font | DM Serif Display |
| Body font | DM Sans |
| Border radius | 16px (cards), 12px (inputs/buttons) |

The fitness pages (`fitness.html`, `hunter-stats.html`) use their own independent dark theme inherited from the original PPL tracker (Barlow Condensed / Barlow, `#0d0d0d` background).

---

## localStorage Keys

| Key | Used by |
|---|---|
| `ppl_v3` | `fitness.html`, `hunter-stats.html` |
| `hunter_ach` | `hunter-stats.html` (achievement unlock state) |
| `lifetracker_v1_habits` | `habits.html`, `index.html` |
| `lifetracker_v1_media` | `media.html`, `index.html` |
| `lifetracker_v1_restaurants` | `restaurants.html`, `index.html` |
| `lifetracker_v1_travel` | `travel.html`, `index.html` |
| `lifetracker_v1_settings` | `index.html` (user name) |

---

## Technical Notes

- Every `.html` file is fully self-contained with inline CSS and JS — no external dependencies except Google Fonts and Lucide icons (both loaded via CDN, pinned versions)
- Lucide icons: `cdn.jsdelivr.net/npm/lucide@0.383.0/dist/umd/lucide.min.js`
- Google Fonts: DM Serif Display + DM Sans (light pages), Barlow Condensed + Barlow (fitness pages), Rajdhani + Orbitron (hunter stats page)
- No frameworks, no npm, no build step — open any file directly in a browser or serve from GitHub Pages
- Works offline after first load (CDN resources cached by browser)
- localStorage quota errors are caught and surface a user-facing message

---

## Deployment

1. Put all files in the root of a GitHub repository
2. Go to **Settings → Pages**, set source to the `main` branch, root folder
3. GitHub Pages will serve the site at `https://yourusername.github.io/your-repo-name/`

No configuration needed.

---

## Changelog

### v1.0.0 — Initial build
- Created `index.html` home dashboard with time-aware greeting, per-tracker live stat cards, today strip with habit nudge and streak badge, and settings panel for user name
- Created `habits.html` with Today / This Week / Manage tabs, streak tracking, 7-day grid, and stats strip
- Created `fitness.html` generic workout logger (later replaced — see v1.1.0)
- Created `restaurants.html` with Beli-style cards, custom lists, Google Takeout / CSV import, stats view, and JSON export
- Created `media.html` with Movies / TV / Books tabs, status filters, and stats strip
- Created `travel.html` with grid card layout, flag emoji, world regions breakdown, and JSON backup
- Shared design system: DM Serif Display + DM Sans, dusty terracotta accent (`#C4714A`), warm off-white background

### v1.0.1 — Lucide icon CDN fix
- Replaced `unpkg.com/lucide@latest` (unreliable `@latest` resolution) with pinned version `cdn.jsdelivr.net/npm/lucide@0.383.0/dist/umd/lucide.min.js` across all six pages
- Added `if(window.lucide)` guard before every `lucide.createIcons()` call so a CDN failure degrades gracefully instead of throwing and crashing the page

### v1.1.0 — Fitness tracker replaced with PPL workout app
- Replaced generic `fitness.html` with a full Push / Pull / Legs workout tracker (original dark-theme app)
- Added `hunter-stats.html` as a gamified companion page (Solo Leveling theme) with muscle attribute SVG diagram, rank system (E→S), quests, and achievements
- Both pages use `ppl_v3` as their shared localStorage key
- Added `← Home` back button to `fitness.html` top bar linking to `index.html`
- Updated `← PPL` back button in `hunter-stats.html` to correctly point to `fitness.html`
- Updated home dashboard (`index.html`) fitness stat card to read from `ppl_v3` instead of `lifetracker_v1_fitness`, now displays weekly sessions and current streak

### v1.1.1 — Restaurant tracker: Google Maps CSV import fix + inline delete

**Problem 1 — CSV parser broken for Google Maps exports**

The original `parseCSV` function split every line on commas naively. Google Maps CSV exports include URLs in the `URL` column that contain commas inside their `data=` query parameters (e.g. `https://www.google.com/maps/place/Name/data=!4m2!3m1!1s0x...`). This caused every row to be misaligned — the URL fragment would be interpreted as extra columns, the restaurant name would be picked up correctly, but the Note field and everything after the URL would be garbage or empty.

Additionally, Google's CSV uses the column names `Title`, `Note`, `URL`, `Tags`, and `Comment`, but the old parser only checked for `name`, `restaurant`, `cuisine`, `neighborhood` etc. — so even with correct parsing, nothing would have mapped to the right fields.

**Fixes applied to `restaurants.html`:**
- Replaced `parseCSV` with a full RFC 4180-compliant parser (`parseCSVRobust`) that correctly handles quoted fields containing commas, embedded newlines, and escaped double-quotes (`""`) — exactly what Google's CSV format requires
- Added `parseGoogleCSV` which auto-detects Google Maps format by checking for `title` and `url` headers, then maps: `Title` → name, `Note` + `Comment` (joined with ` · ` if both present) → notes, `URL` → stored maps URL
- Falls back to a generic column-name mapping for non-Google CSV files (`name`/`restaurant`/`title`, `cuisine`, `neighborhood`/`city`, `status`, `rating`, `notes`, `url`)
- Google imports default to status `Want to Go` (saved lists = places you want to visit)
- Blank rows (Google's CSV has a blank second row as a separator) are filtered out
- Duplicate detection in the import preview now correctly counts and reports how many entries already exist by name, and skips them on confirm
- Import preview shows the first 3 names plus a count of remaining entries

**Problem 2 — No way to delete entries without opening the edit modal**

**Fix applied:**
- Added a small `✕` delete button (`r-del-btn`) to the right side of every restaurant card in the list view
- Tapping it calls `quickDelete(id)` which confirms via a native dialog and removes the entry immediately without opening any modal
- Delete from within the edit modal (trash icon button) still works as before — both paths are available

**Other improvements in this update:**
- `url` field added to the add/edit modal ("Google Maps URL") so links can be manually entered or edited after import
- Each card now shows a `🗺 Maps` link inline in the meta row when a URL is stored; the link opens in a new tab and does not trigger the edit modal
- Notes search: the search bar now also searches inside notes text, not just name/cuisine/neighborhood
- `escHtml` helper added throughout card rendering to prevent XSS from restaurant names or notes containing `<`, `>`, `&`, or `"`

### v1.2.0 — Restaurant tracker: Leaflet map + Elo ranking minigame

**New: Map view**

A full-width Leaflet.js (OpenStreetMap tiles, no API key required) map added as a second tab in `restaurants.html`. Custom SVG pin markers colored by status — terracotta for Visited/Favorite, slate-blue for Want to Go. Tapping a pin shows a popup with name, cuisine, neighborhood, status, rating, Elo rank (if ranked), a notes preview, and an Edit button.

Geocoding uses Nominatim (OpenStreetMap's free geocoding service). Coordinates are fetched automatically in the background when a new manual Visited entry is saved. For imported entries, a `📍 Locate` button appears on the card in list view and in a "Missing location" panel below the map. Coordinates are stored as `lat`/`lng` on each restaurant object.

**New: Elo ranking minigame**

A head-to-head comparison game for ranking visited restaurants relative to each other. Uses a binary search algorithm over the ranked pool (sorted by Elo score, best to worst). Each round picks the midpoint of the current search window and asks "Better / Same / Worse." The window narrows after each answer until it's ≤ 1 entry wide, "Same" is selected, or the max comparison count (log₂(pool size) + 2, capped at 7) is reached. Standard Elo formula (K=32) applied to both participants for every comparison.

- **Manual entries:** Elo game launches automatically after saving a new Visited/Favorite restaurant, or when an existing entry is changed to Visited status
- **Imported entries:** marked with `imported: true`. A `⚡` rank button appears on the card and in the Ranking view. The game is triggered manually so bulk imports don't immediately bombard with comparison prompts
- Initial Elo seed derived from the numeric rating: rating × 100 + 700 (so a 5/10 → 1200, 10/10 → 1700, 1/10 → 800). Unrated entries start at 1200
- Result screen shows final rank (#N out of N), tier badge (S/A/B/C/D based on percentile), and Elo score

**New: Ranking view (tab)**

A dedicated tab showing the full ranked leaderboard sorted by Elo, with rank number, tier badge, raw Elo score, match count, and the existing 1–10 rating. Unranked visited entries appear below the leaderboard with a `⚡` button to start ranking.

**Tier system** (percentile-based, updates as the pool grows):
- S — top 5%
- A — top 6–20%
- B — top 21–45%
- C — top 46–70%
- D — bottom 30%

**Other changes in this version:**
- Nav stats bar replaced with a 4-tab row: List / Map / Ranking / Stats
- `eloRating`, `eloGames`, `eloRanked`, `imported`, `lat`, `lng` fields added to all restaurant entries (backward-compatible: missing fields are treated as defaults)
- Elo rank badge (`#N`) displayed on list cards for ranked visited entries; top 3 get gold styling
- Sort-by "Elo rank" option added to filter panel
- Stats view gains two new stat cards: "Elo ranked" count and "On map" count
- Import toast updated to remind user to mark entries as Visited and tap ⚡ to rank

### v1.2.1 — "Nommies" rename + fitness page overhaul

**`restaurants.html` — renamed to Nommies**

All user-facing text updated from "Restaurants" / "Restaurant" to "Nommies" / "Nommie": page title, header, search placeholder, add/edit modal titles, save toast, delete confirm dialog, and empty state messages. The `localStorage` key (`lifetracker_v1_restaurants`) and all internal variable/function names are unchanged, so existing data is fully preserved.

**`fitness.html` — replaced with new version**

The fitness page was replaced in full. Key changes in the new version:

- **Desktop sidebar navigation** — a sticky left sidebar appears on screens ≥ 768px, replacing the bottom nav for desktop use. Each day button (Push, Pull, Legs, Abs, Log) is listed vertically with a colored active state. The Hunter link sits at the bottom of the sidebar. The bottom nav is hidden on desktop via media query.
- **Calendar day popout** — tapping any cell in the monthly calendar now opens a bottom sheet where you can select one or more workout types (or Rest) for that day, save, or delete the entry. Previously the calendar was read-only.
- **Multi-type logging per day** — a single day can now have multiple workout types logged (e.g. Push + Abs together). Each day panel shows color-coded tags for everything logged that day. The log button row hides once that day's type is logged.
- **Safe area insets** — `env(safe-area-inset-bottom)` and `env(safe-area-inset-top)` applied throughout for notch/home-indicator support on iOS.
- **Motivational quotes** — a daily rotating quote appears in the top bar subtitle, seeded by the calendar date so it changes each day and is consistent across sessions.
- **Updated exercise content** — revised sets, reps, rest times, technique notes, and muscle tags across Push, Pull, Legs, and Abs panels.
- **`← Home` link** added back to the top bar (was present in the previous version, missing from the uploaded file — restored during integration).
- **Muscle summary + session history** wired back into the Log tab — `renderMuscleSummary()` and `renderHistory()` are now called by `refreshLog()` and their target DOM elements restored. The 7-day muscle volume bar chart and scrollable session history (last 60 entries with delete) both render correctly.
- `localStorage` key remains `ppl_v3` — all existing workout history is preserved.

### v1.3.0 — Home calendar, chores, file renames, travel removed

**`index.html` — full rewrite**

The home dashboard was rebuilt from scratch with two major additions.

The first is a monthly calendar sitting above the tracker cards. Dots appear automatically by reading existing localStorage data — workout dots come from `ppl_v3`, nommie dots come from restaurant entry dates in `lifetracker_v1_restaurants`. Tapping any day opens a bottom sheet where you can manually log Workout, Chores, Nommie, or Rest for that day, with an optional free-text note. Manual entries are stored in a new key `lifetracker_v1_calendar` and are independent of the per-tracker data. Dot colors: orange for workouts, steel blue for chores, terracotta for nommies, light grey for rest.

The second is drag-to-reorder tracker cards. Each card has a grip handle on the right. On desktop, standard HTML5 drag-and-drop is used. On mobile, a touch handler on the grip handle creates a floating ghost clone and tracks the drag. Dropping reorders the list in place. Order is persisted to `lifetracker_v1_card_order` and restored on every page load. If the saved order doesn't match the current card set (e.g. after a card is added or removed), it falls back to the default order.

The today strip and habit/streak logic were removed. Travel and Habits cards removed. Chores and Nommies cards added.

**`habits.html` → `chores.html`**

The habits tracker was copied to `chores.html` with all user-facing text updated from "Habit"/"Habits" to "Chore"/"Chores" throughout: page title, nav header, modal titles, toast messages, empty states, section labels, and button labels. The `localStorage` key was updated from `lifetracker_v1_habits` to `lifetracker_v1_chores`. The underlying data model (`{ habits: [...], completions: {...} }`) is unchanged — the `habits` array key inside the object was not renamed for simplicity. `habits.html` is retained in the repo as-is for reference.

**`restaurants.html` → `nommies.html`**

File renamed to match the in-app branding. All internal `href` and self-referencing links inside the file updated to `nommies.html`. `index.html` updated to link to `nommies.html`. `restaurants.html` removed. The `localStorage` key `lifetracker_v1_restaurants` is unchanged — all existing data carries over.

**`travel.html` — removed**

Travel tracker removed from the project. The card was dropped from `index.html`.

**`restaurants.html` Leaflet fix (included in this release)**

Leaflet CDN switched from `unpkg.com` to `cdnjs.cloudflare.com` for reliability. A `typeof L === 'undefined'` guard added to `initMap()` so a failed CDN load shows a friendly error message in the map container instead of throwing an uncaught reference error.

### Current file list

| File | Description |
|---|---|
| `index.html` | Home dashboard with calendar and tracker cards |
| `fitness.html` | PPL workout tracker (dark theme) |
| `hunter-stats.html` | Gamified stats, Solo Leveling theme |
| `nommies.html` | Restaurant tracker with map, Elo ranking |
| `chores.html` | Daily/weekly chore tracker |
| `media.html` | Movies, TV, books tracker |
| `habits.html` | Original habits tracker (retained, superseded by chores.html) |

### v1.3.1 — Hunter Stats removed

`hunter-stats.html` deleted. All references to it removed from `fitness.html`: the Hunter button in the top bar, the Hunter link in the desktop sidebar, the ⚡ Stats button in the mobile bottom nav, and all associated CSS (`.btn-hunter`, `.sidebar-hunter`, `.nav-btn.hunter-nav`, `--hunter` color variable). The `switchDay` nav reset logic was also simplified since it no longer needs to special-case the hunter nav button. Fitness page is otherwise unchanged.

**Updated file list**

| File | Description |
|---|---|
| `index.html` | Home dashboard with calendar and tracker cards |
| `fitness.html` | PPL workout tracker (dark theme) |
| `nommies.html` | Restaurant tracker with map, Elo ranking |
| `chores.html` | Daily/weekly chore tracker |
| `media.html` | Movies, TV, books tracker |
| `habits.html` | Original habits tracker (retained, superseded by chores.html) |
