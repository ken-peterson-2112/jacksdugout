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

## Sharing data with co-coaches

The app is single-device by default (localStorage), but ships with a lightweight "publish via the repo" mechanism so the head coach can broadcast their roster, schedule, lineups, and pitch logs to anyone using the deployed site.

**The mechanism:** a `team-data.json` file lives in the repo root and is served alongside `index.html`. On every page load, the app fetches it. If its `publishedAt` timestamp is newer than what the visitor has applied locally, a banner appears: *"New team data published [date]. Apply it? [Apply] [Skip]"*. **Apply** overwrites the visitor's localStorage with the snapshot and reloads. **Skip** suppresses the banner until you publish a newer version.

### Head coach workflow (you publish)

1. Make your changes in the app as normal — schedule games, build lineups, log pitches, etc.
2. Click the **⚙** icon in the header → **Download team-data.json**.
3. Upload that file to the repo root (overwrite the existing `team-data.json`) via the GitHub web UI's "Upload files" → drag-and-drop → commit.
4. Push (or commit, if using web UI). GitHub Pages updates within ~30 seconds.
5. Co-coaches refreshing the site see the banner and click Apply.

### Co-coach workflow (they view)

1. Visit the deployed site.
2. On first visit, or whenever you publish, a banner offers to apply the latest team data. They click **Apply**.
3. From that point on they're looking at the same roster, schedule, and lineups you published. They can navigate, run reports, and even play with hypothetical lineups locally — those local edits stay on their device and don't propagate back.

### Importing a coach's suggested changes

If a co-coach wants to suggest changes (e.g. they exported their own JSON after playing with hypothetical lineups):

1. Have them click **⚙ → Download team-data.json** and send you the file.
2. On your device, click **⚙ → Choose file…** and pick their file. The app prompts for confirmation, then replaces your localStorage with theirs and reloads.
3. Review the changes. If you want to broadcast them to the team, click **⚙ → Download team-data.json** again and upload it to the repo as in the workflow above.

### The ⚙ menu

- **Download team-data.json** — exports your current view as a JSON snapshot.
- **Choose file…** — imports a JSON file (typically one a coach sent you), overwriting your local view.
- **Sync now** — re-fetches `team-data.json` from the deployed site and offers to apply it. Useful if you skipped an earlier banner and want to opt back in.

The modal also shows two timestamps for context: when you last applied a snapshot, and the current `publishedAt` on the server.

### `team-data.json` format

```json
{
  "publishedAt": 1714752000000,
  "roster": [ ... ],
  "schedule": [ ... ],
  "pitchLogs": [ ... ],
  "benchTiers": { ... }
}
```

`publishedAt` is a Unix milliseconds timestamp set by the export. The app uses it to decide whether the snapshot is newer than what the visitor has locally.

## Files

- `index.html` — the app
- `team-data.json` — published team snapshot (created on your first Export). Not in the repo until you publish.
- `archive/` — older versions, design mocks, sample game-day exports kept for reference
- `HANDOFF.md` — engineering handoff doc for future coding sessions

## Data persistence

State (roster, schedule, lineups, pitch logs) lives in browser `localStorage` per device. The `team-data.json` mechanism is what bridges devices — there is no backend. If you want real multi-device editing, flip `REQUIRE_AUTH = true` near the top of `index.html` to re-enable Supabase magic-link auth and cloud sync (set up custom SMTP first — see `HANDOFF.md`).
