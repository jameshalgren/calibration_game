# Hydrologic Calibration Mode

Calibrate a simplified hydrologic model to match a hidden **"True"** streamflow hydrograph (a reference run using the full physics and the correct warm state). Simulated vs. True is plotted live with a real-time **Nash–Sutcliffe Efficiency (NSE)** score. Part of the **Headwaters** teaching toy.

## The lesson: the calibration trap
Some knobs are **structural**, not just numeric:
- **Elevation bands** and **aspect** (N/S slopes) control how snow melts. Turn them **off** and the model melts snow *uniformly* — no amount of tuning the melt factor can match the multi-band, two-timescale truth, and the **NSE stays poor**. The problem is the *missing knob*, not the wrong knob.
- A wrong **prior state** (cold/dry vs. warm/wet start) cannot be fixed by any slider during the warm-up period.

The diagnostic banner names exactly which structural issue is holding your NSE down.

## What you can tune
Region preset (Rocky Mountain / Eastern Missouri / California Sierras / Central Tropics), event vs. annual run length, gage placement (upstream / reservoir outflow / confluence), degree-day snow parameters, a two-bucket soil model (infiltration, storage, baseflow recession, ET, redistribution), and initial soil-moisture / channel-storage states. Lag/K routing is fixed in this version.

## Model summary
- **Snow:** temperature-index (degree-day) — `melt = DDF·(T−T₀)` when `T > T₀`, with a lapse rate per elevation band and a solar-driven melt split by aspect.
- **Soil:** two buckets — root zone (infiltration, ET, redistribution) and deep groundwater (linear-reservoir baseflow, `baseflow = k·storage`).
- **Routing:** Lag/K (travel-time lag + linear-reservoir attenuation), held fixed.

## Run
Open `index.html` in any modern browser and drag the sliders — the plots redraw live.

## Tech
Single self-contained HTML file. Vanilla JS with a hand-rolled `<canvas>` chart renderer (no Chart.js, no CDN). Chart axes are anchored to the fixed True series so they stay stable and readable while you tune; the renderer is hardened against non-finite values so a redraw can never blank the plot.
