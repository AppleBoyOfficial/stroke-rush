# Stroke Rush — project notes

Rhythm rowing endless runner ("Subway Surfers but rowing"). Installable web app (PWA) for iPhone, hosted on GitHub Pages:
https://appleboyofficial.github.io/stroke-rush/ (deploys from `main`, root folder).

## Files
- `index.html` — the whole game: canvas renderer, game logic, Web Audio music/SFX, input. No build step, no dependencies.
- `manifest.webmanifest` — PWA manifest (fullscreen, portrait).
- `sw.js` — service worker for offline play. Bump `CACHE` (e.g. `stroke-rush-v2`) whenever game files change.
- `icons/` — app icons (180 apple-touch, 192, 512, 512 maskable).

## Game design (as built)
- Pseudo-3D canal, 3 lanes at x = -1.15 / 0 / 1.15. `proj(x, z, y)` maps world to screen.
- Rowing: alternate left/right taps. Beat clock with tempo `T` (0.52s, speeds up to 0.36s with distance).
  Timing error < 0.06s = PERFECT, < 0.13s = GOOD, else OFF BEAT. Same side twice = OFF BALANCE; faster than T/2 = SPLASH.
- Swing meter 0–10 boosts speed, score multiplier, music layers; at 8+ coins are magnetized.
- Cox Cop chase: `gap` 0–100 closes when speed < cop speed; crash = gap -35, speed x0.6. gap <= 0 = game over.
- Obstacles: barge (switch lane), log (jump or switch), boom across canal (jump), low bridge (duck).
- Power-ups (8s): Coxswain (auto perfect rowing; manual taps ignored), Outboard (auto top speed), Eight (smash obstacles).
- Touch: stroke registered with timing from pointerdown; movement > 28px becomes a swipe instead.
- Keyboard: Z/X row, arrows move/jump/duck, P pause.
- Best score and total coins saved in localStorage.
- `window.__SR` is a test hook used by headless simulations.

## Known issues / next steps
- `sw.js` is cache-first, so installed copies won't pick up updates unless `CACHE` is renamed. Consider network-first for `index.html`.
- Not yet play-tested on a real iPhone; tap timing window and difficulty may need tuning.
- Idea backlog from the original pitch: more boats/rowers to unlock, city rotations (Venice, Amsterdam, Philadelphia's Boathouse Row, Atlantis), Bluetooth rowing-machine mode.

## Owner
User is on iPhone. Installs via Safari → Share → Add to Home Screen.
