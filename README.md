# CM Challenge Pro V32

A web-based competition tracker inspired by Championship Manager 01/02. Track managers, seasons, trophies, and random challenges (RAD) among friends.

## Features

- **Dashboard** -- Leaderboard with rankings, progress bars, and fun competition facts
- **Manager Management** -- Add/edit/remove managers (admin only)
- **Season Tracking** -- Record results per season: club, trophies, top scorer, rating
- **RAD Challenge System** -- Random challenge generator with category-based rotation
- **Statistics** -- Per-manager stats with progression graphs, trophy palmares, and point distribution
- **History** -- Filterable table of all recorded seasons
- **Configuration** -- Customize point categories, trophy flags, ranks, and RAD challenges (admin only)
- **4 Themes** -- Dark (default), Light, Classic (retro CM 01/02 style), and Flat Design
- **Cloud Sync** -- Data stored via npoint.io API with localStorage fallback
- **Mobile Responsive** -- Bottom navigation on smaller screens

## Project Structure

```
cm0102/
  index.html        Main HTML (structure only)
  css/
    styles.css      All styles, themes, and responsive layout
  js/
    app.js          Application logic, state, rendering, and cloud sync
  .gitignore        Files excluded from version control
  README.md         This file
```

## Getting Started

Open `index.html` in a browser. No build step or server required.

To access admin features (manage players, enter results, configure points), click the lock icon in the sidebar and enter the admin password.

## Data Storage

- **Local**: `localStorage` keys `CM_DB_V32` (database) and `CM_CFG_V32` (configuration)
- **Cloud**: Syncs to npoint.io when logged in as admin
- **Export/Import**: Available in the Config tab (admin only)

## Language

The UI is in Dutch (Nederlands).
