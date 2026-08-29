# What data actually went into the lung model

Two separate things fed the interactive, and they should not be confused with each other.

## 1. The darkening animation — NOT derived from your data

There's no dataset behind the slider→color curve. Your `health_dataset.csv` doesn't
show a usable dose-response relationship (see part 2), so instead of fitting anything,
I used a small set of named constants pulled from general smoking-pathology knowledge.
These are hardcoded in the HTML/JS, listed here explicitly:

| Constant | Value used | Basis |
|---|---|---|
| Pack-year definition | 20 cigarettes/day × 1 year | Standard clinical convention |
| Tar per cigarette | ~12 mg | Commonly cited average across cigarette brands (illustrative, not brand-specific) |
| Darkening curve | `t = 1 − e^(−packYears / 11)` | Saturating exponential — chosen to reflect that tar staining is described as fastest early and flattening later, not linear |
| Ciliary function estimate | `100% × (1 − 0.8t)` | Illustrative only — cilia impairment is real and progressive, but this exact curve is not from a study |
| Tissue affected % | `t × 100` | Directly tied to the same curve, for display only |

None of these numbers are patient data. They're my modeling choices, and I've flagged them
as illustrative in the interactive itself.

## 2. The "what your dataset showed" panel — IS from your file

Filtered `health_dataset.csv` to `Organ == "Lungs"` → **478 rows**. Full filtered subset
is attached as `lungs_subset_used.csv`.

Damage rate (`Organ_Condition == "Damaged"`) by `Smoking_Status`:

| Smoking Status | Damaged | Total | Rate |
|---|---|---|---|
| Never | 82 | 207 | 39.6% |
| Former | 41 | 132 | 31.1% |
| Current | 45 | 139 | 32.4% |

Correlation between computed pack-years (`Years_of_Smoking × Cigarettes_Per_Day / 20`)
and damage: **r ≈ −0.07** — essentially no relationship, and in the wrong direction.
Never-smokers show the *highest* damage rate. This is why I concluded the file is
likely randomly generated rather than sourced from real patients, and why none of it
was used to shape the animation curve above.

## Not used at all

`lung-cancer.data` / `lung-cancer.names` — UCI dataset, 32 samples, 56 attributes,
all anonymized nominal integers (0–3) with no documented meaning. No way to map these
to anything visualizable without guessing.
