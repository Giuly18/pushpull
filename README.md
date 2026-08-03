# Push / Pull Routine

A single-page workout reference: collapsible days, form cues, RIR targets, and last-done date tracking with a "what's next" suggestion.

## Deploying to GitHub Pages

1. Create a new repo on GitHub (public — Pages requires public on the free tier).
2. Upload every file in this folder to the repo root:
   - `index.html`
   - `manifest.webmanifest`
   - `sw.js`
   - `icon-192.png`, `icon-512.png`, `apple-touch-icon.png`
3. In the repo, go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, pick `main` and the `/ (root)` folder, then Save.
5. Wait a minute or two. Your URL will be:
   `https://<your-username>.github.io/<repo-name>/`

## Adding it to your phone's home screen

**iPhone (Safari — must be Safari, not Chrome):**
Open the URL → Share button → Add to Home Screen.

**Android (Chrome):**
Open the URL → menu (⋮) → Add to Home screen / Install app.

It opens fullscreen with no browser bar, like a normal app.

## Offline

The service worker caches the page after the first visit, so it works without signal at the gym. It checks the network first, so when you push an update you get the new version as soon as you have a connection.

## Updating the routine

The workout data is the `ROUTINE` array near the top of the `<script>` block in `index.html`. Each day looks like:

```js
{
  id:"push1",                    // do not change - keys your saved dates
  name:"Push 1",
  tag:"Chest / Shoulders / Quads",
  finisher:"Sled Push",
  priority:"optional note shown above the main work",
  prep:[ {name, sets, desc}, ... ],
  ex:[ {key, name, sets, rir, desc}, ... ]
}
```

Every exercise needs a unique `key` — that's what its logged sets are stored under. Changing a key orphans that exercise's history, so leave keys alone when you're only editing wording, sets or reps. Add `nolog:true` to an exercise to hide its logging box (used for planks and dead bugs).

Edit, commit, push. The page updates on your next visit with signal.

If you change `sw.js` or want to force a refresh for cached visitors, bump the `CACHE` version string (`pushpull-v1` → `pushpull-v2`).

## What's on the page

- **Up next** — the day you've gone longest without training.
- **Day cards** — tap to expand. Red stripe for push days, blue for pull.
- **Prep block** — posture and hip work, 6–8 minutes before the main lifts.
- **Set logger** — weight and reps per set, with a green chip when you beat last session on that set.
- **Session progress** — how many exercises you've logged so far today.
- **Daily mobility** — separate morning and evening blocks with a done-today tick and a running streak.
- **Reference** — collapsible notes at the bottom: RIR, progression, rest times, nutrition, recovery, posture, and when to see a professional.

## Logging sets

Each exercise has a set-by-set logger: weight and reps per set, with buttons to add or remove sets. Values save automatically when you tap out of a field.

Hitting **Log session** on a day does two things: stamps today's date, and snapshots everything you logged as that exercise's "last session" — the line shown above each logger, so you always know the numbers to beat.

Your typed values stay in the boxes between visits, so next session you just bump the weight up from where you were.

## Where your data lives

Two `localStorage` keys in your browser:

- `pushpull-dates` — when you last did each day
- `pushpull-log` — your sets, weights and reps
- `pushpull-mobility` — which days you completed the morning and evening mobility blocks

This is per-device and per-browser: it does not sync between your phone and laptop, and clearing site data for the domain erases it. Nothing is uploaded anywhere — no account, no server.
