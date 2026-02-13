# CLAUDE.md — CM Challenge Pro

## Project Overview

CM Challenge Pro is a single-file web application for tracking competitive Championship Manager 01/02 game competitions. It tracks manager profiles, season results, trophies, random challenges (RAD system), rankings, and statistics. The UI is in Dutch.

The entire application lives in **`index.html`** (~1310 lines) — there are no other source files, no build system, and no external dependencies beyond Google Fonts and the npoint.io API for optional cloud sync.

## Repository Structure

```
cm0102/
└── index.html    # Complete SPA: HTML + CSS (~500 lines) + JS (~800 lines)
```

## Running the Application

Open `index.html` directly in any modern browser. No server or build step required.

- **Default mode:** READ_ONLY (view data only)
- **Admin mode:** Unlocked via password (`bjorn`) — enables editing managers, seasons, config

## Architecture

### Single-Page App Structure

The file is organized into three embedded sections:

1. **CSS** (`<style>` tag) — Tailwind-inspired design system with CSS custom properties for theming, 57+ component classes, responsive layout with flexbox/grid
2. **HTML** — Fixed sidebar nav (`<nav>`, 260px) + dynamic content area (`<main>`) + modal overlay system
3. **JavaScript** (`<script>` tag) — All application logic, ~60 functions

### Pages/Tabs

| Tab ID | Name | Access | Purpose |
|--------|------|--------|---------|
| `dashboard` | Dashboard | All | Leaderboard, fun facts, rank legend |
| `p` | Managers | Admin | Add/edit/delete manager profiles |
| `s` | Seasons | Admin | Enter season results and trophies |
| `r` | RAD | All | Random challenge system |
| `st` | Statistics | All | Player progress, charts, analytics |
| `h` | History | All | Complete records |
| `c` | Config | Admin | Edit points, ranks, challenges |

### Data Flow

1. Load from `localStorage` (or cloud via npoint.io)
2. State held in globals: `db` (player data), `cfg` (config), `theme`
3. Tab navigation calls render functions → builds HTML → injects into `<main>`
4. Mutations update `db`/`cfg` in memory → `saveAll()` persists to localStorage + cloud

### Key Global State

- `db` — Player database: `{ players: [], currentSeason }` — stored as `CM_DB_V32`
- `cfg` — Config: `{ points, rad_challenges, goals }` — stored as `CM_CFG_V32`
- `theme` — Current theme name — stored as `CM_THEME_V32`
- `APP_MODE` — `"READ_ONLY"` or `"ADMIN"`
- `CLOUD_CONFIG.binId` — npoint.io storage ID: `785b374d331c9c82148e`

### Theme System

Three themes selectable at runtime via `data-theme` attribute on `<body>`:

- **dark** (default) — Slate/indigo color scheme
- **light** — White/light gray with indigo accents
- **classic** — Retro CM 01/02 style: yellow on blue, Verdana font, squared corners

## Code Conventions

### Naming

- **Functions:** camelCase — `renderDashboard()`, `calcLive()`, `addP()`
- **Variables:** camelCase — `curTab`, `editIdx`
- **CSS classes:** kebab-case — `.modal-overlay`, `.btn-main`
- **DOM IDs:** Short names — `#app`, `#livePts`

### Function Organization

| Category | Examples |
|----------|----------|
| UI Rendering | `renderDashboard()`, `renderP()`, `renderS()`, `renderR()`, `renderStats()`, `renderHistory()`, `renderConfig()` |
| State | `saveAll()`, `tab()`, `recalcTotals()`, `render()` |
| Modals | `openModal()`, `closeModal()`, `openLoginModal()` |
| Manager CRUD | `addP()`, `modalEditP()`, `delP()`, `saveEditP()` |
| RAD System | `roll()`, `finalizeRoll()`, `modalManualRad()`, `addManRad()` |
| Statistics | `renderStats()`, `renderTrophies()`, `renderGraph()` |
| Cloud Sync | `Cloud.loadFromCloud()`, `Cloud.saveToCloud()` |
| Utility | `esc()`, `showToast()`, `toggleTheme()`, `getrank()` |

### Security

- `esc()` function escapes HTML entities for all user input — prevents XSS
- Admin mode gated behind password check
- Admin-only UI hidden with `.admin-only` CSS class

### Language

- All UI text, labels, comments, and domain terms are in **Dutch**
- Variable/function names are in English (abbreviated)

## Key Business Concepts

- **Trophy System:** 7 categories (Nationaal, Supercup, Internationaal, etc.) with point values and boolean trophy flags
- **RAD System:** Random challenge generator with spinning animation, category-based task pools, duplicate prevention per manager per season
- **Ranking:** Progressive ranks (Beginner → Silver → Gold → Legend → Champion) based on configurable point thresholds
- **Seasons:** Each manager has per-season records with club, trophies, and RAD challenges

## Development Notes

- **No build process** — edit `index.html` directly
- **No test framework** — no automated tests exist
- **No CI/CD** — manual deployment
- **localStorage versioned** with `V32` suffix — changing this resets all local data
- When modifying CSS, maintain the CSS custom property theming pattern so all three themes continue to work
- When adding UI features, respect the `APP_MODE` check and hide admin features behind `.admin-only`
