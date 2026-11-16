# Project status and research decisions

**Last updated:** 16 November 2026  
**Repository:** [AlCu-Laser-Welding-ML](https://github.com/aadhilahamath07-pixel/AlCu-Laser-Welding-ML)

## Completed

- Repository structure and research documentation established.
- Original source files stored under `data/raw/`.
- Conventional and wobble source datasets standardised through notebooks.
- Master dataset assembled: 84 records across four source groups.
- Notebook 06 QC output reviewed: 84 records, 45 columns, no duplicate master IDs, duplicate source-record keys or exact duplicate rows.
- Target-specific eligibility flags and source-specific modelling subsets generated.
- Notebook 07 source-aware exploratory data analysis completed.
- First wobble-welding ML comparison completed using 29 complete records from 17 process conditions.
- Initial results documented in [INITIAL_WOBBLING_MODEL_RESULTS.md](INITIAL_WOBBLING_MODEL_RESULTS.md).
- Notebook 08 added for fold-wise stability checks and out-of-fold diagnostics.

## Current dataset composition

| Source | Records |
|---|---:|
| Conventional laser welding | 27 |
| Wobble laser welding | 36 |
| Shin et al. (2021) | 11 |
| Lee et al. (2022) | 10 |
| **Total** | **84** |

## Open data-quality questions

1. Interpret the six conventional records whose bead width, interface width and penetration depth are all zero. Keep them until source context resolves whether they represent no weld, no measurable fusion or another reporting convention.
2. Preserve seven missing wobble width measurements. Six have a “Specimen cut” note; one is unexplained.
3. Recheck Lee et al. (2022) force values for four flagged conditions against the original publication. Do not train on the force target until resolved.
4. Complete source-level metadata for the conventional and wobble datasets, including exact publication references and extraction locations.
5. Verify target definitions and experimental configurations before any cross-study pooling.

## Current target subset counts

| Target subset | Rows |
|---|---:|
| Conventional bead width | 21 |
| Conventional interface width | 21 |
| Conventional penetration depth | 21 |
| Wobble face bead width | 29 |
| Wobble interface bead width | 29 |
| Wobble penetration depth | 36 |
| Shin tensile-shear strength | 11 |
| Shin electrical resistance | 11 |
| Shin welding area | 11 |
| Lee average force | 0 |

## Initial ML experiment: wobble bead width

Prepared subset: 29 complete records, 17 unique process conditions, five conditions represented once and twelve represented twice. The three inputs were laser power, tangential wobbling speed and wobbling diameter. Face and interface bead widths were evaluated as separate targets.

Five-fold GroupKFold validation grouped rows by process condition. Pooled out-of-fold metrics:

| Target | Model | MAE (mm) | RMSE (mm) | R² |
|---|---|---:|---:|---:|
| Face bead width | Mean baseline | 0.3948 | 0.5262 | -0.1444 |
| Face bead width | Ridge regression | 0.3901 | 0.5204 | -0.1196 |
| Face bead width | Extra Trees | **0.2830** | **0.3979** | **0.3457** |
| Interface bead width | Mean baseline | **0.1036** | **0.1488** | **-0.0533** |
| Interface bead width | Ridge regression | 0.1231 | 0.1640 | -0.2806 |
| Interface bead width | Extra Trees | 0.1068 | 0.1655 | -0.3040 |

### Interpretation and decision

- Retain face bead width as the primary exploratory ML target for this wobble dataset. Extra Trees was better than the mean baseline on the reported pooled out-of-fold metrics, with an approximately 28.3% lower MAE.
- Interface bead width is not yet predictively supported: neither Ridge nor Extra Trees beat the mean baseline across the reported metrics.
- These results are preliminary. Only 29 observations and 17 process-condition groups are available. They do not establish external generalisation to other studies, equipment or material configurations.
- Fold-level stability and out-of-fold residual diagnostics are still required. Notebook 08 performs these checks.
- Do not claim an optimum process window, causal parameter effects or production-ready prediction from this experiment.

## Next research actions

1. Complete fold-wise performance and residual diagnostics with Notebook 08.
2. Decide whether face bead width performance is sufficiently stable to retain as an exploratory model.
3. Review data coverage and comparable target availability in the conventional dataset before selecting a second source-specific modelling experiment.
4. Do not pool different source groups without verifying material grades, thicknesses, joint configuration, laser setup and measurement definitions.
5. Improve source metadata and record exact publication table/page locations before manuscript preparation.

## Research integrity rules

- No fabricated values, undocumented corrections or silent row deletion.
- No blanket imputation before train/test splitting.
- Keep raw data immutable.
- Report small sample sizes and source heterogeneity honestly.
- Distinguish model association from physical causation.
- Do not claim a validated model or optimized welding window before validation and experimental confirmation.
