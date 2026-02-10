# Run90

A 90-day running conditioning tracker built as a Progressive Web App. Dark, bold, athletic design — no frameworks, no build tools, no dependencies.

**Live:** [mr-menno.github.io/run90](https://mr-menno.github.io/run90/)

## Features

- **90-day training plan** across 4 phases (Build Habit, Build Distance, Build Speed, Peak)
- **Activity logging** — Run, Run/Walk, and Rest days with distance, time, feel rating, and notes
- **Dashboard** — progress ring, streak counter, stat cards, weekly plan, 90-day calendar grid
- **Water tracker** — daily hydration tracking with configurable goal
- **Progress charts** — Canvas-drawn line charts for distance, pace, and duration trends
- **Notifications** — daily reminders at a configurable time
- **Offline support** — service worker caches all assets for full offline use
- **iOS home screen** — installable PWA with standalone display and safe area support
- **Confetti** on milestones (day 30/60/90, distance PRs)
- **Data export** — download all data as JSON

## Tech Stack

- Single `index.html` with inline CSS and JS
- Vanilla JavaScript — no frameworks, no libraries
- localStorage for persistence
- Service Worker for offline caching
- Canvas API for charts
- Google Fonts (Bebas Neue, JetBrains Mono, DM Sans)

## Setup

Serve the project with any static file server:

```bash
# Python
python3 -m http.server

# Node
npx serve .
```

Or just open `index.html` in a browser. For full PWA features (service worker, notifications), use HTTPS or localhost.

## File Structure

```
index.html       # Complete app (HTML + inline CSS + inline JS)
manifest.json    # PWA manifest
sw.js            # Service worker (offline caching + notifications)
icon-192.png     # App icon 192x192
icon-512.png     # App icon 512x512
```
