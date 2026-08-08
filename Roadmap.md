# Next Steps & Questions — Calibration Game

**Single live planning doc.** Supersedes the loose working files (`doNow`, `doLater`,
`triageresponse`, `feedback_triage`, `user_feedback`), which are now archived verbatim in
`docs/archive/` as the primary source for the design history. This file carries the
still-open work forward, proposes the next edits, guesses a priority order, and asks you
the decisions that unblock the next commit.

**Status of the last batch:** Commits A–D + the real optimizer shipped (single headwater
gage, coarse slider grid + live update, DDF/T₀/ET/Aspect tooltips, multi-year run,
constrained `optimizeFit`, held-out warm-state cluster incl. Initial SWE). See
`docs/archive/doLater.md` for the detail.

**Status of THIS batch (2026-08):** shipped in `index.html`, not yet committed —
- **#1 Chart-jump fix (B-1):** `#calibBanner` given constant `height:100px; overflow-y:auto`.
- **#2 Copy pass (§C):** removed "calibration trap" everywhere (S2/A2/M6/M9), rewrote to
  "the model can't reproduce a response it has no process to represent." Applied flagged
  strings: title → "Headwater Calibration Game"; pill → "calibration jumpstarter · v3";
  L2 tightened; M10 trimmed to "Found the best fit: NSE ≈ X."
- **#4 Scramble knobs (B-2):** new **Scramble knobs** button — throws the 7 tunable
  sliders into the farther half of their grid (away from the gentle reset default),
  leaving structure toggles + warm state untouched. Reset stays gentle (Q3 decision).

**Decisions locked this batch:** Q3 = separate Scramble button (Reset stays gentle).
Q4 = optimization-cost display v1 is a **simple iteration counter + distance-traveled
readout** (no minima viz yet). Q5 = **Hint feature parked** ("maybe later", see §F).

---

## Driving user story (the north star for every decision below)

> A computer scientist wants to understand why her hydrologist advisor is frustrated with
> the effort to improve models. She plays this game hoping to learn *which dominant
> hydrologic processes* must be considered to achieve good storm response — so she can
> apply her CS skills to make beneficial improvements. Every tip, banner, and feature
> should move her toward understanding those dominant processes and the impact they have
> on the successful representation of basin hydrology we can expect from a model that does
> or does not include a capability to represent that process — either in the specific
> model formulation, or in the "hydrofabric" representation of the spatial topology of the
> watershed and river network.

---

## A. Carry-forward backlog (already logged, still open)

| Item | Source | State | Note |
|------|--------|-------|------|
| Tooltip **consistency pass** over existing tooltips | doLater | open | Light voice pass on infiltration/storage/baseflow/redistribution/bands/warm-state copy so all share one register. Folds into the instructive-copy pass (§D). |
| **Banner/hint educational design pass** | triageresponse #2 | open, no code | Decide what each banner should *teach*; part of §D. |
| **Scramble / Randomize knobs** | feedback #3 | held → now unblocked | Grid is coarse, so random values land on valid steps. Pairs with the "start-further-off" question (§C-2). |
| **#10 Reintroduce routing + channel warm state** | doLater #10 | held | Wire `chan0` into Lag/K warm state; couple to antecedent wetness (soil-moisture-derived channel quartile). Currently commented out in `index.html` (`TODO(doLater #10)`). |
| **#9 Dual-state view + Play animation** | doLater #9 | held | Largest ask; the simplified single-headwater SVG is the stepping stone. |

---

## B. New items from User feedback

### B-1. Chart jumps when the feedback box resizes — **DECIDED: fixed-size box**
The changing feedback/banner box size makes the chart jump around. **Decision:
reserve a constant height for the feedback/banner area (`#calibBanner`)** so the chart
never moves as the message length changes. (Bottom-relocation and modal overlay were the
alternatives; we're going with fixed-size — smallest, least disruptive.)
- *Proposed edit:* give `#calibBanner` a fixed `min-height` sized to the longest message,
  or a fixed-height scroll container. Verify at event/annual/multi and for every banner
  state (good / bad-with-reasons / perfect / optimized).
- *Priority:* **#1** (small, high daily-annoyance payoff).

### B-2. Start much further off + a "Get me close" / "Hint?" button — **OPEN**
Start from a much further-off state; maybe a "Get me close" or "Hint?" capability.
Overlaps feedback #3 (Scramble knobs).
- *Proposed edit:* a **Scramble / Hard-start** button that lands the sliders far from the
  truth (on-grid), and a **Hint** button that nudges one slider toward its optimum (a
  single coordinate-descent step from the existing `optimizeFit`).
- *Open question — see Q3.*
- *Priority:* **#3.**

### B-3. Show the optimization cost — **OPEN**
Show the cost — from far vs. near, how many iterations, and whether local/global
minima interfere. Purpose: begin to convey the *scaled-up cost of calibration at
continental scale* (the advisor's real frustration).
- *Proposed edit (v1 candidate):* have `optimizeFit` count sim evaluations + report
  "distance traveled" from the start point, surfaced after a run. Richer option: visualize
  the descent path and local-vs-global minima.
- *Open question — see Q4.*
- *Priority:* **#5** (high conceptual value; more design/code).

### B-4. Gradient fill on the mountains (snow accumulation/melt) — **OPEN**
Put a gradient fill on the mountains to simulate snow accumulation and melt. A
cheaper stepping-stone toward the full animation (#9); the SVG already toggles per-band
snow-cap opacity, so a gradient is an incremental step.
- *Priority:* **#6** (visual polish).

User **liked the graphic** and **appreciated the live-updating hydrograph** — no
action, just confirmation those landed.

---

## C. Instructive-elements improvement (the core of the user story)

The tips "could be improved" across the board. Two coordinated pieces:

1. **On-screen text revision** — every UI string is inventoried in the standalone
   **[`onscreen_text.md`](./onscreen_text.md)** with a blank *Your edit* column. Mark it up;
   I'll incorporate the edits in a copy commit. That marked-up file is also earmarked to
   become a GitHub issue (§E, Part 2).

2. **General review prompt** (reusable) — run this over all instructional copy:

   > "Review every piece of instructional text in the game (tooltips, diagnostic banners,
   > button labels, group titles) against the driving user story. For each, judge: does it
   > help the player *name a dominant hydrologic process* and understand the calibration
   > trap (a missing structural knob or wrong warm state sets a hard NSE ceiling no tuning
   > can lift)? Flag copy that (a) over-explains and pre-empts the player's own discovery,
   > (b) states a mechanism without connecting it to the hydrograph the player is watching,
   > or (c) uses jargon without grounding. For each group title, consider adding one line
   > naming *why that process dominates storm response* and what a model that omits it can
   > and cannot represent. Propose tighter replacement copy in the same voice, and note
   > where the graph should teach instead of the text."

- *Priority:* **#2** (directly serves the user story; no new mechanics).

---

## D. Priority order — updated

1. ~~**Chart-jump fix** (B-1)~~ — ✅ done this batch.
2. ~~**Instructive-copy pass** (§C)~~ — ✅ done this batch (core strings). Remaining: optional
   tooltip consistency sweep + refresh of the now-stale `onscreen_text.md` record.
3. ~~**Scramble knobs** (B-2 / #3)~~ — ✅ done this batch.
4. **Optimization-cost display** (B-3) — **NEXT.** Q4 decided: iteration counter + distance
   traveled.
5. **Mountain gradient snow fill** (B-4) — visual polish.
6. **Routing + channel warm state** (#10).
7. **Dual-state + Play animation** (#9).
8. **Documentation & history capture** (§E, two parts).
9. **Hint** (B-2) — parked, see §F.

---

## E. Held ToDo — Documentation & development-history capture (two parts)

Low priority; do after the copy/instructive passes so the pedagogy is settled first.

**Part 1 — Documentation site (motivation, history, background).** Options (pick at build
time):
- **MkDocs + Material theme** — Markdown in, static site out; hosts on GitHub Pages or Read
  the Docs. Best for a browsable "manual + design history."
- **GitHub Pages from `/docs`** — zero build step; GitHub renders the Markdown. Lowest effort.
- **Read the Docs proper** — versioned doc builds; heavier than a one-file game likely needs.

Suggested structure:
```
docs/
  index.md            ← the user story + what the game teaches (the "why")
  design-history.md   ← curated narrative distilled FROM docs/archive/
  archive/            ← the raw planning docs (already moved here; still untracked for now)
```

**Part 2 — Back-populate GitHub issues from the planning docs.** File detailed excerpts
from the archived docs (`doNow`, `doLater`, `triageresponse`, `feedback_triage`,
`user_feedback`, User feedback notes) as issues, grouped by the existing item/commit numbering,
preserving the Q&A annotations (they capture the *why*). Include a dedicated
**"instructional-text / copy revision" issue** built from the marked-up `onscreen_text.md`
— its *location · current · proposed* table becomes the issue checklist.

---

## F. Parked — "maybe do this later"

- **Hint / "Get me close" button (was B-2 / Q5).** Decided **not** to build for now. If
  revived: nudge one slider one grid-step toward its optimum (a single coordinate-descent
  step from `optimizeFit`, the gentle/exploratory option), *or* jump partway to best-fit.
  Reason parked: Scramble + the existing optimizer already cover the "far-off start →
  guided recovery" arc; a Hint risks doing the player's discovery for them.

---

## Questions back to you

1. **Priority order (§D):** confirmed roughly right. Any late reshuffle before we start
   commit #1?

2. **On-screen text:** please mark up [`onscreen_text.md`](./onscreen_text.md). Any global
   direction (e.g. shorten all tooltips, drop emoji, add a "why this process dominates
   storm response" line to each group title)?

3. **"Start further off" vs. Reset (B-2):** ✅ **ANSWERED** — separate **Scramble** button;
   Reset stays gentle. Shipped this batch.

4. **Optimization-cost display (B-3):** ✅ **ANSWERED** — v1 = simple iteration counter +
   "distance traveled" readout (no minima viz yet). This is the NEXT build item.

5. **Hint behavior (B-2):** ✅ **ANSWERED** — parked (see §F). Not building for now.

6. **Docs site (§E Part 1):** which target — MkDocs+Material, GitHub Pages from `/docs`, or
   Read the Docs? (Not needed until we schedule item #9-priority docs work.)

7. **Versioning:** the planning docs are archived but **left untracked** (no commit/push)
   per your call. Confirm they should stay untracked until Part 2 turns them into issues,
   or say the word to commit `docs/archive/` for safekeeping.
