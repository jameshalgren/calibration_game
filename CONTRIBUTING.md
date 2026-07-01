# Contributing to Hydrologic Calibration

Thanks for your interest in improving this teaching toy!

## Ground rules
- **Single file, zero build.** The entire app lives in `index.html` (HTML + CSS + vanilla JS). There is no build step, no bundler, and no runtime dependencies. Keep it that way — a contributor should be able to double-click the file and run it offline.
- **No external network calls or CDNs.** Charts are hand-rolled on `<canvas>`; please don't add a charting library.
- **Physics stays "simple but believable."** This is a teaching toy, not a production hydrologic model. Favor clarity and responsiveness over numerical rigor, and keep any added realism paired with a clear educational payoff.

## Running & testing
1. Open `index.html` in any modern browser.
2. Drag every slider, switch region presets, toggle Elevation bands / Aspect, and change the warm/cold prior-state sliders — confirm the hydrograph, SWE, and soil/ET plots all redraw cleanly and the NSE + diagnostic banner update.
3. There is no automated test runner shipped here. If you change the model math, sanity-check it by loading the page and confirming the visuals/plots update cleanly with no console errors.

## Making changes
- Match the surrounding code style (compact vanilla JS, terse helpers like `$`, `clamp`, `lerp`).
- Keep the frustration/lesson framing intact — the "broken knob" mechanics are the point, not accidents.
- Open a PR with a short description of the behavior before/after. Screenshots or a short screen capture help a lot for visual changes.

## Reporting bugs
Please include: browser + version, which control you changed, and what you expected vs. what happened (a console error message is gold).
