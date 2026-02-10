# Run90 — 90 Day Running Tracker PWA

## Product Requirements Document

### Overview

Build a Progressive Web App (PWA) called **Run90** that tracks a 90-day running conditioning program. The user is a 40-year-old male, 5’6”, 166 lbs, starting from a baseline of 1.17 miles in 17 minutes (run/walk mix). The app should feel like a premium native iOS fitness app.

The app must work as a **fully offline-capable PWA** with iOS home screen support and push notification capability (iOS 16.4+).

-----

### Tech Stack

- **Single-page HTML/CSS/JS** — no build tools, no frameworks, no npm
- All code in a single `index.html` file (inline CSS + JS) plus supporting PWA files
- **localStorage** for all data persistence
- **Service Worker** for offline support and notifications
- **Web App Manifest** for PWA install
- Canvas API for charts (no chart libraries)
- Google Fonts for typography

### File Structure

```
/
├── index.html          # Complete app (HTML + inline CSS + inline JS)
├── manifest.json       # PWA manifest
├── sw.js               # Service worker (offline caching + notification scheduling)
├── icon-192.png        # App icon 192x192 (generate as inline SVG-to-canvas PNG)
├── icon-512.png        # App icon 512x512
└── README.md           # Setup instructions
```

-----

### Design System

**Aesthetic:** Dark, bold, athletic — think Nike Run Club meets a terminal UI. Not generic AI slop.

- **Background:** `#0a0a0a` (near black)
- **Surface cards:** `#141414` with `#2a2a2a` borders
- **Accent:** `#FF4D00` (hot orange) with gradient to `#ff6a00`
- **Success:** `#00E676` (green)
- **Rest/Info:** `#448AFF` (blue)
- **Warning:** `#FFD600` (yellow)
- **Danger:** `#FF1744` (red)
- **Fonts:**
  - Display/Headers: `Bebas Neue` — big, bold, uppercase energy
  - Monospace/Data: `JetBrains Mono` — for numbers, stats, codes
  - Body: `DM Sans` — clean, readable
- **Radius:** 16px for cards, 12px for inputs, 20px for pills
- **iOS safe areas:** respect `env(safe-area-inset-*)` everywhere
- **No emojis in the UI chrome** — use them sparingly only in content (activity type buttons, empty states)
- **Animations:** Smooth transitions on screen changes, ring progress animation, slide-up modals

-----

### Screens & Features

#### 1. Onboarding (first launch only)

Two-step flow:

1. **Welcome screen** — App name, tagline “90 days. One goal.”, CTA button
1. **Baseline setup:**
- Start date (date picker, default today)
- Current distance in miles (default 1.17)
- Current time in minutes (default 17)
- Daily reminder time (time picker, default 06:30)
- “Start My 90 Days” button

Store in localStorage. Show main app after completion.

#### 2. Dashboard (Home)

The primary screen. Top to bottom:

**Header row:**

- Left: “RUN90” logo + current phase label (e.g., “Phase 1: Build Habit”)
- Right: Streak badge showing consecutive days with a log entry ( icon + count)

**Progress Ring:**

- Large circular SVG progress ring (200px)
- Center shows current day number (e.g., “Day 14”) and “of 90”
- Ring fills based on (completed run days / 90)
- Below ring: “X completed · Y to go”

**Stats Row (3 cards):**

- Total Miles (sum of all logged distances)
- Average Pace (total time / total distance, formatted as MM:SS/mi)
- Total Minutes (sum of all logged times)

**Water Tracker:**

- Section with glass icons (default 10 glasses)
- Tap a glass to fill/unfill. Tracks per day.
- Shows “X / 10 glasses” counter
- Resets daily

**Log Button:**

- Big orange gradient button: “+ Log Today’s Run”
- If today is already logged: “✓ Today Logged — Tap to Edit” (muted style)

**This Week Plan:**

- 7-row display (Mon-Sun) showing the recommended activity for each day based on current training phase
- Checkmark indicator for days that have been logged
- Highlight today’s row

**90 Day Calendar Grid:**

- 9 rows × 10 columns grid (days 1-90)
- Color coded: green=completed run, blue=rest day, red=missed (past + no log), dim=future
- Orange ring highlight on today
- Tap any day to open log modal for that day

#### 3. History / Progress Screen

**Chart Section:**

- Tab bar: Distance | Pace | Duration
- Canvas-drawn line chart with gradient fill underneath
- Orange line, dots on data points, date labels on x-axis
- Show “Log at least 2 runs to see trends” as empty state

**Activity List:**

- Reverse chronological list of all logged entries
- Each card shows: date, activity type badge (Run/Run-Walk/Rest), distance, time, pace, notes
- Tap to edit

#### 4. Settings Screen

- **Daily Reminder toggle** (on/off) — triggers notification permission request when enabled
- **Reminder Time** input
- **Water Goal** (glasses per day) input
- **Start Date** and **End Date** display (read-only)
- **Export Data** button — downloads all data as JSON
- **Reset All Data** button — double-confirm, then wipe localStorage and reload

#### 5. Bottom Navigation

Fixed bottom nav with 3 tabs: Home, Progress, Settings. Icons + labels. Active tab highlighted in accent color. Respect safe area bottom inset.

#### 6. Log Activity Modal

Bottom sheet modal (slides up from bottom):

- **Activity Type** toggle buttons: Run | Run/Walk | Rest Day
- **Distance** (miles) input — hidden for Rest
- **Time** (minutes) input — hidden for Rest
- **How did it feel?** toggle: Hard | Good | Easy — hidden for Rest
- **Notes** textarea (optional)
- Save / Cancel buttons
- Delete button (shown only when editing existing entry)

-----

### Training Plan Logic

The 90 days are divided into 4 phases. The weekly plan displayed on the dashboard rotates based on which phase the user is in:

|Phase|Days |Name          |Weekly Plan (Mon-Sun)                                       |
|-----|-----|--------------|------------------------------------------------------------|
|1    |1-21 |Build Habit   |Run/Walk, Rest, Run/Walk, Rest, Run/Walk, Rest or Walk, Rest|
|2    |22-42|Build Distance|Run, Rest, Run, Walk, Run, Long Run, Rest                   |
|3    |43-63|Build Speed   |Tempo Run, Rest, Intervals, Easy Walk, Run, Long Run, Rest  |
|4    |64-90|Peak          |Tempo Run, Rest, Intervals, Easy Run, Run, Long Run, Rest   |

-----

### Data Model (localStorage)

Single key `run90_data` storing JSON:

```json
{
  "onboarded": true,
  "startDate": "2025-02-09",
  "baselineDistance": 1.17,
  "baselineTime": 17,
  "reminderTime": "06:30",
  "reminderEnabled": true,
  "waterGoal": 10,
  "logs": {
    "2025-02-09": {
      "type": "run-walk",
      "distance": 1.17,
      "time": 17,
      "feel": "good",
      "notes": "First day with the dog!",
      "loggedAt": "2025-02-09T07:30:00Z"
    }
  },
  "waterLogs": {
    "2025-02-09": 6
  }
}
```

-----

### PWA Requirements

**manifest.json:**

```json
{
  "name": "Run90 — 90 Day Running Tracker",
  "short_name": "Run90",
  "start_url": "/index.html",
  "display": "standalone",
  "background_color": "#0a0a0a",
  "theme_color": "#0a0a0a",
  "orientation": "portrait",
  "icons": [
    { "src": "icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

**Service Worker (sw.js):**

- Cache-first strategy for all app assets (index.html, manifest, icons, Google Fonts)
- Handle notification click events (open app to dashboard)
- Activate immediately on install

**Notifications:**

- When user enables reminders, request `Notification.permission`
- Schedule daily notification at the configured time using `setTimeout` loop in the main app
- For iOS: notifications only work when added to home screen — show an install banner at top of app when not in standalone mode explaining this
- Notification messages should rotate through motivational messages referencing current day number and streak
- Use `ServiceWorkerRegistration.showNotification()` when available, fall back to `new Notification()`

**App Icons:**

- Generate simple icons programmatically: Create a canvas element, draw a dark background with the “R90” text in the accent gradient, then export as PNG data URLs
- Or provide a simple script that generates them
- The icons should look clean — dark bg, bold orange “R90” text

-----

### Install Banner

When the app detects it’s NOT running in standalone mode (`window.matchMedia('(display-mode: standalone)')` is false and `navigator.standalone` is not true):

- Show a top banner: “ Add to Home Screen for notifications & offline access”
- Dismissible with × button (remember dismissal in localStorage)
- On iOS, this is required for notifications to work

-----

### Key Behaviors

- **Streak calculation:** Count consecutive days (backward from today) where a log entry exists (including rest days)
- **Missed days:** Any past day without a log entry shows as red/missed in the calendar
- **Current day:** Calculated as `daysBetween(startDate, today) + 1`, clamped to 1-90
- **Pace format:** Always MM:SS per mile
- **Water tracker resets daily** — keyed by date string
- **All dates stored as YYYY-MM-DD strings**, parsed with `T12:00:00` suffix to avoid timezone issues
- **Modal closes on overlay tap**
- **Chart redraws on resize/orientation change**
- **Double-confirm on data reset** (two confirm dialogs)

-----

### Stretch Goals (implement if straightforward)

- **Confetti animation** when logging a run that hits a milestone (day 30, 60, 90, or new distance PR)
- **Weight tracker** — optional field in settings, small line chart on progress screen
- **Share card** — generate a shareable image of current progress (canvas → PNG)

-----

### What NOT to Build

- No backend / no API calls / no authentication
- No framework (React, Vue, etc.) — vanilla only
- No build step — must work by opening index.html or serving with any static file server
- No third-party JS libraries (no Chart.js, etc.)
- No complex routing — just show/hide screens

-----

### Testing Checklist

After building, verify:

- [ ] Onboarding flow works and persists data
- [ ] Can log a run, edit it, delete it
- [ ] Calendar grid correctly shows completed/missed/future/today
- [ ] Streak counter is accurate
- [ ] Stats (total miles, avg pace, total time) calculate correctly
- [ ] Water tracker fills/unfills and resets per day
- [ ] Chart renders with 2+ data points
- [ ] History list shows all entries reverse-chronologically
- [ ] Settings toggles and inputs persist
- [ ] Export downloads valid JSON
- [ ] Reset wipes everything and reloads
- [ ] Service worker caches assets for offline use
- [ ] Notification permission request fires when toggle is enabled
- [ ] Install banner shows when not in standalone mode
- [ ] App looks good on iPhone SE through iPhone 15 Pro Max sizes
- [ ] Safe areas are respected (notch, home indicator)
- [ ] Bottom nav doesn’t overlap content
