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
- Notebook 07 source-aware exploratory analysis refined and wobble ML subset exported.
- Wobble grouped ML evaluation completed in Notebook 08; initial results documented in [INITIAL_WOBBLING_MODEL_RESULTS.md](INITIAL_WOBBLING_MODEL_RESULTS.md).
- Conventional grouped ML evaluation completed in Notebook 09; results documented in [INITIAL_CONVENTIONAL_MODEL_RESULTS.md](INITIAL_CONVENTIONAL_MODEL_RESULTS.md).

## Repository storage status

The 84-record master dataset and standardised source CSVs have been generated and reviewed in Colab, but are not currently committed to GitHub. The repository folder structure and README guidance for `data/processed/`, `data/master/` and `results/` are now in place; this is a documentation/structure completion, not a claim that the generated data files have been uploaded. The repository tree currently has only `.gitkeep` placeholders in `data/processed/` and `data/master/`. The original source files are present in `data/raw/`. The Notebook 08/09 output ZIPs and their CSV metrics/predictions are also not committed. Therefore, the repository currently preserves the source files, analysis notebooks and written findings, but it is not yet a complete data/output archive. See [data/README.md](../data/README.md) and [NEXT_STEPS.md](NEXT_STEPS.md).

## Current dataset composition

| Source | Records |
|---|---:|
| Conventional laser welding | 27 |
| Wobble laser welding | 36 |
| Shin et al. (2021) | 11 |
| Lee et al. (2022) | 10 |
| **Total** | **84** |

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

## Conventional ML results (Notebook 09)

The evaluation used 21 eligible records from seven power–speed conditions, with three repeats per condition. Leave-one-process-condition-out validation kept each condition's repeats together. Pooled out-of-fold metrics:

| Target | Model | MAE (mm) | RMSE (mm) | R² |
|---|---|---:|---:|---:|
| Weld bead width | Mean baseline | 0.2742 | 0.3095 | -0.2794 |
| Weld bead width | Ridge | 0.1199 | 0.1488 | 0.7042 |
| Weld bead width | Extra Trees | **0.0984** | **0.1372** | **0.7487** |
| Interface width | Mean baseline | 0.0995 | 0.1291 | -0.1576 |
| Interface width | Ridge | 0.0993 | 0.1212 | -0.0212 |
| Interface width | Extra Trees | **0.0777** | **0.1140** | **0.0972** |
| Penetration depth | Mean baseline | 0.0580 | 0.0660 | -0.1564 |
| Penetration depth | Ridge | **0.0532** | **0.0648** | **-0.1163** |
| Penetration depth | Extra Trees | 0.0596 | 0.0775 | -0.5959 |

### Interpretation

- Conventional weld bead width is the strongest preliminary target: Extra Trees gives the best pooled metrics, with Ridge also substantially outperforming the mean baseline.
- Interface-width prediction is weak; Extra Trees improves error metrics modestly but pooled R² is only 0.0972.
- Penetration-depth prediction is not supported: all pooled R² values are negative.
- Fold-level R² is unstable and negative on average for all target/model combinations. There are only seven independent conditions, so the positive pooled bead-width R² must not be treated as evidence of robust generalization.
- Compared descriptively with the wobble face-width experiment (Extra Trees MAE 0.2830 mm, RMSE 0.3979 mm, R² 0.3457), conventional bead width has lower pooled errors and higher pooled R². These are separate studies/experiments and may differ in measurement definitions and process space; this is not evidence that one welding mode is intrinsically more predictable.

## Open data-quality questions

1. Verify the six conventional records whose bead width, interface width and penetration depth are all zero. Keep them in the master dataset until source context resolves their meaning.
2. Preserve seven missing wobble width measurements: six marked “Specimen cut” and one unexplained.
3. Recheck Lee et al. (2022) force values for four flagged conditions against the original publication; do not train on the force target until resolved.
4. Complete source-level metadata and exact publication table/page locations.
5. Verify target definitions and experimental configurations before cross-study pooling.

## Next research actions

1. Archive the reviewed 84-record master dataset, standardised source CSVs, audit and data dictionary in `data/` after checking redistribution permissions and recording exact source provenance.
2. Add the approved Notebook 08/09 metrics and out-of-fold predictions to a versioned `results/` location, with the notebook version and input-data checksum/version recorded.
3. Verify the original meaning of the six all-zero conventional records.
4. Expand the dataset with additional independent, comparable conventional and wobble process conditions.
5. Complete source metadata (material grades/thicknesses, joint geometry, beam/focus details, laser mode, shielding and measurement protocol).
6. Seek external validation on an independent study before making generalization claims.
7. Keep source-specific datasets separate unless comparability is demonstrated. Do not claim causal effects, an optimum process window or production readiness from the current small datasets.

See the detailed [next-step checklist](NEXT_STEPS.md).

## Research integrity rules

- No fabricated values, undocumented corrections or silent row deletion.
- No blanket imputation before train/test splitting.
- Keep raw data immutable.
- Report small sample sizes and source heterogeneity honestly.
- Distinguish model association from physical causation.
- Do not claim a validated model or optimized welding window before external validation and experimental confirmation.
