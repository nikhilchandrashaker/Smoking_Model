# Cumulative Lung Exposure Model

An interactive, single-file HTML visualization of how smoking exposure accumulates in
lung tissue over time — a pack-years slider drives an SVG lung illustration that darkens
on a saturating exponential curve as tar/particulate deposition builds up.

**Files**
- `lung_smoking_model.html` — the interactive itself. Open directly in any browser, no
  build step, no dependencies.
- `model_data_sources.md` — full breakdown of every constant behind the animation, and
  the real dataset numbers it's checked against.
- `lungs_subset_used.csv` — the 478-row Lungs subset pulled from `health_dataset.csv`,
  with pack-years computed, for anyone who wants to verify the findings independently.

## What it does

Drag the pack-years slider (0–40) and:
- the lung tissue color interpolates from healthy pink to soot black on
  `t = 1 − e^(−packYears / 11)` — fast change early, flattening later, matching how
  tar staining is described in gross pathology
- tar deposits render in as discrete spots at randomized (seeded) thresholds, rather
  than a single uniform fade
- a stats readout tracks cumulative cigarettes, estimated tar mass, a ciliary-function
  estimate, and % tissue visibly affected
- a cigarettes/day input lets you see how many years it takes to reach a given
  pack-years total at that rate

## Design note: model vs. data, kept separate on purpose

This project deliberately does **not** fit the animation to `health_dataset.csv`. I
checked first — the 478 Lungs records show no dose-response relationship between
pack-years and organ damage (r ≈ −0.07, wrong direction, never-smokers show the
*highest* damage rate), which points to the file being synthetic/randomly generated
rather than real patient data. Fitting a curve to noise would produce a confident-looking
but false result.

Instead:
- the animation is built from named, cited constants (pack-year definition, average
  tar/cigarette, a chosen saturation curve) — documented line-by-line in
  `model_data_sources.md`
- the dataset's actual numbers are shown in their own panel in the interactive, labeled
  as what they are, including the null finding

A companion UCI file (`lung-cancer.data`/`.names`) was evaluated and excluded — 32
samples, 56 anonymized 0–3 codes with no documented meaning, not usable without guessing.

## Stack

Vanilla HTML/CSS/SVG/JS. No frameworks, no build tools — chosen so the file is fully
portable and inspectable in one open.

## Status / possible next steps

- [ ] Swap in a real clinical or public-health dataset with an actual dose-response
      signal, if one becomes available
- [ ] Extend the same slider mechanic to the other organs already in
      `health_dataset.csv` (Liver, Kidney, Heart)
- [ ] Add a toggle to overlay the fitted-vs-real-data distinction directly on the chart
