# OLLIE/LOG 🛹

A standalone, installable web app (PWA) that turns your phone into a skate-session
tracker. No accounts, no servers — every byte of data stays on your phone.

## What it does

- **Counts your ollies** using the phone's accelerometer. The detector looks for the
  full signature of a real ollie — *pop spike → sustained near-weightlessness →
  landing impact* — so walking, pushing, and rough ground don't inflate your count.
  Every landed ollie gives you a beep + vibration and its **airtime in ms**.
- **Session stats** from the motion sensors and GPS:
  top speed (mph), distance, peak g-force, max lean angle (relative to how you
  carry the phone), best/average airtime, ollie rate, and a live accelerometer
  waveform while you skate.
- **GPS route logging** — every session records your route. On the session page you
  get a route map, an **Open in Apple Maps / Google Maps** button for the spot, and a
  **GPX export** you can open in any maps or fitness app.
- **Session log** — every session is saved with career totals (all-time ollies, best
  air, fastest ever, hardest hit) on the home screen.

## Get it on your phone

The sensors (motion + GPS) require HTTPS, so the app needs to be served — GitHub
Pages is already wired up:

1. Merge this branch to `main` (the `Deploy Skate Tracker to GitHub Pages` workflow
   deploys `skate-tracker/` automatically and enables Pages on first run).
2. Open `https://<your-username>.github.io/book_bot/` on your phone.
3. **iPhone:** Share → *Add to Home Screen*. It installs like a real app, works
   offline, and launches full-screen.

## Using it at the spot

1. Tap **Start session** — allow *Motion & Fitness* and *Location* when asked.
2. The screen must stay on while recording (iOS pauses sensors when the phone
   locks). Tap **Pocket mode** for a near-black battery-friendly screen, then put
   the phone in your front pocket, screen facing your leg. Triple-tap to exit.
3. Skate. Each clean ollie beeps and buzzes.
4. Tap **End session** — stats, route map, and exports are saved to the log.

**Detector sensitivity** (home screen): *Chill* counts scrappy little hops,
*Standard* is the default, *Strict* only counts crisp pops with real airtime.

Try **Load a demo session** on first launch to explore the stats/route UI before
you skate.

## Tech notes

- Single-file app: `index.html` (no build step, no dependencies).
- `sw.js` caches everything so it opens with zero bars.
- Sensors: `devicemotion` (60 Hz ollie state machine, g-force, low-passed gravity
  for lean angle), `geolocation.watchPosition` (speed/route, accuracy-gated),
  `wakeLock` (keep sensors alive), Web Audio + vibration for landing feedback.
- Data: `localStorage` (`ollielog.v1`), newest 200 sessions.
- The ollie detector is pure and dependency-free (marked `@detector-start/end` in
  `index.html`) and is covered by synthetic-trace tests: stationary, walking,
  rough pushing, dropped-phone freefall, and bailed soft landings all count 0;
  clean ollies count exactly once with correct airtime.
