# Dugout — 11U Lineup System

Single-file HTML app for managing a baseball team's roster, arm care, season schedule, and per-game lineups. Built for an 11U OBA team. Runs entirely in the browser; state persists in `localStorage` per device.

## Run locally

Open `index.html` in any modern browser. No build step.

## Deploy via GitHub Pages

1. Push this repo to GitHub (private or public).
2. In **Settings → Pages**, set the source to `main` branch, root folder.
3. Site is served at `https://<username>.github.io/<repo>/`.

Every push to `main` updates the deployed site within ~30 seconds. No build step, no deploy quota.

## Editing

The app is a single self-contained HTML file (`index.html`) with React loaded via CDN and Babel transforming JSX in the browser. To make changes, edit `index.html` and reload — no compile or install step.

## Files

- `index.html` — the app
- `archive/` — older versions, design mocks, sample game-day exports kept for reference

## Data persistence

State (roster, schedule, lineups, pitch logs) lives in browser `localStorage` and is per-device. To sync across phone/laptop, swap the `load`/`save` helpers near the top of the script for a backend (Supabase recommended).
